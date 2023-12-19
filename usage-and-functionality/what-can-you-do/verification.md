# Verification

During verification, an individual’s biometric information is captured and compared against a single enrolled user ([Enrollment)](enrolment.md).

### User flow <a href="#user-flow" id="user-flow"></a>

Verification is started via a [calling app](../../product-overview/product-overview/data-collection-platforms/).

Depending on the modality configured for a project, the screens displayed in SID will either be:

* [Face Capture](../../product-overview/product-overview/biometrics/face-recognition.md)
* [Fingerprint Capture](../../product-overview/product-overview/biometrics/fingerprints-recognition/)

Verification begins when the user completes the biometrics capture, and a decision will be returned to the calling app.

### Inputs <a href="#inputs" id="inputs"></a>

The calling app must send these values:

* `projectId`
* `userId`
* `moduleId`
* `verifyGuid`

### Return Values <a href="#return-values" id="return-values"></a>

These values are returned to the calling app:

* A single match record, including:
  * `guid`
  * `confidenceScore`
  * `tier`

Trigger Verification

To trigger a **verification** process, you need to do the following:



<pre><code>// get the unique ID from the existing beneficiary's record
<strong>String verifyGuid;
</strong>
// create an instance of SimHelper
SimHelper simHelper = SimHelper("PROJECT ID", "USER ID");

// create an Intent using the verify method on the SimHelper object using the unique ID
Intent verificationIntent = simHelper.verify("MODULE ID", verifyGuid);

// trigger the Intent using the 
startActivityForResult
 method
startActivityForResult(verificationIntent, requestCode); 

// requestCode is required for Android intents
</code></pre>

