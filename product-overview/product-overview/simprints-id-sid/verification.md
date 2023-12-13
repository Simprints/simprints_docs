# Verification

Verification operations primarily differ from identification operations in using some other means of initially identifying participants, such as scanning a card or keying in a username. In practice, this mode is more common.

Where biometrics are attended, this verification stage will likely happen in collaboration with the attendant, who may type in a name, read an ID number from a card, tap a smartcard on a reader, or even recognise the user from prior interactions.&#x20;

In a Verification Operation, the biometric system is likely to be comparing the user to a much smaller pool of possible matches - perhaps five users with identical names, or even one record if the user has presented a card or ID number - where an Identification may require comparison against hundreds or thousands of records.

This makes Verification faster and potentially higher accuracy as the biometric ‘matching’ process is run fewer times (potentially by orders of magnitude if the database held by the system contains thousands of records and must be searched through each time a 1:N match is made) and is not the only source of ‘truth’ for a judgement regarding whether the subject is the correct subject.&#x20;

Where an operation returns multiple possible matches in a scenario with an attendant present - or the biometric system returns a confidence score (for instance, it indicates a probability onscreen that the user is who they say they are as a percentage or using language such as ‘good / bad / poor’), the process of determining whether to proceed is referred to as adjudication.

