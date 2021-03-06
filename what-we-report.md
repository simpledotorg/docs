---
description: A brief description of our key indicators and how they're calculated
---

# What we report

In order to improve hypertension treatment for a clinic or even an entire population, you have to understand how the system is performing. You need to see if patients are getting healthy, if they're being treated, if they're coming back for follow-up visits, and if they're being registered into the system.

Simple collects the bare minimum of information necessary to identify a patient, treat the patient for hypertension, and schedule a follow-up. We aggregate this information to report the key indicators listed below.

## Registrations

**Monthly registered patients:** the number of patients registered at a facility during a month where the patient \(1\) is not deleted and \(2\) is hypertensive.

**Total registered patients:** the number of patients registered at a facility. _Note: this is calculated by adding monthly registered patients at a facility over time_

```text
/* Sample SQL query */
SELECT Count(DISTINCT "patients"."id") AS count_id,
       Date_trunc('month', "patients"."recorded_at"::timestamptz AT TIME ZONE 'ETC/UTC') AT TIME ZONE 'ETC/UTC' AS date_trunc_month_patients_recorded_at_timestamptz_at_time_zone_
FROM "patients"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
WHERE "patients"."deleted_at" IS NULL
  AND "patients"."registration_facility_id" = $1
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = $2
  AND ("patients"."recorded_at" IS NOT NULL)
GROUP BY date_trunc('month', "patients"."recorded_at"::timestamptz AT TIME ZONE 'ETC/UTC') AT TIME ZONE 'ETC/UTC' [["registration_facility_id", "2e768fb2-9de0-4a7d-a64d-9f5b4c1863f4"],["hypertension", "yes"]]"
```

**Why is this important?** Program managers monitor registration numbers to ensure healthcare workers are registering patients into the system.

## Total assigned patients

The number of patients a facility is responsible to control their hypertension where a patient \(1\) is not deleted \(2\) is hypertensive and \(3\) is not dead

```text
/* Sample SQL query */
SELECT COUNT(DISTINCT "patients"."id") AS count_id,
       DATE_TRUNC('month', "patients"."recorded_at"::timestamptz AT TIME ZONE 'ASIA/KOLKATA') AT TIME ZONE 'ASIA/KOLKATA' AS date_trunc_month_patients_recorded_at_timestamptz_at_time_zone_
FROM "patients"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
WHERE "patients"."deleted_at" IS NULL
  AND "patients"."assigned_facility_id" = $1
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = $2
  AND "patients"."status" != $3
  AND ("patients"."recorded_at" IS NOT NULL)
GROUP BY DATE_TRUNC('month', "patients"."recorded_at"::timestamptz AT TIME ZONE 'ASIA/KOLKATA') AT TIME ZONE 'ASIA/KOLKATA' [["assigned_facility_id", "fdbeb339-770f-4021-a81a-44f9bc9a5e79"],["hypertension", "yes"],["status", "dead"]]

```

**Why is this important?** This indicator represents the number of patients a facility is responsible for. It is the population base used to calculate indicators like BP controlled and Missed visits.

## BP controlled

The number of patients assigned to a facility registered before the last 3 months where the patient \(1\) is not deleted \(2\) is hypertensive \(3\) is not dead \(4\) has a BP measure taken within the last 3 months and \(5\) their last BP measure taken is &lt;140/90.

_Note: We display this key indicator as a rate where the denominator is total assigned patients registered before the last 3 months._

