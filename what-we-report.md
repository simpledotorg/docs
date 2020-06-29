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

![Sample monthly cohort report for May 2020](.gitbook/assets/cohort-reports.png)

### Monthly cohorts

Looking at monthly cohorts allows us to quickly identify performance issues. This is particularly useful when trying to provide feedback to clinicians or medical officers.

Monthly cohorts takes all the patients registered in a given month and looks to see if they returned for care in the following two months. This is a rolling indicator, reporting at the completion of each period.

For example, let's look at the cohort analysis reported on April 2019.

* **Registered:** Number of patients first registered in Simple in January 2019
* **Defaulted:** Number of patients who did not return between Feburary 1 and March 30, 2019
* **Uncontrolled:** Number of patients who returned between Feburary 1 and March 30, 2019

  and whose last BP in that period was high

* **Controlled:** Number of patients who returned between Feburary 1 and March 30, 2019

  and whose last BP in that period was controlled

The **control rate** for February 2019 is calculated by dividing the **controlled** number by the **registered** number.

$$
Monthly\ control\ rate = \frac{Controlled\ between\ months\ (X+1)\ and\ (X+2)}{Registered\ in\ month\ X}
$$

We provide monthly cohorts on the Simple dashboard for the previous six periods.

### Quarterly cohorts

To see sustained improvements in a clinic or health system, it's useful to look in terms of quarters instead of months. Quarterly cohorts are calculated in much the same way.

We use a "3–6 month cohort", meaning at the end of a quarter, we look at patients registered 3–6 months previously. This just means we look at patients registered in the previous quarter.

For example, let's look at Q2 2019 \(April–June\).

* **Registered:** Number of patients first registered in Simple in Q1 2019 \(January–March\)
* **Defaulted:** Number of patients who did not return in Q2 2019 \(April–June\)
* **Uncontrolled:** Number of patients who returned in Q2 2019 and whose last BP in that period was high
* **Controlled:** Number of patients who returned in Q2 2019 and whose last BP in that period was controlled

Again, the **control rate** for Q2 2019 is calculated by dividing the **controlled** number by the **registered** number.

$$
Quarterly\ control\ rate = \frac{Controlled\ in\ quarter\ X+1}{Registered\ in\ quarter\ X}
$$

We provide quarterly cohorts on the Simple dashboard for the current quarter and previous two quarters.

## Facility reports

In order to track how a district is performing, we report the following indicators for each facility in the district:

* **Total registered patients:** How many patients have ever been registered at this facility
* **Follow up patients per month:** How many patients had a follow up visit at this facility
* **Newly registrations per month:** How many patients had their first visit at this facility
* **Patient calls per month:** How times did users from this facility attempt to call patients

We show these indicators for the current month and previous 2 months.

![Example of facility-level indicators](.gitbook/assets/image%20%286%29.png)

## User reports

In order to track how a particular facility is performing, we report the following indicators for each user in the facility:

* **Total registered patients:** How many patients have ever been registered by this user
* **Follow up patients per month:** How many patients had a follow up visit with this user
* **Newly registrations per month:** How many patients had their first visit with this user
* **Patient calls per month:** How times did this user attempt to call patients

We show these indicators for the current month and previous 2 months.

![Example for user-level indicators](.gitbook/assets/image%20%281%29.png)

**Note**: A "user" is generally a nurse, doctor, or other healthcare worker. It's anyone who uses the Simple mobile app.

