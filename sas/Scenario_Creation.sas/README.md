
/************************************************************************/
/* Project : Banking Stress Testing Framework                           */
/* Program : Scenario_Creation.sas                                      */
/* Purpose : Create economic stress scenarios for risk analysis         */
/************************************************************************/


/*---------------------------------------------------------------------*/
/* Step 1 - Define Economic Scenarios                                  */
/*---------------------------------------------------------------------*/


data economic_scenarios;

    length scenario_name $20;
    

    /* Baseline Scenario */

    scenario_name = "Baseline";

    gdp_growth_rate       = 6.0;
    inflation_rate        = 5.0;
    unemployment_rate     = 4.0;
    benchmark_int_rate    = 6.5;
    hpi_change_pct        = 4.0;

    output;
    

    /* Mild Stress Scenario */

    scenario_name = "Mild Stress";

    gdp_growth_rate       = 3.0;
    inflation_rate        = 7.0;
    unemployment_rate     = 6.0;
    benchmark_int_rate    = 8.0;
    hpi_change_pct        = -2.0;

    output;


    /* Severe Stress Scenario */

    scenario_name = "Severe Stress";

    gdp_growth_rate       = -2.0;
    inflation_rate        = 9.0;
    unemployment_rate     = 10.0;
    benchmark_int_rate    = 10.0;
    hpi_change_pct        = -10.0;

    output;

run;


/*---------------------------------------------------------------------*/
/* Step 2 - Review Scenario Definitions                                */
/*---------------------------------------------------------------------*/


proc print data=economic_scenarios;
    title "Economic Stress Testing Scenarios";
run;


/*---------------------------------------------------------------------*/
/* Step 3 - Portfolio Stress Factors                                   */
/*---------------------------------------------------------------------*/



/* Example Stress Multipliers */


data stress_factors;

    length scenario_name $20;
    

    scenario_name = "Baseline";
    pd_factor     = 1.00;
    lgd_factor    = 1.00;
    ead_factor    = 1.00;
    output;
    

    scenario_name = "Mild Stress";
    pd_factor     = 1.25;
    lgd_factor    = 1.10;
    ead_factor    = 1.05;
    output;
    

    scenario_name = "Severe Stress";
    pd_factor     = 1.75;
    lgd_factor    = 1.35;
    ead_factor    = 1.10;
    output;

run;


/*---------------------------------------------------------------------*/
/* Step 4 - Validate Stress Factors                                    */
/*---------------------------------------------------------------------*/


proc print data=stress_factors;
    title "Scenario Stress Multipliers";
run;


/*---------------------------------------------------------------------*/
/* Step 5 - Create Combined Scenario Framework                         */
/*---------------------------------------------------------------------*/


proc sql;

create table scenario_framework as

select
    a.scenario_name,
    a.gdp_growth_rate,
    a.inflation_rate,
    a.unemployment_rate,
    a.benchmark_int_rate,
    a.hpi_change_pct,
    b.pd_factor,
    b.lgd_factor,
    b.ead_factor

from economic_scenarios a
left join stress_factors b
on a.scenario_name = b.scenario_name;

quit;


/*---------------------------------------------------------------------*/
/* Step 6 - Final Review                                                */
/*---------------------------------------------------------------------*/


proc print data=scenario_framework;
    title "Integrated Stress Testing Scenario Framework";
run;
