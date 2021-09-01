# Description

Neuroimaging dataset of 22q11 deletion syndrome patients.

# Counts

| Attribute                                        | 22q11 CLIP | 22q11 non-CLIP | Undetermined | Totals |
|--------------------------------------------------|------------|----------------|--------------|--------|
| Number of patients (radiology reports)           | 167        | 137            |  18          | 322    | 
| Number of sessions (radiology reports)           | 228        | 190            | 262          | 680    |
|--------------------------------------------------|------------|----------------|--------------|--------|
| Number of patients (subject metadata and images) |            |                |              | 253    |
| Number of sessions (subject metadata and images) |            |                |              | 403    |
| Number of images                                 | ???        | ???            |   -          | ???    |
| Number of scanners                               | ???        | ???            |   -          | ???    |
| Number of MPRAGEs                                | ???        | ???            |   -          | ???    | 
| Number of DTIs                                   | ???        | ???            |   -          | ???    |

Notes:
- 262 sessions could not be labeled as CLIP or non-CLIP due to the following reasons
  - Missing narrative text (193),
  - Missing scans/external scan session (63), 
  - Not actually being films (4), or 
  - Not having informative narrative text (2).
- 29 subjects had scans that fell into both CLIP and non-CLIP categories. 

Last updated: 2021-08-31
