---
description: >-
  Details on how to creating your own reports using Metabase, and the reporting
  tables.
---

# Building custom reports

## Build reports using Metabase

Metabase is a tool that allows us to explore the data in Simple, and build reports without engineering support. If you are new to Metabase, you can start with one of our [predefined templates](https://metabase.simple.org/collection/38), and explore further from there.

Alternatively, you can just click on "[Ask a new Question](https://metabase.simple.org/question/new)", and choose "Custom Question". You can refer to [Metabase's own documentation](https://www.metabase.com/docs/latest/users-guide/custom-questions.html) for a better understanding of how to use the query building interface.

Once you have generated a report, you can save it, share it with others, or even download the data to further analyse as spreadsheets or in other contexts.

Here's a quick demo exploring patient registration data on Metabase:

{% file src=".gitbook/assets/zoom\_2.mp4" caption="Demo: Building custom reports using Metabase" %}

Read on further to understand the data in each of the reporting tables you can access in the questions on Metabase.

## Overview of the reporting tables

The following table is a summary of each of the reporting tables, and what they contain. Please refer to the sections below for details of the columns in each of these tables.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Table Name</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Contents</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>reporting_patient_blood_pressures</b>
      </td>
      <td style="text-align:left">Summary of a patient&#x2019;s latest blood pressure</td>
      <td style="text-align:left">One row per patient, per month, from the month of the patient&apos;s registration.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>reporting_patient_visits</b>
      </td>
      <td style="text-align:left">Summary of a patient&apos;s latest visit where they had a BP/Sugar recording,
        an appointment, or a prescription drug refilled</td>
      <td style="text-align:left">One row per patient, per month, from the month of the patient&apos;s registration</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p><b>reporting_patient_states</b>
        </p>
      </td>
      <td style="text-align:left">Summary of a patient&apos;s health information and computed hypertension
        indicators.</td>
      <td style="text-align:left">One row per patient, per month, from the month of the patient&apos;s registration.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p><b>reporting_facility_states</b>
        </p>
      </td>
      <td style="text-align:left">Summary of a facility&#x2019;s registrations, treatment outcomes, and
        patients under care</td>
      <td style="text-align:left">One row per facility, per month, for all months since 2018</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p><b>reporting_quarterly_facility_states</b>
        </p>
      </td>
      <td style="text-align:left">A facility&#x2019;s quarterly treatment outcomes</td>
      <td style="text-align:left">One row per facility, per quarter, for all months since 2018</td>
    </tr>
  </tbody>
</table>

## How the reporting tables are created

The reporting tables are building blocks that are built on top of raw data. The following diagram illustrates which raw tables they are based on, and how we build larger even building blocks from these tables.

![Derivation of the reporting tables from raw data](.gitbook/assets/reporting-tables%20%283%29.png)

* **Raw data tables:** Data as recorded in the app goes into the transactional, or raw tables as seen in the first section of the diagram. These rows have all the detailed information collected as and when the nurse syncs the data via the Simple app. These tables have billions of rows, and are not well suited for querying reports.
* **Reporting tables:** These are summarised tables created from the raw data tables for ease of creating reports. The summaries can be per-patient-per-month, or per-facility-per-month. Please refer to the above diagram for the description of each table.
* **Dimension tables:** We can use these tables along with other reporting tables to filter and disaggregate data across time and geographical regions.

## Description of the data

{% tabs %}
{% tab title="reporting\_patient\_states" %}
```text
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

{% tab title="reporting\_facility\_states" %}
```text
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

## Detailed schema of the reporting tables

You can browse through the detailed schema on [github](https://github.com/simpledotorg/simple-server/tree/20104091caa3687f4aa988e5a6dc70a9a8bfff5d/public/documents), or on [metabase](https://metabase.simple.org/reference/databases/2/tables).

