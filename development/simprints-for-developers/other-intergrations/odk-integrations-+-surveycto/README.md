# ODK Integrations + SurveyCTO

Simprints ID can be integrated into any ODK based platform. Make sure you have checked out our [Getting Started](../../integrating-with-simprints/getting-started.md) page before continuing.

### What is ODK? <a href="#h.p_lyw0gue-zzer_l" id="h.p_lyw0gue-zzer_l"></a>

An ODK xml form can collect information about a beneficiary over time. Within an ODK XML form, there is functionality to integrate the Simprints ID app. This will allow frontline workers the ability to scan and capture a beneficiary's biometric records.

In some contexts, asking to scan people’s fingerprints can be unfamiliar and intimidating, leading to some beneficiaries refusing to be scanned. When building an ODK form, we recommend that the question that asks for fingerprint scanning should come much later in the workflow, after other basic demographic information has been collected, in order to build up some comfort level for the user about the process, and warm them up to providing consent for their fingerprints to be scanned.

In this guide, we will show you how to integrate your ODK enrolment/registration, or verification form with Simprints ID. The sample forms we use in this guide are the bare minimum that a form should have, and in real-case scenarios, your forms will likely have more content than what is shown in this guide.

### Prerequisites <a href="#h.p_p3i8pmcxu9zu_l" id="h.p_p3i8pmcxu9zu_l"></a>

### ODK <a href="#h.p_kd5wdjkcv63e_l" id="h.p_kd5wdjkcv63e_l"></a>

* Access to ODK XML forms
* Some basic form-building knowledge with ODK

\*If using SurveyCTO, access to their web platform ([link](https://www.surveycto.com/)) and SurveyCTO collect mobile app ([link](https://play.google.com/store/apps/details?id=com.surveycto.collect.android\&hl=en\_GB\&pli=1))

### Simprints <a href="#h.p_vkfldqqqyekt_l" id="h.p_vkfldqqqyekt_l"></a>

* Simprints Project ID
* Simprints QR code

\*Contact your Simprints Project Manager if you do not have this

### Simprints Parameters <a href="#h.p_on5ii9pkjmmj_l" id="h.p_on5ii9pkjmmj_l"></a>

There are 3 parameters that are important when integrating your ODK form with Simprints ID (click [here](https://sites.google.com/simprints.com/simprints-for-developers/integrating-w-simprints-old) for more information). Information on these parameters will either need to be collected or calculated before the callout to Simprints ID.

* **project\_id** – this is the unique alphanumeric id provided by the Simprints project manager to differentiate data from different projects - it needs to be a string in quotation marks. Please contact your Simprints project manager if you do not have this
* **user\_id** – this variable should be populated when the user logs in to the data collection app. It can be linked to the user filling out the form. There should be an earlier question in the form asking for the username (e.g., data collector’s name, school name, health facility name)
* **module\_id** – this variable should be populated with the category information to break down a database. For example, this can be a district, region, or health centre

### Ready? <a href="#h.p_wvt7h83oatmp_l" id="h.p_wvt7h83oatmp_l"></a>

Simprints offers three distinct callouts that you can use within your ODK XML form:

* [Enrol](enrol.md)
* [Identify](identify.md)
* [Verify](verify.md)
