
/************************************************************************/

/* Project: Banking Stress Testing Framework */

/* Program: Data_Preparation.sas */

/* Purpose: Prepare portfolio data for stress testing analysis */

/************************************************************************/



 
/* Step 1 - Import Portfolio Data */

 
proc import
datafile="portfolio_data.csv"
out=portfolio_raw
dbms=csv
replace;
guessingrows=max;
run;

 
/* Step 2 - Data Validation */

 
proc contents data=portfolio_raw;
run;

 
proc means data=portfolio_raw n nmiss min max mean;
run;

 
/* Step 3 - Missing Value Treatment */

 
data portfolio_clean;
    set portfolio_raw;

    if missing(credit_score)
       and not missing(previous_credit_score)
    then credit_score = previous_credit_score;

run;


/* Missing DPD Handling */

/* Missing DPD values should preferably be recalculated
   using due date and reporting date.
   Historical DPD values may be used if recalculation
   is not possible.
*/

/* Preferred Approach:
   Recalculate DPD from payment due dates
*/

if missing(current_dpd) then
   current_dpd = report_date - due_date;
   
/*If due dates are unavailable*/


if missing(current_dpd)
   and not missing(previous_dpd)
then current_dpd = previous_dpd;


/*If neither available*/

dpd_missing_flag='Y';

 
/* Step 4 - Customer Risk Segmentation */

 
data portfolio_segmented;
set portfolio_clean;
 
if credit_score >= 750 then risk_segment="Low Risk";
else if credit_score >=650 then risk_segment="Medium Risk";
else risk_segment="High Risk";
run;

 
/* Step 5 - DPD Bucketing */

 
data portfolio_dpd;
set portfolio_segmented;
 
if current_dpd = 0 then dpd_bucket="Current";
else if current_dpd <=30 then dpd_bucket="1-30";
else if current_dpd <=60 then dpd_bucket="31-60";
else if current_dpd <=90 then dpd_bucket="61-90";
else dpd_bucket="90+";
run;

 
/* Step 6 - Final Dataset */
 
proc freq data=portfolio_dpd;
tables risk_segment dpd_bucket;
run;
 
proc sort data=portfolio_dpd;
by customer_id;
run;
