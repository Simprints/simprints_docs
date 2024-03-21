# Enrolment & Identification +

There are times during enrolment or identification when you would either find a matching biometric (for enrolment) or find no-matching biometric data (for Identification). Whenever a case like this occurs, you might want to enrol the last captured biometric data, this process is termed enrolment+ or identification+, depending on which was first initiated.



Note:  This feature is disabled by default when a project is created. To have this feature in your project speak with your program manager on how to enable enrolment+.&#x20;

### What is Enrolment+ ?

Continuing with our clinic scenario, assuming a patient came to your clinic and the frontline worker decides to enrol this person with the assumption that they are a new patient. Given that enrolment+ is enabled for your project, Simprints ID would first try to match this person's biometric data with previously registered people and will return a list of potential candidates if any matches exist, else it will just enrol the new biometric data. This process of identifying existing records before enrolment is what we call enrolment+.

This is done to prevent potential duplicates, but if the frontline worker deems the beneficiary to be truly unique, then a process can be triggered to enrol the last captured biometrics without having to recapture the biometric data again. Hence the name enrolment+.

### Enrolment+ Flow

* An enrolment is triggered from calling app ( just like you would in basic [enrolment](enrollment.md))
* Simprints ID captures biometric data then checks to see if matching biometrics exists:
  * If matches exist, it returns a list of potential matching candidates
  * If no matches exist, it proceeds to register the biometric data and return the generated unique id
* Calling app receives the result:
  * If result is a registration, calling app extracts the new unique id, and saves it along with the captured patient's information.
  * If result is a list of matching candidates, the calling app uses the unique ids of the corresponding records in this candidate list to extract matching beneficiaries' information from the calling app's database:
    * Presents these database records to the frontline worker, to choose the actual information of the patient
    * If the frontline worker finds no matching record, then they can choose to enrol last captured biometric data, for which Simprints ID will return a registration object containing the unique id, for the newly registered biometric.

```
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {

   super.onActivityResult(requestCode, resultCode, data);
   // other checks to biometrics completed successfully
   // and the resultCode is okay

 // check if there were possible identified matches
   if (data.hasExtra(Constants.SIMPRINTS_IDENTIFICATIONS)) {

       // present identified matches to user for selection

       // just like you would do with identification

       // ...

   } else if (data.hasExtra(Constants.SIMPRINTS_REGISTRATION)) {

     // code to complete the enrolment as you would do with enrolment 

       //...

   }
}
```

```
// when the user has selected the beneficiary's information from the list,
// the unique id should be used to trigger an identity callout

private void onSelectBeneficiary(String selectedUserUniqueId) {

   // get the session ID, from the resulting intent

   String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");


   // create an intent using the selected beneficiary's uniqueId, and sessionId
   Intent intent = simHelper.confirmIdentity(context, sessionId, selectedUserUniqueId);
   startActivityForResult(intent, confirmIdentityCode);

}
```

```
// in a case where no match was found in the candidate list,
// this function can be called to trigger enrolment of last captured biometrics
private void noMatchFound(Intent data) {

   // in a case where the user cannot find an actual match from the candidate list and
   // chooses to register the captured biometric, you can get the sessionId
   // from the resulting intent and then trigger an enrolment

   String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");

   // create intent, using sessionID, to enrol last captured biometrics
   Intent intent = simHelper.registerLastBiometrics("MODULE ID", sessionId);
   startActivityForResult(intent, enrolmentCode);

}
```

### What is Identification+ ?

In our clinic scenario, assuming a patient comes to the clinic with no means of identification but is presumed to have enrolled before, the frontline worker can then run an identification to get the list of matching candidates, from which the frontline worker will choose the actual patient.

In a case where the patient's information is not amongst the candidate list, the frontline worker can then choose to enrol the captured biometrics that were used for identification, without having to recapture the biometric data again. Hence the name identification +.

### Identification+ Flow

1. An identification is triggered from calling app ( just like you would in basic [identification](identification.md))
2. Simprints ID captures biometric data then checks for potential candidates and returns them
3. Calling app receives the result:
   * If result is a registration, calling app extracts the new unique id, and saves it along with the captured patient's information.
   * If result is a list of matching candidates, the calling app uses the unique ids of the corresponding records in this candidate list, to extract matching beneficiaries' information from the calling app's database:
     * Presents these database records to the frontline worker, to choose the actual information of the patient
     * If the frontline worker finds no matching record, then they can choose to enrol last captured biometric data, for which Simprints ID will return a registration object containing the unique id, for the newly registered biometric.&#x20;

```
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {

   super.onActivityResult(requestCode, resultCode, data);

   // other checks to biometrics completed successfully
   // and the resultCode is okay


 // check if there were possible identified matches

   if (data.hasExtra(Constants.SIMPRINTS_IDENTIFICATIONS)) {

       // present identified matches to user for selection
       // just like you would do with identification
       // ...



   } else if (data.hasExtra(Constants.SIMPRINTS_REGISTRATION)) {

     // code to complete the enrolment as you would do with enrolment 
       //...

   }
}
```

```
// when the user has selected the beneficiary's information from the list,
// the unique id should be used to trigger an identity callout

private void onSelectBeneficiary(String selectedUserUniqueId) {



   // get the session ID, from the resulting intent
   String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");


   // create an intent using the selected beneficiary's uniqueId, and sessionId
   Intent intent = simHelper.confirmIdentity(context, sessionId, selectedUserUniqueId);
   startActivityForResult(intent, confirmIdentityCode);

}
```

```
// in a case where no match was found in the candidate list,
// this function can be called to trigger enrolment of last captured biometrics

private void noMatchFound(Intent data) {

   // in a case where the user cannot find an actual match from the candidate list and
   // chooses to register the captured biometric, you can get the sessionId
   // from the resulting intent and then trigger an enrolment
   String sessionId = data.getString(Constants.SIMPRINTS_SESSION_ID,"");



   // create intent, using sessionID, to enrol last captured biometrics
   Intent intent = simHelper.registerLastBiometrics("MODULE ID", sessionId);
   startActivityForResult(intent, enrolmentCode);

}
```

### Different modules for identification and enrolment (advanced setup) <a href="#h.qmrbfohie14q_l" id="h.qmrbfohie14q_l"></a>

Both Enrolment+ and Identification+ involve sending two Intents from the calling app to Simprints ID - one for the initial action (either enrolment or identification) and one for the follow-up (either confirm identity or register last biometrics).  When the scenario is one of the following:

1. Enrolment -> register last biometrics
2. Identification -> register last biometrics

both the initial and follow up Intents will require a "module ID".  While in most cases the module ID in both will be the same, technically it's possible to send a different one in the follow up Intent. For example you can send an identification intent with "Module A" and then a register last biometrics with "Module B". This will lead to Simprints ID doing an identification search against biometrics registered in "Module A" but then registering last biometrics in "Module B". If such a setup is needed for a project we advise to carefully consider the consequences as it can lead to unlimited duplicates in "Module B".&#x20;
