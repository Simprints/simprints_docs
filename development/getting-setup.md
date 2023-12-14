# Getting setup

**Prerequisites:**

* A Linux, Windows, macOS computer
* Git client installed
* Android Studio installed

**Step 1: Clone the Simprints repository**

Open a terminal window and navigate to the desired location on your computer where you want to clone the repository. Then, run the following command:

```bash
git clone https://github.com/Simprints/**Repo of choice**
```

This will clone the Simprints repository to the specified location.

**Step 2: Create a virtual environment**

We recommend using a virtual environment to isolate the Simprints development environment from your system-wide Python packages. To create a virtual environment, run the following command in the cloned repository directory:

```python
python3 -m venv venv
```

This will create a virtual environment named `venv`. Activate the virtual environment using the following command:

```bash
source venv/bin/activate
```

**Step 3: Install dependencies**

Once the virtual environment is activated, install the required dependencies using the following command:

```bash
pip install -r requirements.txt
```

Use code with caution. [Learn more](https://bard.google.com/faq#coding)content\_copy

**Step 4: Build the Simprints SDK**

To build the Simprints SDK, run the following command:

```gradle
./gradlew build
```

This will build the SDK and produce an APK file in the `build/outputs/apk` directory.

**Step 5: Load the APK onto a phone**

Connect your phone to your computer via USB and enable USB debugging mode on your phone. Then, copy the APK file from the `build/outputs/apk` directory to your phone.

**Step 6: Load the APK into the IDE**

Open the APK file in your Android Studio IDE. This will install the Simprints SDK on your phone and open the app in the IDE.

**Step 7: Make changes to the code**

Make the desired changes to the code in the repository.

**Step 8: Build the updated SDK**

To build the updated SDK, run the following command in the cloned repository directory:

```gradle
./gradlew build
```

This will overwrite the existing APK file in the `build/outputs/apk` directory with the updated version.

**Step 9: Reload the updated SDK onto the phone**

Copy the updated APK file from the `build/outputs/apk` directory to your phone.

**Step 10: Test the updated SDK**

Launch the Simprints app on your phone and test the updated functionality.

**Notes:**

* You can run the `gradlew build` command repeatedly to build the SDK with the latest changes.
* The first time you run the `gradlew build` command, downloading and installing the required dependencies may take some time.
* Check the logs for more information if you encounter any errors during the build process.
