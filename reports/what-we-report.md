---
description: >-
  This document lists and defines the key indicators program managers use to
  track the hypertension and diabetes control programs with Simple.
---

# What we report

## Region reports

To improve hypertension and diabetes treatment for a clinic or even an entire population, you have to understand how the system is performing. You need to see if patients are getting healthy, if they’re being treated, if they’re coming back for follow-up visits, and if they’re being registered into the system.

Simple collects the bare minimum of information necessary to identify a patient, treat the patient for hypertension and diabetes, and schedule a follow-up. We aggregate this information generate reports at 5 region levels, from an organization to a facility.\
Region names are unique for each country:

| **Country**     | **Region 1** | **Region 2** | **Region 3** | **Region 4** | **Region 5** |
| --------------- | ------------ | ------------ | ------------ | ------------ | ------------ |
| 🇮🇳 India      | Organization | State        | District     | Block        | Facility     |
| 🇧🇩 Bangladesh | Organization | Division     | District     | Upazila      | Facility     |
| 🇪🇹 Ethiopia   | Organization | Region       | Zone         | Woreda       | Facility     |
| 🇱🇰 Sri Lanka  | Organization | Province     | District     | Town         | Facility     |

## Hypertension indicators

### Registrations

* **Monthly registrations:** The number of patients registered at a facility during a month where the patient is hypertensive and is not deleted.
* **Total registrations:** The total number of patients registered at a facility._Note: This is calculated by adding monthly registered patients at a facility over time._
* **Why is this important?** Program managers monitor registration numbers to ensure healthcare workers are entering patients into the system.

### Assigned patients

* The number of patients a facility is responsible for where a patient is hypertensive, not deleted, and not dead.
* **Why is this important?** This indicator is the population base used to calculate indicators like BP controlled and Missed visits.

### Patients under care

* The number of patients assigned to a facility where the patient is hypertensive, is not deleted, is not dead, and had at least one of the following within the last 12 months: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Why is this important?** It represents the number of “active” patients.

### Lost to follow-up

* The number of patients assigned to a facility where the patient is hypertensive, is not deleted, is not dead, was registered >12 months ago, and hasn’t had at least one of the following within the last 12 months: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Why is this important?** The main key indicators exclude lost to follow-up patients to allow program managers to only assess the health of active patients.

### Follow-up patients

* **Follow-up patients per user:** For a given period, the number of patients attended by a user where the patient is hypertensive, is not deleted, was registered before that period, and had at least one of the following during that period: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Follow-up patients per facility:** For a given period, the number of patients that visited a facility where the patient is hypertensive, is not deleted, was registered before that period, and had at least one of the following during that period: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Follow-up patients per region:** For a given period, the sum of follow-up patients across all facilities in the region.
* **Why is this important?** This indicator is used to monitor the facility’s activity. Follow-up patients per facility are often compared with total assigned patients because they show what proportion of patients are coming back to care.

### BP controlled

* The number of patients assigned to a facility registered before the last 3 months where the patient is hypertensive, not deleted, not dead, and has a BP measure <140/90 taken within the last 3 months.
* **Why are patients registered within the last 3 months excluded?** Three months gives patients time to take their hypertension medication. Newly registered patients have uncontrolled blood pressure and including them would not reflect an accurate picture of actual controlled patients.
* **Why is this important?** BP controlled reflects the overall health of a hypertension control program and is the most important indicator our system tracks.

### BP not controlled

* The number of patients assigned to a facility registered before the last 3 months where the patient is hypertensive, not deleted, not dead, and has a BP measure ≥140/90 taken within the last 3 months.
* **Why is this important?** BP not controlled shows which patients are coming back to care, but require continued hypertension treatment to control their blood pressure.

### Visited but no BP taken

* The number of patients assigned to a facility registered before the last 3 months where the patient is hypertensive, not deleted, not dead, and did not have a BP taken but had at least one of the following within the last 3 months: an appointment scheduled, a drug refilled, or a blood sugar taken.
* **Why is this important?** We started tracking this indicator more closely during COVID-19 because patients were visiting facilities to pick up medications but didn’t have their BP taken to avoid contact with healthcare workers and prevent COVID infection. This is not very common, but helps capture the entire patient base.

### Missed visits

* The number of patients assigned to a facility registered before the last 3 months where the patient is hypertensive, not deleted, not dead, and did not have a visit within the last 3 months.
* **Why is this important?** This number reflects how good facilities are at reminding patients to come back to care in the 3-month period we’re tracking controlled and uncontrolled patients.

