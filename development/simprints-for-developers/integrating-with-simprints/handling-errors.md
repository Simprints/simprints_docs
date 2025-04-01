# Handling Errors

### How to Handle Errors

When a biometric flow fails, it can be due to a number of reasons outside an [exit form](exit-forms.md). Those errors appear for the user in the form of coloured screens. So when handling the result from a flow and an error occurs, you can get access to what type of error occurred using the **Constants** class.\
Extracting the error:

```kotlin

val simprintsLauncher = registerForActivityResult(SimprintsContract()) { response ->
    
    if (response.resultCode != Activity.RESULT_OK) {
       // the result code will correspond to one of the pre-defined constants

        when (response.resultCode) {
            Constants.SIMPRINTS_INVALID_PROJECT_ID -> {
             // the error is due to invalid project ID
            }
            // handle other codes
        }
    }
 
    // handle response data
}
```

### Data Authentication Errors

Related to configuration data or authentication errors:

* SIMPRINTS\_VERIFY\_GUID\_NOT\_FOUND\_ONLINE - returned when the **unique id** that was passed, is not found on the server
* SIMPRINTS\_VERIFY\_GUID\_NOT\_FOUND\_OFFLINE - returned when the **unique id** that was passed, is not found on the device

### Configuration Errors

Related to configuration errors:

* SIMPRINTS\_INVALID\_PROJECT\_ID - returned when an invalid project ID is used
* SIMPRINTS\_DIFFERENT\_PROJECT\_ID - returned when a project ID used, is different from the one used to authenticate the app
* SIMPRINTS\_INVALID\_USER\_ID - returned when an invalid user ID is used
* SIMPRINTS\_DIFFERENT\_USER\_ID - returned when a user ID used is different from the one used to log in
* SIMPRINTS\_INVALID\_MODULE\_ID - returned when the module ID doesn't exist
* SIMPRINTS\_INVALID\_METADATA - returned when the metadata has an invalid format or value
* SIMPRINTS\_INVALID\_VERIFY\_GUID - returned when an invalid **unique ID** verify GUID
* SIMPRINTS\_AGE\_GROUP\_NOT\_SUPPORTED - returned when the age group of the subject is not supported by the current project configuration

### Other errors:

* SIMPRINTS\_UNEXPECTED\_ERROR
* SIMPRINTS\_ROOTED\_DEVICE
* SIMPRINTS\_LOGIN\_NOT\_COMPLETE
* SIMPRINTS\_ENROLMENT\_LAST\_BIOMETRICS\_FAILED
* SIMPRINTS\_BLUETOOTH\_NOT\_SUPPORTED
* SIMPRINTS\_BLUETOOTH\_NO\_PERMISSION
* SIMPRINTS\_FINGERPRINT\_CONFIGURATION\_ERROR
* SIMPRINTS\_FACE\_CONFIGURATION\_ERROR
* SIMPRINTS\_LICENSE\_MISSING
* SIMPRINTS\_LICENSE\_INVALID
* SIMPRINTS\_BACKEND\_MAINTENANCE\_ERROR
* SIMPRINTS\_PROJECT\_PAUSED
* SIMPRINTS\_PROJECT\_ENDING
