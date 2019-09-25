---
description: A brief description of our key indicators and how they're calculated
---

# What we report

In order to improve hypertension treatment for a clinic or even an entire population, you have to understand how the system is performing. You need to see if patients are getting healthy, if they're being treated, and if they're coming back for follow up visits.

Simple collects the bare minimum of information necessary to identify a patient, treat the patient for hypertension, and schedule a follow up. We anonymize and aggregate this information to report key indicators to help track hypertension care.

## Cohort reports

Hypertension control takes time, and cohorts allow us to track a set of patients receiving treatment over time. We take all the patients registered in a given period of time and look to see if they returned for care during a subsequent period of time. If they returned to care, we look to see if their blood pressure was under control.

We use monthly cohorts to quickly track performance, and quarterly cohorts to determine systematic improvements over longer timeframes.

Cohort reports provide 4 key indicators:

* **Registered:** How many patients were registered in a given period?
* **Defaulted:** How many registered patients did not return in the following period?
* **Uncontrolled:** How many registed patients returned with _high_ blood pressure?
* **Controlled:** How many registered patients returned with _controlled_ blood pressure?

We provide these reports for the overall **district** level and for each **facility.**

![Example quarterly cohort report generated for Q3 2019](.gitbook/assets/image%20%287%29.png)

### Monthly cohorts

Looking at monthly cohorts allows us to quickly identify performance issues. Ths is particularly useful when trying to provide feedback to clinicians or medical officers.

Monthly cohorts takes all the patients registered in a given month and looks to see if they returned for care in the following month.

For example, let's look at the cohort analysis for February 2019.

* **Registered:** Number of patients first registered in Simple in January 2019
* **Defaulted:** Number of patients who did not return in Feburary 2019
* **Uncontrolled:** Number of patients who returned in Feburary 2019 with high BP
* **Controlled:** Number of patients who returned in Feburary 2019 with controlled BP

The **control rate** for February 2019 is simply divinding the **controlled** number by the **registered** number.

$$
Monthly\ control\ rate = \frac{Controlled\ in\ month\ X+1}{Registered\ in\ month\ X}
$$

### Quarterly cohorts

To see sustained improvements in a clinic or health system, it's useful to look in terms of quarters instead of months. Quarterly cohorts are calculated in much the same way.

We use a "3–6 month cohort", meaning at the end of a quarter, we look at patients registered 3–6 months previously. This just means we look at patients registered in the previous quarter.

For example, let's look at Q2 2019 \(April–June\).

* **Registered:** Number of patients first registered in Simple in Q1 2019 \(January–March\)
* **Defaulted:** Number of patients who did not return in Q2 2019 \(April–June\)
* **Uncontrolled:** Number of patients who returned in Q2 2019 with high BP
* **Controlled:** Number of patients who returned in Q2 2019 with controlled BP

Again, the **control rate** for Q2 2019 is simply divinding the **controlled** number by the **registered** number.

$$
Quarterly\ control\ rate = \frac{Controlled\ in\ quarter\ X+1}{Registered\ in\ quarter\ X}
$$

## Facility reports

In order to track how a district is performing, we report the following indicators for each facility in the district:

* **Total registered patients:** How many patients have ever been registered
* **Follow up patients per month:** How many patients had a follow up visit
* **Newly registrations per month:** How many patients had their first visit
* **Patient calls per month:** How times did users attempt to call patients

We show these indicators for the current month and previous 2 months.

![Example of facility-level indicators](.gitbook/assets/image%20%286%29.png)

## User reports

In order to track how a particular facility is performing, we report the following indicators for each user in the facility:

* **Total registered patients:** How many patients have ever been registered
* **Follow up patients per month:** How many patients had a follow up visit
* **Newly registrations per month:** How many patients had their first visit
* **Patient calls per month:** How times did users attempt to call patients

We show these indicators for the current month and previous 2 months.

![Example for user-level indicators](.gitbook/assets/image%20%281%29.png)

**Note**: A "user" is generally a nurse, doctor, or other healthcare worker. It's anyone who uses the Simple mobile app.

