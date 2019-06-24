---
description: How to audit access to patient data in the logs
---

# Audit logs

{% hint style="info" %}
Using the Audit Logs requires access to [Heap Analytics](https://heapanalytics.com/app/) 🔒, which has restricted access.
{% endhint %}

## How to answer "What users accessed Patient X?"

#### Required Information

* The patient UUID

#### **Define an event for viewing the specific patient**

In the Heap UI, go to `Events` and in the list of custom events, select `Viewed Patient X`.

![Select Viewed Patient Event](https://github.com/simpledotorg/documentation/raw/master/assets/img_1.png)

In the event definition section, scroll down to the `Edit Event Criteria` section, update the `patientId` filter with the patient ID and click on "UPDATE EVENT".

![Update Event Criteria](https://github.com/simpledotorg/documentation/raw/master/assets/img_2.png)

#### **Query the reports for users who viewed this specific patient**

In the `Reports/Auditing` section, select the `Users who have viewed patient X in last three months` report.

![Select report](https://github.com/simpledotorg/documentation/raw/master/assets/img_3.png)

In the report page that opens up, click on "RUN QUERY" to generate the report and get all the User IDs that have viewed a particular patient.

![Run Query](https://github.com/simpledotorg/documentation/raw/master/assets/img_4.png)

## How to answer "What patients did User Y access?"

#### Required Information

* The user UUID

#### **Select the report**

In the Heap UI, go to the `Reports/Auditing` section, and select the `All patients viewed by user Y in 3 months` report.

![Select Report](https://github.com/simpledotorg/documentation/raw/master/assets/img_5.png)

#### **Update the User ID**

In the report detail page, update the `userId` filter with the UUID of the user to search for and click on "RUN QUERY".

![Select Viewed Patient Event](https://github.com/simpledotorg/documentation/raw/master/assets/img_6.png)

Click on the "SHOW RAW EVENTS" button to view the detailed list of events.

![Show Raw Events](https://github.com/simpledotorg/documentation/raw/master/assets/img_7.png)

#### **Get Patient IDs**

In the list of raw events that shows up, look for the event `ViewedPatient`. Clicking on it will show detailed properties of that particular event. It should contain a property `patientId` which indicates the patient whose details this user looked at. Looking over all the events \(Don't forget to click on "Show More" to load pages of raw events\), the patient IDs viewed by this user can be extracted.

![Find Patient ID](https://github.com/simpledotorg/documentation/raw/master/assets/img_8.png) 

