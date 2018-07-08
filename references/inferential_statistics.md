# Inferential Statistics

This document summarizes the results of work in the following notebooks:

- [Data Story](../notebooks/3.0-jkg-data-story.ipynb)
- [Inferential Statistics](../notebooks/3.5-jkg-inferential-stats.ipynb)

## Counts (Appointments per Patient)

| Measure | Value |
| ------- | ----- |
| count   | 62299 |
| mean    | 1.77  |
| std     | 1.77  |
| min     | 1     |
| 25%     | 1     |
| 50%     | 1     |
| 75%     | 2     |
| max     | 88    |

It looks like the majority of our patients have only 1 appointment, but there are several patients with multiple appointments.

## Mean Age by Gender

The mean patient age is 33.3 years for males and 38.6 years for females.

## Pearson Correlation Coefficients

### Hypertension and Diabetes

The Pearson Correlation Coefficient between hypertension and diabetes is r = 0.4274

**Crosstab:**

|                     | No Diabetes | Diabetes | Total |
| ------------------: | :---------: | :------: | :---: |
| **No Hypertension** | 0.79        | 0.01     | 0.80  |
| **Hypertension**    | 0.14        | 0.06     | 0.20  |
| **Total**           | 0.93        | 0.07     | 1.00  |


### SMS Sent and No-show

The Pearson Correlation Coefficient between SMS_sent and No_show is r = 0.1264

**Crosstab:**

|              | Show  | No-show | Total |
| -----------: | :---: | :-----: | :---: |
| **No SMS**   | 0.57  | 0.11    | 0.68  |
| **SMS Sent** | 0.23  | 0.09    | 0.32  |
| **Total**    | 0.80  | 0.20    | 1.00  |

### Gender and No-show

The Pearson Correlation Coefficient between Gender and No_show is r = -0.0041

**Crosstab:**

|            | Show  | No-show | Total |
| ---------: | :---: | :-----: | :---: |
| **Female** | 0.52  | 0.13    | 0.65  |
| **Male**   | 0.28  | 0.07    | 0.35  |
| **Total**  | 0.80  | 0.20    | 1.00  |