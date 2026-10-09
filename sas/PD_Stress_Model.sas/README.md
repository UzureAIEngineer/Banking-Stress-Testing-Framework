
/************************************************************************/

/* Project : Banking Stress Testing Framework                           */

/* Program : PD_Stress_Model.sas                                        */

/* Purpose : Estimate baseline and scenario-adjusted Probability of     */

/*           Default for each loan/customer record                      */

/************************************************************************/


/*---------------------------------------------------------------------*/

/* Important Model Note                                                */

/*---------------------------------------------------------------------*/

/*

   This portfolio program demonstrates the PD stress-testing workflow.


   The score-based PD rules and scenario multipliers are illustrative.
   In a production implementation, they must be replaced by:


   1. Institution-approved model coefficients;
   2. Validated credit-risk scorecards;
   3. Calibrated macroeconomic relationships; and
   4. Model Risk Management-approved scenario adjustments.

      
*/



/*---------------------------------------------------------------------*/

/* Step 1 - Validate Required Input Datasets                            */

/*---------------------------------------------------------------------*/



%macro check_dataset(dataset_name);

    %if %sysfunc(exist(&dataset_name.)) %then %do;
        %put NOTE: Required dataset &dataset_name. is available.;
    %end;
    %else %do;
        %put ERROR: Required dataset &dataset_name. does not exist.;
        %abort cancel;
    %end;

%mend check_dataset;

%check_dataset(portfolio_dpd);
%check_dataset(scenario_framework);



/*---------------------------------------------------------------------*/

/* Step 2 - Create Baseline PD                                         */

/*---------------------------------------------------------------------*/

/*

   Existing current-period credit scores must be retained.

   Missing credit scores should be treated in Data_Preparation.sas by:

   1. Using the customer's latest available historical score;
   2. Using an approved segment-level imputation rule when necessary; or
   3. Flagging the record for data-quality remediation.

   The following rules create an illustrative baseline PD using the
   current credit score and current DPD.
   
*/

data portfolio_base_pd;
    set portfolio_dpd;

    length pd_model_status $30;


    /*
       Do not calculate PD when essential risk attributes remain missing.
    */
    

    if missing(credit_score) or missing(current_dpd) then do;

        base_pd         = .;
        pd_model_status = "Insufficient Data";

    end;
    else do;

        pd_model_status = "PD Calculated";


        /*
           Illustrative credit-score-based starting PD.
           These values are not production model coefficients.
        */
        

        if credit_score >= 800 then
            score_based_pd = 0.005;

        else if credit_score >= 750 then
            score_based_pd = 0.010;

        else if credit_score >= 700 then
            score_based_pd = 0.025;

        else if credit_score >= 650 then
            score_based_pd = 0.050;

        else if credit_score >= 600 then
            score_based_pd = 0.100;

        else
            score_based_pd = 0.200;



        /*
           Illustrative delinquency adjustment.
           Higher DPD results in a higher probability of default.
        */
        

        if current_dpd = 0 then
            dpd_adjustment = 1.00;

        else if current_dpd <= 30 then
            dpd_adjustment = 1.25;

        else if current_dpd <= 60 then
            dpd_adjustment = 1.75;

        else if current_dpd <= 90 then
            dpd_adjustment = 2.50;

        else
            dpd_adjustment = 4.00;



        /*
           Calculate baseline PD.
           PD is capped below 100%.
        */
        

        base_pd = min(score_based_pd * dpd_adjustment, 0.9999);

    end;

    format score_based_pd base_pd percent10.2;

run;



/*---------------------------------------------------------------------*/

/* Step 3 - Review Baseline PD Distribution                            */

/*---------------------------------------------------------------------*/



proc means data=portfolio_base_pd
           n nmiss mean median min p25 p75 max;

    var credit_score
        current_dpd
        score_based_pd
        base_pd;

    title "Baseline Probability of Default Summary";

run;

proc freq data=portfolio_base_pd;

    tables risk_segment
           dpd_bucket
           pd_model_status
           / missing;

    title "Baseline PD Data and Risk Segmentation Review";

run;



/*---------------------------------------------------------------------*/

