# Data Dictionary

**Project**: Hospital Readmission Rate Analysis (Diabetes 130-US Hospitals)  
**Dataset**: Diabetes 130-US Hospitals for Years 1999–2008  
**Source**: UCI Machine Learning Repository / Kaggle  
**Raw File**: `data/raw/RAW_HEALTHCARE.csv`  
**Records**: 101,766 hospital encounters | **Features**: 50 columns  

---

## Raw Dataset Columns (50)

### Identifiers

| Column | Type | Description |
|--------|------|-------------|
| `encounter_id` | Integer | Unique identifier for each hospital encounter |
| `patient_nbr` | Integer | Unique identifier for each patient (one patient may have multiple encounters) |

### Demographics

| Column | Type | Values | Description |
|--------|------|--------|-------------|
| `race` | Categorical | Caucasian, AfricanAmerican, Hispanic, Asian, Other, ? | Patient's race. `?` indicates missing (2.2%) |
| `gender` | Categorical | Female, Male, Unknown/Invalid | Patient's gender. 3 rows are `Unknown/Invalid` |
| `age` | Categorical | [0-10), [10-20), ..., [90-100) | Age group in 10-year intervals |
| `weight` | Categorical | [0-25), [25-50), ..., [175-200), ? | Weight range. **96.9% missing** — dropped in cleaning |

### Admission Information

| Column | Type | Values | Description |
|--------|------|--------|-------------|
| `admission_type_id` | Integer | 1–8 | Type of admission: 1=Emergency, 2=Urgent, 3=Elective, 4=Newborn, 5–8=Other/Not Mapped |
| `discharge_disposition_id` | Integer | 1–30 | Discharge location: 1=Home, 3=SNF, 6=Home Health, 11=Expired, etc. |
| `admission_source_id` | Integer | 1–26 | Source of admission: 1=Physician Referral, 7=Emergency Room, etc. |
| `time_in_hospital` | Integer | 1–14 | Number of days in hospital (length of stay) |
| `payer_code` | Categorical | MC, MD, HM, SP, etc., ? | Insurance payer code. **39.6% missing** — dropped in cleaning |

### Clinical Utilisation

| Column | Type | Range | Description |
|--------|------|-------|-------------|
| `medical_specialty` | Categorical | ~73 unique | Admitting physician's specialty. **49.1% missing** |
| `num_lab_procedures` | Integer | 1–132 | Number of lab tests performed during encounter |
| `num_procedures` | Integer | 0–6 | Number of non-lab procedures (e.g., surgeries) |
| `num_medications` | Integer | 1–81 | Number of distinct medications administered |
| `number_outpatient` | Integer | 0–42 | Number of outpatient visits in the year before this encounter |
| `number_emergency` | Integer | 0–76 | Number of emergency visits in the year before this encounter |
| `number_inpatient` | Integer | 0–21 | Number of inpatient visits in the year before this encounter |
| `number_diagnoses` | Integer | 1–16 | Number of diagnoses during this encounter |

### Diagnoses (ICD-9 Codes)

| Column | Type | Description |
|--------|------|-------------|
| `diag_1` | Categorical | Primary diagnosis ICD-9 code (~700 unique) |
| `diag_2` | Categorical | Secondary diagnosis ICD-9 code |
| `diag_3` | Categorical | Tertiary diagnosis ICD-9 code |

### Lab Results

| Column | Type | Values | Description |
|--------|------|--------|-------------|
| `max_glu_serum` | Categorical | None, Norm, >200, >300 | Maximum glucose serum test result. `None` = not measured (**94.8%**) |
| `A1Cresult` | Categorical | None, Norm, >7, >8 | HbA1c test result. `None` = not measured (**83.3%**) |

### Medications (23 columns)

Each medication column indicates whether the drug was prescribed and whether the dosage changed:

| Values | Meaning |
|--------|---------|
| `No` | Not prescribed |
| `Steady` | Prescribed, dosage unchanged |
| `Up` | Prescribed, dosage increased |
| `Down` | Prescribed, dosage decreased |

**Active medications** (retained after cleaning): `metformin`, `repaglinide`, `nateglinide`, `glimepiride`, `glipizide`, `glyburide`, `pioglitazone`, `rosiglitazone`, `insulin`

