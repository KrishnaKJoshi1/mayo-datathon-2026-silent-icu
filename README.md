# Silent De-Escalation in the ICU
### MIMIC-IV Datathon 2026 — Team Research Project · Hosted at Mayo Clinic

> **"The chart said rescue. The care had already quietly shifted."**

[![Data](https://img.shields.io/badge/Data-MIMIC--IV_v3.1-0a3f6e?style=flat-square)](https://physionet.org/content/mimiciv/)
[![Model](https://img.shields.io/badge/Model-XGBoost-e07b39?style=flat-square)](https://xgboost.readthedocs.io/)
[![Engine](https://img.shields.io/badge/Engine-DuckDB-ffd700?style=flat-square)](https://duckdb.org)
[![Cohort](https://img.shields.io/badge/Cohort-BigQuery-4285f4?style=flat-square)](https://cloud.google.com/bigquery)
[![Status](https://img.shields.io/badge/Status-Complete-2ecc71?style=flat-square)]()

---

## 🔬 Research Question

In the ICU, care teams sometimes quietly shift a patient toward comfort care —
ordering fewer labs, writing shorter notes, calling fewer specialists —
**well before the chart reflects any change.**

We asked: **Is that 48-hour behavioral gap measurable from data alone?**
And if so — who does it happen to, and what are their outcomes?

---

## 📊 Key Findings

| Finding | Result |
|---|---|
| ICU stays analyzed | **94,458** (MIMIC-IV v3.1) |
| Silent candidates identified | **1,217** (2.8% of never-transition cohort) |
| Behavioral gap before documentation | **~48 hours** |
| De-escalation before any order written | **42% of total shift** |
| In-hospital mortality (silent vs. full-code) | **2.8× higher** |
| XGBoost AUROC | **0.959** |
| XGBoost AUPRC (held-out validation) | **0.557** |

---

## 🧠 Methodology

**1 — Palliative-ness Score**
Four clinical proxies — lab velocity, diagnostic diversity,
clinical engagement, specialist integration — computed per 24-hour
window and residualized against severity (SOFA, ventilation,
vasopressors) using OLS regression. Captures team intent, not
just patient improvement.

**2 — Transition Vector**
Built from 1,898 documented DNR/CMO transitions.
Within-patient delta defines a unit-length behavioral direction vector.
The palliative-ness score is the dot product of each window's
z-scored residuals against this vector.

**3 — Silent Candidate Filter**
Five sequential criteria applied to never-documented patients:
≥48h LOS → no DNR/CMO → ≥3 scored windows →
rescue-mode baseline → ≥2 consecutive palliative windows
with non-improving SOFA.

**4 — Predictive Model**
XGBoost trained on 3×24h temporal bins + delta features
to predict long ICU stays (≥21 days).
AUPRC chosen as primary metric for class imbalance.
80/20 train/holdout split with no data leakage.

---

## 🏥 Why It Matters

When behavioral signals diverge from the chart, families may not
know the team's true direction. Ethics reviews and fairness audits
operate from documented orders — they miss these patients entirely.
Making the gap visible is the first step toward giving patients
and families the conversation they deserve.

---

## 👥 Team

**Event:**  Datathon 2026 · Hosted at Mayo Clinic Jacksonville FLorida· April 2026

**Mentors:**
Ahram Han (MIT Visiting Researcher) ·
Dr. Robert J. Kahoud, M.D. (Pediatric Intensivist, Mayo Clinic Rochester)

**Data Team:**
Brett Knox · Yu Yin Cheong · Krishna K. Joshi ·
Ashritha Kotagiri · Simon McCormack

**Medical Team:**
Marta E. Berguido · Elizabeth Carey · Ross Reichard

**Full codebase maintained by team lead:**
👉 [github.com/BrettKnox/silent-icu-shifts](https://github.com/BrettKnox/silent-icu-shifts)

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Data Source | MIMIC-IV v3.1 (PhysioNet) |
| Cohort Engine | Google BigQuery |
| Feature Engine | DuckDB |
| Language | Python 3.8 |
| ML | scikit-learn · XGBoost · pandas · joblib |

---

*All patient data accessed under PhysioNet credentialed 
access agreement. MIMIC-IV Datathon 2026.*