```text
/* Sample SQL query */
SELECT COUNT(*)
FROM
  (SELECT DISTINCT ON (latest_blood_pressures_per_patient_per_months.patient_id) *
   FROM "latest_blood_pressures_per_patient_per_months"
   WHERE (medical_history_hypertension = 'yes')
     AND "latest_blood_pressures_per_patient_per_months"."patient_status" != $1
     AND "latest_blood_pressures_per_patient_per_months"."assigned_facility_id" = $2
     AND (patient_recorded_at <= '2019-07-31 18:29:59.999999')
     AND (bp_recorded_at >= '2019-07-31 18:30:00'
          AND bp_recorded_at <= '2019-10-31 18:29:59.999999')
   ORDER BY latest_blood_pressures_per_patient_per_months.patient_id,
            bp_recorded_at DESC, bp_id) latest_blood_pressures_per_patient_per_months
WHERE (systolic < 140
       AND diastolic < 90) [["patient_status", "dead"],["assigned_facility_id", "399c52a8-4b49-41d4-8704-cc3985ac26e6"]]
```

**Why are patients registered within the last 3 months excluded?** Three months gives patients time to take their hypertension medication. Newly registered patients have uncontrolled blood pressure, and including them would not reflect an accurate picture of actual controlled patients.

**Why is this important?** This indicator reflects the overall health of a hypertension control program and is the most important indicator our system tracks.

## BP not controlled

The number of patients assigned to a facility, registered before the last 3 months where the patient \(1\) is not deleted \(2\) is hypertensive \(3\) is not dead \(4\) has a BP measure taken within the last 3 months and \(5\) the last BP measure taken is ≥140/90

_Note: We display this key indicator as a rate where the denominator is total assigned patients registered before the last 3 months._

```text
/* Sample SQL query */
SELECT COUNT(*)
FROM
  (SELECT DISTINCT ON (latest_blood_pressures_per_patient_per_months.patient_id) *
   FROM "latest_blood_pressures_per_patient_per_months"
   WHERE (medical_history_hypertension = 'yes')
     AND "latest_blood_pressures_per_patient_per_months"."patient_status" != $1
     AND "latest_blood_pressures_per_patient_per_months"."assigned_facility_id" = $2
     AND (patient_recorded_at <= '2019-07-31 18:29:59.999999')
     AND (bp_recorded_at >= '2019-07-31 18:30:00'
          AND bp_recorded_at <= '2019-10-31 18:29:59.999999')
   ORDER BY latest_blood_pressures_per_patient_per_months.patient_id,
            bp_recorded_at DESC, bp_id) latest_blood_pressures_per_patient_per_months
WHERE (systolic >= 140
       OR diastolic >= 90) [["patient_status", "dead"],["assigned_facility_id", "399c52a8-4b49-41d4-8704-cc3985ac26e6"]]
```

**Why is this important?** It shows which patients are coming back to care, but require continued hypertension treatment to control their blood pressure.

## Visited but no BP taken

The number of patients assigned to a facility, registered before the last 3 months where the patient \(1\) is not deleted \(2\) is hypertensive \(3\) is not dead \(4\) has at least one of the following in the last 3 months: an appointment created, a drug refilled, a blood sugar taken and \(4\) doesn't have any BPs recorded within the last 3 months.

_Note: We display this key indicator as a rate where the denominator is total assigned patients registered before the last 3 months._

```text
/* Sample SQL query */
SELECT COUNT(DISTINCT "patients"."id")
FROM "patients"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
LEFT OUTER JOIN latest_blood_pressures_per_patient_per_months ON patients.id = latest_blood_pressures_per_patient_per_months.patient_id
LEFT OUTER JOIN appointments ON appointments.patient_id = patients.id
AND appointments.device_created_at >= '2019-02-28 18:30:00'
AND appointments.device_created_at <= '2019-05-31 18:29:59.999999'
LEFT OUTER JOIN prescription_drugs ON prescription_drugs.patient_id = patients.id
AND prescription_drugs.device_created_at >= '2019-02-28 18:30:00'
AND prescription_drugs.device_created_at <= '2019-05-31 18:29:59.999999'
LEFT OUTER JOIN blood_sugars ON blood_sugars.patient_id = patients.id
AND blood_sugars.recorded_at >= '2019-02-28 18:30:00'
AND blood_sugars.recorded_at <= '2019-05-31 18:29:59.999999'
WHERE "patients"."deleted_at" IS NULL
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = $1
  AND "patients"."status" != $2
  AND (bp_recorded_at > '2018-05-31'
       AND bp_recorded_at < '2019-05-31 18:29:59.999999'
       OR patients.recorded_at >= '2018-05-31')
  AND "patients"."assigned_facility_id" IN $3
  AND (patients.recorded_at <= '2019-02-28 18:30:00')
  AND (appointments.id IS NOT NULL
       OR prescription_drugs.id IS NOT NULL
       OR blood_sugars.id IS NOT NULL)
  AND (NOT EXISTS
         (SELECT 1
          FROM blood_pressures bps
          WHERE patients.id = bps.patient_id
            AND bps.recorded_at >= '2019-02-28 18:30:00'
            AND bps.recorded_at <= '2019-05-31 18:29:59.999999')) [["hypertension", "yes"],
                                                                   ["status", "dead"],
                                                                   ["assigned_facility_id", "3a7e86d2-c272-4303-8ffa-d6d1b54874b3"]]
```

