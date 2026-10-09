
/************************************************************************/

/* Project : Banking Stress Testing Framework                           */

/* Program : LGD_Stress_Model.sas                                       */

/* Purpose : Estimate baseline and scenario-adjusted Loss Given Default */

/*           and calculate Expected Credit Loss                         */

/************************************************************************/



/*---------------------------------------------------------------------*/

/* Important Model Note                                                */

/*---------------------------------------------------------------------*/


/*

   This portfolio program demonstrates an illustrative LGD stress-testing
   workflow.

   The product-level LGD assumptions, collateral adjustments, delinquency
   adjustments and scenario multipliers are examples only.

   In a production environment, LGD estimates should be based on:

   1. Historical recovery and write-off data;
   2. Discounted recovery cash flows;
   3. Collection and legal recovery costs;
   4. Collateral type, valuation and liquidation assumptions;
   5. Cure rates and time-to-recovery estimates;
   6. Downturn LGD requirements;
   7. Institution-approved and independently validated models.
      
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

%check_dataset(portfolio_pd_stressed);



/*---------------------------------------------------------------------*/

/* Step 2 - Validate Essential LGD Variables                            */

/*---------------------------------------------------------------------*/

/*


   Expected input variables include:

   customer_id
   loan_id
   product_type
   outstanding_balance
   collateral_value
   current_dpd
   base_pd
   stressed_pd
   scenario_name
   lgd_factor
   ead_factor

   If collateral_value is unavailable for unsecured products, zero may be
   appropriate only when the business definition confirms that no eligible
   collateral exists.

   Missing collateral for secured products must not automatically be
   interpreted as zero collateral. Such records should be flagged for
   investigation.
   
*/


data lgd_input_validation;
    set portfolio_pd_stressed;

    length lgd_data_status $35;

    secured_product_flag = 0;

    if upcase(strip(product_type)) in
       ("MORTGAGE", "HOME LOAN", "AUTO LOAN", "VEHICLE LOAN")
    then secured_product_flag = 1;

    if missing(outstanding_balance) then
        lgd_data_status = "Missing Outstanding Balance";

    else if outstanding_balance <= 0 then
        lgd_data_status = "Invalid Outstanding Balance";

    else if missing(product_type) then
        lgd_data_status = "Missing Product Type";

    else if secured_product_flag = 1
            and missing(collateral_value)
    then
        lgd_data_status = "Missing Secured Collateral";

    else
        lgd_data_status = "Ready for LGD Calculation";

run;



/*---------------------------------------------------------------------*/

/* Step 3 - Assign Illustrative Product-Level Base LGD                  */

/*---------------------------------------------------------------------*/



data portfolio_base_lgd;
    set lgd_input_validation;

    length lgd_model_status $35;

    if lgd_data_status ne "Ready for LGD Calculation" then do;

        product_base_lgd = .;
        base_lgd         = .;
        lgd_model_status = "Insufficient Data";

    end;
    else do;
    

        /*
        
           Illustrative product-level LGD assumptions.

           Secured lending generally has lower LGD because recoveries may
           be supported by eligible collateral. Unsecured lending generally
           has higher LGD.
           
        */

        select (upcase(strip(product_type)));

            when ("MORTGAGE", "HOME LOAN")
                product_base_lgd = 0.20;

            when ("AUTO LOAN", "VEHICLE LOAN")
                product_base_lgd = 0.35;

            when ("SME LOAN")
                product_base_lgd = 0.45;

            when ("PERSONAL LOAN")
                product_base_lgd = 0.60;

            when ("CREDIT CARD")
                product_base_lgd = 0.75;

            otherwise
                product_base_lgd = 0.50;

        end;

        lgd_model_status = "LGD Calculated";

    end;

    format product_base_lgd percent10.2;

run;



/*---------------------------------------------------------------------*/

/* Step 4 - Calculate Collateral Coverage and Recovery Adjustment       */

/*---------------------------------------------------------------------*/

/*

   Collateral Coverage Ratio:

       Collateral Coverage Ratio =
           Eligible Collateral Value / Outstanding Balance

   A conservative haircut is applied to collateral before calculating
   potential recovery.

   The haircut used below is illustrative.
   
*/

