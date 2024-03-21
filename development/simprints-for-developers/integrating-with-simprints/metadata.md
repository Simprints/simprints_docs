# Metadata

### What is Metadata?

Metadata is any added information that you would like **Simprints ID** to capture alongside the biometrics. It is really helpful to capture **metadata** collecting important metrics for your project. The **metadata** is stored as a [JSON](https://www.json.org/json-en.html) string of key-value pairs, and **Simprints ID's Metadata** class only supports values of **Double**, **Long, Boolean,** and **String**.

### Using Metadata

To create the metadata you intend to use for your app, you can create an Instance of the **Metadata** class and pass your **key-value** pairs using the **put** method:

```
// Note: This is the recommended approach, in order to avoid Invalid_JSON errors.
// create an instance of Metadata and attach the key-value pairs using 'put'
Metadata metadata = new Metadata()

                      .put("identificationReason", "postnatal care visit")
                      .put("childPresent", true)
                      .put("childAge", 3);

// pass the Metadata object to an 'identify' or 'verify' call
Intent intent = simHelper.identify("Module ID", metadata);
startActivityForResult(intent, identifyRequestCode); 
```

### Invalid JSON object

The Metadata class will throw an exception for two reason:

* The JSON String is not a valid JSON object
* The JSON object contains parameters that are invalid for Simprints, such as a dictionary or array.

```
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
