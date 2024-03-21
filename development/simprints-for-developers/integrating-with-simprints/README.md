# Integrating with Simprints

This section contains links to information about how to integrate with **Simprints ID**, using a custom Android application. Key features and terminology that **Simprints ID** uses will also be introduced in this section.



What is Simprints ID?

**Simprints ID** is a standalone Android app that is responsible for enrolling and verifying a person's identity, using unique features like the person's fingerprint and face.

The custom app is responsible for capturing any biographical information along with additional data being collected, and **Simprints ID** is responsible for capturing the person's biometric identity. After a new biometric record is created in **Simprints ID,** a unique ID is generated and returned to the custom app. This unique ID is to be stored along with the person's biographical data and any other data collected, within the custom app and servers.

This unique ID, that is stored on the custom app, is what is used to match a newly captured biometric template with a previously enrolled one. When there is the need to verify that a person is who they say they are, a call is made to **Simprints ID** using this unique ID, from which **Simprints ID** will take a new biometric capture with the person present, and validate that it matches the previously enrolled biometric capture with that unique ID.

## General Flow <a href="#h.dkubt29cikys_l" id="h.dkubt29cikys_l"></a>

<figure><img src="../../../.gitbook/assets/SID general flow diagram.jpg" alt=""><figcaption></figcaption></figure>

1. Calling app creates an Intent using **projectID, moduleID** and **userID**
2. Triggers the Intent to start **Simprints ID** flow
3. **Simprints ID** validates parameters and authenticates the device
4. Consent screen is shown, to let the beneficiary agree to terms of biometric data capture
5. Beneficiary's biometrics get captured and triggered flow proceeds
6. **Simprints ID** returns the result from triggered flow to calling app
7. The calling app receives a result and handles success or failure scenarios

That is **Simprints ID** in a nutshell! Enroll, verify or identify a person during a visit. Here are some links to get you up to speed with the flows:

* [Getting Started](getting-started.md)
* [Enrolment](enrollment.md)
* [Identification](../other-intergrations/commcare-integration/identification.md)
* [Verification](verification.md)
* [Enrolment+ & Identification+](enrollment.md)
* [Exit Forms](exit-forms.md)
* [Tiers & Confidence Scores](tiers-and-confidence-scores.md)
* [Handling Errors](handling-errors.md)
* [Metadata](metadata.md)
