# FAQs

### **What is the difference between biometric identification and verification?**  <a href="#what-is-the-difference-between-biometric-identification-and-verification" id="what-is-the-difference-between-biometric-identification-and-verification"></a>

_**Identification**_ is used when you want to identify a person using biometrics rather than using personal data such as name, location, and age. First, the user (e.g. a health worker, data collector, or vendor) must enrol a beneficiary by scanning their fingerprints and attaching their fingerprint data to their record. Later, when the beneficiary returns for a follow-up visit, the user scans their fingerprints again. Simprints ID will search the database and return a list of the closest matches, from which the user selects the correct beneficiary. This is also referred to as a 1:N match.&#x20;

_**Verification**_ is used when you want to verify if a person is who they say they are. First, the user (e.g. a health worker, data collector, or vendor) must enrol a beneficiary by scanning their fingerprints and attaching their fingerprint data to their record. Later, when the beneficiary returns for a follow-up visit, the user pulls up their history and scans their fingerprints again. Simprints ID then matches it to the fingerprint data attached to the beneficiary’s record to confirm if it is a successful match. This is also referred to as a 1:1 match.&#x20;

### **Is identification or verification a better fit for my project?** <a href="#is-identification-or-verification-a-better-fit-for-my-project" id="is-identification-or-verification-a-better-fit-for-my-project"></a>

Identification is a better solution for projects where accurate, reliable, or timely retrieval of records is a concern. This may be in situations where there is no reliable alternative identifier. For example, names are commonly misspelt, and ID cards are easily lost or stolen. Biometrics can serve as a more reliable and accurate identification, allowing a frontline worker to pull up records more quickly, easily, and reliably.  \
\


Verification is a better solution for projects concerned with verifying that services are delivered to the correct person and have a reliable way of first pulling up that individual’s record. For example, in cash distribution projects, biometrics can help prevent fraud and often involve using vouchers or other systems to pull up the beneficiary’s record. Alternatively, in a community health worker program, the community members are usually well-known to the community health worker. In such a case, biometrics can prove that the health worker provided services, for example, following up with a mother and newborn in the first 48 hours after delivery.&#x20;

### **What is a GUID?** <a href="#what-is-a-guid" id="what-is-a-guid"></a>

GUID stands for “Globally Unique Identifier.” GUIDs are used so that biometric data can be stored in Simprints’ database separately from other personally identifiable data that is stored in the primary data collection app. During the fingerprint capture process, the Vero scanner captures an image of the fingerprint and saves it to Simprints’ encrypted database on the device and the cloud. The fingerprint data is inaccessible through the device’s camera roll, photo album, or file management system.

Simprints generates a random GUID for each set of fingerprints captured and shares that GUID with the data collection app so that there is a link to the beneficiary’s data. The GUID provides an added layer of security by allowing biometric data to be stored separately from personal data.&#x20;

| Simprints’ database |      | Data collection app database |      |                      |
| ------------------- | ---- | ---------------------------- | ---- | -------------------- |
| Fingerprint data    | GUID | →                            | GUID | Personal information |

### **Does Simprints work without the internet?**  <a href="#does-simprints-work-without-the-internet" id="does-simprints-work-without-the-internet"></a>

Yes, Simprints ID can be installed offline using an APK, and a user can biometrically enrol and identify/verify beneficiaries offline. Internet is only required to log in to Simprints ID, and if it is necessary to sync records to the cloud (e.g. for backup or deduplication) and between devices (e.g. multiple devices are used within one health facility, and the records of all patients need to be on those devices).&#x20;

### **What are the minimum device requirements to run Simprints ID?** <a href="#what-are-the-minimum-device-requirements-to-run-simprints-id" id="what-are-the-minimum-device-requirements-to-run-simprints-id"></a>

Simprints ID can be run on phones and tablets using the Android operating system. Minimum and recommended specs can be found [here](../installation/installation-quick-start-and-device-requirements/device-requirements.md). Simprints ID will not work on rooted (jailbroken) devices.&#x20;
