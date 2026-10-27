# Archived Experiment (AAMOS-00 Dataset)

This folder contains an earlier phase of the project that was archived.

## What was explored here
Before settling on the main clinical dataset, I initially explored the **AAMOS-00** dataset to predict whether a patient would need more systemic corticosteroids than usual (`weekly_oral`).

This folder preserves:
- `aa_prediction.ipynb`: The notebook used to explore the AAMOS-00 data and test a Gaussian Naive Bayes model.
- `data/`: Raw AAMOS-00 data files, including weekly questionnaires, daily logs, peak flow recordings, and smartwatch sensor exports.
- `documentation.md` and `biology.md`: Early notes documenting the AAMOS-00 data cleaning and symptom definitions.

## Why this approach was archived
The AAMOS-00 dataset had several practical challenges for this project:
- Peak flow and questionnaire dates did not align neatly, requiring complex temporal merging.
- High rates of missing values across weekly symptom responses.
- Small cohort size (22 participants).

To build a clearer and more reliable classification workflow, the project pivoted to the peer-reviewed clinical dataset from **Spyroglou et al. (2018)** located in the main `data/` directory.

These files are kept here for transparency and to document the learning process.
