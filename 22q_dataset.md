# Description

Neuroimaging dataset of 22q11 deletion syndrome patients.

# Counts

## Patients and sessions, various stages of data set cleaning

Table 1. The radiology reports were used to identify images with limited pathology (CLIP), images without limited pathology (non-CLIP), and images whose usability was undetermined. The list of subject ids generated in this step were used in combination with a metadata file and a directory of images to determine which scans could be used in the initial analysis. 

| Attribute                                        | 22q11 CLIP | 22q11 non-CLIP | Undetermined | Totals |
|--------------------------------------------------|------------|----------------|--------------|--------|
| Number of patients (radiology reports)           | 167        | 137            |  18          | 322    | 
| Number of sessions (radiology reports)           | 228        | 190            | 262          | 680    |
|                                                  |            |                |              |        |
| Number of patients (subject metadata and images) | 107        |  77            |  69          | 253    |
| Number of sessions (subject metadata and images) | 161        | 108            | 134          | 403    |

In total, there are 107 subjects across 161 sessions that are CLIP and have both scan session metadata and the images.

Notes:
- 262 sessions could not be labeled as CLIP or non-CLIP due to the following reasons
  - Missing narrative text (193),
  - Missing scans/external scan session (63), 
  - Not actually being films (4), or 
  - Not having informative narrative text (2).
- 29 subjects had scans that fell into both CLIP and non-CLIP categories and 40 subjects were missing either images or metadata, resulting in the 69 subjects in the "Number of patients (subject metadata and images)" row and "Undetermined" column. 

## Age

The following table is based on the data represented by the final two rows of the previous table.

Table 2. The number of patients older and younger than two years old can be seen for the CLIP, non-CLIP, and undetermined categories.

| Attribute                                        | 22q11 CLIP | 22q11 non-CLIP | Undetermined | Totals |
|--------------------------------------------------|------------|----------------|--------------|--------|
| Number of patients <= 2 years old                |  40        |  36            |  22          |  98    |
| Number of patients > 2 years old                 |  70        |  46            |  52          | 168    |
| Total number of patients                         | 110        |  82            |  74          | 266    |
|                                                  |            |                |              |        |
| Number of sessions for patients <= 2 years old   |  70        |  48            |  44          | 162    |
| Number of sessions for patients > 2 years old    |  91        |  60            |  90          | 241    |
| Total number of sessions                         | 161        | 108            | 134          | 403    |

The total number of subjects (266) in Table 2 does not match the number of subjets in Table 1, but the number of sessions is consistent across these two tables. This discrepancy demonstrates the presence of multiple longitudinal scans for some patients. 

We filtered the metadata to look at only the first scan per subject.

Table 3. The number of patients older and younger than two years at the time of their first scan for CLIP, non-CLIP, and undetermined categories. In this table, the number of scans in each category is the same as the number of patients.

| Attribute                                        | 22q11 CLIP | 22q11 non-CLIP | Undetermined | Totals |
|--------------------------------------------------|------------|----------------|--------------|--------|
| Number of patients <= 2 years old                |  40        |  36            |  22          |  98    |
| Number of patients > 2 years old                 |  67        |  41            |  47          | 155    |
| Total number of patients                         | 107        |  77            |  69          | 253    |

Comparing Tables 2 and 3, we find that there are 3 22q11 CLIP subjects and 5 non-CLIP subjects who have scans before and after the age of 2 years.


## Final data set counts

The following table is based on the data represented by the final two rows of the initial table on this page.

Table 4. Counts of specific image things for the CLIP 22q11 subjects.

| Attribute                                        | 22q11 CLIP | 22q11 non-CLIP | Undetermined | Totals |
|--------------------------------------------------|------------|----------------|--------------|--------|
| Number of images                                 | ???        | ???            |   -          | ???    |
| Number of scanners                               | ???        | ???            |   -          | ???    |
| Number of MPRAGEs                                | ???        | ???            |   -          | ???    | 
| Number of DTIs                                   | ???        | ???            |   -          | ???    |



Last updated: 2021-08-31
