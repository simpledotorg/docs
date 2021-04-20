---
description: A brief description of our key indicators and how they're calculated
---

# What we report

In order to improve hypertension treatment for a clinic or even an entire population, you have to understand how the system is performing. You need to see if patients are getting healthy, if they're being treated, if they're coming back for follow-up visits, and if they're being registered into the system.

Simple collects the bare minimum of information necessary to identify a patient, treat the patient for hypertension, and schedule a follow-up. We aggregate this information to report the following key indicators to help track hypertension care:

1. [Registrations](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#registrations)
2. [Total assigned patients](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#total-assigned-patients)
3. [BP controlled](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#bp-controlled)
4. [BP not controlled](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#bp-not-controlled)
5. [Visited but no BP taken](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#visited-but-no-bp-taken)
6. [Missed visits](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#missed-visits)
7. [Lost to follow-up](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#lost-to-follow-up)
8. [Follow-up patients](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#follow-up-patients)
9. [Inactive facilities](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#inactive-facilities)
10. [BP measure taken](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#bp-measure-taken)
11. [BP log](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#bp-log)
12. [Cohort reports](https://app.gitbook.com/@simpledotorg/s/docs/~/drafts/-MYfp73eLYHeDsbjbL5z/what-we-report#cohort-reports)

## Registrations

**Monthly registered patients:** the number of patients registered at a facility during a month where the patient \(1\) is not deleted and \(2\) is hypertensive.

**Total registered patients:** the number of patients registered at a facility. _Note: this is calculated by adding monthly registrations at a facility over time_

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

**Why is this important?** Tracking registrations allows program managers to identify facilities that aren't registering patients into the system. If patients aren't being registered in the system, we can't help track their hypertension treatment.

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

**Why is this important?** Patients often travel multiple hours to visit a district hospital, and it's unlikely they will make that trip every month. Simple allows healthcare workers to re-assign patients to a smaller clinic closer to the patient's home to make it easier for them to get hypertensive treatment and medication. Total assigned patients is the number of patients that visit a facility on a regular basis. This number is used to calculate the main key indicators \(BP controlled, Follow-ups, Lost to follow-up\) and is the number used to estimate medication supply needs.

##  BP controlled

The number of patients assigned to a facility registered before the last 3 months where the patient \(1\) is not deleted \(2\) is hypertensive \(3\) is not dead \(4\) has a BP measure taken within the last 3 months and \(5\) last BP measure taken is &lt;140/90.

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

**Why are patients registered within the last 3 months excluded?** Three months gives patients time to take their hypertension medication. Most newly registered patients are hypertensive, and including them would not reflect an accurate picture of actual controlled patients.

**Why is this important?** The goal of a hypertension control program is to treat hypertension and this metric represents how well the system is doing at controlling patient high blood pressure.

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

**Why is this important?** This indicator is important because it allows program managers to see which patients are coming back to care, but require continued hypertension treatment to control their blood pressure.

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

**Why is this important?** This indicator allows program managers to see which patients are coming back to care but aren't getting their blood pressure taken at the facility. This is not very common, but has happened more regularly during COVID-19 where patients visit a facility to pick up medications and avoid contact with healthcare workers to protect themselves from infection.

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

**Why is this important?** Program managers focus a lot of their attention on bringing this number down as much as possible by urging healthcare workers at facilities to call patients that have missed their appointments and reminding them to continue treatment.

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

**Why is this important?** The main key indicators exclude lost to follow-up patients to allow program managers to better estimate medication supply needs and to better assess the health of patients that are coming back to care.

## Follow-up patients

The number of patients assigned to a facility during a month where the patient \(1\) had a BP taken during a month \(2\) is hypertensive \(3\) is not deleted and \(4\) was not registered during that month.

```text
/* Sample SQL query */
SELECT COUNT (DISTINCT "patients"."id") AS count_id,
             DATE_TRUNC('month', blood_pressures.recorded_at::timestamptz AT TIME ZONE 'ASIA/KOLKATA') AT TIME ZONE 'ASIA/KOLKATA' AS date_trunc_month_blood_pressures_recorded_at_timestamptz_at_tim,
             "blood_pressures"."facility_id" AS blood_pressures_facility_id
FROM "patients"
INNER JOIN "blood_pressures" ON "blood_pressures"."deleted_at" IS NULL
AND "blood_pressures"."patient_id" = "patients"."id"
INNER JOIN "medical_histories" ON "medical_histories"."deleted_at" IS NULL
AND "medical_histories"."patient_id" = "patients"."id"
WHERE "patients"."deleted_at" IS NULL
  AND (patients.recorded_at < (DATE_TRUNC('month', (blood_pressures.recorded_at::timestamptz) AT TIME ZONE 'ASIA/KOLKATA')) AT TIME ZONE 'ASIA/KOLKATA')
  AND (blood_pressures.recorded_at IS NOT NULL)
  AND "blood_pressures"."facility_id" IN $1
  AND "medical_histories"."deleted_at" IS NULL
  AND "medical_histories"."hypertension" = $24
GROUP BY DATE_TRUNC('month', blood_pressures.recorded_at::timestamptz AT TIME ZONE 'ASIA/KOLKATA') AT TIME ZONE 'ASIA/KOLKATA', "blood_pressures"."facility_id" [["facility_id", "3a7e86d2-c272-4303-8ffa-d6d1b54874b3"],
                                                                                                                                                                 ["hypertension", "yes"]]
```

**Why is this important?** This indicator is a reflection of a facility's effectiveness at bringing patients back to care to continue with their treatment. This indicator is often compared with total assigned patients because it shows what proportion of patients are getting treated.

## Inactive facilities

Facilities where &lt;10 patients had any BPs recorded in the last 7 days

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

**Missed visits numerator:** The number of patients assigned to a facility registered during a month \(minus\) the number of patients with a BP taken in the following 2 months.

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

