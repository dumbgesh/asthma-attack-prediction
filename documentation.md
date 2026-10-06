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

## Step 23 — Target Labels
Inspected the unique values of the target variable. The target is binary, with two classes: `No Exacerbation` and `Exacerbation`. The original categorical labels were retained at this stage for interpretability and will be encoded during model preprocessing.

## Step 24 — Observation Time and Feature Selection
Examined asthma exacerbation rates across the observation points represented by `t`. The observed rates were 12.3% at `t = 1`, 15.4% at `t = 2`, 20.8% at `t = 3`, and 0% at `t = 4`. Because `t` represents the longitudinal observation point rather than a direct patient characteristic, and the final observation contains only 12 observations with no exacerbation cases, it was excluded from the final predictive feature set. The variable was retained in the original dataset for longitudinal analysis and documentation.

The resulting feature matrix contains 22 candidate predictors and 195 observations.

## Step 25 — Predictor Data Types
Inspected the data types of the selected predictors. The feature set contains 21 categorical variables and one numerical count variable, `Ath`. The categorical variables will require categorical encoding during preprocessing, while `Ath` can be handled as a numerical feature.

## Step 26 — Ath Distribution
Inspected the frequency distribution of the numerical `Ath` variable. The variable is concentrated at zero, with 103 of 195 observations having a value of 0, while higher values occur less frequently. No missing or unusual values were identified. The original count representation was retained rather than applying arbitrary categories.

## Step 27 — Preprocessing Strategy
Reviewed the source paper's variable definitions and encoding scheme. The dataset already provides most clinical predictors as categorical/discretized variables, while `Ath` is represented as a numerical count. The supplied category definitions were retained rather than recreating categories from the underlying measurements. The planned preprocessing therefore keeps categorical predictors as categorical variables, retains `Ath` as a numerical count, and encodes the binary target during model preparation.

## Step 28 — Categorical Feature Encoding
Converted the 22 selected predictor variables into numerical category codes using `OrdinalEncoder`. This produced an encoded feature matrix with 195 observations and 22 predictors. Unknown categories were configured to receive a dedicated encoded value during later preprocessing. The encoding is used as preparation for the categorical Naive Bayes model.

## Step 29 — Encoded Feature Verification
Inspected the encoded feature matrix after categorical encoding. All 22 predictors were successfully converted into numerical category codes, while the discrete values of `Ath` were preserved. The resulting matrix contains 195 observations and 22 encoded predictors and is ready for model preparation.

## Step 30 — Target Encoding
Encoded the binary target variable using `LabelEncoder`. The classes were mapped to numerical labels as `Exacerbation = 0` and `No Exacerbation = 1`. The original class labels were retained through the fitted label encoder for later interpretation of model predictions.

## Step 31 — Patient-Level Train/Test Split
Created a patient-level train/test split to prevent observations from the same patient appearing in both sets. Of the 65 unique patients, 52 patients were assigned to training and 13 patients to testing using an 80/20 split with a fixed random state for reproducibility.

## Step 32 — Patient Separation Verification
Verified that there is no overlap between the patient identifiers assigned to the training and testing sets. The intersection contained zero patients, ensuring that observations from the same patient cannot appear in both datasets.

## Step 33 — Create Training and Testing Sets
Created the training and testing feature matrices and target vectors using the patient-level split. The training set contains 157 observations from 52 patients, while the testing set contains 38 observations from 13 patients. Both sets contain the same 22 candidate predictors.

## Step 34 — Training-Only Feature Encoding
Fitted the categorical encoder using only the training data and then applied the fitted encoder to the testing data. This prevents information from the testing set from influencing the preprocessing stage. The resulting encoded matrices contain 157 training observations and 38 testing observations, each with 22 predictors.

## Step 35 — Training and Testing Target Encoding
Encoded the target variable using a single `LabelEncoder` fitted on the training labels and applied to the testing labels. Both datasets contain the two target classes, `Exacerbation` and `No Exacerbation`, with consistent numerical encoding.

