# Asthma Exacerbation Risk Prediction

## Step 1 — Project Setup
Set up the project for studying asthma exacerbation risk using a clinical dataset and developing a Naive Bayes classification model.

## Step 2 — Dataset Loading
Loaded the asthma exacerbation dataset from the XLSX file. The dataset contains 195 observations and 25 variables, including demographic, clinical, spirometry, asthma-control, symptom, and asthma-exacerbation outcome variables.

## Step 3 — Dataset Structure
Inspected the dataset columns and identified `id` as the patient identifier, `t` as the observation/time indicator, and `Asthma exacerbation` as the target variable. The remaining 22 variables contain demographic, clinical, asthma-history, spirometry, asthma-control, and symptom information.

## Step 4 — Target Distribution
Examined the distribution of the target variable. The dataset contains 166 observations with no asthma exacerbation and 29 observations with an exacerbation, giving an exacerbation rate of approximately 14.9%.

## Step 5 — Data Types and Missing Values
Inspected the dataset structure using data types and non-null counts. All 195 observations are complete across all 25 variables, with no missing values. Most variables are categorical, while `id`, `t`, and `Ath` are stored as integers.

## Step 6 — Variable Categories
Inspected the unique values and distributions of the dataset variables. The dataset contains 65 unique patients with repeated observations across up to four time points. Most predictors are categorical and already discretized, while `Ath` is a numerical count variable. The target contains 166 no-exacerbation observations and 29 exacerbation observations.

## Step 7 — Observation Structure
Reviewed the longitudinal structure of the dataset using the study methodology. The dataset contains repeated measurements from 65 patients, with 2–4 observations per patient. The time variable `t` is ordinal, with measurements separated by approximately six-month medical surveillance intervals. The first assessment (`t = 1`) occurs after medication discontinuation.

## Step 8 — Gender and Exacerbation
Compared gender with the asthma exacerbation outcome. Exacerbation occurred in 9 of 69 female observations (13.0%) and 20 of 126 male observations (15.9%). The difference is relatively small, so gender alone does not appear to show a strong separation between the two outcome groups.

## Step 9 — Age and Exacerbation
Compared age categories with the asthma exacerbation outcome. The observed exacerbation rates were 16.0% for toddlers, 24.0% for pre-schoolers, 12.4% for gradeschoolers, and 25.0% for highschoolers. The highschooler group contains only eight observations, so these differences should be interpreted cautiously.

## Step 10 — BMI and Exacerbation
Compared BMI categories with the asthma exacerbation outcome. The observed exacerbation rates were 16.7% for underweight, 18.5% for normal weight, 7.7% for overweight, and 14.5% for obese observations. No clear monotonic relationship was observed, suggesting that BMI alone may not strongly separate the two outcome groups.

## Step 11 — Asthma Control and Exacerbation
Compared asthma control categories with the asthma exacerbation outcome. The observed exacerbation rates were 5.3% for good asthma control, 31.0% for moderate asthma control, and 80.0% for poor asthma control. This shows a strong observed relationship between poorer asthma control and exacerbation, although the poor-control category contains only five observations and should therefore be interpreted cautiously.

## Step 12 — FEV1 and Exacerbation
Compared FEV1 categories with the asthma exacerbation outcome. The observed exacerbation rate was 14.7% among observations with normal FEV1 and 16.0% among observations with hypoactive FEV1. The difference is small, suggesting that FEV1 category alone does not strongly separate the two outcome groups in this dataset.

## Step 13 — FVC and Exacerbation
Compared FVC categories with the asthma exacerbation outcome. The observed exacerbation rate was 15.9% for observations with hypoactive FVC and 14.6% for observations with normal FVC. The difference is small, suggesting that FVC category alone does not strongly distinguish exacerbation from non-exacerbation observations.

## Step 14 — FEV1 Reversibility and Exacerbation
Compared FEV1 reversibility categories with the asthma exacerbation outcome. The observed exacerbation rate was 14.5% for observations with non-significant reversibility and 22.2% for observations with significant reversibility. The significant category contains only nine observations, so the difference should be interpreted cautiously.

## Step 15 — Daily Symptoms and Exacerbation
Compared daily symptoms with the asthma exacerbation outcome. The observed exacerbation rate was 43.2% among observations with daily symptoms and 8.2% among observations without daily symptoms. This represents a substantial difference and suggests that daily symptoms may provide useful predictive information.

## Step 16 — Daily Activity Symptoms and Exacerbation
Compared daily activity symptoms with the asthma exacerbation outcome. The observed exacerbation rate was 51.5% among observations with daily activity symptoms and 7.4% among observations without them. This substantial difference suggests that daily activity symptoms may provide useful predictive information.

## Step 17 — Nocturnal Symptoms and Exacerbation
Compared nocturnal symptoms with the asthma exacerbation outcome. The observed exacerbation rate was 66.7% among observations with nocturnal symptoms and 11.5% among observations without them. This is a substantial difference, although the nocturnal-symptom category contains only 12 observations and should therefore be interpreted cautiously.

## Step 18 — Dyspnea and Exacerbation
Compared dyspnea with the asthma exacerbation outcome. The observed exacerbation rate was 75.0% among observations with dyspnea and 13.6% among observations without dyspnea. Although this is a large observed difference, only four observations reported dyspnea, so the relationship should be interpreted cautiously.

## Step 19 — Asthma History and Exacerbation
Examined the relationship between `Ath` and asthma exacerbation. The variable ranges from 0 to 11 and is concentrated at lower values. Exacerbation rates vary across `Ath` values, but several higher values contain very few observations, so no strong relationship was inferred from the observed distribution. The variable was retained for further modeling rather than manually categorized at this stage.

## Step 20 — Variable Cardinality
Examined the number of unique values in each variable. Most predictors are binary categorical variables, while `ACTcat`, `ataq`, `bmicat`, and `agecat` contain three or four categories. `Ath` is a numerical count variable with 11 observed values, while `id` uniquely identifies patients. The target variable is binary. The categorical structure of the dataset is suitable for further evaluation of a categorical Naive Bayes approach.

## Step 21 — Duplicate Records
Checked the dataset for completely duplicated rows. No duplicate records were found, so no duplicate observations were removed.

## Step 22 — Feature and Target Identification
Separated the dataset into the target variable, patient identifier, observation-time variable, and potential predictors. `Asthma exacerbation` was identified as the target, while `id` was excluded as a patient identifier rather than a clinical predictor. The observation variable `t` was retained for further consideration because it represents the ordinal measurement point in the longitudinal dataset. This produced 23 candidate predictor variables and 195 target observations.