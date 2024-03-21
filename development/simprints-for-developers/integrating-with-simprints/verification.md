# Verification

### What is Verification?

Still continuing with the clinic scenario, if you have a beneficiary come to your clinic with their clinic card, you will need to verify that the beneficiary is who they say they are.

This **verification** process involves matching previously enrolled biometric data with the present beneficiary's biometric.

### Verification Flow

1. Calling app creates a **Verify** Intent using **projectID, moduleID, userID,** and beneficiary's **uniqueID** and sends intent to **Simprints ID**
2. Beneficiary's biometrics gets captured and processed to validate that it matches with previously enrolled biometrics using provided **uniqueID**
3. **Simprints ID** returns the verification result to calling app
4. Calling app receives verification results, and handles the success or failure workflow.

### Trigger Verification

To trigger a **verification** process, you need to do the following:

```
public void onActivityResult(int requestCode, int resultCode, Intent data) {

super.onActivityResult(requestCode, resultCode, data)



// ensure the resultCode is okay

if (resultCode == Activity.RESULT_OK) {

   // ensure the requestCode is for verification

   if (requestCode == verifyRequestCode) {

      // handle the verification result

      handleVerification(data);

   }

} else {

   // check out Handling Errors page for reference

}

}
```

### Handling Verification Response

During the verification process, the present beneficiary's biometrics are captured, and the **verifyGuid** which is passed in the **Intent** is used to retrieve the previously enroled beneficiary's biometrics. The two biometric records are then compared.

If this **verification** process completes successfully, a verification result containing the following properties is returned:

* **Tier** - the ranking tier for the biometric record
* **Confidence** - the percentage to which the record **matches** the captured biometric
* **Guid -** the unique id for the biometric record

Note: The **confidence** and **tier** values can then be used to determine the ranking and accuracy for the matched biometric record, to get more info on this, [check here](tiers-and-confidence-scores.md).

```
import com.simprints.libsimprints.Constants;



private void handleVerification(Intent data) {



// get the boolean flag to check if biometrics completed successfully

Boolean biometricsCompleted = data.getBooleanExtra(Constants.SIMPRINTS_BIOMETRICS_COMPLETE_CHECK);



if (biometricsCompleted) {

   // extract the returned Verification object

   Verification verification = data.getParcelableExtra(Constants.SIMPRINTS_VERIFICATION);



   // get tier and confidence score

   Tier tier = verification.getTier()

   float confidence = verification.getConfidence();

}

}
```

### Handling Alternate Scenarios

During the enrolment process, the biometric capture and processing might not complete due to two main reasons:

1. A system/application error occurred. To read more on how to handle application errors [check here](handling-errors.md).
2. The user backed out of completing the process. To read more on how to handle uncompleted workflow issues [check here](exit-forms.md).
