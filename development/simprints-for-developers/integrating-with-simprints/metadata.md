# Metadata

### What is Metadata?

Metadata is any added information that you would like **Simprints ID** to capture alongside the biometrics. It is really helpful to capture **metadata** collecting important metrics for your project. The **metadata** is stored as a [JSON](https://www.json.org/json-en.html) string of key-value pairs, and **Simprints ID's Metadata** class only supports values of **Double**, **Long, Boolean,** and **String**.

### Using Metadata

To create the metadata you intend to use for your app, you can create an Instance of the **Metadata** class and pass your **key-value** pairs using the **put** method:

```kotlin

// Note: This is the recommended approach, in order to avoid Invalid_JSON errors.
// create an instance of Metadata and attach the key-value pairs using 'put'
val metadata = Metadata()
    .put("identificationReason", "postnatal care visit")
    .put("childPresent", true)
    .put("childAge", 3)

// pass the Metadata object to the request data
simprintsLauncher.launch(SimprintsRequest.Enrol(
  projectId = "Project ID", 
  userId = "User ID",
  moduleId = "Module ID",
  metadata = metadata,
))

```

### Invalid JSON object

The Metadata class will throw an exception for two reasons:

* The JSON String is not a valid JSON object
* The JSON object contains parameters that are invalid for Simprints, such as a dictionary or array.

```json
// Invalid JSON string: arrays are not allowed
{
    "identificationReason": "postnatal care visit",
    "childPresent": true,
    "required": [        // Dictionaries and arrays are not allowed 
        "firstName", 
        "lastName"
    ]
}
```

### Subject Age metadata parameter

For projects that have age restrictions (e.g. only work with subjects between 9 and 60 months old) **Simprints ID** will expect the subject's age to be passed along in the **Metadata** for Enrollment and Verification callouts. The name of the parameter is **subjectAge**, it should be of type **Long** and contain the subject's age **in months**:

```kotlin
val metadata = Metadata().put("subjectAge", 30) // subject is 30 months old
```