### Patient coverage

* Total registered patients divided by the region’s total estimated hypertensive population. We currently show the patient coverage for “Region 2” and “Region 3” (see region table above for reference). A “Region 2”s patient coverage value is displayed _only_ if all of its child regions have an estimated population value entered on the Dashboard.

### Cohort reports

* Cohorts allow program managers to track a set of patients receiving treatment over time. We take all the patients registered during a month or quarter and see the outcome of their visit in the following two months/quarters.
* Monthly cohorts are used to quickly track patient performance. And quarterly cohorts are used to determine systematic improvements.

#### **Monthly cohort reports**

* **BP controlled numerator:** The number of patients with a BP <140/90 within the two months after registration.
* **BP not controlled numerator:** The number of patients with a BP ≥140/90 within the two months after registration.
* **Visited but no BP taken numerator:** The number of patients with no BP taken but at least one of the following within the two months after registration: an appointment scheduled, their drugs refilled, or a blood sugar taken.
* **Missed visits numerator:** The number of patients with no visit within the two months after registration.
* **Denominator:** The number of patients assigned to a facility where the patient is hypertensive, is not deleted, is not dead, and was registered during a month.

\
**Quarterly cohort reports**

* **BP controlled numerator:** The number of patients with a BP <140/90 within the two quarters after registration.
* **BP not controlled numerator:** The number of patients with a BP ≥140/90 within the two quarters after registration.
* **Visited but no BP taken numerator:** The number of patients with no BP taken but at least one of the following within the two quarters after registration: an appointment scheduled, their drugs refilled, or a blood sugar taken.
* **Missed visits numerator:** The number of patients with no visit within the two quarters after registration.
* **Denominator:** The number of patients assigned to a facility where the patient is hypertensive, is not deleted, is not dead, and was registered during a quarter.

## Diabetes indicators

#### Registrations

* **Monthly registrations:** the number of patients registered at a facility during a month where the patient is diabetic and is not deleted.
* **Total registrations:** the total number of patients registered at a facility where the patient is diabetic and is not deleted.
* _Note: This is calculated by adding monthly registered patients at a facility over time._

### Assigned patients

* The number of patients a facility is responsible for where a patient is diabetic, not deleted, and not dead.
* **Why is this important?** This indicator is the population base used to calculate indicators like BP controlled and Missed visits.

### Patients under care

* The number of patients assigned to a facility where the patient is diabetic, is not deleted, is not dead, and had at least one of the following within the last 12 months: an appointment scheduled, a their drugs refilled, a BP taken, or a BS taken.
* **Why is this important?** It represents the number of “active” patients.

### Lost to follow-up

* The number of patients assigned to a facility where the patient is diabetic, is not deleted, is not dead, was registered >12 months ago, and hasn’t had at least one of the following within the last 12 months: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Why is this important?** The main key indicators exclude lost to follow-up patients to allow program managers to only assess the health of active patients.

### Follow-up patients

* **Follow-up patients per user:** For a given period, the number of patients attended by a user where the patient is diabetic, is not deleted, was registered before that period, and had at least one of the following during that period: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Follow-up patients per facility:** For a given period, the number of patients that visited a facility where the patient is diabetic, is not deleted, was registered before that period, and had at least one of the following during that period: an appointment scheduled, their drugs refilled, a BP taken, or a BS taken.
* **Follow-up patients per region:** For a given period, the sum of follow-up patients across all facilities in the region.
* **Why is this important?** This indicator is used to monitor the facility’s activity. Follow-up patients per facility are often compared with total assigned patients because they show what proportion of patients are coming back to care.

### BS <200

* The number of patients assigned to a facility registered before the last 3 months where the patient is diabetic, not deleted, not dead, and has an RBS/PPBS <200, FBS <126, or HbA1c <7.0 within the last 3 months.
* **Why are patients registered within the last 3 months excluded?** Three months gives patients time to take their diabetic medication. Newly registered patients have uncontrolled blood sugar and including them would not reflect an accurate picture of actual controlled patients.
* **Why is this important?** BS <200 reflects the overall health of a diabetes control program and is the most important indicator our system tracks.

### BS 200-299

* The number of patients assigned to a facility registered before the last 3 months where the patient is diabetic, not deleted, not dead, and has an RBS/PPBS 200-299, FBS 126-199, or HbA1c 7.0-8.9 within the last 3 months.
* **Why is this important?** BS 200-299 shows how many patients are coming back to care, but require continued diabetic treatment to control their blood sugar.

