# Project Documentation: Asthma Exacerbation Risk Prediction

## 1. Project Overview
This project explores whether clinical indicators, spirometry measurements, and symptom patterns can help predict asthma exacerbations. 

Using a longitudinal clinical dataset, I trained and evaluated a **Categorical Naive Bayes** model, compared its performance to a majority-class baseline, and built an interactive **Power BI dashboard** to communicate the results.

---

## 2. Dataset & Clinical Variables

### Dataset Origin
The dataset comes from the study by **Spyroglou et al. (2018)** (*BMC Research Notes*, 11:522), which evaluated Bayesian classifiers for predicting asthma exacerbations in children and adolescents monitored after medication discontinuation.

### Dataset Structure
* **Total observations:** 195
* **Total unique patients:** 65 (2 to 4 visits per patient, approximately 6 months apart)
* **Total columns:** 25
* **Missing values:** 0
* **Target variable:** `Asthma exacerbation`
  * `No Exacerbation`: 166 observations (85.1%)
  * `Exacerbation`: 29 observations (14.9%)

### Feature Selection
From the 25 original columns, **22 predictor variables** were selected for modeling:
* **Patient demographics & history:** Gender, age category (`agecat`), BMI category (`bmicat`), breastfeeding history, prenatal smoking exposure (`Smokingpre`), allergic rhinitis, eczema, history of bronchiolitis (`Bronch`), asthma severity category (`Asthmacat`), food allergies (`Foodalle`), drug allergies (`Drugsalle`), animal exposure (`Animalshom`), and count of positive allergy tests (`Ath`).
* **Lung function (spirometry):** FVC category (`FVCcat`), FEV1 category (`FEV1cat`), and bronchodilator reversibility category (`REVcat`).
* **Asthma control & symptoms:** Asthma Control Test category (`ACTcat`), ATAQ score category (`ataq`), daily symptoms (`ACTds`), daily activity symptoms (`ACTda`), nocturnal symptoms (`NocSym`), and dyspnea.

### Excluded Variables
* `id`: Excluded as a feature because it is a patient identifier, not a clinical risk factor.
* `t`: Excluded because it denotes the sequential visit number. At visit `t = 4`, there were only 12 observations and zero exacerbation events. Dropping `t` ensures the model classifies risk based on the patient's current health status rather than their visit count.

---

## 3. Preprocessing & Validation Strategy

### Patient-Level Train/Test Split
In longitudinal healthcare data, random row splitting causes **data leakage** because visits from the same patient would appear in both training and test sets. 

To prevent this:
* The 65 patients were split into **52 training patients** (80%) and **13 test patients** (20%) using a fixed random seed (`random_state=42`).
* This produced:
  * **Training set:** 157 observations from 52 patients
  * **Testing set:** 38 observations from 13 patients
* A set intersection check verified zero patient overlap between train and test sets.

### Categorical Encoding
* **Training-only fitting:** `OrdinalEncoder` for features and `LabelEncoder` for the target were fitted strictly on the training set and applied to the test set.
* **Handling unseen test categories:** If a category appeared in the test set that was not present during training, it was assigned `-1` by `OrdinalEncoder`. These values were remapped to a dedicated unknown index, and `min_categories` was passed to `CategoricalNB` so the model could evaluate test observations without crashing.

---

## 4. Model Training & Evaluation

### Why Categorical Naive Bayes?
`CategoricalNB` is designed for datasets where features are discrete categories. It estimates the probability of each category given the class using Bayes' theorem with Laplace smoothing.

### Results on Test Set (38 observations)
* **Overall Accuracy:** 76.3% (29 out of 38 correct)
* **Exacerbation Precision:** 33.3% (2 correctly detected out of 6 predicted)
* **Exacerbation Recall:** 28.6% (2 correctly detected out of 7 actual)
* **Exacerbation F1-Score:** 30.8%
* **No Exacerbation Recall:** 87.1% (27 correctly identified out of 31)

### Confusion Matrix
```
                   Predicted: Exacerbation    Predicted: No Exacerbation
Actual: Exacerbation               2                             5
Actual: No Exacerbation            4                            27
```

### Majority-Class Baseline Comparison
Because non-exacerbation accounts for ~85% of observations, a dummy model that always predicts `No Exacerbation` was evaluated:
* **Baseline Accuracy:** 81.6%
* **Baseline Exacerbation Recall:** 0.0%

**Key Takeaway:** The majority baseline has higher overall accuracy simply because it ignores every exacerbation. Accuracy is therefore a misleading metric on imbalanced medical data. The Naive Bayes model provided some ability to detect exacerbations (28.6% recall), though with a high rate of false alarms.

### Balanced Class Prior Experiment
Testing equal class priors (`class_prior=[0.5, 0.5]`) in `CategoricalNB` yielded identical test predictions and metrics, showing that adjusting priors alone did not alter the decision boundary for this test set.

---

## 5. Power BI Dashboard (`dashboard.pbix`)

Test set predictions and estimated exacerbation probabilities were exported to `data/asthma_prediction_results.csv` to build a Power BI reporting dashboard.

### Core DAX Measures
* **Actual Exacerbations:**
  ```dax
  Actual Exacerbations = 
  CALCULATE(
      COUNTROWS(asthma_prediction_results),
      asthma_prediction_results[Actual] = "Exacerbation"
  )
  ```
* **Exacerbation Recall:**
  ```dax
  Exacerbation Recall = 
  DIVIDE(
      CALCULATE(
          COUNTROWS(asthma_prediction_results),
          asthma_prediction_results[Actual] = "Exacerbation",
          asthma_prediction_results[Predicted] = "Exacerbation"
      ),
      [Actual Exacerbations],
      0
  )
  ```
* **Model Accuracy:**
  ```dax
  Model Accuracy = 
  DIVIDE(
      CALCULATE(
          COUNTROWS(asthma_prediction_results),
          asthma_prediction_results[Actual] = asthma_prediction_results[Predicted]
      ),
      COUNTROWS(asthma_prediction_results),
      0
  )
  ```

### Dashboard Components
1. **Summary KPI Cards:** Test Observations (38), Actual Exacerbations (7), Exacerbation Recall (28.6%), Model Accuracy (76.3%).
2. **Actual vs. Predicted Outcomes:** Clustered column chart visualizing true positives, false positives, false negatives, and true negatives.
3. **Predicted Risk Bands:** Grouping estimated exacerbation probabilities into descriptive exploratory ranges:
   * Low: 0%–20%
   * Moderate: 20%–50%
   * High: 50%–80%
   * Very High: 80%–100%
4. **Predicted Risk Scatter Chart:** Plotting predicted risk against patient IDs, color-coded by actual outcome.
5. **Prediction Details Table:** Row-by-row view of individual test visits (`id`, `t`, actual outcome, predicted outcome, and estimated probability).
6. **Limitations Note:** A transparent notice on the report reminding viewers that the model is educational and not clinically validated.

---

## 6. What I Learned & Limitations

1. **Accuracy is not enough for imbalanced data:** When one class is rare, a model that does nothing can look "accurate." Focusing on recall and precision is essential.
2. **Small sample sensitivity:** With only 7 exacerbation cases in the test set, each individual prediction changed recall by ~14.3%. A larger test cohort is needed for reliable statistical confidence.
3. **Repeated patient visits:** Healthcare data requires patient-grouped splitting to avoid optimistic leakage.
4. **Assumption of feature independence:** Naive Bayes assumes features are independent given the class label. In reality, symptoms like dyspnea and nocturnal coughing are clinically correlated.
5. **Disclaimer:** This project was developed as a student learning exercise and is not clinically validated for diagnostic or treatment decisions.