**Why is this important?** We started tracking this indicator during COVID-19, because patients were visiting facilities to pick up medications but didn't have their BP taken to avoid contact with healthcare workers and prevent COVID infection. This is not very common, but helps capture the entire patient base.

## Missed visits

The number of patients assigned to a facility, registered before the last 3 months where the patient \(1\) is not deleted \(2\) is hypertensive \(3\) is not dead and \(4\) doesn't have a visit or BP recorded within the last 3 months.

_Note: The SQL query is a combination of all the queries in the equation above_

_Note: We display this key indicator as a rate where the denominator is total assigned patients registered before the last 3 months._

```text
  Total assigned patients (registered before the last 3 months)
- Patients with a visit but no BP taken
- Patients with controlled BP
- Patients with uncontrolled BP
=
  Missed vists
```

**Why is this important?** This number reflects how good facilities are at reminding patients to come back to care in the 3-month period we're tracking controlled and uncontrolled patients.

## Lost to follow-up

The number of patients assigned to a facility where the patient \(1\) didn't have a BP recorded within the last year \(2\) was registered more than a year ago \(3\) is hypertensive \(4\) is not dead and \(5\) is not deleted.

_Note: We display this key indicator as a rate where the denominator is total assigned patients._

```text
/* Sample SQL query */
SELECT DISTINCT "patients".*
FROM "patients"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
LEFT OUTER JOIN latest_blood_pressures_per_patient_per_months ON patients.id = latest_blood_pressures_per_patient_per_months.patient_id
AND bp_recorded_at > '2020-04-30' /* Date from 1 year ago: 1 year is the LTFU period */
AND bp_recorded_at < '2021-04-30 23:59:59.999999' /* Current date */
WHERE "patients"."deleted_at" IS NULL
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = 'yes'
  AND "patients"."status" != 'dead'
  AND "patients"."assigned_facility_id" = 'ad0ff5e2-7f0f-467b-9855-07b894e4a6a7'
  AND (bp_recorded_at IS NULL
       AND patients.recorded_at < '2020-04-30') /* Date from 1 year ago */
```

**Why is this important?** The main key indicators exclude lost to follow-up patients to allow program managers to assess the health of patients that are coming back to care.

## Patients under care

The number of patients assigned to a facility where the patient \(1\) had a BP recorded within the last year \(2\) is hypertensive \(3\) is not dead and \(4\) is not deleted.

```text
SELECT DISTINCT "patients".*
FROM "patients"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
LEFT OUTER JOIN latest_blood_pressures_per_patient_per_months ON patients.id =  latest_blood_pressures_per_patient_per_months.patient_id
WHERE "patients"."deleted_at" IS NULL
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = 'yes'
  AND "patients"."status" != 'dead'
  AND "patients"."assigned_facility_id" = '1f783f44-3153-44f2-a99e-6587f31da6c5'
  AND (bp_recorded_at > '2020-05-31'              /* Date from 1 year ago: 1 year is the LTFU period */
       AND bp_recorded_at < '2021-05-13'          /* Current Date */
       OR patients.recorded_at >= '2020-05-31')   /* Date from 1 year ago */
```

