---
description: How we localize the Simple apps (mobile and backend) and other projects.
---

# Localization

## Projects that need localization

Currently, there are four separate projects that are localized, along with the resources on each project:

* The android application \([LINK](https://github.com/simpledotorg/simple-android)\)
  * Strings displayed in the user interface
* The server application \([LINK](https://github.com/simpledotorg/simple-server)\)
  * HTML for the progress tab in the app
  * HTML for the help section in the app
  * Strings returned in API responses that are displayed to the end user
    * Login
  * WhatsApp graphic
  * SMS reminders
* The YouTube training video \([LINK](https://youtu.be/Gm_Fnp6ffaM)\)
  * Subtitle files
* The printed material that is delivered to the health centres Simple operates in
  * BP Passports
  * SOP training manual

## Translation process

We started by using Google Sheets to manage translations for the projects. This quickly got problematic since syncing the source strings with the projects got cumbersome and started requiring a lot of manual effort to manage. In addition, this process was also error-prone.

We evaluated and decided on a platform called [Transifex](https://www.transifex.com/) to manage translations across the board for all the projects. This platform has many benefits, primarily that it integrates with our project version control to automatically keep the source strings and translations in sync.

### Workflow

#### For a new language

1. Add the language to the project on Transifex.
2. Invite translators from the translation service for the language on the project.
3. The translator begins translations.
4. The turnaround time is 6-7 business days from the date of approval of the quotation.
5. Invite the reviewer\(s\) to Transifex.
6. Share the instructions on how to use Transifex \(screen recording + guidelines below\).
7. The reviewer will go through the translations for the app and the BP passport
   1. When there are proposed changes, the reviewer can propose a change for translator by clicking on **Add issue** and leaving a note.
   2. If the overall quality of the translation is poor, Kate will set up a call between the reviewer and the translation service.
8. We will email the translation service to let them know that the review is complete. They will address any issues that have been added by the reviewer by either
   1. Accepting the change by editing the translation.
   2. Leaving a comment to explain why the recommendation has not been accepted.
9. Translation service completes all the translations for all projects on Transifex and the design files.
10. Once &gt;95% of the strings have been translated for a particular language, they will be pulled back into the app and the server.

#### Ongoing translations

1. The translation service will work on any new strings in existing languages on an ongoing basis, and will send Kate a weekly quote every Monday. Kate will approve amount as needed. 
2. The turnaround time for new strings is 2-3 working days i.e. any new strings added on Monday will be ready latest by Thursday the same week.
3. The translation service will also address any issues that have been raised weekly, and incorporate them in future translations.
4. Once &gt;95% of the strings have been translated for a particular language, they will be pulled back into the app and the server.
5. Every month, the reviewer can look at the strings that were translated in the last month using the date filter \(we can send an email to remind the reviewer to do this?\). The reviewer can propose changes by adding an issue.

### Special cases

#### Print materials

1. RTSL will send GP the print materials as InDesign files, and also upload all the text into Transifex for all languages
2. GP will translate in Transifex
3. RTSL reviewers will review in Transifex
4. Once the translation has been approved in Transifex, GP will import the translation into the InDesign file/do any necessary layout adjustment
5. GP will post the InDesign file with the approved translation to Transifex

