# Verification

### What is Verification?

Continuing with the clinic scenario, if you have a beneficiary come to your clinic with their clinic card, you will need to verify that the beneficiary is who they say they are.

This **verification** process involves matching previously enrolled biometric data with the present beneficiary's biometric.

### Verification Flow

1. Calling app creates a **Verify** Intent using **projectID, moduleID, userID,** and beneficiary's **uniqueID** and sends intent to **Simprints ID**
2. Beneficiary's biometrics get captured and processed to validate that they match with previously enrolled biometrics using the provided **unique ID**
3. **Simprints ID** returns the verification result to the calling app
4. The calling app receives verification results, and handles the success or failure workflow.

### Trigger Verification

LibSimprints provides an activity result contract for streamlined and type-safe integration.&#x20;

To verify a beneficiary, you need to:&#x20;

#### Register activity result handler

During the verification process, the present beneficiary's biometrics are captured, and the **verifyGuid** which is passed in the **Intent** is used to retrieve the previously enrolled beneficiary's biometrics. The two biometric records are then compared.

If this **verification** process completes successfully, a verification result containing the following properties is returned:

* **guid -** the unique ID for the biometric record
* **confidence** - the percentage to which the record **matches** the captured biometric
* **confidenceBand** - descriptor of confidence level based on the project configuration (HIGH/MEDIUM/LOW)
* **isSuccess** - indicates whether the verification is considered successful based on a verification match threshold set in the project settings.

{% hint style="warning" %}
Note: In **Simprints ID** v2024.2.0 the verification judgement flag is provided separately from the Verification object. In  **Simprints ID** v2025.1.0 with **LibSimprints** v2025.1.2 it is available directly in the Verification object, in earlier versions it can be extracted from response intent directly:

`data.getBooleanExtra(Constants.SIMPRINTS_VERIFICATION_SUCCESS)`
{% endhint %}

Note: The **confidence** and **confidenceBand** values can then be used to determine the ranking and accuracy for the matched biometric record, to get more info on this, [check here](confidence-score-bands.md).

```kotlin

val simprintsLauncher = registerForActivityResult(SimprintsContract()) { response ->

    if (response.resultCode != Activity.RESULT_OK) {
        // check-out Handling Errors page for reference
    }

    if (response.refusalForm != null) {
       // check-out Exit Form handling
    }

    if (response.biometricsComplete && response.verification != null) {
        // get confidence score
        val isSuccess = respose.verification.isSuccess
        val band = response.verification.confidenceBand
        val confidence = response.verification.confidence
    }
}
```

#### Launch the request

```kotlin

simprintsLauncher.launch(SimprintsRequest.Verify(
  projectId = "Project ID", 
  userId = "User ID",
  moduleId = "Module ID",
  verifyId = "GUID to verify",
))
```

Simprints ID also accepts normal Android intents as requests but using the activity result contract is recommended to simplify the cross-application communication.&#x20;

#### Verification Judgement

Starting with **Simprints ID** v2024.2.0 along with the Verification object, a "verification judgement" result will be returned. It's name is **SIMPRINTS\_VERIFICATION\_SUCCESS**, it's a **Boolean** and will indicate whether the verification is considered successful based on a verification match threshold set in the project settings.

```kotlin

private fun handleVerification(intentData: Intent) {
   if (biometricsCompleted) {
      // extract the verification judgement
      val verificationIsSuccessful = intentData.getBooleanExtra(Constants.SIMPRINTS_VERIFICATION_SUCCESS);
   }
}
```

### Handling Alternate Scenarios

During the enrolment process, the biometric capture and processing might not be completed due to two main reasons:

1. A system/application error occurred. To read more on how to handle application errors [check here](handling-errors.md).
2. The user backed out of completing the process. To read more on how to handle uncompleted workflow issues [check here](exit-forms.md).

### Legacy versions usage

To call Simprints ID versions **before 2025.1.0** use the deprecated `SimHelper` class and handle the regular intent results:

```kotlin

// create an instance of SimHelper​
val simHelper = SimHelper("Project ID", "User ID")
​
// create an Intent using the identify method on the SimHelper object
val identifyIntent = simHelper.verify("Module ID")
​
// trigger the Intent using the startActivityForResult method​
startActivityForResult(enrolIntent, requestCode) // requestCode is required for Android intents
```

```kotlin

override fun onActivityResult(requestCode: Int, resultCode: Int, intentData: Intent) {
   super.onActivityResult(requestCode, resultCode, intentData)

   // check that neither Simprints or Android returned an error
   if (resultCode == Activity.RESULT_OK) {
      // ensure the requestCode is the one you sent for the verification request
      if (requestCode == enrolRequestCode) {
         // call method to handle verification
         handleVerification(intentData)
      }
   } else {
      // check out Handling Errors page for reference
   }
}

fun handleVerification(intentData: Intent) {
   // get the boolean flag to check if biometrics completed successfully
   val biometricsCompleted = intentData.getBooleanExtra(Constants.SIMPRINTS_BIOMETRICS_COMPLETE_CHECK)

   if (biometricsCompleted) {
      // extract the verification judgement
      val verificationIsSuccessful = intentData.getBooleanExtra(Constants.SIMPRINTS_VERIFICATION_SUCCESS)
   
      // extract the returned Verification object
      val verification = intentData.getParcelableExtra<Verification>(Constants.SIMPRINTS_VERIFICATION)
      // get confidence score
      val confidence = verification.getConfidence()
   }
}
```