### BS ≥300

* The number of patients assigned to a facility registered before the last 3 months where the patient is diabetic, not deleted, not dead, and has an RBS/PPBS ≥300, FBS ≥200, or HbA1c ≥9.0 within the last 3 months.
* **Why is this important?** BS ≥300 shows how many patients are coming back to care, but require continued diabetic treatment to control their high blood sugar.

### Visited but no BS taken

* The number of patients assigned to a facility registered before the last 3 months where the patient is diabetic, not deleted, not dead, and did not have a BS taken but had at least one of the following within the last 3 months: an appointment scheduled, a drug refilled, or a blood pressure taken.
* **Why is this important?** We started tracking this indicator more closely during COVID-19 because patients were visiting facilities to pick up medications but didn’t have their BS taken to avoid contact with healthcare workers and prevent COVID infection. This is not very common, but helps capture the entire patient base.

### Missed visits

* The number of patients assigned to a facility registered before the last 3 months where the patient is diabetic, not deleted, not dead, and did not have a visit within the last 3 months.
* **Why is this important?** This number reflects how good facilities are at reminding patients to come back to care in the 3-month period we’re tracking controlled and uncontrolled patients.

### Patient coverage

* Total registered patients divided by the region’s total estimated diabetic population. We currently show the patient coverage for “Region 2” and “Region 3” (see region table above for reference). A “Region 2”s patient coverage value is displayed _only_ if all of its child regions have an estimated population value entered on the Dashboard.

## Other data we report

### Overdue patients

* All patients with an appointment scheduled at a facility managed by a program manager where their appointment date has passed, the appointment status is scheduled, and they haven’t been temporarily removed from the overdue list.
* Patients can be temporarily removed for 7 days if they’ve been contacted by a healthcare worker and marked as “Agreed to visit” in the Simple App. Patients can also be removed for 30 days if they’ve been marked as “Remind to call later” by a healthcare worker in the Simple App.
* **Why is this important?** It’s important for patients to come back to care to continue with their treatment and get their hypertension and diabetes under control. Healthcare workers are crucial at calling patients to remind them to attend their appointments.

**How the overdue list is sorted:** The overdue list orders patients by two factors:

1. Risk level (high risk first, then regular risk)
2. Days overdue (ordered by least to most overdue)

\
**Risk levels:** Patients are categorized into “High” risk and “Regular” risk. A patient is “High” risk if they are ≥30 days overdue and one of the following is true:

1. Their latest BP is ≥180/110
2. Their medical history indicates prior heart attack or stroke AND their latest BP is ≥140/90
3. Their latest blood sugar is not controlled

### Overdue patients contacted

* All patients called during a month by healthcare workers in a facility or region. These patients appear in the “Overdue” tab in the Simple App and Dashboard. A patient is marked as “contacted” whenever a healthcare worker or Dashboard admin marks an overdue patient with one of the following results on the Simple App: (1) Agreed to visit (2) Remind to call later (3) Remove from the overdue list.
* **Why is this important?** It’s important for patients to come back to care to continue with their treatment and get their hypertension and diabetes under control. Healthcare workers are crucial at calling patients to remind them to attend their appointments.

### Inactive facilities

* Facilities registered in Simple where the current user can view reports and where <10 patients had a BP recorded in the last 7 days.
* **How is it calculated?** First we calculate total active facilities (facilities where >10 patients had any BPs recorded in the last 7 days). Then we grab all facilities the admin has access to, count the total number of patients with a BP taken in a day for the last 7 days at each facility, and return the number of facilities where the facility has had more than 10 patients with a BP taken in the last week.
* **Why is this important?** It allows program managers to see which facilities may be facing technical issues with the system or healthcare workers that are forgetting to record BP measures into the system.

### Patients with a BP measure taken

* Counts all BPs recorded by each healthcare worker at a facility where the patient is hypertensive and not deleted.
* **Why is this important?** Similar to inactive facilities, it allows program managers to see which facilities are facing technical issues or facilities with healthcare workers that aren’t recording BP measures into the system.

### BP log

* All blood pressures recorded at a facility.
* **Why is this important?** This allows program managers to see if healthcare workers are inputting accurate BP readings into the system. It’s common for healthcare workers to round systolic and diastolic numbers or to enter BPs that are just one point below the threshold (e.g. 139/89).
