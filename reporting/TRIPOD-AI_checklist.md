# TRIPOD+AI checklist

Manuscript: *Predicting 30-Day Emergency or Inpatient Return After Encounters at a Brazilian Public High-Risk Maternity Hospital: Temporal Validation, Triage-Text Enrichment and a Leakage Audit* (ACM SAC 2027, ASH track, double-blind submission).

Checklist: TRIPOD+AI statement, Collins et al., BMJ 2024;385:e078378, Table 2 (27 items, 52 sub-items) and Table 3 (TRIPOD+AI for Abstracts, 13 items). Item wording is abbreviated; the numbering is the official one.

Study type: development and temporal (internal) validation of one primary model (LightGBM), with logistic regression as the pre-specified reference. D = development item, E = evaluation item.

Status: **Yes** = reported; **Partly** = reported in part (the note says what is missing); **No** = not reported; **NA** = not applicable or withheld for double-blind review. Location gives section and page of the submitted PDF (version of 16 October 2026).

## Main checklist

| Item | D/E | Topic (abbreviated) | Status | Location | Note |
|---|---|---|---|---|---|
| 1 | D;E | Title: development/evaluation, target population, outcome | Partly | Title, p.1 | Population, outcome and temporal validation are named; the word "development" is not. |
| 2 | D;E | Abstract | see below | Abstract, p.1 | Assessed with TRIPOD+AI for Abstracts. |
| 3a | D;E | Healthcare context, diagnostic/prognostic, rationale, existing models | Yes | §1 p.1; §2 p.2 | Prognostic model; existing obstetric and 30-day readmission models reviewed. |
| 3b | D;E | Target population, intended purpose in the care pathway, intended users | Yes | §4.2 "Time zero and intended use" p.3; §5.4 p.5 | Time zero (end of triage or admission), users, action and population of intended use (antepartum emergency encounters, chosen post hoc) are stated, with performance, calibration and alert load in that population. |
| 3c | D;E | Known health inequalities between sociodemographic groups | No | — | Race and residence appear only in results and limitations (§5.6, §6). |
| 4 | D;E | Objectives, development and/or validation | Yes | §1 contributions p.1; Abstract | |
| 5a | D;E | Data sources for development and evaluation, rationale, representativeness | Partly | §3.1 p.2 | Routine electronic health record of one centre, same source for both. Representativeness is described only through residence shares. |
| 5b | D;E | Dates of accrual and end of follow-up | Yes | §3.1 p.2; §3.2 p.2; §4.2 p.3 | January 2017 to March 2023; follow-up ends with the extraction (truncated windows, §3.2). |
| 6a | D;E | Setting, number and location of centres | Yes | §3.1 p.2 | One public tertiary maternity hospital in Northeast Brazil (name withheld for review). |
| 6b | D;E | Eligibility criteria | Yes | §3.1 p.2 | |
| 6c | D;E | Treatments received and their handling | Partly | §4.5 "Outcome decomposition" p.4 | Delivery during the stay defines the pregnancy phase. No other treatment is considered. |
| 7 | D;E | Pre-processing and quality checking, similarity across sociodemographic groups | Partly | §3.1 p.2; §4.1 p.3; §4.3 p.4 | Deduplication and text normalisation are described. Extractor accuracy was not measured, and no check across groups was made. |
| 8a | D;E | Outcome definition, horizon, how and when assessed, rationale, consistency across groups | Yes | §3.2 p.2; Table 3 p.6; §6 "Limitations" p.8 | Ascertainment that differs by residence is acknowledged in the limitations. |
| 8b | D;E | Qualifications of outcome assessors if subjective | NA | §3.2 p.2 | Outcome derived automatically from encounter records; no human assessment. |
| 8c | D;E | Blinding of outcome assessment | Yes | §3.2 p.2 | The outcome is derived automatically from later encounter records, without access to the predictors. |
| 9a | D | Choice of initial predictors and any pre-selection | Partly | §4.2 p.3 | The admission-time restriction and the dropped predictors are explained. How the initial list was chosen is not. |
| 9b | D;E | Definition of all predictors, how and when measured | Partly | §4.1 p.3; §4.2 p.3; §5.6 p.6 | Predictors, extraction rule families and the multiple-mention rule are described. The record does not show when the principal ICD-10 code is entered; the share of delivery-encoding codes and a sensitivity analysis without them are reported. |
| 9c | D;E | Qualifications of predictor assessors if subjective | Yes | §4.2 p.3 | The triage colour and triage fields are recorded by the triage nursing staff; race is self-declared. |
| 10 | D;E | Study size, justification, sample-size calculation | Yes | §4.5 "Sample size, reporting and open science" p.5 | Development criteria of Riley et al. (2020) and validation criteria (2021) are both addressed; the precision of the 210-event delivery stratum is stated. |
| 11 | D;E | Handling of missing data; reasons for omitting data | Yes | §4.3 p.4; §3.1–3.2 p.2 | Method, missing share of the main predictors, single imputation, and the 25 and 839 exclusions are reported. |
| 12a | D | How data were used, partitioning | Yes | §4.2 p.3 | Temporal split; post hoc uses of the test partition are declared. |
| 12b | D | Handling of predictors (functional form, scaling) | Yes | §4.3 p.4 | |
| 12c | D | Model type, rationale, building steps, hyperparameter tuning, internal validation | Partly | §4.3 p.4 | Per-model search budgets and the reuse of the primary hyper-parameters in audit and ablation refits are reported. The search spaces and selected values are not. |
| 12d | D;E | Heterogeneity across clusters | Yes | §4.5 p.4 | Single centre, so there are no clusters. Heterogeneity over time (rolling origin) and across phase and sector strata is analysed. |
| 12e | D;E | Performance measures and plots, model comparison | Yes | §4.4 p.4 | |
| 12f | E | Model updating (e.g., recalibration) | Yes | §4.4 p.4; §4.5 p.4 | Recalibration on the validation year; phase-specific refits. |
| 12g | E | How predictions were calculated (formula, code, object, API) | No | — | The analysis notebooks are public; the scripts that produce the reported tables are promised for the published version. |
| 13 | D;E | Class imbalance methods | Yes | §4.3 p.4 | None used, with reasons. |
| 14 | D;E | Approaches to model fairness and rationale | Partly | §4.5 "Robustness" p.4; §5.6 p.6 | A race-free refit and results by race and residence are reported. No fairness approach or rationale is stated in the Methods. |
| 15 | D | Model output, classification thresholds and how identified | Yes | §4.4 p.4; §4.2 p.3 | Calibrated probability; a 30% threshold fixed a priori and the Youden threshold from validation. |
| 16 | D;E | Differences between development and evaluation data | Yes | Table 1 p.3; §5.1–5.2 p.5 | Drift in prevalence, sector mix and documentation is reported. |
| 17 | D;E | Ethics committee and consent or waiver | Yes | §3.3 p.2 | Committee and protocol number withheld for review; waiver of consent stated. |
| 18a | D;E | Funding | NA | — | Withheld for double-blind review. |
| 18b | D;E | Conflicts of interest | NA | — | Withheld for double-blind review. |
| 18c | D;E | Protocol | Yes | §4.5 p.5 | No protocol was published. |
| 18d | D;E | Registration | Yes | §4.5 p.5 | Not registered. |
| 18e | D;E | Data sharing | Yes | §4.5 p.5 | Records cannot be shared under Brazilian data-protection law. |
| 18f | D;E | Code sharing | Partly | §4.5 p.5 | The analysis notebooks are in a public repository (link withheld for review); the table-producing scripts, extraction rules and hyper-parameter grids are promised for the published version. |
| 19 | D;E | Patient and public involvement | Yes | §4.5 p.5 | None. |
| 20a | D;E | Flow of participants, numbers with and without the outcome | Partly | §3.1–3.2 p.2; §4.2 p.3; §4.5 p.4–5 | The counts from 78,339 to 78,314 and the 839 truncated encounters are given. Events are counted only for training and test, and there is no flow diagram. |
| 20b | D;E | Characteristics overall and by setting, including missing data and demographic differences | Partly | Table 1 p.3; §4.3 p.4 | Age and race are not in Table 1, and there is no comparison across demographic groups. |
| 20c | E | Comparison of evaluation with development data | Yes | Table 1 p.3 | By temporal partition. |
| 21 | D;E | Participants and events in each analysis | Partly | §4.5 p.5; §5.4 p.5; Table 4C p.7 | Training (11,881 events), test (2,481) and the phase strata are given. Validation-year and tuning-fold events are not. |
| 22 | D | Full model specification for use by others | No | — | No model object, formula or code is available. |
| 23a | D;E | Performance with confidence intervals, including key subgroups | Partly | Tables 2–4 p.6–7; §5.4 p.5; §5.6 p.6 | Intervals are given for the main and segment results. The race subgroups have no intervals. |
| 23b | D;E | Heterogeneity across clusters | NA | Table 4B p.7; §5.2 p.5 | Single centre. Results by phase and sector are reported. |
| 24 | E | Results of model updating | Yes | §5.3 p.5; Table 4C p.7 | |
| 25 | D;E | Overall interpretation, including fairness | Partly | §6 p.7–8 | Fairness is discussed only in the limitations. |
| 26 | D;E | Limitations | Yes | §6 "Limitations" p.8 | |
| 27a | D | Handling of poor-quality or unavailable inputs in implementation | Yes | §4.2 "Time zero and intended use" p.3 | Missing predictors are imputed at the point of use as in training, and an alert should show when the obstetric block was imputed. |
| 27b | D | User interaction and required expertise | Partly | §4.2 "Time zero and intended use" p.3 | Users and actions are named. The text does not say whether the score is computed automatically from the record or what expertise users need. |
| 27c | D;E | Next steps for applicability and generalisability | Yes | §6 "Limitations" p.8 | External validation, linkage, annotation study, prospective evaluation. |

