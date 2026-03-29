# Week 2 — EDA Findings (Group 7)

**Date:** 3/28/2026 | **Sprint:** Week 2 (EDA) | **Status:** Distributions analyzed

---

## Task Division

| Task | Assignee | Deadline |
|------|----------|----------|
| Audit / Audit Table | Mason | 3/28 |
| Missing Values, Data Dictionary | Lasya | 3/28 |
| Heatmap and Interpretation | Bryson | 3/28 |
| Categorical Variable Analysis, Repo Setup | Abhi | 3/28 |

---

## Findings Summary

### Skewness & Outliers

Variables analyzed (in order): Age, Cholesterol, MaxHR, RestingBP

- **Cholesterol** is highly skewed with a large number of 0 values and several high-value outliers
- **Age, MaxHR, RestingBP** have mostly uniform distributions with no major skew

---

### Audit Table

| Feature | Mean | Median | Std Dev | % Missing |
|---------|------|--------|---------|-----------|
| Age | 53.5 | 54 | 9.4 | 0% |
| RestingBP | 132.4 | 130 | 18.5 | 0% |
| Cholesterol | 198.8 | 223 | 109.4 | 0% |
| MaxHR | 136.8 | 138 | 25.5 | 0% |
| Oldpeak | 0.9 | 0.6 | 1.1 | 0% |

> **Note:** 0% missing is misleading — 172 Cholesterol values are `0`, which is medically implausible and should be treated as missing.

---

### Anomaly Log

| Anomaly | Count | Notes |
|---------|-------|-------|
| Cholesterol = 0 | 172 records | Medically implausible; likely missing lab measurements or data entry placeholders |
| RestingBP = 0 | 1 record | Should be treated as missing/anomalous |

---

### Missing Value Findings

- Standard detection shows **no missing values** — but this is deceptive
- **172 records** have Cholesterol = 0, which is clinically impossible
- After recoding these as `NaN`, the dataset reveals **previously hidden missing data**
- These zeros likely represent:
  - Missing lab measurements
  - Data entry placeholders
- **Impact:** Models trained on incorrect "0" values would be biased
- **Action needed:** Treat Cholesterol = 0 as missing data (imputation or removal in Week 3)

---

### Data Dictionary

| Column | Type | Description |
|--------|------|-------------|
| **Age** | Numeric | Age of the patient (years) |
| **Sex** | Categorical | Biological sex (M = Male, F = Female) |
| **ChestPainType** | Categorical | ATA = Atypical Angina, NAP = Non-Anginal Pain, ASY = Asymptomatic, TA = Typical Angina |
| **RestingBP** | Numeric | Resting blood pressure (mm Hg) |
| **Cholesterol** | Numeric | Serum cholesterol (mg/dL) — 0 indicates missing data |
| **FastingBS** | Binary | Fasting blood sugar > 120 mg/dL (1 = True, 0 = False) |
| **RestingECG** | Categorical | Normal, ST = ST-T wave abnormality, LVH = Left ventricular hypertrophy |
| **MaxHR** | Numeric | Maximum heart rate achieved during exercise |
| **ExerciseAngina** | Binary | Exercise-induced angina (Y = Yes, N = No) |
| **Oldpeak** | Numeric | ST depression induced by exercise relative to rest — measures how much heart's electrical activity deviates under stress |
| **ST_Slope** | Categorical | Up = Upsloping (healthy), Flat = Possible abnormality, Down = High risk of heart disease |
| **HeartDisease** | Binary (Target) | 1 = Yes, 0 = No |

---

### Heatmap Analysis

**Key finding: MaxHR is the most influential feature** in terms of linear correlation with heart disease.

| Feature | Correlation with HeartDisease | Interpretation |
|---------|------------------------------|----------------|
| **MaxHR** | Moderate **negative** | Higher max heart rate → lower disease likelihood (strongest relationship) |
| **Age** | Weak positive | Older patients → slightly increased risk |
| **FastingBS** | Weak positive | Higher fasting blood sugar → slightly increased risk |
| **Cholesterol** | Little to none | No significant linear relationship |
| **RestingBP** | Little to none | No significant linear relationship |

> **Takeaway:** MaxHR stands out as the most correlated feature. Cholesterol and RestingBP show surprisingly little linear relationship with heart disease in this dataset — this is worth investigating further in Week 3 (the "Cholesterol Paradox" question).

---

### Categorical Variable Distributions

#### Sex

| Value | Count | % of Total | Disease Rate |
|-------|-------|------------|-------------|
| Male (M) | 725 | 79.0% | 63.2% |
| Female (F) | 193 | 21.0% | 25.9% |

> **Key insight:** Dataset is heavily male-dominated (79%). Males have a much higher disease rate (63.2% vs 25.9%). This imbalance is critical for the Week 4-5 fairness analysis — the model will have far fewer female samples to learn from.

#### ChestPainType

| Value | Count | % of Total | Disease Rate |
|-------|-------|------------|-------------|
| ASY (Asymptomatic) | 496 | 54.0% | 79.0% |
| NAP (Non-Anginal Pain) | 203 | 22.1% | 35.5% |
| ATA (Atypical Angina) | 173 | 18.8% | 13.9% |
| TA (Typical Angina) | 46 | 5.0% | 43.5% |

> **Key insight:** Asymptomatic patients dominate the dataset AND have the highest disease rate (79%). This is the most dangerous category — patients show no chest pain symptoms but are most likely to have heart disease. This connects directly to the Week 3 "ASY Deep-Dive" question.

#### FastingBS (Fasting Blood Sugar > 120 mg/dL)

| Value | Count | % of Total |
|-------|-------|------------|
| 0 (Normal) | 704 | 76.7% |
| 1 (High) | 214 | 23.3% |

#### RestingECG

| Value | Count | % of Total |
|-------|-------|------------|
| Normal | 552 | 60.1% |
| LVH (Left Ventricular Hypertrophy) | 188 | 20.5% |
| ST (ST-T Wave Abnormality) | 178 | 19.4% |

#### ExerciseAngina

| Value | Count | % of Total |
|-------|-------|------------|
| N (No) | 547 | 59.6% |
| Y (Yes) | 371 | 40.4% |

#### ST_Slope

| Value | Count | % of Total |
|-------|-------|------------|
| Flat | 460 | 50.1% |
| Up (Upsloping) | 395 | 43.0% |
| Down (Downsloping) | 63 | 6.9% |

#### HeartDisease (Target Variable)

| Value | Count | % of Total |
|-------|-------|------------|
| 0 (No Disease) | 410 | 44.7% |
| 1 (Has Disease) | 508 | 55.3% |

> **Key insight:** The dataset is slightly imbalanced toward disease cases (55.3% vs 44.7%). Not extreme, but stratified splitting is still important to preserve this ratio.

---

## Looking Ahead to Week 3

Based on these findings, next week's tasks should focus on:

1. **Handling the 172 Cholesterol = 0 records** — decide on imputation strategy (mean, regression, flag)
2. **Handling the 1 RestingBP = 0 record**
3. **Answering the 5 clinical questions** — especially the Cholesterol Paradox, given the weak correlation found here
4. **Feature engineering** — HR Reserve Ratio (`MaxHR / (220 - Age)`) to see if it outperforms raw MaxHR
5. **Producing the cleaned CSV** for the Week 4 modeling pipeline
