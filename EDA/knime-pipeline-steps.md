# KNIME Pipeline Steps — Full Workflow

## Step 1 — Data Input
- **Node:** CSV Reader
- **Action:** Load heart.csv dataset

## Step 2 — Handle Invalid Cholesterol Values
- **Node:** Rule Engine → Missing Value
- **Rule Engine config:**
  - `$Cholesterol$ = 0 => 1` / `TRUE => 0`
  - Output column: `Cholesterol_missing_flag`
- **Missing Value config:**
  - Column: Cholesterol
  - Treat 0 as missing
  - Imputation: Mean

## Step 3 — Encode Categorical Variables (Abhi)
- **Node:** One to Many
- **Columns:** Sex, ChestPainType, RestingECG, ExerciseAngina, ST_Slope
- Creates dummy variables (e.g., Sex_M, Sex_F, ChestPainType_ASY, etc.)

## Step 4 — Handle Remaining Missing Values (Abhi)
- **Node:** Missing Value
- Numeric columns → Mean
- Categorical columns → Most Frequent

## Step 5 — Normalize Features
- **Node:** Normalizer
- **Method:** Z-score normalization
- Applied to all numeric columns

## Step 6 — Prepare Target Variable
- **Node:** Number to String
- Column: HeartDisease
- Convert 0/1 → "0"/"1" (categorical)

## Step 7 — Remove Unnecessary Columns
- **Node:** Column Filter
- Remove original categorical columns (after encoding)
- Remove any irrelevant columns
