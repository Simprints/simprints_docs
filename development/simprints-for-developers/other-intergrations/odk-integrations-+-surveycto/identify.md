# Identify

**Identification** is used when you want to identify a beneficiary from a group of people using their biometrics. During an identification, Simprints ID will collect biometrics from a beneficiary, search through a group of people and return a list of the most likely matches to the ODK form.

**The steps below references this example form for identification (**[**link**](https://docs.google.com/spreadsheets/d/1FqB\_\_8lOO9mq\_9pvtCLsqCjVrMWg7Tvk/edit#gid=1155146989)**).**

### Run an identification <a href="#h.p_fvtdli8isyej_l" id="h.p_fvtdli8isyej_l"></a>

**Step 1:** Before starting to build your identification form, do check you have the reference form and the correct columns in your form:

Open your XML identification form and have the sample form provided as a reference ([link](https://docs.google.com/spreadsheets/d/1FqB\_\_8lOO9mq\_9pvtCLsqCjVrMWg7Tvk/edit#gid=1155146989)).

**Step 2:** Check that these columns are present in your XML form. If not, add them:

* type
* name
* label
* intent
* appearance
* calculation

As part of the Simprints integration, you will need to collect information about the user (User ID) and module (Module ID). Please click [here](../../integrating-with-simprints/getting-started.md) for more information on user ID and module ID.

**Step 3:** Add questions to collect information about the user and module (refer to rows 11-12 in the sample form)

Add questions to collect basic demographic information from a beneficiary as required for your project.

**Step 4:** Create 3 "calculate" questions to store the 3 parameters needed for Simprints ID (Refer to rows 13-15 in the sample form)

In your XML form, add three rows for three calculate questions and the following names in the 'name' column:

* user\_id
* module\_id
* project\_id

In the calculation column:

* user\_id: link this to the user filling out the form (if there’s a user question earlier in the form, if not input a string “user”)
* module\_id: link this to the module related question earlier in the form (e.g. question on district), if not input a string “module”
* project\_id: input the project credentials provided by the Simprints project manager. This is an alphanumeric string in quotation marks: “xxxxxx”

**Step 5:** To create a Simprints callout and begin fingerprint scanning, create a new group in your ODK form (refer to row 16 in the sample form).

Add a new group of questions (after the calculate questions) to callout to Simprints with the name:

* begin group: identificationgrp

In the appearance column:

* identificationgrp: field-list

The column "intent" is not part of a regular ODK form and you will need to add it as a column in your ODK form. This is needed to trigger a callout to Simprints ID app from the ODK form.

**Step 6:** In the intent column for identificationgrp add:

```
com.simprints.simodkadapter.IDENTIFY(projectId=${project_id}, userId=${user_id}, moduleId=${module_id})
```

**Notes:**

**identificationgrp**: callout to Simprints to collect a beneficiary’s biometrics and identify them against a group of enrolled biometrics

**Step 7:** After collecting a beneficiary's biometrics and matching their biometrics across a database, you will want to extract the list of top matches. The user will then be able to select the correct beneficiary from the list of top matches.

Add the following questions to extract the top match scores, [tiers](../../integrating-with-simprints/tiers-and-confidence-scores.md), GUIDs and session ID. The _type_ for all of these is **text**. The following go in the _name_ column (to see what we put in the label column, please reference the sample form at the bottom of this page).

* odk-guids
* odk-confidences
* odk-match-confidence-flags
* odk-highest-match-confidence-flag
* odk-tiers
* odk-identify-biometrics-complete
  * In the constraint column:
    * odk-biometrics-complete: input .=`'true'`
  * In the constraint message column:
    * odk-biometrics-complete: Please click launch
  * In the required column:
    * odk-biometrics-complete: yes
* odk-exit-reason
* odk-exit-extra
* odk-session-id

**Notes:**

**odk-guids**: callback from Simprints with the list of GUIDs with high matches to the beneficiary’s fingerprints

**odk-confidences** and **odk-tiers**: callback from Simprints, with each high match, the confidence score that it is a match and corresponding score tier (Tier 1 is the best and Tier 5 the worst)

**odk-session-id**: unique session id for each Simprints identification

**odk-match-confidence-flags:** when running identification, we send back a flag denoting the confidence of each results. This way, in the logic set in the form you can read the flag to determine whether or not to display results. This can be used in replace of the 'odk-confidences' in logic you will see later in the form

**odk-highest-match-confidence-flag:** this is a separate response which denotes the highest flag out of the returned results. Similar to odk-match-confidence-flags, this can help make the logic used in the form more flexible.

**odk-identify-biometrics-check:** callback from Simprints that denotes when a Simprints session has been completed by a user. Making this question required will ensure that a user has to callout to Simprints and can not skip the Simprints question. Adding a constraint (when the check = true) will mean a user can not move forward in their workflow unless they have completed a session in Simprints. To find out more about Biometrics Complete check, click [here](../../integrating-with-simprints/tiers-and-confidence-scores.md).

\***odk-exit-reason:** when a user submits an exit form in Simprints, ODK will be able to capture the exit reason. This field will only populate, when a user submits an exit form in Simprints.

\***odk-exit-extra:** when a user submits an exit form with additional information in Simprints, ODK will be able to capture the additional information a user provided here. This field will only populate, when a user submits an exit reason with additional detail in Simprints.

\* Optional fields that you can add to your identification form. Please get in touch with your Simprints project manager on when to use these fields.

**Step 10:** End the Simprints identification question group with the following question (refer to row 24 in sample form)

* end group

To display the set of possible matches to the user so that they can select the correct person from the list, you'll need to create another group of questions. The steps below highlight how to set this up in your ODK form (refer to rows 25-33 in the sample form).

In the background, ODK will need to match the GUIDs, match scores and tiers from Simprints with other beneficiary information from ODK.

**Step 11:** Begin a repeat group with the following (refer to the table on the right on the calculation fields):

* extract\_guid: lists each GUID returned from Simprints (odk-guids from the step above)
* extract\_confidence: for each GUID, extracts the confidence score
* extract\_star: for each GUID, based on the tier/confidence score provides a star rating (\*\*\*\*\* is the highest match and \* the lowest match)\*
* extract\_tier: for each GUID, extracts the tier based on confidence score
* pullname: based on the enrolment data (sampledatareg CSV) pulls the name corresponding to the GUID
* pullage: based on the enrolment data (sampledatareg CSV) pulls the age corresponding to the GUID
* concatinfo: combines all the info above for each GUID

The pulldata() function in the calculation column is used to link the identification form to existing data already captured in the enrolment/registration form.

Presenting information in step 11 as a multiple choice question will allow a user to select the correct person from a list of top matches.

Create the choices in the choices tab:

**Step 12:** On the choices tab, add the following choices displayed on the right. You can change the number of choices displayed based on the number of GUIDs returned from Simprints ID (e.g. for a project where only five results is presented post Simprints identification, have six choices displayed in the choices tab)

For the last choice add the label "None of the above"

Each choice can be displayed linking the GUID to a corresponding name, age and GUID match score (refer to rows 34-39)

**Step 13:** To display the choices in your form:

* Name: match\_1\_info
* Calculation: `indexed-repeat(${concatinfo}, ${extractids}, 1)`

On the survey sheet add calculate questions (same number as those on the choices sheet) to capture each choice. Amend the number of choices based on the number of GUIDs returns list for the project (e.g. for a project where only five results is presented post Simprints identification, have six choices displayed)

**Step 14:** Create a multiple-choice question to allow a user to select the correct beneficiary from the list of choices (refer to rows 41-43 in the sample form). This has relevance **only if a result comes back at confidence LOW, MEDIUM, HIGH.** If the highest confidence is NONE then the logic included will skip this section and go to the Register Last Biometrics flow.

Add a multiple choice question for a user to select the correct beneficiary from the list of choices

* select\_match: choices question that displays the choices from step 11
* selected\_id: records the correct record selected by the user

In the choice\_filter column of row 43:

* relevance column:

```
${odk-highest-match-confidence-flag}='HIGH' or ${odk-highest-match-confidence-flag}='MEDIUM' or ${odk-highest-match-confidence-flag}='LOW' 
```

* select\_match: Insert the code snippet below

```
filter <= count-selected(${odk-guids}) or filter=99
```

In the calculation column of row 44:

* selected\_id: Insert the code snippet below

```
if(${select_match}=6,"NONE_SELECTED",indexed-repeat(${extract_guid},${extractids},${select_match}))
```

### Identification outcome callout <a href="#h.p_atx0k_ehtz6q_l" id="h.p_atx0k_ehtz6q_l"></a>

Once the user has made a selection\*, you want to notify the Simprints ID app of the selected individual (for accuracy metrics and algorithm improvement). To do this you will need to create another group of questions for an Identification Outcome Callout.

The Identification Outcome Callout will send Simprints ID the GUID the user selected.

Follow the steps below to create the identification outcome callout (refer to rows 46-49 in sample form).

\*If the individual was not listed in the top match results, the user may want to register that person's biometrics into the project. To do this refer to the section below on Register Last Biometrics.

**Step 15:** Add a new group to callout to Simprints with the selected GUID:

* begin group: identifycallback

In the intent column:

* input the intent below

```
com.simprints.simodkadapter.CONFIRM_IDENTITY(projectId=${project_id}, sessionId=${odk-session-id}, selectedGuid=${selected_id})
```

**Notes:**

**identifycallback**: callout to Simprints to provide the GUID from the selected record (selected\_id), this will enable Simprints to assess biometric performance

To add a check on whether a user has completed the identification outcome callout, use the Biometrics Complete check boolean (refer to row 48 of sample form).

To find out more about Biometrics Complete check, click [here](../../integrating-with-simprints/tiers-and-confidence-scores.md).

**Step 16:** Add a new question with the name

* text: odk-biometrics-check

In the label column:

* odk-biometrics-complete: Biometrics complete

In the constraint column:

* odk-biometrics-complete: input `.='true'`

In the constraint message column:

* odk-biometrics-complete: Please click launch

In the required column:

* odk-biometrics-complete: yes

**Notes:**

**odk-biometrics-check:** callback from Simprints that denotes that a user has successfully launched the identification outcome session in Simprints. Making this question required will ensure that a user has to callout to Simprints and can not skip the Simprints question.

**Step 17:** End the Simprints Identification Outcome Callout question group with the following question (refer to row 49 in sample form)

* end group

### Register last biometrics <a href="#h.p_hfgwf6gsx1zc_l" id="h.p_hfgwf6gsx1zc_l"></a>

**This feature is only available in Simprints ID 2020.2.1 or later. Please discuss with your Simprints Project Manager whether this feature is appropriate for your project.**

If the beneficiary was not listed in the top match results, the project may want a user to continue and register that individual's biometrics into the project. To do this you will need to create another group of questions for a Register Last Biometrics Callout.

This will send Simprints ID a prompt to enrol the biometrics collected and provide an enrolment GUID to the calling app.

Follow the steps below to create the enrol last biometrics callout (refer to rows 51 - 62 in sample form).

**Step 18:** Add a question for the user to confirm that they would like to register the individual into the project. (refer to rows 51-57 in the sample form)

* If the user decides to register the individual to the project continue registering the individual (refer to row 59 in the sample form)

In **rows 51-54**, we have a section to flag to the user if the results had a HIGH or MEDIUM confidence result but they decided to choose 'None of the above' from the results. In the logic we show, if there was a HIGH match, we will not allow them to enrol last biometric. If there was a MEDIUM match as the top result, we say that they can still enrol but it will be flagged in the system.

In **rows 55-57,** It shows a page to the user the other scenarios. If the top result was LOW or NONE, then they will be able to move forward with an enrolment (Register Last Biometrics). If there was a refusal and no fingerprints were captured, they will skip over this last section and go to the end of the form.

To enrol the individual's collected biometrics into the project, follow the steps below:

**Step 19:** Add a new group to callout to Simprints to enrol the biometrics:

* begin group: registrationgrp

In the intent column:

* input the intent below

```
com.simprints.simodkadapter.REGISTER_LAST_BIOMETRICS(projectId=${project_id}, sessionId=${odk-session-id}, userId=${user_id},moduleId=${module_id})
```

**Notes:**

**Register Last Biometrics**: callout to Simprints to register the last biometrics collected (i.e. from the initial identification) into the Simprints database.

Simprints will enrol the biometrics collected and return a GUID to ODK

Simprints will enrol the biometrics collected and return a GUID to your ODK app. In your ODK app you will want to retrieve and store the GUID to the beneficiary's record (refer to row 45-47 in sample form).

**Step 20:** Add a question with the name (row 60):

* text: odk-registration-id

In the label column:

* odk-registration-id: GUID

**Step 21:** Add a new question with the name (row 61)

* text: odk-biometrics-check

In the label column:

* odk-biometrics-complete: Biometrics complete

In the constraint column:

* odk-biometrics-complete: input .=`'true'`

In the constraint message column:

* odk-biometrics-complete: Please click launch

In the required column:

* odk-biometrics-complete: yes

**Notes:**

**odk-registration-id**: callback from Simprints with the GUID (randomly generated unique identifier that links a beneficiary’s ODK record to their biometrics)

**odk-biometrics-check:** callback from Simprints that denotes that a user has successfully launched the identification outcome session in Simprints. Making this question required will ensure that a user has to callout to Simprints and can not skip the Simprints question.

**Step 21:** End the Simprints Register Last Biometrics Callout question group with the following question (refer to row 52 in sample form)

* end group

### Worked example <a href="#h.p_etzs39ocjrwa_l" id="h.p_etzs39ocjrwa_l"></a>

{% embed url="https://docs.google.com/spreadsheets/d/1QfbWmSdmZJcJFxxekm1lyShFiEUIsInZ/edit?ouid=115631550410207806466&rtpof=true&sd=true&usp=sharing" %}