**Summary of the 52 sub-items.**

| Status | Sub-items |
|---|---|
| Yes | 29 |
| Partly | 15 |
| No | 3 |
| NA, withheld, or assessed separately | 5 |

Adherence counting Partly as one half is 36.5 of 47 applicable items, or 78%. The three items not reported are 3c (known health inequalities), 12g and 22 (model object or formula).

## TRIPOD+AI for Abstracts

| Item | Topic | Status | Note |
|---|---|---|---|
| 1 | Title | Partly | As item 1 above. |
| 2 | Background and rationale | Yes | |
| 3 | Objectives (development and/or evaluation) | Yes | "developed and temporally validated". |
| 4 | Sources of data | Partly | Hospital and years are named; the record type is not. |
| 5 | Eligibility and setting | Yes | |
| 6 | Outcome and time horizon | Yes | |
| 7 | Model type, building steps, internal validation | Yes | LightGBM; temporal train, calibration and test split. |
| 8 | Performance measures | Yes | AUROC, net benefit and observed-to-expected ratio are named. |
| 9 | Number of participants and outcome events | Partly | Encounters and women are counted; events are not. |
| 10 | Predictors in the final model | Partly | Structured admission data plus text-derived obstetric history. |
| 11 | Performance estimates with confidence intervals | Yes | |
| 12 | Overall interpretation | Yes | |
| 13 | Registration or repository | No | |