**Why is this important?** Represents the number of "active" patients or the number of patients that aren't lost to follow-up.

## Follow-up patients

**Follow-up patients per user:** For a given period, the number of patients attended by a user where the patient \(1\) is hypertensive \(2\) is not deleted \(3\) was registered before that period and \(4\) had a BP taken, a blood sugar taken, an appointment scheduled, or their medications refilled during that period by that user.

**Follow-up patients per facility:** For a given period, the number of patients that visited a facility where the patient \(1\) is hypertensive \(2\) is not deleted \(3\) was registered before that period and \(4\) had a BP taken, a blood sugar taken, an appointment scheduled, or their medications refilled during that period at that facility.

**Follow-up patients per region:** For a given period, the sum of follow-up patients across all facilities in the region.

**Why is this important?** This indicator is an indicator used to monitor the facility's activity. Follow-up patients are often compared with total assigned patients because it shows what proportion of patients are getting treated.

## Total facilities

Facilities registered in Simple where the current user can view reports.

**Why is this important?** This allows program managers to see all the facilities they are responsible for.

## Inactive facilities

Facilities registered in Simple where the current user can view reports and where &lt;10 patients had a BP recorded in the last 7 days.

**How it's calculated:** First we calculate total active facilities \(Facilities where &gt;10 patients had any BPs recorded in the last 7 days\). Then we \(1\) grab all facilities the admin has access to \(2\) count the total number of patients with a BP taken in a day for the last 7 days at each facility and \(3\) return the number of facilities where the facility has had more than 10 patients with a BP taken in the last week.

```text
/* Sample SQL query */
SELECT "blood_pressures_per_facility_per_days"."facility_id"
FROM "blood_pressures_per_facility_per_days"
WHERE "blood_pressures_per_facility_per_days"."deleted_at" IS NULL
  AND "blood_pressures_per_facility_per_days"."facility_id" IN
    (SELECT "facilities"."id"
     FROM "facilities"
     WHERE "facilities"."deleted_at" IS NULL
       AND "facilities"."id" IN
         (SELECT "facilities"."id"
          FROM (
                  (SELECT "facilities".*
                   FROM "facilities"
                   WHERE "facilities"."deleted_at" IS NULL)
                UNION
                  (SELECT "facilities".*
                   FROM "facilities"
                   WHERE "facilities"."deleted_at" IS NULL
                     AND "facilities"."facility_group_id" IN
                       (SELECT id
                        FROM (
                                (SELECT "facility_groups".*
                                 FROM "facility_groups"
                                 WHERE "facility_groups"."deleted_at" IS NULL
                                   AND "facility_groups"."deleted_at" IS NULL)
                              UNION
                                (SELECT "facility_groups".*
                                 FROM "facility_groups"
                                 WHERE "facility_groups"."deleted_at" IS NULL
                                   AND "facility_groups"."deleted_at" IS NULL
                                   AND "facility_groups"."organization_id" IN
                                     (SELECT "organizations"."id"
                                      FROM "organizations"
                                      WHERE "organizations"."deleted_at" IS NULL))) "facility_groups"))) "facilities"))
  AND (((YEAR,
         DAY) IN (('2021',
                   '91'),('2021',
                          '90'),('2021',
                                 '89'),('2021',
                                        '88'),('2021',
                                               '87'),('2021',
                                                      '86'),('2021',
                                                             '85'))))
GROUP BY "blood_pressures_per_facility_per_days"."facility_id"
HAVING (SUM(bp_count) >= 10)
```

**Why is this important?** This indicator allows program managers to see which facilities may be facing technical issues with the system or healthcare workers that are forgetting to record BP measures into the system.

## Patients with BP measure taken

Number of unique patients with a BP measure taken in a given number of days. If a patient has multiple BPs in one day, they are only counted once.

