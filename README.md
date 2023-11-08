# Lending Club Case Study

## Table of Contents
* [Problem Statement](#problem-statement)
* [Objectives](#objectives)
* [Dataset Analysis](#dataset-analysis)
* [Approach](#approach)
* [Technologies Used](#technologies-used)
* [Acknowledgements](#acknowledgements)
* [Glossary](#glossary)
* [Team](#team)

## Problem Statement

Lending Club is a platform that connects people seeking personal loans with investors willing to lend money. It primarily deals with urban customers and needs to decide whether to approve a loan application based on the applicant's profile.

Credit loss, or financial loss, primarily comes from lending to risky applicants, those who might not repay the loan. These defaulters, often labeled as 'charged-off,' cause the most significant loss to lenders.

The main goal is to reduce credit loss. There are two scenarios to consider:

Approving loans for applicants likely to repay them is profitable for the company due to interest earnings. Rejecting such applicants results in lost business opportunities.

On the other hand, approving loans for applicants unlikely to repay may lead to financial losses for the company when they default.

In essence, the company's challenge is to strike a balance between maximizing profitable loans and minimizing the risk of financial losses.
 
## Objectives
The goal is to find risky loan applicants in order to reduce credit loss. This case study focuses on identifying such applicants using exploratory data analysis (EDA).

In simpler terms, the company aims to discover the key factors that lead to loan default, the strong indicators of default. This knowledge will help the company manage its portfolio and assess risk effectively.

## DataSet Analysis

The dataset analyzes past loan applicants and their loan repayment behavior to identify patterns indicating loan default risk. It focuses on post-approval data and doesn't consider application rejection criteria. The analysis revolves around three key steps in the loan process: the borrower's requested loan amount, the lender's approval based on historical data, and the final loan amount offered by investors. This analysis informs strategies for loan approval, risk assessment, and actions like adjusting loan amounts and interest rates for risky applicants.

### Analysis based on Domain Understanding

#### Decision Matrix (loan_status)
* *Loan Accepted* - Three Scenarios
    * The decision matrix for loan status includes two main categories:
      1.Loan Accepted: This category has three possible scenarios:
      2.Fully Paid: The applicant has successfully repaid both the principal and interest.
      3.Current: The applicant is actively making payments, and the loan's tenure is ongoing; these applicants are not labeled as "defaulted."4
      4.Charged-off: The applicant has failed to make timely payments for an extended period, indicating loan default.
      5.Loan Rejected: In this case, the company rejected the loan application due to various reasons, and no transactional history for these applicants is available in the dataset.
#### Important Columns
  * Annual Income (annual_inc) 
  * Home Ownership (home_ownership) .
  * Employment Length (emp_length) 
  * Debt to Income (dti)  
  * State (addr_state) 

* **Loan Attributes**
  * Loan Ammount (loan_amt) 
  * Grade (grade)
  * Term (term)
  * Loan Date (issue_date)
  * Purpose of Loan (purpose)
  * Verification Status (verification_status)
  * Interest Rate (int_rate)
  * Installment (installment)
  * Public Records (public_rec) 
  * Public Records Bankruptcy  
  

### Data Set Analysis based on understanding of EDA 


#### Rows Analysis
- Summary Rows: No summary rows were there in the dataset
- Header & Footer Rows - No header or footer rows in the dataset
- Extra Rows - No column number, indicators etc. found in the dataset
- Find duplicate rows in the dataset and drop if there are
- Row Cleaning
    Delete summary row if any
    delete incorrect row
    delete empty row
    delete incorrect row
- Columns Cleaning
    Merge columns for creating unique identifiers if needed
    Split columns for more data: Split address to get State and City to analyse each separately
    Add column names: Add column names if missing
    Rename columns consistently: Abbreviations, encoded columns
    Delete columns: Delete unnecessary columns
    Align misaligned columns: Dataset may have shifted columns


### Columns Analysis of the Dataset

#### Drop Columns 

- Delete the Loan columns containing only null values **NA values** only. The **columns will be dropped**. 
    - `(next_pymnt_d, mths_since_last_major_derog, annual_inc_joint, dti_joint, verification_status_joint, tot_coll_amt, tot_cur_bal, open_acc_6m, open_il_6m, open_il_12m, open_il_24m, mths_since_rcnt_il, total_bal_il, il_util, open_rv_12m, open_rv_24m, max_bal_bc, all_util, total_rev_hi_lim, inq_fi, total_cu_tl, inq_last_12m, acc_open_past_24mths, avg_cur_bal, bc_open_to_buy, bc_util, mo_sin_old_il_acct, mo_sin_old_rev_tl_op, mo_sin_rcnt_rev_tl_op, mo_sin_rcnt_tl, mort_acc, mths_since_recent_bc, mths_since_recent_bc_dlq, mths_since_recent_inq, mths_since_recent_revol_delinq, num_accts_ever_120_pd, num_actv_bc_tl, num_actv_rev_tl, num_bc_sats, num_bc_tl, num_il_tl, num_op_rev_tl, num_rev_accts, num_rev_tl_bal_gt_0, num_sats, num_tl_120dpd_2m, num_tl_30dpd, num_tl_90g_dpd_24m, num_tl_op_past_12m, pct_tl_nvr_dlq, percent_bc_gt_75, tot_hi_cred_lim, total_bal_ex_mort, total_bc_limit, total_il_high_credit_limit)`
    
- There are multiple columns where the **values are only zero**, the **columns will be dropped**
- There are columns where the **values are constant**. They dont contribute to the analysis, **columns will be dropped**
- There are columns where the **value is constant but the other values are NA**. The column will be considered as constant. **columns will be dropped**
- There are columns where **more than 65% of data is empty** `(mths_since_last_delinq, mths_since_last_record)` - **columns will be dropped**
- **Drop columns `(id, member_id)`** as they are **index variables and have unique values** and dont contribute to the analysis
- **Drop columns `(emp_title, desc, title)`** as they are **discriptive and text (nouns) and dont contribute to analysis**
- **Drop redundant columns `(url)`**. On closer analysis url is a static path with the loan id appended as query. It's a redundant column to `(id)` column
- **Drop customer behaviour columns which represent data post the approval of loan** 
    - They contribute to the behaviour of the customer. Behaviour of the customer is recorded post approval of loan and not available at the time of loan approval. Thus these variables will not be considered in analysis and thus dropped
    - `(delinq_2yrs, earliest_cr_line, inq_last_6mths, open_acc, pub_rec, revol_bal, revol_util, total_acc, out_prncp, out_prncp_inv, total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, total_rec_late_fee, recoveries, collection_recovery_fee, last_pymnt_d, last_pymnt_amnt, last_credit_pull_d, application_type)`
    
#### Convert Column Format
- `(loan_amnt, funded_amnt, funded_amnt_inv)` columns are Object and will be converted to float
- `(int_rate, installment, dti)` columns are Object and will be converted to float
- **strip "month"** text from `term` column and convert to integer
- Percentage columns `(int_rate)` are object. **Strip "%"** characters and convert column to float
- `issue_d` column **converted to datetime format**

#### Standardise Values
- All currency columns are rounded off to 2 decimal places as currency are limited to cents/paise etc only.

#### Convert Column Values
- `loan_status` column converted to boolean **Charged Off = False and Fully Paid = True**. This converts the column into ordinal values
- `emp_length` converted to integer with following logic. Note < 1 year is converted to zero and 10+ converted to 10.
    - < 1 year: 0,  
    - 2 years: 2,  
    - 3 years: 3,  
    - 7 years: 7,  
    - 4 years: 4,
    - 5 years: 5,
    - 1 year: 1,
    - 6 years: 6,
    - 8 years: 8,
    - 9 years: 9,
    - 10+ years: 10

#### Added new columns
- verification_status_n added. Considering domain knowledge of lending = Verified > Source Verified > Not Verified. verification_status_n correspond to {Verified: 3, Source Verified: 2. Not Verified: 1} for better analysis
- issue_y is year extracted from issue_d
- issue_m is month extracted from issue_d

### Ignored Rows and Columns because of missing data
*  Columns with high percentage of missing values will be dropped **(65% above for this case study)**
*  Columns with less percentage of missing value will be imputed
*  Rows with high percentage of missing values will be removed **(65% above for this case study)**

## Approach
- Step 0 - Data Cleaning & Manipulation Checklist
- Step 1 - Dropping Rows - where loan_status = "Current"
- Step 2 - Dropping Columns based on EDA and Domain Knowledge
- Step 3 - Convert the data types
- Step 4 - Identify columns with blank values which need to be imputed
- Step 5 - Analysis of the dataset post cleanup
- Step 6 - Outlier Treatment
- Step 7 - **Analysis** - Univariate, Bivariate and Derived Metrics Analysis
- Step 8 - **Conclusions** Inferences and Recommendations


## Technologies Used
- [Python - Version 3.9.12](https://www.python.org/download/releases/3.0/)
- [numpy - Version 1.21.5](https://github.com/numpy)
- [pandas - Version 1.4.2](https://github.com/pandas-dev/pandas)
- [matplotlib - Version 3.5.1](https://github.com/matplotlib)
- [seaborn - Version 0.11.2](https://github.com/seaborn)
- [Jupyter Notebook - Version 3.3.2]()
- [JupyterLab - Version 6.4.11]()
- [Anaconda - Version 2.1.4]()
## Team
- Kshitiz Jaiswal
- Senbagam