data portfolio_base_lgd;
    set portfolio_base_lgd;

    if lgd_model_status = "LGD Calculated" then do;

        if secured_product_flag = 1 then do;

            collateral_haircut = 0.20;

            adjusted_collateral_value =
                max(collateral_value * (1 - collateral_haircut), 0);

            collateral_coverage_ratio =
                adjusted_collateral_value / outstanding_balance;

            /*
               Recovery is capped at the outstanding exposure.
            */

            estimated_recovery_amount =
                min(adjusted_collateral_value, outstanding_balance);

            collateral_recovery_rate =
                estimated_recovery_amount / outstanding_balance;

            collateral_based_lgd =
                1 - collateral_recovery_rate;

            /*
               Use the higher of product-level LGD and collateral-based LGD
               as a conservative portfolio assumption.
            */

            pre_dpd_lgd =
                max(product_base_lgd, collateral_based_lgd);

        end;
        else do;

            collateral_haircut         = .;
            adjusted_collateral_value  = 0;
            collateral_coverage_ratio  = 0;
            estimated_recovery_amount  = 0;
            collateral_recovery_rate   = 0;
            collateral_based_lgd       = .;
            pre_dpd_lgd                = product_base_lgd;

        end;

    end;

    format collateral_haircut
           collateral_recovery_rate
           collateral_based_lgd
           pre_dpd_lgd percent10.2;

    format collateral_coverage_ratio 10.4;

run;




/*---------------------------------------------------------------------*/

/* Step 5 - Apply Delinquency Adjustment                               */

/*---------------------------------------------------------------------*/

/*

   More severe delinquency may reduce expected recoveries because of:

   - Longer recovery timelines;
   - Collection and legal expenses;
   - Collateral deterioration;
   - Increased cure uncertainty.

   These adjustments are illustrative.
   
*/


data portfolio_base_lgd;
    set portfolio_base_lgd;

    if lgd_model_status = "LGD Calculated" then do;

        if missing(current_dpd) then do;

            dpd_lgd_factor   = .;
            base_lgd         = .;
            lgd_model_status = "Missing DPD";

        end;
        else do;

            if current_dpd = 0 then
                dpd_lgd_factor = 1.00;

            else if current_dpd <= 30 then
                dpd_lgd_factor = 1.05;

            else if current_dpd <= 60 then
                dpd_lgd_factor = 1.10;

            else if current_dpd <= 90 then
                dpd_lgd_factor = 1.20;

            else
                dpd_lgd_factor = 1.35;

            /*
               Cap LGD below 100%.
            */

            base_lgd =
                min(pre_dpd_lgd * dpd_lgd_factor, 0.9999);

        end;

    end;

    format base_lgd percent10.2;

run;



/*---------------------------------------------------------------------*/

/* Step 6 - Apply Scenario-Based LGD and EAD Stress                     */

/*---------------------------------------------------------------------*/

/*

   stressed_lgd = base_lgd * lgd_factor
   stressed_ead = outstanding_balance * ead_factor

   The scenario multipliers originate from Scenario_Creation.sas.
   
*/


data portfolio_lgd_stressed;
    set portfolio_base_lgd;

    if lgd_model_status = "LGD Calculated" then do;

        if missing(lgd_factor) then do;

            stressed_lgd      = .;
            stressed_ead      = .;
            lgd_model_status  = "Missing LGD Scenario Factor";

        end;
        else if missing(ead_factor) then do;

            stressed_lgd      = .;
            stressed_ead      = .;
            lgd_model_status  = "Missing EAD Scenario Factor";

        end;
        else do;

            stressed_lgd =
                min(base_lgd * lgd_factor, 0.9999);

            stressed_ead =
                max(outstanding_balance * ead_factor, 0);

        end;

    end;

    format base_lgd stressed_lgd percent10.2;
    format outstanding_balance
           collateral_value
           adjusted_collateral_value
           estimated_recovery_amount
           stressed_ead comma18.2;

run;



/*---------------------------------------------------------------------*/

/* Step 7 - Calculate Baseline and Stressed Expected Credit Loss        */

/*---------------------------------------------------------------------*/

/*

   Expected Credit Loss:

       ECL = PD x LGD x EAD

   The calculation below assumes that PD and LGD use compatible horizons.

   For IFRS 9 or CECL implementations, the model must distinguish between:

   - 12-month PD and lifetime PD;
   - Point-in-Time and Through-the-Cycle PD;
   - Scenario weighting;
   - Discounted cash shortfalls;
   - Expected prepayments and contractual maturities.
   - 
*/