**Why is this important?** This indicator allows program managers to quickly see if a facility is actively taking BP measurements.

## BP measure taken

Counts all blood pressures recorded by each healthcare worker at a facility where the patient \(1\) is hypertensive and \(2\) is not deleted.

```text
/* Sample SQL query */
SELECT Count(DISTINCT "blood_pressures"."id") AS count_id,
       Date_trunc('month', "blood_pressures"."recorded_at"),
       "blood_pressures"."user_id" AS blood_pressures_user_id
FROM "blood_pressures"
INNER JOIN "patients" ON "patients"."deleted_at" IS NULL
AND "patients"."id" = "blood_pressures"."patient_id"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
WHERE "blood_pressures"."deleted_at" IS NULL
  AND "patients"."deleted_at" IS NULL
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = $1
  AND ("blood_pressures"."recorded_at" IS NOT NULL)
  AND "blood_pressures"."facility_id" IN
    (SELECT "facilities"."id"
     FROM "facilities"
     WHERE "facilities"."deleted_at" IS NULL
       AND "facilities"."id" = $2) GROUP  BY Date_trunc('month', "blood_pressures"."recorded_at"),
                                             "blood_pressures"."user_id"
```

**Why is this important?** Similar to inactive facilities, this allows program managers to see which facilities are facing technical issues or facilities with healthcare workers that aren't recording BP measures into the system.

## BP log

All blood pressures recorded at a facility.

```text
/* Sample SQL query */
SELECT "blood_pressures".*
FROM "blood_pressures"
INNER JOIN "observations" ON "blood_pressures"."id" = "observations"."observable_id"
INNER JOIN "encounters" ON "observations"."encounter_id" = "encounters"."id"
WHERE "blood_pressures"."deleted_at" IS NULL
  AND "observations"."deleted_at" IS NULL
  AND "encounters"."deleted_at" IS NULL
  AND "encounters"."facility_id" = $1
  AND "observations"."observable_type" = $2
ORDER BY DATE(recorded_at) DESC, recorded_at ASC
LIMIT $3
OFFSET $4 [["facility_id", "acc3da36-c5d2-42e1-a1fe-29d6a40b0580"],
           ["observable_type", "BloodPressure"],
           ["LIMIT", 20],
           ["OFFSET", 0]]
```

**Why is this important?** This allows program managers to see if healthcare workers are inputting accurate BP readings into the system. There are cases when healthcare workers round systolic and diastolic numbers, and it's really important for them to enter the exact reading, otherwise the higher-level indicators will be inaccurate.

## Cohort reports

![Sample monthly cohort report for May 2020](.gitbook/assets/cohort-reports.png)

Hypertension control takes time, and cohorts allow us to track a set of patients receiving treatment over time. We take all the patients registered in a given period of time and look to see if they returned for care during a subsequent period of time. If they returned to care, we look to see if their blood pressure was under control.

We use monthly cohorts to quickly track performance, and quarterly cohorts to determine systematic improvements over longer timeframes.

The calculations for monthly and quarterly cohorts are the same. The only difference is in the registration period \(month versus quarter\).

### Monthly cohort reports

**BP controlled numerator:** The number of patients assigned to a facility registered during a month where the patient \(1\) is hypertensive \(2\) is not deleted \(3\) is not dead \(4\) has a last BP in the following 2 months and \(5\) the last BP is &lt;140/90.

