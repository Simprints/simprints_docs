# Enrollment

### What is Enrolment?

Say you are a health clinic providing health care to your community's beneficiaries. You would need a way to keep track of the beneficiary's health records at your clinic, hence you would want to **register** them in your system and store information for subsequent visits, health issues, and treatments.

Registering a new beneficiary is the **enrolment** process in **Simprints ID**.

### Enrolment Flow

1. Calling app creates a **Register** Intent using **projectID, moduleID** and **userID** and sends intent to **Simprints ID**
2. Beneficiary's biometrics get captured and processed to generate a **GUID** and **biometric templates**
3. **Simprints ID** returns the generated **GUID** to the calling app
4. The calling app receives the resulting **GUID** and stores it with the beneficiary's personal info.

### Trigger Enrolment

LibSimptints provides an activity result contract for streamlined and type-safe integration.&#x20;

To register a new beneficiary, you need to:&#x20;

#### Register activity result handler

During the enrolment process, **Simprints ID** captures the beneficiary's biometric data, processes it, and then generates both a biometric **template** and a **unique ID.** Both are saved in **Simprints ID**, and the **unique ID** is returned to the calling app.

This **unique ID** should be extracted from the response and saved in your Android application and server together with the beneficiary information. This is how you link a record on your side with a Simprints record.

```kotlin

val simprintsLauncher = registerForActivityResult(SimprintsContract()) { response ->
    
    if (response.resultCode != Activity.RESULT_OK) {
        // check-out Handling Errors page for reference
    }

    if (response.refusalForm != null) {
        // check-out Exit Form handling
    }
    
    if (response.biometricsComplete  && response.enrolment != null) {
        // this is the GUID to be saved with the person's data
        val simprintsUniqueId = response.enrolment.guid
    }
}
```

#### Launch the request

```kotlin

simprintsLauncher.launch(SimprintsRequest.Enrol(
  projectId = "Project ID", 
  userId = "User ID",
  moduleId = "Module ID",
))
```

Simprints ID also accepts normal Android intents as requests but using the activity result contract is recommended to simplify the cross-application communication.&#x20;

### Handling Alternate Scenarios

During the enrolment process, the biometric capture and processing might not be completed due to two main reasons:

1. A system/application error occurred. To read more on how to handle application errors [check here](handling-errors.md).
2. The user backed out of completing the process. To read more on how to handle uncompleted workflow issues [check here](exit-forms.md).

We have a feature called **Enrolment+,** that lets you check if a beneficiary has been enrolled before making a new enrolment. To see how to implement this [check here](enrolment-and-identification-+.md).

### Legacy versions usage

To call Simprints ID versions **before 2025.1.0** use the deprecated `SimHelper` class and handle the regular intent results:

```kotlin

// create an instance of SimHelper​
val simHelper = SimHelper("Project ID", "User ID")
​
// create an Intent using the register method on the SimHelper object
val enrolIntent = simHelper.register("Module ID")
​
// trigger the Intent using the startActivityForResult method​
startActivityForResult(enrolIntent, requestCode) // requestCode is required for Android intents
```

```kotlin

override fun onActivityResult(requestCode: Int, resultCode: Int, intentData: Intent) {
   super.onActivityResult(requestCode, resultCode, intentData)

   // check that neither Simprints or Android returned an error
   if (resultCode == Activity.RESULT_OK) {
      // ensure the requestCode is the one you sent for the enrolment request
      if (requestCode == enrolRequestCode) {
         // call method to handle enrolment
         handleEnrolment(intentData)
      }
   } else {
      // check out Handling Errors page for reference
   }
}

fun handleEnrolment(intentData: Intent) {
   // get the boolean flag to check if biometrics completed successfully
   val biometricsCompleted = intentData.getBooleanExtra(Constants.SIMPRINTS_BIOMETRICS_COMPLETE_CHECK)

   if (biometricsCompleted) {
      // extract the returned Registration object
      val registration = intentData.getParcelableExtra<Registration>(Constants.SIMPRINTS_REGISTRATION)
      // this is the unique id to be saved with the persons data
      val simprintsUniqueId = registration.getGuid()
   }
}
```
