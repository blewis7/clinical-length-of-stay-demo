# Clinical Length of Stay Prediction

## Project Overview

This project demonstrates an end-to-end machine learning pipeline for predicting **hospital length of stay (LOS)** at the time of patient admission. Accurate LOS prediction is a common and high-impact problem in hospital operations, supporting:

- Bed capacity planning
- Staffing decisions
- Discharge coordination

The dataset used in this project is **synthetic but clinically realistic**, designed to mirror real-world hospital admission data.

The project emphasizes:

- Data quality and clinical realism
- Thoughtful preprocessing
- Model comparison and interpretability
- Sound analytical judgment over model complexity

---

## Project Structure

clinical-length-of-stay-demo/
│
├── data/
│ ├── hospital_length_of_stay_raw.csv
│ └── hospital_length_of_stay_clean.csv
│
├── notebooks/
│ ├── 01_exploration_and_cleaning.ipynb
│ ├── 02_linear_regression_baseline.ipynb
│ └── 03_ridge_and_tree_models.ipynb
│
├── requirements.txt
├── .gitignore
└── README.md

---

## Phase 1: Data Exploration and Cleaning

### Raw Data Ingestion

- Loaded raw CSV data containing patient demographics, vitals, diagnoses, comorbidities, lab abnormalities, and LOS.
- Preserved the raw dataset unchanged as a source of truth.

### Initial Exploration

- Inspected data types, missing values, and summary statistics.
- Identified missingness in vital signs and lab abnormality counts.
- Observed physiologically implausible outliers in heart rate and blood pressure.

### Outlier Handling (Clinical Validation)

- Applied clinically reasonable clipping to physiologic variables:
  - Heart rate clipped to plausible human ranges.
  - Systolic blood pressure clipped to plausible admission ranges.
- Ensured data realism without removing valid patient records.

### Missing Data Handling

- Created explicit missingness indicator flags for:
  - Heart rate
  - Systolic blood pressure
  - Lab abnormality count
- Imputed missing numeric values using the median after clipping.
- Preserved information about missingness for downstream modeling.

### Data Realism Adjustments

- Converted appropriate clinical variables to integers:
  - Age
  - Vital signs
  - Count-based features
- Maintained human-readable clinical units (e.g., days, beats per minute).

### Column Standardization

- Renamed columns to standardized, descriptive names for analysis.
- Raw column names were preserved in the original dataset.
- Saved a cleaned dataset for reuse in modeling.

**Output of Phase 1**

- A clean, validated, and clinically interpretable dataset
- Raw and clean datasets clearly separated

---

## Phase 2: Modeling and Evaluation

### Target Engineering

- Modeled LOS using a `log(1 + LOS)` transformation.
- Reduced skew and stabilized variance for regression models.

### Linear Regression (Baseline)

- One-hot encoded categorical variables during modeling.
- Trained a linear regression model on log-transformed LOS.
- Evaluated using MAE, RMSE, and R².
- Converted predictions back to days for clinical interpretation.
- Achieved approximately **2.2 days MAE**, establishing a strong baseline.

### Ridge Regression (Regularized Linear Model)

- Used a modeling pipeline with:
  - Standard scaling for numeric features
  - One-hot encoding for categorical variables
- Performance was nearly identical to linear regression.
- Indicated stable feature relationships and limited overfitting.
- Ridge primarily improved coefficient stability rather than accuracy.

### Random Forest (Nonlinear Model)

- Explicitly encoded all features as numeric.
- Trained a Random Forest regressor without scaling.
- Evaluated using the same metrics and units.
- Random Forest underperformed linear models:
  - Higher MAE
  - Lower R²
- Indicated limited nonlinear signal after preprocessing.

### Model Comparison and Conclusion

| Model             | MAE (days) | R²    |
| ----------------- | ---------- | ----- |
| Linear Regression | ~2.22      | ~0.33 |
| Ridge Regression  | ~2.22      | ~0.33 |
| Random Forest     | ~2.38      | ~0.24 |

**Key Insight**

> Simpler linear and regularized linear models generalized better than more complex nonlinear models for this dataset.

This highlights the importance of:

- Thoughtful preprocessing
- Model selection based on data structure
- Avoiding unnecessary complexity

---

## Phase 3: Project Synthesis

- Clear separation between cleaning, baseline modeling, and advanced models.
- Reproducible results using fixed random seeds.
- Evaluation focused on real-world units (days), not just abstract metrics.
- Strong emphasis on interpretability and clinical plausibility.

---

## How to Run

### Setup

git clone https://github.com/blewis7/clinical-length-of-stay-demo.git
cd clinical-length-of-stay-demo
python -m venv venv
venv\Scripts\activate (for Windows)
source venv/bin/activate (for macOS / Linux)
pip install -r requirements.txt

### Run Notebooks in Order

Open Jupyter Lab or VS Code and run:

1. 01_exploration_and_cleaning.ipynb
2. 02_linear_regression_baseline.ipynb
3. 03_ridge_and_tree_models.ipynb
   Each notebook is self-contained and builds logically on the previous phase.

---

## Final Notes

This project demonstrates a realistic machine learning workflow for healthcare analytics, emphasizing data quality, interpretability, and disciplined model evaluation.

It reflects real-world considerations where simpler models often outperform more complex ones when data is well-prepared and signals are largely linear.