```text
/* Sample SQL query */
SELECT COUNT(*)
FROM
  (SELECT DISTINCT ON (patient_id) *
   FROM "latest_blood_pressures_per_patient_per_months"
   WHERE "latest_blood_pressures_per_patient_per_months"."deleted_at" IS NULL
     AND "latest_blood_pressures_per_patient_per_months"."patient_id" IN
       (SELECT DISTINCT "patients"."id"
        FROM "patients"
        INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
        AND "medical_histories"."patient_id" = "patients"."id"
        WHERE "patients"."deleted_at" IS NULL
          AND "medical_histories"."deleted_at" IS NULL
          AND "medical_histories"."hypertension" = $1
          AND "patients"."status" != $2
          AND "patients"."assigned_facility_id" IN
            (SELECT "facilities"."id"
             FROM "facilities"
             WHERE "facilities"."deleted_at" IS NULL
               AND "facilities"."id" IN
                 (SELECT "facilities"."id"
                  FROM "facilities"
                  WHERE "facilities"."deleted_at" IS NULL
                    AND "facilities"."id" = $3))
          AND (recorded_at >= '2021-02-28 18:30:00'
               AND recorded_at <= '2021-03-31 18:29:59.999999'))
     AND ((YEAR = '2021'
           AND MONTH = '4')
          OR (YEAR = '2021'
              AND MONTH = '5'))
   ORDER BY patient_id,
            bp_recorded_at DESC, bp_id) latest_blood_pressures_per_patient_per_months
WHERE "latest_blood_pressures_per_patient_per_months"."deleted_at" IS NULL
  AND (systolic < 140
       AND diastolic < 90) [["hypertension", "yes"],
                            ["status", "dead"],
                            ["id", "acc3da36-c5d2-42e1-a1fe-29d6a40b0580"]]
```

**BP not controlled numerator:** The number of patients assigned to a facility registered during a month where the patient \(1\) is hypertensive \(2\) is not deleted \(3\) is not dead \(4\) has a last BP in the following 2 months and \(5\) the last BP is ≥140/90.

```text
/* Sample SQL query */
SELECT COUNT(*)
FROM
  (SELECT DISTINCT ON (patient_id) *
   FROM "latest_blood_pressures_per_patient_per_months"
   WHERE "latest_blood_pressures_per_patient_per_months"."deleted_at" IS NULL
     AND "latest_blood_pressures_per_patient_per_months"."patient_id" IN
       (SELECT DISTINCT "patients"."id"
        FROM "patients"
        INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
        AND "medical_histories"."patient_id" = "patients"."id"
        WHERE "patients"."deleted_at" IS NULL
          AND "medical_histories"."deleted_at" IS NULL
          AND "medical_histories"."hypertension" = $1
          AND "patients"."status" != $2
          AND "patients"."assigned_facility_id" IN
            (SELECT "facilities"."id"
             FROM "facilities"
             WHERE "facilities"."deleted_at" IS NULL
               AND "facilities"."id" IN
                 (SELECT "facilities"."id"
                  FROM "facilities"
                  WHERE "facilities"."deleted_at" IS NULL
                    AND "facilities"."id" = $3))
          AND (recorded_at >= '2021-02-28 18:30:00'
               AND recorded_at <= '2021-03-31 18:29:59.999999'))
     AND ((YEAR = '2021'
           AND MONTH = '4')
          OR (YEAR = '2021'
              AND MONTH = '5'))
   ORDER BY patient_id,
            bp_recorded_at DESC, bp_id) latest_blood_pressures_per_patient_per_months
WHERE "latest_blood_pressures_per_patient_per_months"."deleted_at" IS NULL
  AND (systolic >= 140
       OR diastolic >= 90) [["hypertension", "yes"],
                            ["status", "dead"],
                            ["id", "acc3da36-c5d2-42e1-a1fe-29d6a40b0580"]]
```

**No BP taken numerator:** The number of patients assigned to a facility registered during a month minus the number of patients with a BP taken in the following 2 months.