/* Step 4 - Apply Stress Scenarios                                     */

/*---------------------------------------------------------------------*/


/*
   Each portfolio record is combined with every economic scenario.

   stressed_pd = base_pd * pd_factor

   The pd_factor comes from Scenario_Creation.sas.
*/


proc sql;

    create table portfolio_pd_stressed as

    select
        a.*,
        b.scenario_name,
        b.gdp_growth_rate,
        b.inflation_rate,
        b.unemployment_rate,
        b.benchmark_int_rate,
        b.hpi_change_pct,
        b.pd_factor,

        case
            when missing(a.base_pd) then .
            else min(a.base_pd * b.pd_factor, 0.9999)
        end as stressed_pd format=percent10.2

    from portfolio_base_pd as a,
         scenario_framework as b

    order by
        a.customer_id,
        b.scenario_name;

quit;



/*---------------------------------------------------------------------*/

/* Step 5 - Calculate Absolute and Relative PD Changes                  */

/*---------------------------------------------------------------------*/


data portfolio_pd_stressed;
    set portfolio_pd_stressed;

    if not missing(base_pd) and not missing(stressed_pd) then do;

        pd_absolute_change = stressed_pd - base_pd;

        if base_pd > 0 then
            pd_relative_change_pct =
                ((stressed_pd / base_pd) - 1) * 100;
        else
            pd_relative_change_pct = .;

    end;

    format base_pd
           stressed_pd
           pd_absolute_change percent10.2;

    format pd_relative_change_pct 10.2;

run;



/*---------------------------------------------------------------------*/

/* Step 6 - Create PD Risk Bands                                       */

/*---------------------------------------------------------------------*/


data portfolio_pd_stressed;
    set portfolio_pd_stressed;

    length stressed_pd_band $20;

    if missing(stressed_pd) then
        stressed_pd_band = "Not Scored";

    else if stressed_pd < 0.02 then
        stressed_pd_band = "Very Low";

    else if stressed_pd < 0.05 then
        stressed_pd_band = "Low";

    else if stressed_pd < 0.10 then
        stressed_pd_band = "Moderate";

    else if stressed_pd < 0.20 then
        stressed_pd_band = "High";

    else
        stressed_pd_band = "Very High";

run;



/*---------------------------------------------------------------------*/

/* Step 7 - Perform Scenario-Level Validation                          */

/*---------------------------------------------------------------------*/


proc means data=portfolio_pd_stressed
           n nmiss mean median min p25 p75 max;

    class scenario_name;

    var base_pd
        stressed_pd
        pd_absolute_change
        pd_relative_change_pct;

    title "PD Results by Economic Scenario";

run;

proc freq data=portfolio_pd_stressed;

    tables scenario_name * stressed_pd_band
           / missing norow nocol;

    title "Stressed PD Band Distribution by Scenario";

run;



/*---------------------------------------------------------------------*/

/* Step 8 - Create Segment-Level PD Summary                            */

/*---------------------------------------------------------------------*/



proc sql;

    create table pd_segment_summary as

    select
        scenario_name,
        risk_segment,
        dpd_bucket,

        count(*) as account_count,

        sum(
            case
                when not missing(stressed_pd) then 1
                else 0
            end
        ) as scored_account_count,

        sum(
            case
                when missing(stressed_pd) then 1
                else 0
            end
        ) as unscored_account_count,

        mean(base_pd)
            as average_base_pd
            format=percent10.2,

        mean(stressed_pd)
            as average_stressed_pd
            format=percent10.2,

        max(stressed_pd)
            as maximum_stressed_pd
            format=percent10.2

    from portfolio_pd_stressed

    group by
        scenario_name,
        risk_segment,
        dpd_bucket

    order by
        scenario_name,
        risk_segment,
        dpd_bucket;

quit;



/*---------------------------------------------------------------------*/

/* Step 9 - Review Final Summary                                       */

/*---------------------------------------------------------------------*/


proc print data=pd_segment_summary noobs;

    title "Segment-Level Stressed PD Summary";

run;



/*---------------------------------------------------------------------*/

/* End of Program                                                      */

/*---------------------------------------------------------------------*/


title;

