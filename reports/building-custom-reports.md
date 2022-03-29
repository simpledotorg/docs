---
description: >-
  Details on how to creating your own reports using Metabase, and the reporting
  tables.
---

# Building custom reports

## Metabase

Metabase is a tool that allows us to explore the data in Simple, and build reports without engineering support. If you are new to Metabase, you can start with one of our [predefined templates](https://metabase.simple.org/collection/38), and explore further from there.

Alternatively, you can just click on "[Ask a new Question](https://metabase.simple.org/question/new)", and choose "Custom Question". You can refer to [Metabase's own documentation](https://www.metabase.com/docs/latest/users-guide/custom-questions.html) for a better understanding of how to use the query building interface.

Once you have generated a report, you can save it, share it with others, or even download the data to further analyse as spreadsheets or in other contexts.

Metabase is available for the following Simple environments.

| **Environment**                                              | **URL**                                                                     | **Purpose**                                        |
| ------------------------------------------------------------ | --------------------------------------------------------------------------- | -------------------------------------------------- |
| <p><strong>Production</strong></p><p><em>India</em></p>      | [https://metabase.simple.org](https://metabase.simple.org)                  | India’s IHCI program                               |
| <p><strong>Production</strong></p><p><em>Bangladesh</em></p> | [https://metabase.bd.simple.org](https://metabase.bd.simple.org)            | Bangladesh’s NHF hypertension control program      |
| **Sandbox**                                                  | [https://metabase-sandbox.simple.org](https://metabase-sandbox.simple.org)  | A stable playground for the Simple team to work in |

## Quickstart

If you're already familiar with Simple's data model and Metabase, you can get started with this quick demo.

{% embed url="https://youtu.be/ooyUfKSJlXU" %}

## **Getting Started**

To get started, open up the custom report template. Template links are provided below.

* India: [https://metabase.simple.org/question/343](https://metabase.simple.org/question/343)
* Bangladesh: [https://metabase.bd.simple.org/question/46](https://metabase.bd.simple.org/question/46)&#x20;
* Sandbox: [https://metabase-sandbox.simple.org/question/86](https://metabase-sandbox.simple.org/question/86) &#x20;

### 1. Open the template

To start writing your custom report, click on the “Show Editor” button shown below.\


![](https://lh4.googleusercontent.com/1nata6tVwUR28OgMikCAoN0iHxLYJAB-CTmDnpugo-f57fSUqkbYDJSb6Jg2tsGmV8qWVX47gXC0O3pztyAYPA9INpX\_4Oi5ZZbEwMCRE0Cdsej\_Cv5cSas7eXwFIx-ro6tZHOC9=s0)

This will open the report editor. It will look something like this. The key features are labeled in the image below.

![](https://lh6.googleusercontent.com/gDtn93HD3rcZOS\_XdpcnVn\_f1V3N1zTT4i\_GS970bFMU5lquNoRYaXILtmzvq4e6HV5GbKkpC3l\_d9OJNIoC4vzDmmn1iD\_qkpdxn9mzH6QReSAKihK4o6CcH6P4URTg1hL4nn2K=s0)

As is, this template generates a monthly report of cumulative hypertensive patients assigned per district in a single state over the last 24 months.

### 2. Filter the results

Add filters to the purple “Filter” section to limit the report to what you care about. For example, if you want a monthly cumulative report of controlled patients in this state, add the following filter:&#x20;

`Htn Treatment Outcome in Last 3 Months = controlled`

![](https://lh5.googleusercontent.com/9BJ7aQLBameBLZfzOuQAvhhOwhTwbakAFEqDTtgsJs8Qnvxr7awQRG9Tb36BRYIkeIuSvR30vkoqP\_gtK8SghXI4peosFf0sLBfzzsCS8Nh4RX5o8Teji9UBPBnRmVO-o6Vvl3dG=s0)

See the Reference at the end of this document for a full list of options.

### 3. Group the results

You can modify the green “Summarize” section on the right to add more facets to segment the results. For example, you can break down the results by facility by adding the following grouping:

`Assigned Facility Slug`

![](https://lh3.googleusercontent.com/-vGZxIolWAy6bVzWL6SX-eVykZQRmMPtIxcQY8w7hkn\_742cOB\_4HJ7Ygq9D3SIyHe4Sh9zTairiZBzzT08trntJPy-DuAJcXHygBLApHWlXil9sBZ4GQNC2\_DOBK2vm29PXSe9E=s0)

See the Reference below for a full list of grouping options.\


Read on further to understand the data in each of the reporting tables you can access in the questions on Metabase.

## **Key Reference**

The following are the key properties that can be used to filter and group the report’s results in the purple “Filter” section.

| **Field name**                                                                                                                                                                                                                                                                                                            | **Field description**                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`Month date`**                                                                                                                                                                                                                                                                                                          | The reporting month                                                                                                                                                               |
| <p><strong><code>Assigned organization slug</code></strong></p><p><strong><code>Assigned state slug</code></strong></p><p><strong><code>Assigned district slug</code></strong></p><p><strong><code>Assigned block slug</code></strong></p><p><strong><code>Assigned facility slug</code></strong></p>                     | The organization, state, district, block, or facility that the patients are assigned to.                                                                                          |
| <p><strong><code>Registration organization slug</code></strong></p><p><strong><code>Registration state slug</code></strong></p><p><strong><code>Registration district slug</code></strong></p><p><strong><code>Registration block slug</code></strong></p><p><strong><code>Registration facility slug</code></strong></p> | The organization, state, district, block, or facility that the patients were registered in.                                                                                       |
| **`Hypertension`**                                                                                                                                                                                                                                                                                                        | Is the patient diagnosed with hypertension? (yes/no)                                                                                                                              |
| **`Diabetes`**                                                                                                                                                                                                                                                                                                            | Is the patient diagnosed with diabetes? (yes/no)                                                                                                                                  |
| **`Htn Care State`**                                                                                                                                                                                                                                                                                                      | Is the patient under care, lost to follow-up, or dead                                                                                                                             |
| **`Treatment Outcome In Last 3 Months`**                                                                                                                                                                                                                                                                                  | <p>Patient’s controlled status based on the last 3 months. Possible values are:</p><ul><li>controlled</li><li>uncontrolled</li><li>visit but no BP</li><li>missed visit</li></ul> |
| **`Months since registration`**                                                                                                                                                                                                                                                                                           | Months since the patient’s registration, as of the reporting month                                                                                                                |
| **`Months since visit`**                                                                                                                                                                                                                                                                                                  | Months since the patient’s last visit (with or without a BP), as of the reporting month                                                                                           |
| **`Months since BP`**                                                                                                                                                                                                                                                                                                     | Months since the patient’s last BP measure, as of the reporting month                                                                                                             |

## **Tips & Tricks**

### **Registrations**

To run a report on registrations, add the following filter to the purple “Filter” section.

`Months since registration = 0`

![](https://lh6.googleusercontent.com/cPRpJc7V64Egd6\_g\_ekw6Nl1yQZFvcl-kZVnsxZAwPE2kfG-BUABCJMicpH0JE\_nff9JO-eNnnWIj0eS6BrbdsDjEhBq7pBIcCpjVAUETZnEhVWvGDJPNiowdNgbdSQzLj2bHAvl=s0)

### **Multiple filters**

If you want to run a report that includes multiple filters such as controlled patients, uncontrolled patients, missed visit patients, in a single report, you can use a grouping instead of a filter. Group by the appropriate property in the green “Summarize” section.

For example, to generate a report of controlled, uncontrolled, visit-but-no-bp, and missed-visit patients, add the following grouping:

`Treatment Outcome In Last 3 Months`

![](https://lh3.googleusercontent.com/F2kDjhhdTPZRHMPvaMWaeYvK2TYQEU5HzmgRjRsGZfNmFErOdYiNl\_OoIZYHnUrWfp2ZfAJWM62cj91wr4J7MPbBndwVnz7hzP\_9EZX1ekwj1U49GaBHu8ajixro6TUGnwL1ETm7=s0)

### **Enriching results**

Sometimes, you want to run a report where results are grouped by facility, but you also want block and district information next to each facility. In order to do so, simply add block and district as additional groupings in the green “Summarize” section, and they will be shown in the results.

![](https://lh5.googleusercontent.com/Dq215fW2Hjz9V3rD4RrzkeO8rQyV43uNe5YqsWqGtXxruPcSvsis4y8yXkXsDJszHIvvm-t5VP8WcsZcM2AcuLCHK0nPzJ1FldQxOo4Ft6Ws0cpsnnAH0RR4iXPtKH8tr7yC9jey=s0)

The results will have block and district information next to each facility.

![](https://lh4.googleusercontent.com/nJakfzN610UrdG1uKYxPIbApyVV\_bUgMeXz\_pRXm45pwCncUqZf\_0PUsfhHLrSDN9QnXNOwCUhfjPx4Kz5TEQrdQInKf\_9qgxHWNHvHi1IO1sJpv\_lULZV5k5qmtVh53RNDGojGE=s0)

## Detailed Reference

### Overview of the reporting tables

The following table is a summary of each of the reporting tables, and what they contain. Please refer to the sections below for details of the columns in each of these tables.

| Table Name                                                                      | Description                                                                                                              | Contents                                                                      |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
| **`reporting_patient_blood_pressures`**                                         | Summary of a patient’s latest blood pressure                                                                             | One row per patient, per month, from the month of the patient's registration. |
| **`reporting_patient_visits`**                                                  | Summary of a patient's latest visit where they had a BP/Sugar recording, an appointment, or a prescription drug refilled | One row per patient, per month, from the month of the patient's registration  |
| <p></p><p><strong><code>reporting_patient_states</code></strong></p>            |  Summary of a patient's health information and computed hypertension indicators.                                         | One row per patient, per month, from the month of the patient's registration. |
| <p></p><p><strong><code>reporting_facility_states</code></strong></p>           | Summary of a facility’s registrations, treatment outcomes, and patients under care                                       | One row per facility, per month, for all months since 2018                    |
| <p></p><p><strong><code>reporting_quarterly_facility_states</code></strong></p> | A facility’s quarterly treatment outcomes                                                                                | One row per facility, per quarter, for all months since 2018                  |

### How the reporting tables are created

The reporting tables are building blocks that are built on top of raw data. The following diagram illustrates which raw tables they are based on, and how we build larger even building blocks from these tables.

![Derivation of the reporting tables from raw data](<../.gitbook/assets/reporting-tables (3).png>)

* **Raw data tables:** Data as recorded in the app goes into the transactional, or raw tables as seen in the first section of the diagram. These rows have all the detailed information collected as and when the nurse syncs the data via the Simple app. These tables have billions of rows, and are not well suited for querying reports.
* **Reporting tables:** These are summarised tables created from the raw data tables for ease of creating reports. The summaries can be per-patient-per-month, or per-facility-per-month. Please refer to the above diagram for the description of each table.
* **Dimension tables:** We can use these tables along with other reporting tables to filter and disaggregate data across time and geographical regions.

### Description of the data

{% tabs %}
{% tab title="reporting_patient_states" %}
```
|----------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------|
| column_name                            | data_type         | description                                                                                                                                       |
|----------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------|
| patient_id                             | uuid              | ID of the patient                                                                                                                                 |
| gender                                 | character varying | Gender of the patient                                                                                                                             |
| current_age                            | double precision  | "Patient's age as of today, based on 'age', 'age_updated_at' and 'date_of_birth'. This will have the same value for a patient across all rows."   |
| month_string                           | text              | "String that represents a month, in YYYY-MM format"                                                                                               |
| hypertension                           | text              | "Has the patient been diagnosed with hypertension? Values can be yes, no, unknown, or null if the data is unavailable."                           |
| assigned_facility_id                   | uuid              | ID of the patient's assigned facility                                                                                                             |
| assigned_state_slug                    | character varying | Human readable ID of the patient's assigned facility's state                                                                                      |
| registration_facility_id               | uuid              | ID of the patient's registration facility                                                                                                         |
| registration_state_slug                | character varying | Human readable ID of the patient's registration facility's state                                                                                  |
| months_since_registration              | double precision  | "Number of months since registration. If a patient was registered on 31st Jan, it would be 1 month since registration on 1st Feb."                |
| quarters_since_registration            | double precision  | "Number of quarters since registration. If a patient was registered on 31st Dec, it would be 1 quarter since registration on 1st Jan."            |
| months_since_visit                     | double precision  | "Number of months since the patient's last visit. If a patient visited on 31st Jan, it would be 1 month since the visit on 1st Feb."              |
| quarters_since_visit                   | double precision  | "Number of quarters since the patient's last visit. If a patient visited on 31st Jan, it would be 1 quarter since the visit on 1st Jan."          |
| months_since_bp                        | double precision  | "Number of months since the patient's last BP recording. If a patient had a BP reading on 31st Jan, it would be 1 month since BP on 1st Feb."     |
| quarters_since_bp                      | double precision  | "Number of quarters since the patient's last BP recording. If a patient had a BP reading on 31st Jan, it would be 1 quarter since BP on 1st Jan." |
| htn_care_state                         | text              | "Is the patient under_care, lost_to_follow_up, or dead as of this month?"                                                                         |
| htn_treatment_outcome_in_last_3_months | text              | "For the visiting period of the last 3 months, is this patient's treatment outcome controlled, uncontrolled, missed_visit, or visited_no_bp?"     |
| htn_treatment_outcome_in_last_2_months | text              | "For the visiting period of the last 2 months, is this patient's treatment outcome controlled, uncontrolled, missed_visit, or visited_no_bp?"     |
| htn_treatment_outcome_in_quarter       | text              | "For the visiting period of the current quarter, is this patient's treatment outcome controlled, uncontrolled, missed_visit, or visited_no_bp?"   |
|----------------------------------------+-------------------+---------------------------------------------------------------------------------------------------------------------------------------------------|
```
{% endtab %}

{% tab title="reporting_facility_states" %}
```
             Column              |       Type        | Collation | Nullable | Default
---------------------------------+-------------------+-----------+----------+---------
 month_date                      | date              |           |          |
 month_string                    | text              |           |          |
 quarter_string                  | text              |           |          |
 facility_id                     | uuid              |           |          |
 facility_name                   | character varying |           |          |
 facility_type                   | character varying |           |          |
 facility_size                   | character varying |           |          |
 state_slug                      | character varying |           |          |
 cumulative_registrations        | bigint            |           |          |
 monthly_registrations           | bigint            |           |          |
 under_care                      | bigint            |           |          |
 lost_to_follow_up               | bigint            |           |          |
 dead                            | bigint            |           |          |
 cumulative_assigned_patients    | bigint            |           |          |
 controlled_under_care           | bigint            |           |          |
 uncontrolled_under_care         | bigint            |           |          |
 missed_visit_under_care         | bigint            |           |          |
 visited_no_bp_under_care        | bigint            |           |          |
 missed_visit_lost_to_follow_up  | bigint            |           |          |
 visited_no_bp_lost_to_follow_up | bigint            |           |          |
 patients_under_care             | bigint            |           |          |
 patients_lost_to_follow_up      | bigint            |           |          |
```
{% endtab %}
{% endtabs %}

### Detailed schema of the reporting tables

You can browse through the detailed schema on [github](https://github.com/simpledotorg/simple-server/tree/20104091caa3687f4aa988e5a6dc70a9a8bfff5d/public/documents), or on [metabase](https://metabase.simple.org/reference/databases/2/tables).
