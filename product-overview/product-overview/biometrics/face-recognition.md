---
cover: ../../../.gitbook/assets/kenya-048.jpg
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Face Recognition

**The facial recognition modality** is enabled within our Simprints ID app, using the camera hardware within the Android device used. The facial recognition algorithm has been extensively tested to ensure it can perform to a high standard in the extreme conditions of the last mile, with a variety of populations.

**The device's camera captures a good quality image of a face:** A field worker captures a beneficiary’s face image using the device's built-in camera, following the on-screen prompts to ensure the beneficiary is situated at the correct distance from the lens and in good lighting.&#x20;

The face recognition algorithm checks the quality of the face image, and if it is of low quality, it prompts the field worker to retake the photo. The face recognition algorithm generates a unique face template (string of numbers) which is stored on the phone until it is synced to the Cloud.

Simprints ID is currently configured to use the [Rank One V1 face SDK](http://https/roc.ai/sdk/). The code is not available as open source, and there is a small license cost. Please contact us for more information.



**Recommended requirements for Face Product.** It equates roughly to a Samsung A40 or A20 for recommended devices and A10s for minimum viability.

* CPU:
  * Minimum: Octa-core processor (1.6GHz)
* RAM:
  * Minimum: 3GB of RAM, 4GB if available
* Memory:
  * Minimum: 32GB of internal space
* Camera
  * Minimum: 16MP camera with autofocus
