# Confidence Score Bands

### Confidence score thresholds

Confidence bands and Decision policy are terms used to classify the degree to which an **existing** **biometric template** matches a currently captured one. Thus, this terms are used for **Identification** and **Verification** workflows.

The **Decision Policy** **thresholds** are configured in the project configuration in **Simprints ID** servers, and can be adjusted to affect the outcome of the resulting matched beneficiaries. Thus **Simprints** **ID** uses the specified **thresholds** to determine if a potential matching biometric record is worthy of being added to the **candidate list.**

The **ConfidenceBand** is an **enum** with 4 values indicating the rankings and can be accessed using the **getConfidenceBand** method of the **Identification** or **Verification** objects.

```kotlin

enum class ConfidenceBand {
    HIGH,   // Almost certainly a match (the biometrics are very similar)
    MEDIUM, // Possible match
    LOW,    // Not a match (the biometrics are very different)
    NONE,   // Confidence is below "Low" (likely a capture error)
}
```

### Confidence Scores

The **confidence score** is a numerical value that indicates the likelihood of a match. The returned number is dependent on the biometric modality selected. The **confidence score** adapts over time, as the match results become more accurate.

```kotlin

// accessing the value
val score = verification.confidence
```
