# Clinical-Documentation-Review-Medical-Record-Quality-Audit

# Clinical Report Abstraction & Validation

**A Medical Data Analyst portfolio project:** extracting, validating, and coding key data from public sample clinical reports (oncology and hematology) using Microsoft Excel.

![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)
![Status](https://img.shields.io/badge/Status-15%20of%2045%20reports%20abstracted-orange)

## Project summary

I sampled 45 oncology and hematology-related reports from a public clinical transcription dataset, then abstracted **15** of them into a structured workbook. For each report I extracted clinical fields, flagged incomplete records, screened for HIPAA identifiers, mapped diagnoses to ICD-10-CM codes with written reasoning, and recorded review time.

The goal was to practice the core Medical Data Analyst workflow: read a clinical document, capture the right data points, validate them, and document what is missing or inconsistent rather than guessing.

## Data source and privacy

- **Source:** MTSamples-style public medical transcription samples (Kaggle copy, `mtsamples.csv`).
- All reports are public samples, **not real patient records**.
- The source text is not redistributed in this repository. The `source_row` column refers to the row in the original CSV, and `sample_reports.py` recreates my sample from it. Check the source site's terms before using its content.

## Dataset profile (measured with pandas)

| Check | Result |
|---|---|
| Total rows | 4,999 |
| Rows with a transcription | 4,966 (33 missing) |
| Unique transcriptions | 2,357 |
| Redundant duplicate rows | 2,609 (same report text under multiple specialties) |
| Specialty values | 40, all with leading or trailing spaces (trimmed before filtering) |
| Hematology - Oncology rows | 90 (all unique) |

**Sampling:** duplicates removed, reports of 800 to 6,000 characters kept, then a stratified random sample of 45 (24 Hematology-Oncology plus 21 oncology-related Radiology, Consult/Progress, Surgery, Urology, and General Medicine reports), seed 42.

## What I extracted for each of the 15 reports

`Report_ID`, `medical_specialty`, `Patient_Age`, `Gender`, `Primary_Diagnosis`, `Symptoms`, `Past_Medical_History`, `Medications`, `Procedure`, `Lab_Finding`, `Allergies`, `Smoking_Status`, `Alcohol_Status`, `Report_Type`, `Organ_Site`, `Stage`, `Treatment_Planned`, `Text_Truncated`, `Data_Quality_Status`, `PHI_Present`, `Date_Check`, `Discrepancy_Log`, `Review_Time`, `ICD10_Mapped_Code`, `ICD10_Description`, `Coding_Reasoning`, `ICD10_Verified`, `Audit_Status`

Where a report did not state a value, I recorded "Not mentioned" or "Not stated" instead of assuming one.

## Results

| Metric | Result |
|---|---|
| Reports abstracted | 15 of the 45 sampled |
| Report types | 9 consultations, 2 imaging, 1 discharge summary, 1 operative note, 1 progress note, 1 radiation planning note |
| Truncated (incomplete) reports | **5 of 15** (R-001, R-002, R-003, R-005, R-007) end before the exam or plan section; marked Incomplete, with Stage and Treatment recorded as "Not stated" |
| Complete reports | 10 of 15 |
| ICD-10-CM codes mapped | 15, each with an official description and written reasoning |
| HIPAA identifier screen | Dates and/or a location name appear in 6 of 15 reports (R-001, R-003, R-005, R-006, R-007, R-008); no patient names, contact details, or record numbers found |
| Review time | 90 minutes in total, **6.0 minutes per report on average** (range 4 to 8), about 10 reports per hour |

## Key findings

1. **The source dataset is heavily duplicated.** More than half of its rows (2,609 of 4,966) repeat another row's text, so counting rows without de-duplicating would overstate the data.
2. **Some reports are cut off.** A third of my abstracted sample ends before the plan, so a "Complete" label based on row presence alone would be wrong. I flagged them as Incomplete.
3. **Dates and place names remain even in public samples.** A date or location in a clinical note counts as an identifier under HIPAA Safe Harbor, so de-identified-looking text still needs a check.
4. **Documentation wording affects coding.** Phrases like "consistent with" and "probable" mean a diagnosis may not be confirmed, which changes how it should be coded. I recorded my reasoning for each code.

## Workbook structure

| Sheet | Purpose |
|---|---|
| `Clinical_Data_Abstraction` | One row per report with all extracted fields and quality columns |
| `PHI` | PHI screen, date check, truncation flag, discrepancy notes, and time per report |
| `ICD-10CM` | Code, official description, reasoning, and verification flag per report |
| `data_validation_use` | Dropdown lists used for validation |

## Repository contents

```
clinical-report-abstraction-validation/
├── README.md
├── sample_reports.py          # recreates the 45-report sample from mtsamples.csv
└── Clinical_Report_Abstraction.xlsx
```

## How to reproduce the sample

```bash
pip install pandas
python sample_reports.py mtsamples.csv   # writes my_45_reports.csv
```

## Tools and skills used

- **Microsoft Excel:** structured tracker, dropdown data validation, conditional formatting
- **Python (pandas):** dataset profiling, de-duplication, reproducible sampling
- **Healthcare data skills:** medical record review, clinical data abstraction, data quality checks, HIPAA identifier screening, ICD-10-CM mapping practice

