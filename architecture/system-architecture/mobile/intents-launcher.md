---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Intents Launcher

{% hint style="info" %}
**Coming Q1 2024**
{% endhint %}

The Simprints Intents Launcher is an app that simulates the [calling app](../../../product-overview/product-overview/data-collection-platforms/) for testing.

### What does it do? (to POs) <a href="#what-does-it-do-to-pos" id="what-does-it-do-to-pos"></a>

The main purpose is to ask [SID ](../../../product-overview/product-overview/simprints-id-sid.md)to do something and take a look at the responses we get. The process creates a call to SID (the action) and passes some parameters (the **key values**).

It is easy to create intents, add extras, and look at the returned code. You get the returned bundle and parse it to a JSON, so it's easy to see the return.

## Intents <a href="#intents" id="intents"></a>

* Enrol/Register
* Identify
* Confirm Identification
* Verify
* Register last biometrics
* Results Log