```text
/* Sample SQL query */
SELECT COUNT(*)
FROM
  (SELECT DISTINCT ON (patient_id) *
   FROM "latest_blood_pressures_per_patient_per_months"
   WHERE "latest_blood_pressures_per_patient_per_months"."deleted_at" IS NULL
     AND "latest_blood_pressures_per_patient_per_months"."patient_id" IN
       (SELECT DISTINCT "patients"."id"
        FROM "patients"
        INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
        AND "medical_histories"."patient_id" = "patients"."id"
        WHERE "patients"."deleted_at" IS NULL
          AND "medical_histories"."deleted_at" IS NULL
          AND "medical_histories"."hypertension" = $1
          AND "patients"."status" != $2
          AND "patients"."assigned_facility_id" IN
            (SELECT "facilities"."id"
             FROM "facilities"
             WHERE "facilities"."deleted_at" IS NULL
               AND "facilities"."id" IN
                 (SELECT "facilities"."id"
                  FROM "facilities"
                  WHERE "facilities"."deleted_at" IS NULL
                    AND "facilities"."id" = $3))
          AND (recorded_at >= '2021-02-28 18:30:00'
               AND recorded_at <= '2021-03-31 18:29:59.999999'))
     AND ((YEAR = '2021'
           AND MONTH = '4')
          OR (YEAR = '2021'
              AND MONTH = '5'))
   ORDER BY patient_id,
            bp_recorded_at DESC, bp_id) latest_blood_pressures_per_patient_per_months
WHERE "latest_blood_pressures_per_patient_per_months"."deleted_at" IS NULL
```

**Denominator:** The number of patients assigned to a facility where the patient \(1\) is hypertensive \(2\) is not deleted and \(3\) is not dead.

```text
/* Sample SQL query */
SELECT COUNT(DISTINCT "patients"."id")
FROM "patients"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
WHERE "patients"."deleted_at" IS NULL
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = $1
  AND "patients"."status" != $2
  AND "patients"."assigned_facility_id" IN
    (SELECT "facilities"."id"
     FROM "facilities"
     WHERE "facilities"."deleted_at" IS NULL
       AND "facilities"."id" IN
         (SELECT "facilities"."id"
          FROM "facilities"
          WHERE "facilities"."deleted_at" IS NULL
            AND "facilities"."id" = $3))
  AND (recorded_at >= '2021-02-28 18:30:00'
       AND recorded_at <= '2021-03-31 18:29:59.999999') [["hypertension", "yes"],
                                                         ["status", "dead"],
                                                         ["id", "acc3da36-c5d2-42e1-a1fe-29d6a40b0580"]]
```

**Why is this important?** This allows program managers to track a group of patients over time and see how long it takes patients to get controlled or to be lost to follow-up.

## Overdue patients

All patients with an appointment scheduled at a facility managed by a program manager where \(1\) their appointment date has passed \(2\) the appointment status is scheduled \(3\) and they haven't been temporarily removed from the overdue list.

Patients can be temporarily removed for 7 days if they've been contacted by a healthcare worker and marked "Agreed to visit" in the Simple App. Patients can also be removed for 30 days if they've been marked as "Remind to call later" by a healthcare worker in the Simple App.

**Why is this important?** It's important for patients to come back to care to continue with their hypertension treatment and get their hypertension under control. Healthcare workers are crucial at calling patients and reminding them to attend their appointments and continue with their treatment.

### How the overdue list is sorted

The overdue list orders patients by two factors:

1. Risk level \(high risk first, then regular risk\)
2. Days overdue \(ordered by least to most overdue\)

#### Risk level

Patients are categorized into High risk and Regular risk. A patient is High risk if they are ≥30 days overdue and:

* Their latest BP is ≥180/110
* OR if their medical history indicates prior heart attack or stroke AND their latest BP is ≥140/90
* OR if their latest blood sugar is not controlled \(see [Blood sugar controlled](https://docs.simple.org/what-we-report#blood-sugar-controlled)\)

## Blood sugar controlled

A blood sugar is considered controlled if it is under the following thresholds:

* Random blood sugar: &lt;300 mg/DL
* Post prandial blood sugar: &lt;300 mg/DL
* Fasting blood sugar: &lt;200 mg/DL
* HbA1c: &lt;9.0%

## Dead patients

Patients who have been marked as dead. The patient's date of death is not recorded.

**Why is this important?** Dead patients are excluded from all key indicators.

