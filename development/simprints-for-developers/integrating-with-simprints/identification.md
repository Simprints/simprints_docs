# Identification

### What is Identification?

To continue with the clinic analogy, say you have been able to enrol enough beneficiaries at your clinic and you have a visit from a beneficiary who says they have been enroled at your clinic but you are now unable to find their records, due to misspelling or unavailable clinic card for example.&#x20;

You will need to identify this beneficiary by capturing their biometrics and running it through the system for potential matches. The beneficiary would then be selected from this list of potential matches. This is the Identification process.

## Identification Flow <a href="#h.26b2mahnvau3_l" id="h.26b2mahnvau3_l"></a>

1. Calling app creates and Identify Intent using projectID, moduleID, and userID and sends intent to Simprints ID
2. Beneficiary's biometrics gets captured and compared to find potential matching candidates and ranked in order of match accuracy
3. Simprints ID returns the generated ranked list of potential candidates
4. Calling app receives the resulting candidate list and uses each unique ID to find corresponding beneficiary info that's been stored within the calling app
5. The list of beneficiary info is then presented to the user to choose which belongs to the person
6. Calling app triggers a confirm identity callout, by creating intent with the selected records unique Id
7. Simprints ID processes request, boosting the accuracy of a match on subsequent requests, and returns result to the user
8. Calling app ensures the flow is completed successfully and then moves on to other procedures within the calling app

### Trigger Identification

Simprints ID accepts normal Android intents as requests and you can also use SimHelper to streamline creating the intent. The steps for triggering an Identification are:

```
// create an instance of SimHelper

SimHelper simHelper = new SimHelper("Project ID", "User ID");

// create an Intent using the identify method on the SimHelper object

Intent identifyIntent = simHelper.identify("Module ID");

// trigger the Intent using the startActivityForResult method

startActivityForResult(enrolIntent, requestCode); // requestCode is required for Android intents
```

### Handling Identification Response

When an Identification request is sent to Simprints ID, the beneficiary's biometrics are first captured and then matched with existing biometrics, which is compiled into a ranked list of possible matches and sent back to the calling app.

This generated ranked list contains records with each having the following properties:

* Tier - the ranking tier for the biometric record
* Confidence Score - the level of confidence to which the record matches the captured biometric
* Guid - the unique id for the biometric record

Note:  The confidence and tier values can then be used to determine the ranking and accuracy for the matched biometric record. To get more info on this, [check here](tiers-and-confidence-scores.md).

```
public void onActivityResult(int requestCode, int resultCode, Intent data) {

super.onActivityResult(requestCode, resultCode, data)


// ensure the resultCode is okay

if (resultCode == Activity.RESULT_OK) {

   // ensure this is a result to identification request

   if (requestCode == identifyRequestCode) {

      handleIdentification(data);

   }

} else {

   // check out Handling Errors page for reference

}
}
```

```
import com.simprints.libsimprints.Constants;

private void handleIdentification(Intent data) {

// get the boolean flag to check if biometrics completed successfully

Boolean biometricsCompleted = data.getBooleanExtra(Constants.SIMPRINTS_BIOMETRICS_COMPLETE_CHECK);

if (biometricsCompleted) {

   // get the session ID, from the resulting intent

     String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");

   // extract the returned list

   ArrayList<Identification> identifications = 	data.getParcelableArrayList(Constants.SIMPRINTS_IDENTIFICATIONS);

     //... TODO present the matching list to the user, following these steps

     // STEP 1 - use the unique id, from the top matches, to retrieve beneficiaries

   // within your app.

     // STEP 2 - present this list of beneficiaries to the user to select the correct beneficiary

}
}
```

### Confirmation Callout - (Confirm Identity)

Once the user has selected the correct beneficiary there needs to be another callout to Simprints ID to indicate which beneficiary was selected. The calling app needs to make a Confirmation Callout indicating the unique id of the selected beneficiary.

To make the confirmation callout, you would need to:

1. Get the sessionId and the selected beneficiary's unique ID
2. Create an Intent using the confirmIdentity method on the SimHelper object
3. Trigger the Intent for the Identification callout

```
private void onSelectBeneficiary(String selectedUserUniqueId) {

   // get the session ID, from the resulting intent

   String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");

   // create an intent using the selected beneficiary's uniqueId, and sessionId

   Intent intent = simHelper.confirmIdentity(context, sessionId, selectedUserUniqueId);

   startActivityForResult(intent, confirmIdentityRequestCode);
}
```

Sometimes the health worker might receive a list of beneficiaries but none of them is the correct beneficiary. This can happen for a couple of reasons, like if the beneficiary was not previously enrolled, or if the beneficiary was enrolled with another health worker and SimprintsID didn't download the record yet. In those situations, we advise calling apps to have a button "none of the above" that can send this special case to SimprintsID.

```
// get the session ID, from the resulting intent

String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");

// create an intent using the sessionId and stating that no beneficiary was selected

Intent intent = simHelper.confirmIdentity(context, sessionId, "none_selected");

startActivityForResult(intent, confirmIdentityRequestCode);
```

### Handling Alternate Scenarios

During the identification process, the biometric capture and processing might not complete due to two main reasons:

1. A system/application error occurred. To read more on how to handle application errors [check here](handling-errors.md).
2. The user backed out of completing the process. To read more on how to handle uncompleted workflow issues [check here](exit-forms.md).

There is a situation when you wouldn't have a match within the list of identified beneficiaries, so might want to go ahead and enrol the last captured biometric, this feature is called Identification+. For more information on this, [check here](enrolment-and-identification-+.md).
