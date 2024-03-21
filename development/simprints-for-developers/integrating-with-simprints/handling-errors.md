# Handling Errors

### How to Handle Errors

When a biometric flow fails, it can be due to a number of reasons outside an [exit form](exit-forms.md). Those errors appear for the user in the form of coloured screens. So when handling the result from a flow and an error occurs, you can get access to what type of error occurred using the **Constants** class.\
Extracting the error:

```
import com.simprints.libsimprints.Constants;

override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {

   super.onActivityResult(requestCode, resultCode, data)

   // ensure that the result status is OK
   if (resultCode != Activity.RESULT_OK) {

      //...code to extract the error that occurred
      // using the Constants class

      // for instance, an error due to invalid project id
      boolean isProjectIdError = resultCode == Constants.SIMPRINTS_INVALID_PROJECT_ID


   } else {
      //...code to handle success case
   }
}
```

### Data Authentication Errors

Related to configuration data or authentication errors:

* SIMPRINTS\_VERIFY\_GUID\_NOT\_FOUND\_ONLINE - returned when the **unique id** that was passed, is not found on the server
* SIMPRINTS\_VERIFY\_GUID\_NOT\_FOUND\_OFFLINE - returned when the **unique id** that was passed, is not found on the device

### Configuration Errors

Related to configuration errors:

* SIMPRINTS\_INVALID\_PROJECT\_ID - returned when an invalid project ID is used
* SIMPRINTS\_MISSING\_PROJECT\_ID - returned when no project ID was passed
* SIMPRINTS\_DIFFERENT\_PROJECT\_ID - returned when a project ID used, is different from the one used to authenticate the app
* SIMPRINTS\_INVALID\_USER\_ID - returned when an invalid user ID is used
* SIMPRINTS\_MISSING\_USER\_ID - returned when no user ID is passed
* SIMPRINTS\_DIFFERENT\_USER\_ID - returned when a user ID used is different from the one used to log in
* SIMPRINTS\_INVALID\_MODULE\_ID - returned when the module ID doesn't exist
* SIMPRINTS\_MISSING\_MODULE\_ID - returned when no module ID is passed
* SIMPRINTS\_DIFFERENT\_MODULE\_ID - returned when the module ID is different
* SIMPRINTS\_INVALID\_METADATA - returned when the metadata has an invalid format or value
* SIMPRINTS\_MISSING\_VERIFY\_GUID - returned when no **unique ID** is passed, during **Verification**
* SIMPRINTS\_INVALID\_VERIFY\_GUID - returned when an invalid **unique ID** verify GUID
* SIMPRINTS\_INVALID\_RESULT\_FORMAT - returned when the result format is invalid
