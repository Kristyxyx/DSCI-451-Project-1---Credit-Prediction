# HW09

For the questions designed in the article, I picked 11 most interesting and non-trivial questions to answer for the dataset [**Public HMDA - LAR Data Fields**](https://ffiec.cfpb.gov/documentation/publications/loan-level-datasets/lar-data-fields) in data consumers' perspective across **all** 7 key stages of the dataset life cycle.\
The answers about dataset background highly referenced [FFIEC-CFPB](https://ffiec.cfpb.gov/) and [FFIEC](https://www.ffiec.gov/hmda/).

## Motivation

---

### 1. For what purpose was the dataset created?

The dataset was created to cater to the requirement of **The Home Mortgage Disclosure Act (HMDA)** to provides the public loan data that can be used to assist:

* in determining whether financial institutions are serving the housing needs of their communities;
* public officials in distributing public-sector investments so as to attract private investment to areas where it is needed;
* and in identifying possible discriminatory lending patterns.

> **The Home Mortgage Disclosure Act (HMDA)** was enacted by Congress in 1975 and was implemented by the Federal Reserve Board's Regulation C. On July 21, 2011, the rule-writing authority of Regulation C was transferred to the Consumer Financial Protection Bureau (CFPB).

### 2. Who created the dataset (for example, which team, research group) and on behalf of which entity (for example, company, institution, organization)?

The dataset is collected and maintained by the **Federal Financial Institutions Examination Council (FFIEC)**, which is a formal interagency body composed of several U.S. financial regulatory agencies:

* **Federal Reserve System (FRS)**
* **Federal Deposit Insurance Corporation (FDIC)**
* **National Credit Union Administration (NCUA)**
* **Office of the Comptroller of the Currency (OCC)**
* **Consumer Financial Protection Bureau (CFPB)**

The **CFPB** is the **primary agency** responsible for overseeing the **HMDA** data collection process, following the Dodd-Frank Wall Street Reform and Consumer Protection Act of 2010.

Actually, HMDA data is reported and also partially created by **qualified** financial institutions, including banks, savings associations, credit unions, and mortgage companies:

* **Depository Institutions**
  * **Banks, savings associations, and credit unions** that meet the following criteria:
    * Have assets exceeding a threshold set by the **Consumer Financial Protection Bureau (CFPB)** (this threshold is adjusted annually for inflation).
    * Have a home or branch office located in a **Metropolitan Statistical Area (MSA)**.
    * Originate at least one home purchase loan or refinancing of a home purchase loan secured by a first lien on a one-to-four unit dwelling.
    * Are federally insured or regulated, or originate loans insured by federal programs (like FHA or VA loans).

* **Nondepository Institutions (e.g., Mortgage Companies)**
  * Nondepository institutions, such as independent mortgage companies, must submit HMDA data if they meet the following conditions:
    * Originate at least 100 covered closed-end mortgage loans or 200 covered open-end lines of credit in each of the two preceding calendar years.
    * Meet a specific loan-volume threshold.
    * Conduct business in an **MSA**.

* **Other Financial Institutions**
  * Other financial institutions that meet the loan origination thresholds and operate in specific geographic areas are also required to submit HMDA data.

### 3. Who funded the creation of the dataset?

The financial institutions themselves are required to report this data as part of their regulatory compliance, so the cost of data collection is primarily borne by the **reporting institutions**, while the infrastructure for collecting and maintaining the data is funded by the above regulatory agencies, especially **CFPB**.

## Composition

---

### 5. What do the instances that comprise the dataset represent (for example, documents, photos, people, countries)?

The instances in the dataset are loaning records showing information from **4** parties:

* **Applicant**:
  * Income
  * Credit History
  * Down Payment
  * Demographic Information
  * No Direct Identifying Information

* **Loan**:
  * Mortgage Application
  * Loan Amount
  * Type of loan
  * Loan Purpose
  * Denial Reason

* **Lender**
  * Name of Lender
  * Regulator

* **Property**
  * Type of Property
  * Owner Occupancy
  * Census Tract

### 13. Are there any errors, sources of noise, or redundancies in the dataset?

* **Redundancies**:
  * The features groups `applicant_ethnicity`s, `co-applicant_ethnicity`s `aus`s and `denial_reason`s are highly redundant. Within each group of features, they reveal highly similar and parse information which also introduces noise. In addition, the feature `action_taken` and `denial_reason-1` also contain redundant information for: `action_taken` $= 1 \leftrightarrow$ `denial_reason-1` $= 9$.
* **Errors & noise**:
  * The are also missing values in the dataset bringing errors and noise.

## Collection Process

---

### 21. How was the data associated with each instance acquired? Was the data directly observable (for example, raw text, movie ratings), reported by subjects (for example, survey responses), or indirectly inferred/derived from other data (for example, part-of-speech tags, model-based guesses for age or language)?

The dataset is integrated from the reports of financial institutions by the **FFIEC**. For the base-level data collection in financial institutions, most of the features is formally filled while part of them is left for derivation (from handwritings in forms) \inference and further validation:

* **Derivation**:
  * `derived_msa-md`: From applicant/borrower and co-applicant/co-borrower census tract field.
  * `derived_loan_product_type`: From Loan Type and Lien Status fields for easier querying of specific records.
  * `derived_dwelling_category`: From Construction Method and Total Units fields for easier querying of specific records.
  * `derived_ethnicity`: From applicant/borrower and co-applicant/co-borrower ethnicity fields.
  * `derived_race`: From applicant/borrower and co-applicant/co-borrower race fields.
  * `derived_sex`: From applicant/borrower and co-applicant/co-borrower sex fields.
  * `action_taken`: From `denial_reason-1` feature.

* **Inference**
  * `applicant_ethnicity_observed`: On the basis of visual observation or surname.
  * `co-applicant_ethnicity_observed`: On the basis of visual observation or surname.
  * `applicant_race_observed`: On the basis of visual observation or surname.
  * `co-applicant_race_observed`: On the basis of visual observation or surname.
  * `applicant_sex_observed`: On the basis of visual observation or surname.
  * `co-applicant_sex_observed`:On the basis of visual observation or surname.

No explicit validation are provided.

### 22. What mechanisms or procedures were used to collect the data (for example, hardware apparatuses or sensors, manual human curation, software programs, software APIs)?

The process of data collection is designed to obey the following mechanism:

1. **Software Programs and HMDA Platform**
   * **Qualified financial institutions (QFIs)** use proprietary/third-party **LAR** software to compile mortgage data
   * Data formatted per **HMDA** requirements (loan amounts, rates, demographics, outcomes)

2. **HMDA Filing Instructions Guide (FIG) and Data Specifications**
   * **CFPB** provides **FIG** and Data Specs with file requirements and validation rules
   * **QFIs** must use compliant software for data collection

3. **Online Submission via the HMDA Platform**
   * **QFIs** submit **LAR** files through **HMDA** Platform (web-based system by **FFIEC**/**CFPB**)
   * Electronic submission with data quality feedback

4. **Automated Data Validation**
   * Platform performs:
   * Syntactic validation (format checks)
   * Logical validation (data consistency)
   * Quality edits (pattern checks)

   Errors must be corrected before acceptance

5. **Manual Review and Correction**
   * **QFIs** conduct internal review
   * Can correct and resubmit if needed

6. **Post-Submission Procedures**
   * **CFPB**/**FFIEC** perform additional quality reviews
   * May request revisions for significant issues

Accordingly, the mechanisms and procedures used to collect HMDA data are validated through a combination of:

* **Regulatory oversight**
* **Regular updates**
* **Audit processes**
* **Feedback loops**

## Preprocessing/cleaning/labeling

---

### 33. Was any preprocessing/cleaning/labeling of the data done (for example, discretization or bucketing, tokenization, part-of-speech tagging, SIFT feature extraction, removal of instances, processing of missing values)?

The dataset was performed with **bucketing** and **labeling**.

* **Bucketing**\
  The dataset frequently uses bucketing for to preserve the privacy of loaners for the following features:
  * `total_units`
  * `debt_to_income_ratio`
  * `applicant_age`
  * `co.applicant_age`

* **Labeling**\
  The dataset uses labeling to outstand the key information `action_taken` from `denial_reason` as an ideal predicted feature, indicated by that `action_taken` $= 1 \leftrightarrow$ `denial_reason-1` $= 9$.

## Uses

---

### 40. Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses?

> The dataset was created to cater to the requirement of **The Home Mortgage Disclosure Act (HMDA)** to provides the public loan data that can be used to assist:
>
> * in determining whether financial institutions are serving the housing needs of their communities;
> * public officials in distributing public-sector investments so as to attract private investment to areas where it is needed;
> * and in identifying possible discriminatory lending patterns.

As the **Question 1** suggests, the dataset intentionally designs the following demographic features to split the instances and perform one of its purpose: **Identifying possible discriminatory lending patterns**:

* `derived_ethnicity`
* `derived_race`
* `derived_sex`
* `applicant_ethnicity.1`
* `co.applicant_ethnicity.1`
* `applicant_ethnicity_observed`
* `co.applicant_ethnicity_observed`
* `applicant_race.1`
* `co.applicant_race.1`
* `applicant_race_observed`
* `co.applicant_race_observed`
* `applicant_sex`
* `co.applicant_sex`
* `applicant_sex_observed`
* `co.applicant_sex_observed`
* `applicant_age_above_62`

> * **Labeling**\
  The dataset uses labeling to outstand the key information `action_taken` from `denial_reason` as an ideal predicted feature, indicated by that `action_taken` $= 1 \leftrightarrow$ `denial_reason-1` $= 9$.

As **Question 33** shows, the dataset labeled and distinguished accepted loaners and rejected loaners, highly possibly for the future use of **loan decision making prediction**.

## Distribution

---

### 48. Do any export controls or other regulatory restrictions apply to the dataset or to individual instances?

The dataset is subject to certain regulatory restrictions, but it is not subject to traditional export controls like those governing sensitive technologies or defense-related data. Instead, the key regulatory frameworks focus on **privacy protections and data security** to ensure that sensitive personal information is not improperly disclosed.

## Maintenance

---

### 56. If others want to extend/augment/build on/contribute to the dataset, is there a mechanism for them to do so?

Yes, you're absolutely right! **Submitting data** is indeed a key way that financial institutions contribute to the **Home Mortgage Disclosure Act (HMDA)** dataset. Let's clarify this point further.

### Financial Institutions' Role in **Submitting Data**

The primary way the dataset is extended and augmented is through **data submissions** from **financial institutions**. Besides normal submission, here's more it works:

* **Voluntary Submissions**
  * While certain institutions are **required** to submit HMDA data, some smaller institutions (based on asset size, location, or loan volume) may be **exempt**. However, these institutions can still **voluntarily submit** data if they choose to do so, thereby augmenting the overall dataset.
  * Similarly, financial institutions can voluntarily submit **additional data fields** beyond the minimum required, if they believe it would enhance transparency.

* **Corrected or Updated Submissions**
  * Institutions can also submit **corrected or updated data** after their initial submission, particularly if errors or discrepancies are identified.

* **Contributing to Future Data Collection**
  * Public comments and feedback during regulatory review periods can also influence the **future scope** of the data collected.
