---
description: Common terms you'll run into on the project.
---

# Glossary

{% hint style="info" %}
Please add new terms as you come across them. If you don't know what something means, please ask — there's no embarrassment in not knowing a term.
{% endhint %}

## Roles

**Admin:** Anyone with access to the back-end dashboard, roles and permissions are listed below.

* **Owner**: Simple tech team members. Has all permissions listed below:
  * View Dashboard for all Organizations
  * View and Download the Overdue list \(facility-wise\)
  * View and update Adherence follow-ups
  * Manage Organizations, Facility groups and Facilities
  * Manage Admins
  * Manage Protocols
  * Manage Users and approve / deny their access to the Simple mobile app 
  * Search Audit Logs
* **Organization Owner**: Administrators for one or more Organizations, e.g. PATH admins. Has the same permissions as **Owner**, but limited to the organizations they control. They also can't add organizations.
* **Supervisor**: Program supervisors, primarily CVHO and STS. Permissions limited to:
  * View Dashboard for Facilities that they have access to
  * View and Download the Overdue list \(facility-wise\)
  * View Facilities they have access to
  * Manage Users and approve / deny their access to the Simple mobile app
* **Analyst**: People who just need dashboard data, e.g. an epidemiologist or health expert Permissions are read-only, limited to:
  * View Dashboard for Facilities they have access to \(District page only, not User page
  * No overdue list or management access
* **Counselor**: Call center employee or counselor who will follow up with the patient. Permissions limited to:
  * View and Update the Overdue list
  * View and Update Adherence follow-ups
  * No dashboard or management access

## Terms

* **Blood Pressure \(BP\):** Combination of systolic and diastolic BP readings in mm Hg

  Recorded as Systolic/Diastolic `Ex. 120/80`

* **BP medicines**: Name\(s\) and dosage\(s\) of medication currently prescribed to a patient
* **BP passport:** Physical ID provided to the patient
* **BP passport code:** UUID printed on BP passport as a QR code, which can be scanned to find the patient.
* **BP passport short code:** A 7-digit number that summarizes the UUID, which can be entered if the scanner does not work.
* **Business ID:** IDs other than BP passport that may be associated with a patient \(such as Driving License, State Health ID etc.\)
* **Call list:** Used interchangeably with overdue list
* **Clinic:** Used interchangeably with Facility
* **Facility:**
  * **Sub-centre**
  * **PHC** \(Primary Health Centre\)
  * **CHC** \(Community Health Centre\)
  * **District Hospital** \(or\)
  * Any Public or Private Healthcare Facility
  * All Facilities are identified by a Facility ID
  * Users can be linked to one or more Facilities
* **Facility group:** Group of one or more facilities
  * Users within a facility group sync patient data amongst themselves
  * Users outside a facility group do not sync data amongst themselves
* **Follow up list:** List of all patients who have missed their appointment \(including ones without a phone number\). Displayed on the web dashboard.
* **ICMR:** Indian Council of Medical Research
* **IHMI/IHCI:** Indian Hypertension Management Initiative, now called India Hypertension Control Initiative
* **Overdue list:** List of patients who have a phone number and who have missed their appointment. Displayed on the nurses' phone.
* **Organization:** One or more facility groups that are part of a single administrative unit
* **Protocol:** Name\(s\) and dosage\(s\) of medication recommended to be prescribed to a patient based on their current BP and BP history.
* **Patient log:** List of all patients with a BP recorded in that facility. Displayed on the nurses' phones.
* **Result of a phone call made with the user's number masked:**
  * Completed: Connected to the patient 
  * Canceled: User cut the call before it was connected
  * Busy: Patient’s number is giving a busy tone
  * No answer: Patient did not answer
  * Failed: Call failed \(likely due to bad phone number or operator-level disconnections\)
  * Unknown: Result of the call is unknown 
* **Security PIN:** 4 digit PIN used by the Nurse to login to the app
* **User:** User of Simple app \(i.e. a healthcare work _not_ a patient\)

