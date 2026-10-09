
/************************************************************************/

/* Project : Banking Stress Testing Framework                           */

/* Program : Summary_Reporting.sas                                      */

/* Purpose : Generate Stress Testing Summary Reports                    */

/************************************************************************/


/*---------------------------------------------------------------------*/

/* Step 1 - Validate Input Dataset                                     */

/*---------------------------------------------------------------------*/



%macro check_dataset(dataset_name);

    %if %sysfunc(exist(&dataset_name.)) %then %do;
        %put NOTE: Dataset &dataset_name exists.;
    %end;

    %else %do;
        %put ERROR: Dataset &dataset_name does not exist.;
        %abort cancel;
    %end;

%mend check_dataset;


%check_dataset(portfolio_ecl_results);



/*---------------------------------------------------------------------*/

/* Step 2 - Portfolio Summary Report                                   */

/*---------------------------------------------------------------------*/


proc sql;

create table portfolio_summary as

select

    scenario_name,

    count(*) as total_accounts,

    sum(outstanding_balance)
        format=comma18.2
        as total_exposure,

    avg(base_pd)
        format=percent10.2
        as avg_base_pd,

    avg(stressed_pd)
        format=percent10.2
        as avg_stressed_pd,

    avg(base_lgd)
        format=percent10.2
        as avg_base_lgd,

    avg(stressed_lgd)
        format=percent10.2
        as avg_stressed_lgd,

    sum(baseline_ecl)
        format=comma18.2
        as total_baseline_ecl,

    sum(stressed_ecl)
        format=comma18.2
        as total_stressed_ecl,

    sum(ecl_absolute_change)
        format=comma18.2
        as total_ecl_increase

from portfolio_ecl_results

group by scenario_name

order by scenario_name;

quit;



/*---------------------------------------------------------------------*/

/* Step 3 - Product Level Summary                                      */

/*---------------------------------------------------------------------*/


proc sql;

create table product_summary as

select

    scenario_name,
    product_type,

    count(*) as account_count,

    sum(outstanding_balance)
        format=comma18.2
        as total_exposure,

    avg(stressed_pd)
        format=percent10.2
        as avg_stressed_pd,

    avg(stressed_lgd)
        format=percent10.2
        as avg_stressed_lgd,

    sum(stressed_ecl)
        format=comma18.2
        as total_stressed_ecl

from portfolio_ecl_results

group by scenario_name,
         product_type

order by scenario_name,
         product_type;

quit;



/*---------------------------------------------------------------------*/

/* Step 4 - Risk Segment Summary                                       */

/*---------------------------------------------------------------------*/


proc sql;

create table risk_segment_summary as

select

    scenario_name,
    risk_segment,

    count(*) as account_count,

    avg(stressed_pd)
        format=percent10.2
        as avg_stressed_pd,

    avg(stressed_lgd)
        format=percent10.2
        as avg_stressed_lgd,

    sum(stressed_ecl)
        format=comma18.2
        as total_stressed_ecl

from portfolio_ecl_results

group by scenario_name,
         risk_segment

order by scenario_name,
         risk_segment;

quit;



/*---------------------------------------------------------------------*/

/* Step 5 - DPD Bucket Summary                                         */

/*---------------------------------------------------------------------*/


proc sql;

create table dpd_summary as

select

    scenario_name,
    dpd_bucket,

    count(*) as account_count,
    

    avg(stressed_pd)
        format=percent10.2
        as avg_stressed_pd,


    sum(stressed_ecl)
        format=comma18.2
        as total_stressed_ecl


from portfolio_ecl_results


group by scenario_name,
         dpd_bucket


order by scenario_name,
         dpd_bucket;

quit;





/*---------------------------------------------------------------------*/

/* Step 6 - Portfolio Validation                                       */

/*---------------------------------------------------------------------*/



/*Scenario wise comparison*/


proc means data=portfolio_ecl_results

           n
           nmiss
           mean
           median
           std
           min
           p25
           p75
           max;
           

class scenario_name;


var

    base_pd
    stressed_pd
    base_lgd
    stressed_lgd
    baseline_ecl
    stressed_ecl;
    

title "Portfolio Validation Summary By Scenario";


run;



/*LGD Distribution By Scenario*/



proc freq data=portfolio_ecl_results;

tables scenario_name *
       stressed_lgd_band
       / missing;

title "LGD Distribution By Scenario";


run;


