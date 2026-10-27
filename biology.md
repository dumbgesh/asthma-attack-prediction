# Asthma Exacerbation — Biology & Clinical Notes

This document provides simple background context on the clinical and biological variables used in this project.

---

## 1. Asthma
Asthma is a chronic respiratory condition where airways become inflamed and narrowed, making breathing difficult. Symptoms can vary over time in frequency and intensity.

## 2. Asthma Exacerbation
An exacerbation (or asthma attack) is an acute worsening of symptoms—such as breathlessness, wheezing, or chest tightness—accompanied by decreased lung function.

In this project, our binary classification target is:
* `No Exacerbation`: Normal surveillance visit without an acute worsening event.
* `Exacerbation`: Episode of clinical asthma worsening.

## 3. Asthma Control Tests
Asthma control measures how well a patient's symptoms are stabilized by their treatment:
* **ACT (Asthma Control Test):** Grouped in the dataset into *Good*, *Moderate*, and *Poor* control.
* **ATAQ (Asthma Therapy Assessment Questionnaire):** Scored from 0 to 3 based on asthma control barriers.

Poorer control scores strongly correlate with increased exacerbation risk.

## 4. Lung Function (Spirometry)
Spirometry tests evaluate airflow and lung capacity:
* **FVC (Forced Vital Capacity):** The maximum volume of air forcefully exhaled after taking a full breath.
* **FEV1 (Forced Expiratory Volume in 1 second):** The volume exhaled during the first second of a forced breath.
* **REV (FEV1 Reversibility):** Evaluates whether lung function significantly improves after administering a bronchodilator inhaler (*Significant* vs. *Non-Significant*).

## 5. Symptoms
The dataset tracks key self-reported symptom indicators:
* **Daily symptoms (`ACTds`):** Experiencing asthma symptoms on a daily basis.
* **Daily activity limitations (`ACTda`):** Asthma interfering with everyday physical tasks.
* **Nocturnal symptoms (`NocSym`):** Waking up at night due to coughing or breathing difficulty.
* **Dyspnea:** Experiencing noticeable shortness of breath.

## 6. Allergies & Clinical History
Allergic sensitization often triggers or aggravates asthma:
* Allergic rhinitis
* Eczema (atopic dermatitis)
* Food and drug allergies
* Animal exposure in the home
* History of infant bronchiolitis
* Prenatal smoking exposure

## 7. Other Patient Characteristics
* **Demographics:** Gender, age category (`Toddler`, `Pre-school`, `Gradeschooler`, `Highschooler`), and BMI category.
* **Asthma Severity:** `Intermittent` vs. `Persistent`.
* **Breastfeeding history:** Early-life nutrition status.
* **`Ath`:** The count of positive skin-prick test reactions, reflecting the degree of allergic sensitization.

## 8. Key EDA Observation: Multifactorial Risk
During data exploration, symptoms showed prominent differences between groups:
* **Daily symptoms:** 43.2% exacerbation rate vs. 8.2% without daily symptoms.
* **Daily activity limitations:** 51.5% vs. 7.4%.
* **Nocturnal symptoms:** 66.7% vs. 11.5%.
* **Dyspnea:** 75.0% vs. 13.6%.

These figures represent empirical associations in this specific cohort rather than direct independent causes. In clinical practice, asthma exacerbation is multifactorial—shaped by a combined interplay of symptoms, airway physiology, allergic history, and environment.
