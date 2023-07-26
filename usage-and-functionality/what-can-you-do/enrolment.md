# Enrolment

## **What is Enrolment**

Say you are a health clinic that provides health care to beneficiaries in your community. You would need a way to keep track of the beneficiary's health records at your clinic, hence you would want to **register** them in your system and store information for subsequent visits, health issues, and treatments.

Registering a new beneficiary is the enrolment process in **Simprints ID**.

### Enrolment Flow

1. Calling app creates a **Register** Intent using **projectID**, **moduleID** and **userID** and sends intent to **Simprints ID**&#x20;
2. Beneficiary's biometrics get captured and processed to generate a **uniqueId** and **biometric template**&#x20;
3. **Simprints ID** returns the generated **uniqueId** to the calling app&#x20;
4. Calling app receives the resulting **uniqueId** and stores it with the beneficiary's personal info.

### Trigger Enrolment

Simprints ID accepts normal Android intents as requests and you can also use SimHelper to streamline creating the intent. To register a new beneficiary, you need to:

{% code lineNumbers="true" %}
```kotlin
// create an instance of SimHelper
SimHelper simHelper = new SimHelper("Project ID", "User ID");

// create an Intent using the register method on the SimHelper object
Intent enrolIntent = simHelper.register("Module ID");

// trigger the Intent using the startActivityForResult method
startActivityForResult(enrolIntent, requestCode); // requestCode is required for Android intents
```
{% endcode %}



### Handling Enrolment Response

During the enrolment process, **Simprints ID** captures the beneficiary's biometric data, processes it, and then generates both a biometric **template** and a **unique ID** which are saved in Simprints ID, and the unique ID is returned to the calling app.&#x20;

If the enrolment was successful, this **unique ID** is sent back in an **Intent** to the calling Android application.&#x20;

This unique ID should be extracted from the response and saved in your Android application and server together with the beneficiary information. This is how you link a record on your side with a Simprints record.&#x20;

The response comes via Android as part of the **onActivityResult**:

```kotlin
@Override 
public void onActivityResult(int requestCode, int resultCode, Intent data) {
   super.onActivityResult(requestCode, resultCode, data)

// check that neither Simprints or Android returned an error
if (resultCode == Activity.RESULT_OK) {
   // ensure the requestCode is the one you sent for the enrolment request
   if (requestCode == enrolRequestCode) {
      // call method to handle enrolment
      handleEnrolment(data);
   }
} else {
     // check out Handling Errors page for reference
  }
}
```

```kotlin
import com.simprints.libsimprints.Constants;

public void handleEnrolment(Intent data) {

// get the boolean flag to check if biometrics completed successfully
Boolean biometricsCompleted = data.getBooleanExtra(Constants.SIMPRINTS_BIOMETRICS_COMPLETE_CHECK);

if (biometricsCompleted) {
   // extract the returned Registration object
   Registration registration = data.getParcelableExtra(Constants.SIMPRINTS_REGISTRATION);
    
   // this is the unique id to be saved with the persons data
     String simprintsUniqueId = registration.getGuid();
}
}
```

Note: the properties of the Registration object are:&#x20;



* guid - a String value containing the generated unique identifier (which can be accessed through the getGuid method)&#x20;
* \[deprecated] template - a byte array containing the generated biometric template (which can be accessed through the getTemplate method)
* fsdfdsfds
* dsfdsfds
