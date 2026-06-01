## View Validation Results
[Click here to view the fully rendered validation results](https://htmlpreview.github.io/?https://raw.githubusercontent.com/SubramanyaAttota/AI-Enhanced-SIEM-Framework-Validation/main/SIEM_Framework_Validation.html)

# AI-Enhanced SIEM Framework — Validation

Validation code for the research paper **"AI-Enhanced SIEM Framework for Incident Response"**.  
Experiments are run on the [Microsoft GUIDE Dataset](https://www.kaggle.com/datasets/Microsoft/microsoft-security-incident-prediction) and cover four areas where the proposed framework improves on current AI-SIEM approaches.

---

## Repository Structure

```
AI-Enhanced-SIEM-Framework-Validation/
│
├── SIEM_Framework_Validation.html   ← Main notebook (all experiments and results)
├── requirements.txt                  ← Python dependencies
├── README.md                         ← This file
│
└── outputs/                          ← Generated after running the notebook
    ├── table_detection_performance.csv
    ├── table_prioritisation_comparison.csv
    ├── table_investigation_workload.csv
    ├── table_response_summary.csv
    ├── table_response_action_distribution.csv
    ├── SIEM_Framework_Results_Tables.xlsx
    ├── figure_normalised_confusion_matrix.png
    ├── figure_prioritisation_comparison.png
    ├── figure_investigation_workload.png
    └── figure_response_summary.png
```

---

## Dataset

| Field | Detail |
|-------|--------|
| Name | GUIDE (Microsoft Security Incident Prediction) |
| Source | [Kaggle](https://www.kaggle.com/datasets/Microsoft/microsoft-security-incident-prediction) |
| File needed | `GUIDE_Train.csv` |
| Rows loaded | 300,000 (configurable via `nrows` in Cell 2) |

**Download and place `GUIDE_Train.csv` in the same directory as the notebook before running.**

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/SubramanyaAttota/AI-Enhanced-SIEM-Framework-Validation.git
cd AI-Enhanced-SIEM-Framework-Validation
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add the dataset

Download `GUIDE_Train.csv` from Kaggle and place it in the repository root.

### 4. Run the notebook

```bash
jupyter notebook SIEM_results.ipynb
```

Run all cells top-to-bottom (**Kernel → Restart & Run All**).

---

## Experiments

### Experiment 1 — Incident Detection Performance

**What it does:** Trains an XGBoost multi-class classifier to predict `IncidentGrade` (`TruePositive`, `BenignPositive`, `FalsePositive`).

**Model:** XGBoostClassifier — 200 estimators, max depth 6, learning rate 0.1, 80/20 train-test split.

**Outputs:**

| Output | Description |
|--------|-------------|
| `table_detection_performance.csv` | Accuracy, Macro Precision, Recall, F1 |
| `figure_normalised_confusion_matrix.png` | Row-normalised confusion matrix heatmap |

---

### Experiment 2 — Alert Prioritisation with Hybrid Risk Score

**What it does:** Compares two ranking strategies for surfacing True Positives in the top reviewed alerts:

- **Current AI-SIEM:** Ranks by TruePositive class probability only (ML confidence).
- **Proposed Framework:** Hybrid score = `0.80 × ML_probability + 0.20 × context_score`

**Context score** is a weighted combination of:

| Feature | Weight |
|---------|--------|
| High-risk Category (Malware, Exfiltration, etc.) | 0.30 |
| EvidenceRole contains Impacted/Compromised | 0.20 |
| SuspicionLevel is populated | 0.15 |
| ThreatFamily is populated | 0.15 |
| MitreTechniques is populated | 0.10 |
| LastVerdict contains Malicious/Suspicious | 0.10 |

Evaluated at Top 10%, 20%, 25%, and 30% thresholds.

**Outputs:**

| Output | Description |
|--------|-------------|
| `table_prioritisation_comparison.csv` | TP rate at each threshold for both approaches |
| `figure_prioritisation_comparison.png` | Line chart comparing both approaches |

---

### Experiment 3 — Investigation Workload Reduction

**What it does:** Measures how much analyst workload is reduced by grouping individual alert rows into incident-level units.

- **Current AI-SIEM:** Analysts review each alert/evidence row individually.
- **Proposed Framework:** Rows are grouped by `IncidentId` — analysts review one record per incident.

**Outputs:**

| Output | Description |
|--------|-------------|
| `table_investigation_workload.csv` | Row count vs incident count + workload reduction % |
| `figure_investigation_workload.png` | Bar chart comparing review item counts |

---

### Experiment 4 — Automated Response Recommendation

**What it does:** Maps each alert to a recommended response action using a `Category × IncidentGrade` playbook matrix.

**Decision logic:**
- `TruePositive` → Immediate containment action specific to the attack category
- `BenignPositive` → Validate and monitor
- `FalsePositive` → No containment; tune detection rule
- Unknown → Analyst review required

**Categories covered:** Malware, Phishing, CredentialAccess, CommandAndControl, Exfiltration, Reconnaissance, Persistence, PrivilegeEscalation, Execution, InitialAccess, LateralMovement.

**Outputs:**

| Output | Description |
|--------|-------------|
| `table_response_summary.csv` | Record count and unique response types per IncidentGrade |
| `table_response_action_distribution.csv` | Top 15 most-issued response actions |
| `figure_response_summary.png` | Bar chart of records per IncidentGrade |

---

## All Generated Outputs

| File | Type |
|------|------|
| `table_detection_performance.csv` | CSV |
| `table_prioritisation_comparison.csv` | CSV |
| `table_investigation_workload.csv` | CSV |
| `table_response_summary.csv` | CSV |
| `table_response_action_distribution.csv` | CSV |
| `SIEM_Framework_Results_Tables.xlsx` | Excel (all tables, one workbook) |
| `figure_normalised_confusion_matrix.png` | Figure |
| `figure_prioritisation_comparison.png` | Figure |
| `figure_investigation_workload.png` | Figure |
| `figure_response_summary.png` | Figure |

---

## Requirements

```
pandas
numpy
scikit-learn
xgboost
matplotlib
seaborn
openpyxl
jupyter
```

---

## Citation

If you use this code, please cite:

```
Attota, S. (2026). AI-Enhanced SIEM Framework for Incident Response.
GitHub: https://github.com/SubramanyaAttota/AI-Enhanced-SIEM-Framework-Validation
```

---

## License

MIT License — free to use and adapt with attribution.
