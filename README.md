**Springboard Capstone Project 1**

> First capstone project for Springboard Data Science course using patient appointment data to predict no-shows

Based on: https://www.kaggle.com/joniarroba/noshowappointments

**Overview**

This repo contains the work for my first capstone project as part of the Springboard [Data Science Career Track](https://www.springboard.com/workshops/data-science-career-track/) curriculum. 

**Contents**

- [Project Proposal](#project-proposal)
    - [Problem Statement](#problem-statement)
    - [Potential Clients](#potential-clients)
- [Problem Solving Approach](#problem-solving-approach)
    - [Dataset](#dataset)
    - [Inital Investigation](#inital-investigation)
- [Methods](#methods)
    - [Data Wrangling](#data-wrangling)
- [Results](#results)
    - [Inferential Statistics](#inferential-statistics)
    - [Models](#models)
- [Limitations](#limitations)
- [Conclusions](#conclusions)
- [Reference](#reference)

# Project Proposal

## Problem Statement

Missed appointments are inefficient and costly to both patients and clinicians. Patients often must pay a fee, clinics waste time and effort adjusting their schedule, and other patients miss an opportunity to be seen. Without a method for determining which patients are likely to miss their appointments, clinic staff must spend time contacting all patients with reminders. By identifying those patients most likely to miss an appointment, clinics can take a proactive approach and focus their efforts on reminding and/or rescheduling these patients.

## Potential Clients

Physicians, dentists, and other clinicians who schedule outpatient appointments would all benefit from having a method to determine which patients were prone to miss appointments. While patients would certainly benefit from an improved scheduling system, clinics possess the data necessary to predict patient behavior, and would therefore be the intended clients for this project. Clinics could use the methodology developed in this project to predict when a patient is likely to miss an appointment. With this knowledge, they could contact patients in advance of the appointment and identify alternate dates and times. Depending on the results of this analysis, clinics could also predict when appointments are likely to be cancelled due to outside events, such as inclement weather.

# Problem Solving Approach
## Dataset

I will use a Kaggle [dataset](https://www.kaggle.com/joniarroba/noshowappointments) as the primary dataset for this project. Depending on the completeness and granularity of the data, I may also include data on weather conditions - likely from [NOAA](https://www.ncdc.noaa.gov/data-access).

The Kaggle dataset contains patient-level information, including when the appointment was scheduled, the time of the appointment, whether or not a reminder was sent, some basic demographics, and an outcome variable showing whether or not the patient was a no-show. The goal of this analysis will be to develop a method for predicting whether or not the patient kept the appointment using the data provided (and perhaps additional data if applicable). Some of the variables can be used as-is, but some additional variables that might be predictive can be calculated from the data. For example, the dataset contains fields for both the time of the appointment and the time the appointment was made. Calculating a new variable for the difference between these times could be useful in predicting the outcome of a no-show appointment.

## Inital Investigation

This notebook contains the results of my first exploration of the data for this project. Experiments first performed in this notebook were transferred and refined in the following notebooks in this repo.

My initial findings and plan for exploration are as follows:

**Raw Data** *(csv)*

- Fix column names
- Description for each column
- Convert data types as needed
- Recode missing values
- Split/combine columns as needed (ex: Date and Time could be in separate fields)

**Clean Data** *(pandas df)*

- Null count per column
- "use" for each column (what its purpose is in the analysis)
- look at unique values (ex: Neighborhoods)

**Preliminary Analysis**

- Datetime field investigation
    - overall histogram
    - histogram of appointments per day of week
- Counts for any fields used for categorization/grouping

**Notes on Raw Data**

- All fields appear to have no null values.
- PatientId is type float64, but AppointmentID is type int64
- Date(time?) fields: (need to convert)
    - ScheduledDay
    - AppointmentDay
- Rename:
    - 'PatientId' --> 'Patient_ID'
    - 'AppointmentID' --> 'Appointment_ID'
    - 'ScheduledDay' --> 'Scheduled_Date'
    - 'AppointmentDay' --> 'Appointment_Date'
    - 'Neighbourhood' --> 'Neighborhood'
    - 'Scholarship' --> 'Welfare' (see note in reference https://en.wikipedia.org/wiki/Bolsa_Fam%C3%ADlia)
    - 'Hipertension' --> 'Hypertension'
    - 'Handcap' --> 'Disability'
    - 'SMS_received' --> 'SMS_sent'
    - 'No-show' --> 'No_show'
- Appointment_ID is unique ID
- Age ranges from -1 to 115 (plot this)
- Welfare, Hypertension, Diabetes, Alcoholism, and SMS_sent seem ok
- Disability has a max of 4 (thought this was supposed to be binary...)

**Next steps:**

- Plot Age distribution
- Histograms for Date columns
- Unique values/counts for:
    - Gender
    - Neighborhood
    - Disability
    - No_show

**Age Distribution**

Looks like there are plenty of newborns. That seems reasonable, but the -1 is obviously impossible and the 115 is highly unlikely.

**Neighborhoods**

Export list of neighborhoods to search for a reference list online
https://pt.wikipedia.org/wiki/Lista_de_bairros_de_Vitória_(Espírito_Santo)

# Methods

## Data Wrangling

This notebook begins with importing the raw dataset for my capstone project and moves through some basic cleaning and reorganization to produce a pandas DataFrame ready for further analysis. Future exploration will undoubtedly reveal additional data wrangling steps that need to be taken, but for now the dataset produced by this notebook is clean enough that it can be investigated in much more detail than in its raw form. The overall steps taken in this notebook are as follows:

1. Load data into pandas DataFrame
2. Rename columns
3. Convert data types (dates)
4. Set AppointmentID as index
5. Copy to new, clean DataFrame
6. Create Patients and Appointments DataFrames

This notebook utilizes the cleaned dataframes generated in the previous notebook and explores some interesting data points visually.

**Counts (Appointments per Patient)**

The records in this dataset represent specific appointments, not just individual patients. There may be redundant data which is really patient level data that is contained in the appointment level dataset. To start investigating this, it will be helpful to get a general sense of how many appointments the average patient has in our dataset.

It looks like the majority of our patients have only 1 appointment, but there are several patients with multiple appointments.

**Bar Plot (Appointments per Day of Week)**

Having an idea of how appointments are spread out throughout the week will provide some context for the data and how appointments are scheduled in these clinics. If, for example, there were no appointments scheduled on a specific day, that might indicate that we either have some missing data or the clinic is closed on that day, which might have implications for appointments on the surrounding weekdays.

![png](references/output_8_0.png)

Most appointments are clustered in the beginning of the week, with the least coming on Thursday followed by Friday. This may just be representative of the scheduling practices of the clinics, or it could indicate that our dataset ends on a Wednesday.

**Histogram (Patients by Age)**

Getting an overall sense of the age distribution of the patients in our dataset provides context for the patient population at these clinics.

![png](references/output_11_0.png)

By increasing the number of bins in this histogram, we are seeing age groups of roughly one year. The regular spikes in patient counts indicates that ages in our dataset may have been rounded, and are not entirely accurate.

**Comparison (Percent of Patients by Gender vs Percent of Appointments by Gender)**

It would be helpful to know if males and females schedule appointments at a rate representative of the patient population, or if one gender schedules appointments at a higher rate than the other. For example, a higher number of appointments by females could indicate that many of the appointments are for pregnancy-related visits.

![png](references/output_14_0.png)

The ratio of males to females is nearly identical for both patients and appointments, meaning there is no significant difference between the genders in how many appointments they have.

**Time Series Plot (Appointments by Date)**

Lastly, it would be helpful to get a general overview of the timeline of our data. A simple line plot showing the number of appointments over time will help give some context.

![png](references/output_17_0.png)

Interestingly, there is a dramatic drop off around June 15th. If this was a holiday, or some other major event, that could have implications for appointments on surrounding dates.

# Results
## Inferential Statistics

This notebook explores some basic statistics regarding the data, in particular some pearson correlation coefficients between various sets of binary variables.

**Means**

The mean patient age is 33.3 years for males and 38.6 years for females.

**Pearson Correlation Coefficient**

*Hypertension and Diabetes*

The Pearson Correlation Coefficient between hypertension and diabetes is r = 0.4274

|                     | No Diabetes | Diabetes | Total |
| ------------------: | :---------: | :------: | :---: |
| **No Hypertension** | 0.79        | 0.01     | 0.80  |
| **Hypertension**    | 0.14        | 0.06     | 0.20  |
| **Total**           | 0.93        | 0.07     | 1.00  |

*SMS Sent and No-show*

The Pearson Correlation Coefficient between SMS_sent and No_show is r = 0.1264

|              | Show  | No-show | Total |
| -----------: | :---: | :-----: | :---: |
| **No SMS**   | 0.57  | 0.11    | 0.68  |
| **SMS Sent** | 0.23  | 0.09    | 0.32  |
| **Total**    | 0.80  | 0.20    | 1.00  |

*Gender and No-show*

The Pearson Correlation Coefficient between Gender and No_show is r = -0.0041

|            | Show  | No-show | Total |
| ---------: | :---: | :-----: | :---: |
| **Female** | 0.52  | 0.13    | 0.65  |
| **Male**   | 0.28  | 0.07    | 0.35  |
| **Total**  | 0.80  | 0.20    | 1.00  |

## Models

**Setup**

```python
# package imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix, classification_report
```

```python
# import pickle file
df_appointments = pd.read_pickle('../data/interim/appointments_df.pickle')
df_clean = pd.read_pickle('../data/interim/clean_df.pickle')
```

```python
# set index for dataframe
df_clean.set_index('Appointment_ID', inplace=True)
```

```python
# add columns from df_clean back into df_appointments
df_model = df_appointments.join(df_clean, rsuffix='_clean')
```

```python
# remove duplicate columns from model dataframe
drop_columns = ['Patient_ID_clean',
                'Gender_clean',
                'Scheduled_Date_clean',
                'Appointment_Date_clean',
                'SMS_sent_clean',
                'No_show_clean',
                'Neighborhood']
df_model.drop(columns=drop_columns, inplace=True)
```

```python
# create new column: days_diff to show number of days between scheduled date and appointment date
df_model['days_diff'] = df_model.date_diff.dt.days
```

```python
# create dummy vars for days of week
df_model = df_model.join(pd.get_dummies(df_model.Appointment_Date.dt.dayofweek))
```

```python
# clean up column names
df_model.columns = ['Patient_ID',
                    'Gender',
                    'Scheduled_Date',
                    'Appointment_Date',
                    'SMS_sent',
                    'No_show',
                    'date_diff',
                    'Age',
                    'Welfare',
                    'Hypertension',
                    'Diabetes',
                    'Alcoholism',
                    'Disability',
                    'days_diff',
                    'Mon',
                    'Tue',
                    'Wed',
                    'Thu',
                    'Fri',
                    'Sat']
```

```python
# extraneous columns not needed for modeling
unneeded_columns = ['Patient_ID',
                    'Scheduled_Date',
                    'Appointment_Date',
                    'No_show',
                    'date_diff',
                    'Gender']
```

```python
# Need to produce arrays for the features and the response variable
y = df_model['No_show'].values
X = df_model.drop(columns=unneeded_columns).values
```

```python
# rename 'Yes'/'No' to 'No-show'/'Show' for clarity
rename = y.copy()
for key, value in {'Yes': 'No-show', 'No': 'Show'}.items(): rename[y == key] = value
y = rename
```

**Logistic Regression**

```python
# Create training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.4, random_state=42)

# Create the classifier: logreg
logreg = LogisticRegression()

# Fit the classifier to the training data
logreg.fit(X_train, y_train)

# Predict the labels of the test set: y_pred
y_pred = logreg.predict(X_test)

# Compute and print the confusion matrix and classification report
logreg_cm = confusion_matrix(y_test, y_pred)
true_neg, false_pos, false_neg, true_pos = logreg_cm.ravel()
print('\nConfusion Matrix')
print(logreg_cm)
print('True Negative (True Show):\t', true_neg)
print('False Positive (False No-show):\t', false_pos)
print('False Negative (False Show):\t', false_neg)
print('True Positive (True No-show):\t', true_pos)

logreg_cr = classification_report(y_test, y_pred)
print('\nClassification Report')
print(logreg_cr)
```
    
    Confusion Matrix
    [[  154  8688]
     [  319 35050]]
    True Negative (True Show):	 154
    False Positive (False No-show):	 8688
    False Negative (False Show):	 319
    True Positive (True No-show):	 35050
    
    Classification Report
                 precision    recall  f1-score   support
    
        No-show       0.33      0.02      0.03      8842
           Show       0.80      0.99      0.89     35369
    
    avg / total       0.71      0.80      0.72     44211

**GridSearchCV**

```python
# setup hyperparameter grid
c_space = np.logspace(-5, 8, 15)
param_grid = {'C': c_space}
```

```python
# instantiate GridSearchCV object
logreg_cv = GridSearchCV(logreg, param_grid, cv=5)
```

```python
# fit data
logreg_cv.fit(X_train, y_train)
```

    GridSearchCV(cv=5, error_score='raise',
           estimator=LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False),
           fit_params=None, iid=True, n_jobs=1,
           param_grid={'C': array([1.00000e-05, 8.48343e-05, 7.19686e-04, 6.10540e-03, 5.17947e-02,
           4.39397e-01, 3.72759e+00, 3.16228e+01, 2.68270e+02, 2.27585e+03,
           1.93070e+04, 1.63789e+05, 1.38950e+06, 1.17877e+07, 1.00000e+08])},
           pre_dispatch='2*n_jobs', refit=True, return_train_score='warn',
           scoring=None, verbose=0)

```python
# print tuned parameter and score
print('Tuned Logistic Regression Parameters: {}'.format(logreg_cv.best_params_))
print('Best score is {}'.format(logreg_cv.best_score_))
```

    Tuned Logistic Regression Parameters: {'C': 0.0007196856730011522}
    Best score is 0.7945141443995416

```python
y_pred_cv = logreg_cv.predict(X_test)
```

```python
# Compute and print the confusion matrix and classification report
logreg_cv_cm = confusion_matrix(y_test, y_pred_cv)
true_neg_cv, false_pos_cv, false_neg_cv, true_pos_cv = logreg_cv_cm.ravel()
print('\nConfusion Matrix (GridSearchCV)')
print(logreg_cv_cm)
print('True Negative (True Show):\t', true_neg_cv)
print('False Positive (False No-show):\t', false_pos_cv)
print('False Negative (False Show):\t', false_neg_cv)
print('True Positive (True No-show):\t', true_pos_cv)

logreg_cv_cr = classification_report(y_test, y_pred_cv)
print('\nClassification Report (GridSearchCV)')
print(logreg_cv_cr)
```
    
    Confusion Matrix (GridSearchCV)
    [[  162  8680]
     [  253 35116]]
    True Negative (True Show):	 162
    False Positive (False No-show):	 8680
    False Negative (False Show):	 253
    True Positive (True No-show):	 35116
    
    Classification Report (GridSearchCV)
                 precision    recall  f1-score   support
    
        No-show       0.39      0.02      0.04      8842
           Show       0.80      0.99      0.89     35369
    
    avg / total       0.72      0.80      0.72     44211

# Limitations



# Conclusions



# Reference

- Project [proposal](references/proposal.md)
- Original dataset [documentation](references/noshowappointments.md)
- [Statistics write-up](references/inferential_statistics.md)
- [Milestone report](references/milestone_report.md)
- [Data Acquisition and Initial Exploration](notebooks/1.0-jkg-initial-data-exploration.ipynb)
- [Data Wrangling](notebooks/2.0-jkg-data-wrangling.ipynb)
- [Data Story](notebooks/3.0-jkg-data-story.ipynb)
- [Inferential Statistics](notebooks/3.5-jkg-inferential-stats.ipynb)
- [Modeling](notebooks/4.0-jkg-models.ipynb)
