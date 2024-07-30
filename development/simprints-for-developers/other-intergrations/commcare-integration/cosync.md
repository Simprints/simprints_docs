# ↔️ CoSync

Simprints offers two independent ways of synchronising data between devices:

1. Simprints' own backend (a.k.a. BFSID)\
   Data is synced up to Simprints' backend (currently built on the Google Cloud Platform) and then down to each device according to the project synchronisation settings.
2. CoSync\
   Data is returned back to the calling app (CommCare in this case) which takes care to synchronise it between devices according to its logic. Simprints ID can then read the data back when fulfilling identification requests.

Both synchronisation options are independent so they can be enabled together (to have double synchronisation) or only one of them can be selected.

### Vulcan Configuration

To enable CoSync you have to open the projects 'Configurations' tab in [Vulcan](https://vulcan.simprints.com/) and go the 'Synchronization' sub-tab. There you will find two dropdowns for 'Simprints sync' and 'Co sync'.

There are two types of data to be synchronised - analytics and biometrics. Analytics includes all the steps (both user- and app-driven) that were performed by Simprints ID. Biometrics is the biometric data of the enrolled subjects. For both Simprints and CoSync you can configure 'None', 'Only analytics', 'Only biometrics' and 'Everything' and any combination between the two is possible.

<figure><img src="../../../../.gitbook/assets/Screenshot 2024-06-24 at 18.51.07 (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/Screenshot 2024-06-24 at 18.51.30.png" alt=""><figcaption></figcaption></figure>

(Note: Analytics for CoSync are currently being revamped so are unavailable as of v2024.1.3 of Simprints ID)

In order to use CoSync for synchronising biometric data you need to select either 'Only biometrics' or 'Everything'.

### CommCareHQ configuration

#### Saving the biometric data

In order to save the biometric data returned by Simprints ID you need to save the 'subjectActions' key from the enrolment callout response:

<figure><img src="../../../../.gitbook/assets/Screenshot 2024-06-24 at 17.36.59 copy.png" alt=""><figcaption></figcaption></figure>

#### Providing biometric data to Simprints ID

In order for Simprints ID to use CommCare as the data source for biometric data for identification callouts they need to have an extra flag 'biometricDataSource' that's set to 'CommCare':

<figure><img src="../../../../.gitbook/assets/Screenshot 2024-06-24 at 18.08.52.png" alt=""><figcaption></figcaption></figure>

(Note: Reading biometric data from CommCare is only available from Simprints ID v2024.1.3 on)
