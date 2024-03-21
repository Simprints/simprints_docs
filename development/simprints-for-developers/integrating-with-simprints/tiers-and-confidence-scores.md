# Tiers & Confidence Scores

### Tiers & Thresholds

Tiers and Thresholds are terms used to classify the degree to which an **existing** **biometric template** matches a currently captured one. Thus, this terms are used for **Identification** and **Verification** workflows.

The **Threshold is** a project configuration done in **Simprints ID** servers, and can be adjusted to affect the outcome of the resulting matched beneficiaries. Thus **Simprints** **ID** uses the specified **Thresholds** to determine if a potential matching biometric record is worthy of being added to the **candidate list.** For more information on Tiers and Thresholds, have a look at the **IDb4Enrol** section here.

The **Tier** is an **enum** with 5 values indicating the rankings, and can be accessed using the **getTier** method of the **Identification** or **Verification** objects.

```
public enum Tier {

    TIER_1,    // Almost certain match   (the biometrics are very similar)
    TIER_2,    // Very good match
    TIER_3,    // Good match
    TIER_4,    // Okay match
    TIER_5,    // No match               (the biometrics are very different)
}

// accessing the value

Tier tier = identification.getTier()
```

### Confidence Scores

The **confidence score** is a numerical value that indicates the likelihood of a match. The returned number is dependent on the biometric modality selected. The **confidence score** adapts over time, as the match results become more accurate.

```
// accessing the value
float score = verification.confidence
```
