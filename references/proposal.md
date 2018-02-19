# First Capstone Project Proposal

## The Problem
Missed appointments are inefficient and costly to both patients and clinicians. Patients often must pay a fee, clinics waste time and effort adjusting their schedule, and other patients miss an opportunity to be seen. Without a method for determining which patients are likely to miss their appointments, clinic staff must spend time contacting all patients with reminders. By identifying those patients most likely to miss an appointment, clinics can take a proactive approach and focus their efforts on reminding and/or rescheduling these patients.

## Potential Clients
Physicians, dentists, and other clinicians who schedule outpatient appointments would all benefit from having a method to determine which patients were prone to miss appointments. While patients would certainly benefit from an improved scheduling system, clinics possess the data necessary to predict patient behavior, and would therefore be the intended clients for this project. Clinics could use the methodology developed in this project to predict when a patient is likely to miss an appointment. With this knowledge, they could contact patients in advance of the appointment and identify alternate dates and times. Depending on the results of this analysis, clinics could also predict when appointments are likely to be cancelled due to outside events, such as inclement weather.

## Data
I will use a Kaggle [dataset](https://www.kaggle.com/joniarroba/noshowappointments) as the primary dataset for this project. Depending on the completeness and granularity of the data, I may also include data on weather conditions - likely from [NOAA](https://www.ncdc.noaa.gov/data-access).

## Problem Solving Approach
The Kaggle dataset contains patient-level information, including when the appointment was scheduled, the time of the appointment, whether or not a reminder was sent, some basic demographics, and an outcome variable showing whether or not the patient was a no-show. The goal of this analysis will be to develop a method for predicting whether or not the patient kept the appointment using the data provided (and perhaps additional data if applicable). Some of the variables can be used as-is, but some additional variables that might be predictive can be calculated from the data. For example, the dataset contains fields for both the time of the appointment and the time the appointment was made. Calculating a new variable for the difference between these times could be useful in predicting the outcome of a no-show appointment.

## Deliverables
For this project, I will provide the code used to perform the data cleaning and analysis, as well as a slide deck providing a visual overview of the project, as well as summarizing the findings and any recommendations for clinics and other potential clients.
