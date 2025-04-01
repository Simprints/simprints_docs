# Verify

**Verification** is used when you want to verify if a beneficiary is who they say they are (1:1 search). Simprints will match the biometrics collected against those enrolled for a particular beneficiary and send the results to the ODK form

**The steps below references this example form for identification** ([**link**](https://docs.google.com/spreadsheets/d/1RsaW7pVkzTixhufQLNEHbQJY7UtWQhiP/edit?dls=true)).

### Run a verification <a href="#h.p_fvtdli8isyej_l" id="h.p_fvtdli8isyej_l"></a>

Before starting to build your verification form, do check you have the reference form and the correct columns in your form:

**Step 1:** Open your XML verification form and have the sample form provided as reference ([link](https://docs.google.com/spreadsheets/d/1RsaW7pVkzTixhufQLNEHbQJY7UtWQhiP/edit?dls=true)).

**Step 2:** Check that these columns are present in your XML form. If not, add them:

*
  * type
  * name
  * label
  * intent
  * appearance
  * calculation

For a verification, a user will need to select the beneficiary record that they are trying to verify their biometrics against. This is often an existing secondary unique identifier (e.g. first name, phone number). For this example, names enrolled will be used as a secondary unique identifier (refer to row 11 in sample form)

**Step 3:** Add a question that will provide a list of all names enrolled in the project for a user to select the beneficiary's name from

* appearance: search('sampledatareg')

As part of the Simprints integration, you will need to collect information about the user (User ID) and module (Module ID). Please click [here](../../integrating-with-simprints/getting-started.md) for more information on user ID and module ID.

**Step 4:** Add questions to collect information about the user and module (refer to rows 12-13 in the sample form)

Add questions to collect basic demographic information from a beneficiary as required for your project.

\
Create 4 "calculate" questions to store the parameters needed for Simprints ID (Refer to rows 14-17 in the sample form)

The verify\_GUID will provide Simprints with the GUID to match the beneficiary's biometrics against

**Step 5**: In your ODK form, add foru rows for four calculate questions and the following names in the 'name' column:

* user\_id
* module\_id
* project\_id
* verify\_GUID

In the calculation column:

* user\_id: link this to the user filling out the form (if there’s a user question earlier in the form, if not input a string “user”)
* module\_id: link this to the module related question earlier in the form (e.g. question on district), if not input a string “module”
* project\_id: input the project credentials provided by the Simprints project manager. This is an alphanumeric string: “xxxxxx”
* verify\_GUID: link this to the odk-registration-id for the name selected in the question above

To create a Simprints callout and begin fingerprint scanning, create a new group in your ODK form (refer to row 18 in the sample form).

**Step 6:** Add a new group of questions (after the calculate questions) to callout to Simprints with the name:

* begin group: verificationgrp

In the appearance column:

* verificationgrp: field-list

The column "intent" is not part of a regular ODK form and you will need to add it as a column in your ODK form. This is needed to trigger a callout to Simprints ID app from the ODK form.

**Step 7:** In the intent column for verificationgrp add:

```
com.simprints.simodkadapter.VERIFY(projectId=${project_id}, userId=${user_id}, moduleId=${module_id},verifyGuid=${verify_GUID})
```

**Notes:**

**verificationgrp**: callout to Simprints to collect a beneficiary’s fingerprints

After collecting a beneficiary's biometrics you will want to check if the biometrics collected matches the enrolled GUID record (refer to rows 19-22 in the sample form).

**Step 8:** Add the following questions to extract the top match scores, [tiers](../../integrating-with-simprints/confidence-score-bands.md), GUIDs and session ID:

* text: odk-confidences
* text: odk-tiers
* text: odk-session-id

In the label column:

* odk-guids: GUIDs:
* odk-confidences: Confidences:
* odk-tiers: Tiers:
* odk-session-id: Simprints Session ID:

**Notes:**

**odk-confidences** and **odk-tiers**: callback from Simprints, with match score and tier when verifying fingerprints collected against enroled GUID (Tier 1 is the best and Tier 5 the worst)

**odk-session-id**: unique session id for each Simprints verification

To add a check on whether a user has completed a Simprints flow, use the Biometrics Complete check boolean (refer to row 23 of sample form).

To find out more about Biometrics Complete check, click [here](../../integrating-with-simprints/confidence-score-bands.md).

**Step 9:** Add a new question with the name

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

**odk-biometrics-check:** callback from Simprints that denotes when a Simprints session has been completed by a user. Making this question required will ensure that a user has to callout to Simprints and can not skip the Simprints question. Adding a constraint (when the check = true) will mean a user can not move forward in their workflow unless they have completed a session in Simprints.

In your ODK form, you can record reasons to why a user left the Simprints application without collecting a beneficiary's biometrics. To enable this check in your ODK form, follow the steps below (refer to rows 24-25 in sample form)

**Step 10:** Add a new group of questions with the names

* text: odk-exit-reason\*
* text: odk-exit-extra\*

In the label column:

* odk-exit-reason: Simprints exit reason\*
* odk-exit-extra: Simprints exit details\*

**Notes:**

\***odk-exit-reason:** when a user submits an exit form in Simprints, ODK will be able to capture the exit reason. This field will only populate, when a user submits an exit form in Simprints.

\***odk-exit-extra:** when a user submits an exit form with additional information in Simprints, ODK will be able to capture the additional information a user provided here. This field will only populate, when a user submits an exit reason with additional detail in Simprints.

\* Optional fields that you can add to your identification form. Please get in touch with your Simprints project manager on when to use these fields.

**Step 11:** End the Simprints verification question group with the following question (refer to row 26 in sample form)

* end group

The last step is to check whether the biometrics collected matched the GUID from enrolment, based on the verification results received from Simprints

**Step 12:** Add the following questions to check the verification results received from Simprints (refer to rows 27-18 in sample form)

* calculate: verified
* note: verified\_note

In the calculation column:

* verified: Please include the code snippet below

```
if (${odk-tiers}='', '', if ((string-length(${odk-tiers})>0 and ${odk-tiers}!= 'TIER_5'), 'SUCCESSFUL', 'FAILED'))
```

In the label column:

* verified\_note: Include the code snippet below

```
${verified}
```

You have successfully integrated verification in your workflow!

### Worked example <a href="#h.p_hfgwf6gsx1zc_l" id="h.p_hfgwf6gsx1zc_l"></a>

{% embed url="https://docs.google.com/spreadsheets/d/1RsaW7pVkzTixhufQLNEHbQJY7UtWQhiP/edit?ouid=115631550410207806466&rtpof=true&sd=true&usp=sharing" %}