**Near-zero variance medications** (>99% `No`, dropped): `chlorpropamide`, `acetohexamide`, `tolbutamide`, `acarbose`, `miglitol`, `troglitazone`, `tolazamide`, `examide`, `citoglipton`, `glyburide-metformin`, `glipizide-metformin`, `glimepiride-pioglitazone`, `metformin-rosiglitazone`, `metformin-pioglitazone`

### Treatment Flags

| Column | Type | Values | Description |
|--------|------|--------|-------------|
| `change` | Categorical | Ch, No | Whether any diabetes medication dosage was changed |
| `diabetesMed` | Categorical | Yes, No | Whether any diabetes medication was prescribed |

### Target Variable

| Column | Type | Values | Description |
|--------|------|--------|-------------|
| `readmitted` | Categorical | `<30`, `>30`, `NO` | Readmission status: within 30 days, after 30 days, or not readmitted |

---

## Engineered Features (added in `02_cleaning.ipynb`)

| Column | Type | Formula / Logic | Description |
|--------|------|-----------------|-------------|
| `age_group` | Categorical | Original `age` column preserved | Age range for Tableau filtering |
| `age_midpoint` | Integer | Midpoint of age range (e.g., [70-80) → 75) | Numeric age for statistical analysis |
| `admission_type` | Categorical | Mapped from `admission_type_id` | Readable admission type label |
| `discharge_disposition` | Categorical | Mapped from `discharge_disposition_id` | Readable discharge disposition label |
| `admission_source` | Categorical | Mapped from `admission_source_id` | Readable admission source label |
| `glu_test_performed` | Binary (0/1) | 1 if `max_glu_serum != 'Not Measured'` | Whether glucose serum test was performed |
| `a1c_test_performed` | Binary (0/1) | 1 if `A1Cresult != 'Not Measured'` | Whether HbA1c test was performed |
| `diag_1_category` | Categorical | ICD-9 chapter mapping | Primary diagnosis grouped into clinical family |
| `diag_2_category` | Categorical | ICD-9 chapter mapping | Secondary diagnosis grouped |
| `diag_3_category` | Categorical | ICD-9 chapter mapping | Tertiary diagnosis grouped |
| `primary_diag_diabetes` | Binary (0/1) | 1 if `diag_1` starts with '250' | Whether primary diagnosis is diabetes |
| `total_prior_visits` | Integer | `outpatient + emergency + inpatient` | Total healthcare visits in prior year |
| `n_active_medications` | Integer | Count of medication columns ≠ 'No' | Number of active diabetes medications |
| `n_med_changes` | Integer | Count of medication columns = 'Up' or 'Down' | Number of medication dosage changes |
| `readmitted_binary` | Binary (0/1) | 1 if `readmitted == '<30'` | Binary 30-day readmission target |
| `readmission_status` | Categorical | Readable mapping of `readmitted` | Descriptive readmission label for Tableau |
| `med_changed` | Binary (0/1) | 1 if `change == 'Ch'` | Whether any medication was changed |
| `diabetes_med_flag` | Binary (0/1) | 1 if `diabetesMed == 'Yes'` | Whether diabetes medication was prescribed |
| `n_distinct_diag_categories` | Integer | Number of unique diagnosis categories across diag_1/2/3 | Comorbidity diversity indicator |
| `high_utiliser` | Binary (0/1) | 1 if `total_prior_visits > 3` | Flag for high healthcare utiliser |
| `medical_specialty_grouped` | Categorical | Top specialties kept; rest → 'Other' | Grouped specialty to reduce sparsity |

---

## Cleaning Decisions Summary

| Decision | Rationale |
|----------|-----------|
| Dropped `weight` column | 96.9% missing — imputation indefensible |
| Dropped `payer_code` column | 39.6% missing; not clinically relevant to readmission |
| Dropped near-zero variance meds | >99% `No` — no analytical signal |
| Removed 3 rows with Unknown/Invalid gender | Data quality; negligible loss |
| Kept first encounter per patient only | Prevents data leakage from repeated visits |
| Filled `medical_specialty` NaN → `Other` | Preserves analytical value, 49% missing |
| Filled `race` NaN → `Unknown` | Low missingness (2.2%); clean representation |
| `None` in lab results → `Not Measured` | Clinically meaningful — absence of a test is informative |
| ICD-9 codes → clinical categories | ~700 codes → ~18 interpretable groups |
| Grouped rare specialties (<1% volume) → `Other` | Prevents sparsity in categorical analysis |
