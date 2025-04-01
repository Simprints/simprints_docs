# Identification

### What is Identification?

To continue with the clinic analogy, say you have been able to enrol enough beneficiaries at your clinic and you have a visit from a beneficiary who says they have been enroled at your clinic but you are now unable to find their records, due to misspelling or unavailable clinic card for example.&#x20;

You will need to identify this beneficiary by capturing their biometrics and running it through the system for potential matches. The beneficiary would then be selected from this list of potential matches. This is the Identification process.

## Identification Flow <a href="#h.26b2mahnvau3_l" id="h.26b2mahnvau3_l"></a>

1. Calling app creates and Identify Intent using project ID, module ID, and user ID and sends the intent to Simprints ID
2. Beneficiary's biometrics get captured and compared to find potential matching candidates and ranked in order of match accuracy
3. Simprints ID returns the generated ranked list of potential candidates
4. The calling app receives the resulting candidate list and uses each unique ID to find corresponding beneficiary info that's been stored within the calling app
5. The list of beneficiary info is then presented to the user to choose which belongs to the person
6. Calling app triggers a confirm identity callout, by creating intent with the selected records' unique ID
7. Simprints ID processes the request, boosting the accuracy of a match on subsequent requests, and returns the result to the user
8. The calling app ensures the flow is completed successfully and then moves on to other procedures within the calling app

### Trigger Identification

LibSimptints provides an activity result contract for streamlined and type-safe integration.&#x20;

To identify a beneficiary, you need to:&#x20;

#### Register activity result handler

When an Identification request is sent to Simprints ID, the beneficiary's biometrics are first captured and then matched with existing biometrics, which is compiled into a ranked list of possible matches and sent back to the calling app.

This generated ranked list contains records with each having the following properties:

* **guid** - the unique ID for the biometric record
* **confidence** - the level of confidence to which the record matches the captured biometric
* **confidenceBand** - descriptor of confidence level based on the project configuration (HIGH/MEDIUM/LOW)

Note:  The confidence and tier values can then be used to determine the ranking and accuracy for the matched biometric record. To get more info on this, [check here](confidence-score-bands.md).

```kotlin

val simprintsLauncher = registerForActivityResult(SimprintsContract()) { response ->

   if (response.resultCode != Activity.RESULT_OK) {
        // check-out Handling Errors page for reference
   }

   if (response.refusalForm != null) {
       // check-out Exit Form handling
   }

   if (response.biometricsComplete && response.identifications != null) {
        // get the session ID, from the resulting intent
      val sessionId = response.sessionId

        // extract the returned list
        val identifications = response.identifications

        // present the matching list to the user, following these steps
        //
        // STEP 1 - use the unique id, from the top matches, to retrieve 
        //          beneficiaries within your app.
        // STEP 2 - present this list of beneficiaries to the user to 
        //          select the correct beneficiary
    }
}
```

#### Launch the request

```kotlin

simprintsLauncher.launch(SimprintsRequest.Identify(
   projectId = "Project ID", 
   userId = "User ID",
   moduleId = "Module ID",
))
```

Simprints ID also accepts normal Android intents as requests but using the activity result contract is recommended to simplify the cross-application communication.&#x20;

### Confirmation Callout - (Confirm Identity)

Once the user has selected the correct beneficiary there needs to be another callout to Simprints ID to indicate which beneficiary was selected. The calling app needs to make a Confirmation Callout indicating the unique id of the selected beneficiary.

To make the confirmation callout, you would need to:

1. Get the sessionId and the selected beneficiary's unique ID
2. Trigger the ConfirmIdentity request on an activity result SimprintsContract launcher

```kotlin

val confirmationLauncher = registerForActivityResult(SimprintsContract()) { response ->
   if (response.resultCode != Activity.RESULT_OK) {
       // check-out Handling Errors page for reference
   }
}

fun onSelectBeneficiary(sessionId: String, selectedUserUniqueId: String) {
   identifyLauncher.launch(SimprintsRequest.ConfirmIdentity(
      projectId = "Project ID", 
      userId = "User ID",
      sessionId = sessionId,
      selectedGuid = selectedUserUniqueId,
   ))
}

```



Sometimes the health worker might receive a list of beneficiaries but none of them is the correct beneficiary. This can happen for a couple of reasons, like if the beneficiary was not previously enrolled, or if the beneficiary was enrolled with another health worker and SimprintsID didn't download the record yet. In those situations, we advise calling apps to have a button "none of the above" that can send this special case to SimprintsID.

```kotlin

identifyLauncher.launch(SimprintsRequest.ConfirmIdentity(
   projectId = "Project ID", 
   userId = "User ID",
   sessionId = "Session ID", 
   selectedGuid = Constants.SIMPRINTS_SELECTED_GUID_NONE,
))
```

### Handling Alternate Scenarios

During the identification process, the biometric capture and processing might not be completed due to two main reasons:

1. A system/application error occurred. To read more on how to handle application errors [check here](handling-errors.md).
2. The user backed out of completing the process. To read more on how to handle uncompleted workflow issues [check here](exit-forms.md).

There is a situation when you wouldn't have a match within the list of identified beneficiaries, so might want to go ahead and enrol the last captured biometric, this feature is called Identification+. For more information on this, [check here](enrolment-and-identification-+.md).

### Legacy versions usage

To call Simprints ID versions **before 2025.1.0** use the deprecated `SimHelper` class and handle the regular intent results:

```kotlin

// create an instance of SimHelper​
val simHelper = SimHelper("Project ID", "User ID")
​
// create an Intent using the identify method on the SimHelper object
val identifyIntent = simHelper.identify("Module ID")
​
// trigger the Intent using the startActivityForResult method​
startActivityForResult(identifyIntent, requestCode) // requestCode is required for Android intents
```

```kotlin

override fun onActivityResult(requestCode: Int, resultCode: Int, intentData: Intent) {
   super.onActivityResult(requestCode, resultCode, intentData)

   // check that neither Simprints or Android returned an error
   if (resultCode == Activity.RESULT_OK) {
      // ensure the requestCode is the one you sent for the identification request
      if (requestCode == enrolRequestCode) {
         // call method to handle enrolment
         handleIdentification(intentData)
      }
   } else {
      // check out Handling Errors page for reference
   }
}

fun handleIdentification(intentData: Intent) {
   if (biometricsCompleted) {
      // get the session ID, from the resulting intent
      val sessionId = intentData.getString(Constants.SIMPRINTS_SESSION_ID,"")
      // extract the returned list
      val identifications = intentData.getParcelableArrayList<Identification>(Constants.SIMPRINTS_IDENTIFICATIONS)
      
      //... TODO present the matching list to the user, following these steps
      // STEP 1 - use the unique id, from the top matches, to retrieve beneficiaries
      // within your app.
      // STEP 2 - present this list of beneficiaries to the user to select the correct beneficiary
   }
}
```
