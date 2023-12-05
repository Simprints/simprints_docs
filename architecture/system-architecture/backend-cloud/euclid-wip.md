# EUCLID (WIP)

Euclid is our Deduplication Service, built with the purpose of reducing duplicates in projects to ensure the uniqueness of biometric records. Below we will summarise [what deduplication is](euclid-wip.md#what-is-biometric-deduplication), [why it's beneficial](euclid-wip.md#why-is-deduplication-important) and Euclid's various components that make it all happen.&#x20;

## What is biometric deduplication?

Broadly speaking, biometric deduplication is a three-step process:

1. **Analysis**: A dedicated platform (like Euclid!) runs a duplication analysis to identify pairs of biometric records with a “match score” above a certain threshold; these pairs are flagged as potential duplicates.&#x20;
2. **Adjudication**: A human being combines the above-mentioned list of potential duplicates with program data (e.g. names, dates of birth) to determine if the suspected duplicates are true duplicates.
3. **Resolution**: All relevant parties each delete the duplicate records from their respective databases.&#x20;

<details>

<summary>More details on match scores, thresholds and the limitations of deduplication</summary>

**What is a match score?**

A match score, also referred to as a “confidence score”, is a mathematical similarity score between two sets of fingerprints. It is _not_ a percentage and ranges from 0 to 200+. Match scores are used to identify or verify a beneficiary.

#### **How is the match score threshold set?**  <a href="#how-is-the-match-score-threshold-set" id="how-is-the-match-score-threshold-set"></a>

The starting threshold is based on the data we have on confirmed matches from other projects. Based on our experiences, we typically set the threshold between 20 and 25. The lower the match score, the greater the risk of including unique individuals within the list of suspected duplicates, which creates more work during the adjudication process. The higher the match score, the greater the risk of excluding real duplicates. To improve the precision of the deduplication threshold, we usually need identification data. The precise match score used will be determined after the analysis is completed and we are able to see the distribution of match scores. After we run additional duplication analyses with more data, we will adjust the threshold to be more precise for the project’s population based on the distribution of match scores. &#x20;

\
**How are the limitations of biometric deduplication?**&#x20;

Biometric deduplication is not an exact science as it is a statistical analysis. We cannot have 100% certainty that any suspected duplicates are real duplicates, but we can be fairly confident in match scores 50 and above.&#x20;

There are limits to the accuracy of the technology. The larger the pool size, the greater the likelihood of similar, unique fingerprints. Therefore, we recommend running duplication analyses on pool sizes no larger than 15,000 individuals.

</details>



## Why is deduplication important?

Reducing duplicate records helps to provide the right treatment to the right person during the continuity of care. Let’s say you’ve enrolled a community into a program, and then you run our biometric deduplication analysis, you can catch where there are multiple records of the same person. Once you’ve removed the duplicates, it will make sure you don’t duplicate service delivery or cash payments to the same person twice

Not having a deduplication process could mean that NO duplication analyses is done OR you have to rely purely on biographic data (name, address, etc..) to deduplicate. The problem with biographic data is if one individual intentionally provides two different sets of biographic data, it won’t show up in the biographic deduplication - you would need something reliable like biometric duplication analysis to provide this additional layer of rigor.

<details>

<summary>Euclid's Use Cases</summary>

1. **Misuse**

* By frontline workers (poor performance or fraud)
* Abuse by beneficiaries

2. **Crossover**

* Migration Patterns - people crossing over districts and therefore have multiple enrolments
* Census - purely an enrolment period, and will want to determine what the true denominator is
* Merging Databases (potentially across project IDs) - \*need this to be filled out by Joyce

3. **Aggregate Reporting**

* They don’t necessarily need to delete the duplicates, but they need to report high level numbers of how much duplication occurred in the project

4. **Data Systems Intelligence**

* By telling them which records were duplicates, they can use this information to inform their own algorithms to determine fraud

</details>



## How does Euclid work?

EUCLID is made up of five components that depend on each other. The following is a summary:&#x20;

1. ReferenceStore (maintains a cache of available references)
2. Resolver (responsible for maintaining a list of attributes for all references)
3. Task Manager (a task repository to store one-off/ongoing requests and tasks)
4. Matcher (used for comparing references)
5. Worker (sort of comprises the Matcher - uses the Task manager and ReferenceStore to get and load appropriate references)



\[diagram here once everything is done]



The worker component depends on the reference store, matcher and task manager components. It uses the task manager to get work to do, use the reference store to load the appropriate reference for this work, and the matcher to compare these references.



\




##
