# CISAM — Predicting 30-Day Return After High-Risk Obstetric Care

Notebooks supporting the paper *Predicting 30-Day Emergency or Inpatient Return After Encounters
at a Brazilian Public High-Risk Maternity Hospital: Temporal Validation, Triage-Text Enrichment
and a Leakage Audit*, and the doctoral thesis it derives from. They use structured and free-text
electronic health records of a high-risk pregnancy cohort from a public maternity hospital in
Recife, Brazil.

## What is (and is not) in this repository

- **Included:** the Jupyter notebooks of the analysis pipeline, in [`notebooks/`](./notebooks/),
  with the **figures kept** as rendered outputs.
- **Removed:** every textual output (printed tables, `DataFrame` previews, logs). Some of them
  displayed record-level identifiers, so they were stripped before publication. Running the
  notebooks again regenerates them.
- **Reporting:** the completed [TRIPOD+AI checklist](./reporting/TRIPOD-AI_checklist.md) and a
  [PROBAST+AI self-assessment](./reporting/PROBAST-AI_self-assessment.md) of the paper.
- **Not included:** the data, the trained artifacts, and the auxiliary source modules and SQL
  extraction queries.

## Notebooks

| Notebook | Stage |
|---|---|
| `01_eda_gestante` | Exploratory analysis of the high-risk pregnancy cohort |
| `01.1_analise_temporal_comorbidades` | Time to return, 30/90-day return rates, comorbidity profile |
| `03.1_preparacao_dados` | Admission-time predictor space, temporal split (train ≤2020 / validation 2021 / test ≥2022), coverage checks |
| `03.2_treinamento_modelos` | Training and tuning of the models (logistic regression, decision tree, random forest, XGBoost, LightGBM, CatBoost) |
| `03.3_calibracao_testes` | Calibration (Platt/isotonic), ROC/PR curves, DeLong and McNemar tests, confusion matrices |
| `03.4_xai_explicabilidade` | SHAP (global, local, dependence), permutation importance, ALE, decision-curve analysis |
| `03.5_equidade_fairness` | Performance by subgroup (ICD-10 segment, age, race, parity, encounter type), temporal drift, learning curve |
| `03.6_inferencia_classica` | Threshold analysis and odds ratios |
| `03.7_leakage_export` | Temporal leakage audit: admission-only vs. admission + post-encounter text |
| `04_modelos_com_llm` | Ablation with LLM-derived features (explored in the thesis; not part of the paper) |
| `05*`, `06*` | Causal inference (IPTW/AIPW) on early documentation and obstetric profile (thesis) |
| `07*` | Newborn analyses (thesis) |
| `08*` | Lower-risk profile among documented patients (thesis) |

Notebooks `03.1`–`03.7` correspond to the prediction pipeline reported in the thesis. The paper
re-runs this pipeline on a predictor space restricted to triage-time text, so the
figures here can differ numerically from the published tables.

## Running

The notebooks expect the cohort extract at `data/gestante_quadro_clinico.parquet` and write their
artifacts to `out/`. Each notebook's first cell lists its upstream dependency. Python 3.12 and the
usual scientific stack (`pandas`, `scikit-learn`, `xgboost`, `lightgbm`, `catboost`, `shap`,
`lime`, `statsmodels`, `matplotlib`) are required.

## Data access and ethics

The records contain sensitive clinical information and cannot be shared openly. They are
available for academic use on request, using the access-request template of the companion
[CISAM-EHR repository](https://github.com/tiagoalvesber/cisam-ehr).

> **Ethics approval:** CAAE 80278024.3.0000.5191, approved on September 16, 2024, with waiver of
> informed consent for retrospective secondary use of anonymised records.

## Citation

If you use this code, please cite the paper (reference to be added upon publication) and this
repository: <https://github.com/tiagoalvesber/cisam-predicting30-day>.

## Contact

Tiago Lima (tabl [at] ecomp.poli.br)
