# Asthma Attack Prediction

## Step 1 — Project Setup
Created the project environment for studying asthma-related patterns in AAMOS-00 and building a classification model.

## Step 2 — Dataset Loading
Loaded four datasets:
- Daily questionnaire: 1,583 × 8
- Weekly questionnaire: 324 × 12
- Peak flow: 1,516 × 6
- Patient information: 22 × 28

## Step 3 — Dataset Structure
Inspected columns and identified `user_key` as the common participant identifier. `weekly_oral` was identified as the potential target.

## Step 4 — Weekly Variables
Inspected weekly symptom variables, their distributions, and missing values.

## Step 5 — Data Dictionary
Verified variable definitions and coding using the AAMOS-00 data dictionary. Symptom-day variables use 0–7 days, while shortness of breath and wheezing use ordinal 1–5 scales.

## Step 6 — Data Quality
Found one invalid `weekly_night_symp` value (`1.2`) for participant 113. It will be treated as missing during preprocessing.

## Step 7 — Additional Weekly Variables
Verified relief-inhaler and healthcare-visit variables. Healthcare-visit values represent days since an event rather than event counts.

## Step 8 — Missing Values
Weekly data contains substantial missingness in some symptom variables. Missing observations will not automatically be treated as zero.

## Step 9 — Target Creation
Created a binary target:
- `0` = no increased systemic corticosteroid use
- `1` = systemic corticosteroid use more than usual

Current target distribution: 280 negative, 43 positive, 1 unknown.

## Step 10 — Patient Data
Checked participant-level data and missing values. Main characteristics are largely complete.

## Step 11 — Patient Characteristics
Inspected sex, age, BMI, smoking history, asthma severity, age at diagnosis, and number of inhalers. These will be merged with weekly observations using `user_key`.

## Step 12 — Peak Flow
Inspected 1,516 peak-flow observations from the 22 participants. `pef_max` ranges from 120 to 639.

Examined peak-flow measurements by participant. The number of readings varies considerably between participants, and average peak-flow values also differ substantially.

Because peak flow is participant-dependent, raw `pef_max` values will not be used directly without considering each participant's expected value.

## Step 13 — Peak Flow Timing
Compared weekly and peak-flow dates. Weekly observations occur approximately weekly, while peak-flow measurements are more frequent and irregular.

Therefore, peak-flow data cannot be directly merged by date. A previous-7-day summary will be considered so that only measurements occurring before the weekly observation are used.

## Step 14 — Basic Modeling Dataset
Combined weekly questionnaire observations with participant-level information using `user_key`. This creates the initial dataset for model preparation.

## Step 15 — Healthcare Visit Variables
Inspected doctor, hospital, and emergency-room variables before modeling because their values represent time since an event and may require preprocessing.

## Step 16 — Feature Selection
Removed healthcare-visit variables because their event-based coding requires additional interpretation. The model will focus on weekly symptoms, treatment use, and participant characteristics.

## Step 17 — Missing Values in Modeling Data
Checked missing values across the selected features before preprocessing and model training.

## Step 18 — Missingness Impact
Checked how many observations have complete values for the main weekly symptom variables before choosing an imputation strategy.

## Step 19 — Unknown Target
Removed the single observation with a missing target because the outcome cannot be safely imputed.

## Step 20 — Median Imputation
Filled missing values in the three weekly symptom variables using their respective median values, avoiding loss of a large portion of the dataset.

## Step 21 — Train-Test Split
Split the dataset into 80% training and 20% testing data using stratified sampling to preserve the target-class distribution.

## Step 22 — Naive Bayes Model
Trained a Gaussian Naive Bayes classifier using the training dataset to predict increased systemic corticosteroid use.

## Step 23 — Initial Model Evaluation
Evaluated the Naive Bayes predictions using accuracy and a confusion matrix to compare predicted and actual outcomes.

## Step 24 — Classification Metrics
Calculated precision, recall, and F1-score to evaluate the model beyond accuracy, especially because the target classes are imbalanced.

## Step 25 — Baseline Comparison
Calculated the majority-class baseline accuracy to determine whether the Naive Bayes model performs better than a simple classifier that always predicts the majority class.

## Step 26 — Naive Bayes Tuning
Tested different `var_smoothing` values to determine whether simple parameter tuning can improve the Naive Bayes model without changing the required algorithm.

## Step 27 — Extended Naive Bayes Tuning
Tested larger `var_smoothing` values to determine whether stronger smoothing could further improve model accuracy.

## Step 28 — Tuned Model Evaluation
Evaluated the best-performing tuned Naive Bayes model (`var_smoothing=0.1`) using accuracy, confusion matrix, precision, recall, and F1-score. The model achieved 81.54% accuracy, with 56% recall for the positive class.

## Step 29 — Feature Comparison
Compared average feature values between the two target groups to identify variables showing stronger differences between observations with and without increased systemic corticosteroid use.

## Step 39 — Feature Selection Test
Tested a smaller feature set based on the strongest differences between target groups. The reduced feature set performed worse than the full feature set, so the full set was retained.

## Step 40 — Participant Observation Structure
Checked the number of weekly observations contributed by each participant before evaluating whether a participant-level train-test split is more appropriate.