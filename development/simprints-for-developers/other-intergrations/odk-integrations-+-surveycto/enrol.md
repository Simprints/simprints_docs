# Enrol

During an enrolment, a frontline worker will collect a beneficiary's biometrics. Simprints then generates a biometrics template and links that to a Globally Unique Identifier (GUID). The GUID is a random alphanumeric string that links the template in Simprints to the ODK record. The GUID is shared with the ODK form and stored along with other demographic information collected through the ODK form.

**The steps below references this example form for enrolment (**[**link**](https://docs.google.com/spreadsheets/d/1kBtckh8Wl8LtL-6qWYiY3S5cMX7OO6qV/edit#gid=1042672063)**).**

### XML enrolment form <a href="#h.p_frihaocs3ivo_l" id="h.p_frihaocs3ivo_l"></a>

Before starting to build your enrolment form, do check you have the reference form and the correct columns in your form:

**Step 1:** Open your XML registration form and have the sample form provided as reference ([link](https://docs.google.com/spreadsheets/d/1kBtckh8Wl8LtL-6qWYiY3S5cMX7OO6qV/edit#gid=1042672063)).

**Step 2:** Check that these columns are present in your XML form. If not, add them:

*
  * type
  * name
  * label
  * intent
  * appearance
  * calculation

As part of the Simprints integration, you will need to collect information about the user (User ID) and module (Module ID). Please click [here](https://sites.google.com/simprints.com/simprints-for-developers/integrating-w-simprints-old) for more information on user ID and module ID.

**Step 3:** Add questions to collect information about the user and module (refer to rows 12-13 in the sample form)

Add questions to collect basic demographic information from a beneficiary as required for your project.

**Step 4:** In your XML form, add three rows for three calculate questions and the following names in the 'name' column (Refer to rows 15-17 in the sample form):

* user\_id
* module\_id
* project\_id

In the calculation column:

* user\_id: link this to the user filling out the form (if there’s a user question earlier in the form, if not input a string “user”)
* module\_id: link this to the module related question earlier in the form (e.g. question on district), if not input a string “module”
* project\_id: input the project credentials provided by the Simprints project manager. This is an alphanumeric string in quotation marks: “xxxxxx”

**Step 5:** Add a new group of questions (after the calculate questions) to callout to Simprints with the name (refer to row 18 in the sample form):

* begin group: registrationgrp

In the appearance column:

* registrationgrp: field-list

The column "intent" is not part of a regular ODK form and you will need to add it as a column in your ODK form. This is needed to trigger a callout to Simprints ID app from the ODK form.

**Step 6:** In the intent column for registrationgrp add:

```
com.simprints.simodkadapter.REGISTER(projectId=${project_id}, moduleId=${module_id}, userId=${user_id})
```

**Notes:**

**registrationgrp**: callout to Simprints to collect a beneficiary’s fingerprints

**Step 7:** After registering a beneficiary's biometrics, you will want to retrieve and store the GUID (refer above) to the beneficiary's record in your ODK app (refer to row 19 in sample form).

Add a question with the name:

* text: odk-registration-id

In the label column:

* odk-registration-id: ID

**Notes:**

**odk-registration-id**: callback from Simprints with the GUID (randomly generated unique identifier that links a beneficiary’s ODK record to their biometrics)

**Step 8:** To add a check on whether a user has completed a Simprints flow, use the Biometrics Complete check boolean (refer to row 20 of sample form).

To find out more about Biometrics Complete check, click [here](https://sites.google.com/simprints.com/simprints-for-developers/integrating-w-simprints-old/custom-integrations/biometrics-complete).

Add a new `Text` row with the name:

* odk-biometrics-check

In the label column:

* input the following phrase: `Biometrics` complete:

In the constraint column:

* input the following statement: .=`'true'`

In the constraint message column:

* input the following phrase: `Please click launch`

In the required column:

* input the following phrase: `yes`

**Notes:**

**odk-biometrics-check:** callback from Simprints that denotes when a Simprints session has been completed by a user. Making this question required will ensure that a user has to call out to Simprints and can not skip the Simprints question. Adding a constraint (when the check = true) will mean a user can not move forward in their workflow unless they have completed a session in Simprints.

**Step 9:** In your ODK form, you can record reasons why a user left the Simprints application without collecting a beneficiary's biometrics. To enable this check in your ODK form, follow the steps below (refer to rows 21-22 in the sample form)

Add a new group of questions with the names:

* text: odk-exit-reason\*
* text: odk-exit-extra\*

In the label column:

* odk-exit-reason: Simprints exit reason\*
* odk-exit-extra: Simprints exit details\*

**Notes:**

\***odk-exit-reason:** when a user submits an exit form in Simprints, ODK will be able to capture the exit reason. This field will only populate when a user submits an exit form in Simprints.

\***odk-exit-extra:** when a user submits an exit form with additional information in Simprints, ODK will be able to capture the additional information a user provided here. This field will only populate when a user submits an exit reason with additional detail in Simprints.

\* Optional fields that you can add to your enrolment form. Please get in touch with your Simprints project manager on when to use these fields.

**Step 10:** End the Simprints enrolment question group with the following questions (refer to rows 23-24 in sample form)

* note: enrolnote
* end group

In the label column:

* enrolnote: Once fingerprints have been collected, please swipe to the next screen\*

**Step 11:** Save the form and upload it on the ODK web platform

### Handling enrolment data <a href="#h.p_nsrbr9bl3u6j_l" id="h.p_nsrbr9bl3u6j_l"></a>

Data from completed enrolment forms will need to be formatted and stored for the successful identification of a beneficiary. Data can be stored in a CSV file.

Store enrolment data as a CSV file (in the identification and verification forms, the file is saved as "sampledatareg"). The file must include columns on Simprints GUID and a secondary identifier from the ODK enrolment form (e.g. age, name, study ID). The title of the columns need to correlate to the labels used on the enrolment form.

If you are using SurveyCTO, there's another option of storing enrolment data as a dataset on the SurveyCTO web platform. Refer to SurveyCTO documentation on how to do this [here](https://www.google.com/url?q=https%3A%2F%2Fsimprints.surveycto.com%2Fhelp.html%23Help\_Designing-forms\_advanced-topics\_preloading\&sa=D\&sntz=1\&usg=AOvVaw1nmm4ieE2zKWxyQgOyoKiP)

### Example <a href="#h.p_ks7nwjsegueb_l" id="h.p_ks7nwjsegueb_l"></a>

{% embed url="https://docs.google.com/spreadsheets/d/1kBtckh8Wl8LtL-6qWYiY3S5cMX7OO6qV/edit#gid=1042672063" %}

### Using Enrolment+ <a href="#h.p_ionvsuz-24x__l" id="h.p_ionvsuz-24x__l"></a>

The difference during Enrolment+ is Simprints checks for a suspicious or potential duplicate in the system. Think of it like any registration process when opening a new account online with a bank. If you try to enrol the same person twice it will flag it in the system. Here are the biggest differences.

**Step 1:** If we see a potential duplicate, rather than enrolling the biometrics we send back the potential duplicate results for the frontline worker to adjudicate. Therefore we need to add the following rows in the **registrationgrp**:

* type: `text` ; name: `odk-guids` ; label: `GUIDs:`
* type: `text` ; name: `odk-confidences` ; label: `Confidences:`
* type: `text` ; name: `odk-match-confidence-flags` ; label: `Match confidence flags:`
* type: `text` ; name: `odk-highest-match-confidence-flag` ; label: `Highest match confidence flag:`
* type: `text` ; name: `odk-tiers` ; label: `Tiers:`

The `odk-match-confidence-flags` is returned with each result, depending on how we determine our thresholds on confidence. So along with a pure confidence score, it will be accompanied with a flag NONE, LOW, MEDIUM, HIGH. This doesn't occur in any of the logic, but it may be useful metadata to have a hold of.

The `odk-highest-match-confidence-flag` is returned with one value for the entire results list. It represents the Confidence Flag of the top result. This is used to determine logic throughout the flow.

#### **Step 2:** Next we have to add the identification rows in case results come back with a potential duplicate. This corresponds to rows 28-42 in the spreadsheet below. This operation will only run if the condition in the **relevance** column in row 28 is met, which is `${odk-guids} != ""` <a href="#h.21lr8zrnppyw_l" id="h.21lr8zrnppyw_l"></a>

**Step 3:** Rows 44-49 are also identical to what you would find in an identification flow. If one of the results is in fact corresponding to the person in front of you, these rows will allow you to select that correct beneficiary so that you do not enrol the beneficiary a second time and then return that result to simprints using the `com.simprints.simodkadapter.CONFIRM_IDENTITY(projectId=${project_id}, sessionId=${odk-session-id}, selectedGuid=${selected_id})` intent.

**Step 4:** Rows 51-54 we have a group called `flag_duplicate`. This only displays if the frontline worker decides to try to enrol a beneficiary when we send back a Medium or High confidence potential duplicate.

#### **Step 5:** In the next group called `registrationgrp_1` you have an opportunity to enrol the beneficiary (in the case in the documentation we provide, we have relevance that restricts a beneficiary from being enrolled if a HIGH confidence flag is present. Here is the callout in the 'INTENT' column. <a href="#h.ja21vzydpebx_l" id="h.ja21vzydpebx_l"></a>

`com.simprints.simodkadapter.REGISTER_LAST_BIOMETRICS(projectId=${project_id}, sessionId=${odk-session-id}, userId=${user_id},moduleId=${module_id})`

**Step 6:** The next group we have in this form is to add personal details only if you have registered biometrics. In this case we added name and age.

**Step 7:** We have configured different notes the display at the end of this form depending on the endpoint of the workflow. This can be configured however you like but we provide examples in Rows 66-72

{% embed url="https://drive.google.com/file/d/1X0ZMRK-VokqwbW3_PhKKNUZBxMmYQCPq/view" %}