data portfolio_ecl_results;
    set portfolio_lgd_stressed;

    if not missing(base_pd)
       and not missing(base_lgd)
       and not missing(outstanding_balance)
    then
        baseline_ecl =
            base_pd * base_lgd * outstanding_balance;
    else
        baseline_ecl = .;


    if not missing(stressed_pd)
       and not missing(stressed_lgd)
       and not missing(stressed_ead)
    then
        stressed_ecl =
            stressed_pd * stressed_lgd * stressed_ead;
    else
        stressed_ecl = .;


    if not missing(baseline_ecl)
       and not missing(stressed_ecl)
    then do;

        ecl_absolute_change =
            stressed_ecl - baseline_ecl;

        if baseline_ecl > 0 then
            ecl_relative_change_pct =
                ((stressed_ecl / baseline_ecl) - 1) * 100;
        else
            ecl_relative_change_pct = .;

    end;


    format baseline_ecl
           stressed_ecl
           ecl_absolute_change comma18.2;

    format ecl_relative_change_pct 10.2;

run;



/*---------------------------------------------------------------------*/

/* Step 8 - Create Stressed LGD Risk Bands                             */

/*---------------------------------------------------------------------*/


data portfolio_ecl_results;
    set portfolio_ecl_results;

    length stressed_lgd_band $20;

    if missing(stressed_lgd) then
        stressed_lgd_band = "Not Calculated";

    else if stressed_lgd < 0.20 then
        stressed_lgd_band = "Very Low";

    else if stressed_lgd < 0.40 then
        stressed_lgd_band = "Low";

    else if stressed_lgd < 0.60 then
        stressed_lgd_band = "Moderate";

    else if stressed_lgd < 0.80 then
        stressed_lgd_band = "High";

    else
        stressed_lgd_band = "Very High";

run;



/*---------------------------------------------------------------------*/

/* Step 9 - Perform Data and Model Validation                          */

/*---------------------------------------------------------------------*/



proc freq data=portfolio_ecl_results;

    tables lgd_data_status
           lgd_model_status
           scenario_name * stressed_lgd_band
           / missing;

    title "LGD Data Quality and Risk-Band Validation";

run;


proc means data=portfolio_ecl_results
           n nmiss mean median min p25 p75 max;

    class scenario_name;

    var product_base_lgd
        collateral_coverage_ratio
        base_lgd
        stressed_lgd
        stressed_ead
        baseline_ecl
        stressed_ecl;

    title "LGD and Expected Credit Loss Results by Scenario";

run;



/*---------------------------------------------------------------------*/

/* Step 10 - Create Scenario and Product Summary                       */

/*---------------------------------------------------------------------*/


proc sql;

    create table lgd_ecl_summary as

    select
        scenario_name,
        product_type,

        count(*) as account_count,

        sum(
            case
                when lgd_model_status = "LGD Calculated"
                then 1
                else 0
            end
        ) as calculated_account_count,

        sum(
            case
                when lgd_model_status ne "LGD Calculated"
                then 1
                else 0
            end
        ) as exception_account_count,

        sum(stressed_ead)
            as total_stressed_ead
            format=comma18.2,

        mean(base_lgd)
            as average_base_lgd
            format=percent10.2,

        mean(stressed_lgd)
            as average_stressed_lgd
            format=percent10.2,

        sum(baseline_ecl)
            as total_baseline_ecl
            format=comma18.2,

        sum(stressed_ecl)
            as total_stressed_ecl
            format=comma18.2,

        sum(ecl_absolute_change)
            as total_ecl_increase
            format=comma18.2

    from portfolio_ecl_results

    group by
        scenario_name,
        product_type

    order by
        scenario_name,
        product_type;

quit;



/*---------------------------------------------------------------------*/

/* Step 11 - Review Final Summary                                      */

/*---------------------------------------------------------------------*/


proc print data=lgd_ecl_summary noobs;

    title "Product-Level LGD and Expected Credit Loss Summary";

run;



/*---------------------------------------------------------------------*/

/* End of Program                                                      */

/*---------------------------------------------------------------------*/


title;

