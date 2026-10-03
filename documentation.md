# Asthma Attack Prediction

## Step 1 — Project Setup

Created a project environment to study asthma-related data from the AAMOS-00 dataset and build a classification model.

## Step 2 — Dataset Loading

Loaded four AAMOS-00 datasets:

- Daily questionnaire — 1,583 rows, 8 columns
- Weekly questionnaire — 324 rows, 12 columns
- Peak flow — 1,516 rows, 6 columns
- Patient information — 22 rows, 28 columns

## Step 3 — Dataset Structure

Inspected the columns of each dataset to understand the available variables and how the datasets can be connected through `user_key`.

The weekly questionnaire contains the treatment-related variable `weekly_oral`, which will be investigated as the prediction target.

## Step 4 — Weekly Symptom Variables

Inspected five weekly symptom-related variables.

`weekly_night_symp`, `weekly_day_symp`, and `weekly_limit_activity` use values from 1–7 and contain missing observations. `weekly_short_breath` and `weekly_wheeze` use values from 1–5 and have complete observations.

The variables use different scales and have different levels of missingness, so their coding and missing-value handling need to be examined before preprocessing.