## Step 36 — Handling Unseen Test Categories
Checked the encoded testing data for categories that were not present in the training data. Some unseen categories were detected and represented by `-1`; these were replaced with a new valid category index for each feature so that the testing data could be processed by `CategoricalNB` without removing observations.

## Step 37 — Naive Bayes Model Training
Trained a Categorical Naive Bayes classifier using the encoded training data. The model was configured with the observed category counts for each predictor, including the additional category reserved for unseen test values.

## Step 38 — Model Predictions
Generated predictions for the 38 observations in the patient-level test set using the trained Categorical Naive Bayes model. The model predicted both target classes, `Exacerbation` and `No Exacerbation`, allowing both classes to be evaluated separately.

## Step 39 — Model Evaluation
Evaluated the Categorical Naive Bayes model on the 38-observation patient-level test set. The model achieved 76.3% accuracy. It correctly identified 2 of 7 exacerbation cases and 27 of 31 non-exacerbation cases. The recall for the `Exacerbation` class was 29%, while recall for `No Exacerbation` was 87%, indicating that the model performs substantially better at identifying non-exacerbation observations than exacerbation cases.

## Step 40 — Majority-Class Baseline
Created a majority-class baseline that predicts `No Exacerbation` for every test observation. The baseline achieved 81.6% accuracy, higher than the Naive Bayes model's 76.3% accuracy, but detected none of the seven exacerbation cases. This shows that accuracy alone is misleading for this imbalanced prediction problem, while the Naive Bayes model provides some ability to identify exacerbation cases.

## Step 41 — Model vs Baseline Comparison
Compared the Categorical Naive Bayes model with a majority-class baseline. Naive Bayes achieved 76.3% accuracy compared with 81.6% for the baseline, but achieved 28.6% recall for exacerbation compared with 0% for the baseline. This demonstrates that the baseline's higher accuracy comes from always predicting the majority class, while Naive Bayes provides some ability to identify exacerbation cases.

## Step 43 — Balanced Prior Experiment
Tested a second Categorical Naive Bayes model using equal class priors of 0.5 for both exacerbation and non-exacerbation. The resulting predictions and evaluation metrics were identical to the original model, with 76.3% accuracy and 28.6% exacerbation recall. Therefore, changing the class prior alone did not improve minority-class detection.

## Step 44 — Predicted Risk Probabilities
Generated class probabilities for the test observations using the trained Naive Bayes model. The probabilities represent the model's estimated likelihood for each target class, with the first probability column corresponding to `Exacerbation` and the second to `No Exacerbation`. The model produced both low- and high-risk predictions, providing a probability-based view of predicted exacerbation risk in addition to binary classifications.

## Step 45 — Exacerbation Risk Range
Examined the predicted probability of asthma exacerbation for the test observations. The estimated probabilities ranged from approximately 0.02% to 99.997%. These values represent the model's estimated probabilities rather than clinically validated risk percentages.

## Step 46 — Prediction Results Table
Created a prediction results table for the test observations containing patient ID, observation time, actual outcome, predicted outcome, and estimated exacerbation probability. This table provides a consolidated model-output dataset that can be used for further analysis and visualization.

## Step 47 — Export Model Results
Exported the test-set prediction results to `data/asthma_prediction_results.csv`. The file contains 38 test observations with patient ID, observation time, actual outcome, predicted outcome, and estimated exacerbation probability, providing the dataset for the BI dashboard stage.

## Step 48 — Final Model Summary
Summarized the final performance of the Categorical Naive Bayes model on 38 unseen test observations. The model achieved 76.3% accuracy, compared with 81.6% for the majority-class baseline. For the `Exacerbation` class, precision was 33.3%, recall was 28.6%, and F1-score was 30.8%. The model therefore showed limited ability to detect exacerbation cases and did not outperform the baseline in overall accuracy.

## From Step 49 we'll start with Power BI
Done with installment, we'll move forward in a while