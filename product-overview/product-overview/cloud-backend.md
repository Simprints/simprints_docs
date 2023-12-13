# Cloud Backend

**Note: Simprints Cloud is not currently available as open source. Please see our Roadmap for next steps and contact us for more information.**

Simprints maintains a cloud infrastructure that enables key operations to allow our platform to produce the best results. Our backend handles the configuration of our projects, data storage of biometrics and analytics, syncing across devices, and maintains interoperability and top levels of security.&#x20;

**Our backend securely stores the biometric templates and images:** We follow the highest encryption standards throughout our application (from phone to cloud) and ensure that the biometrics and images are stored securely. We conduct regular penetration testing to keep up to date with the latest evolutions in data security.&#x20;

Since our focus is generating templates/images, we minimise other data about the beneficiary (which is stored by the data collection app, not us). To protect the data, we will ensure that all sensitive data, such as biometric data on individuals, is encrypted throughout its lifecycle, whether on our platform or through others.&#x20;

Our partner applications only receive a randomly generated ID that they store with the patient/beneficiary form (a GUID), but we do not transmit the biometric templates/images. The templates/images are stored in our secure cloud database.

**Our Cloud handles syncing across devices to ensure each frontline worker has the right data to identify/verify beneficiaries.** Our product works offline for registering and subsequent matching. But as soon as the frontline workers reach network connectivity, our system immediately syncs data to the cloud and then pushes any new records to other frontline workers conducting work in the same region.

**Generates analytics for suggestions on improving program performance and fraud prevention:** There is a lot of activity occurring in Simprints ID. We can learn a lot about how often consent is positive, how long it takes to capture fingerprints, the quality of the fingerprint scans, and why people may leave the app. This information empowers our project managers to work with partners on optimisation and program learning, including fraud prevention.
