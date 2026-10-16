# PROBAST+AI self-assessment

Manuscript: *Predicting 30-Day Emergency or Inpatient Return After Encounters at a Brazilian Public High-Risk Maternity Hospital: Temporal Validation, Triage-Text Enrichment and a Leakage Audit* (ACM SAC 2027, ASH track, double-blind submission).

Tool: PROBAST+AI, Moons et al., BMJ 2025;388:e082505, Table 2. Signalling questions are numbered as in that table and answered Y (yes), PY (probably yes), PN (probably no), N (no), NI (no information) or NA (not applicable). Domain and overall judgements are low, high or unclear.

This is a self-assessment by the authors. PROBAST+AI is designed for appraisers independent of the study, so these judgements are the authors' reading of their own report, made to be checked rather than trusted.

## Step 1. Intended use (PICOTS)

| Element | This study |
|---|---|
| Population | Every encounter of a pregnant or postpartum woman (emergency, inpatient or outpatient) at one public high-risk maternity hospital, 2017–2023. Each encounter is an index encounter. |
| Index model | LightGBM on 32 predictors restricted to admission-time information, including obstetric history extracted by rules from the triage fields, with isotonic recalibration. |
| Comparator | L2-regularised logistic regression, pre-specified as reference. Other learners are secondary. |
| Outcome | Emergency or inpatient encounter at the same hospital within 30 days after discharge. |
| Timing | Score computed at the end of an emergency visit or at discharge, using admission-time predictors; 30-day horizon. |
| Setting and use | Discharge team of a public referral maternity hospital deciding who receives an early contact. |

## Step 2. Type of study

The study covers model development and internal validation by temporal split: training 2017–2020, calibration and model selection 2021, test 2022–2023. Both parts of PROBAST+AI are completed for the primary model. Domains 1–3 are shared by the logistic-regression reference, which uses the same data, predictors and outcome.

The "episode" variant of the leakage audit is an experimental contrast, not a model intended for use, and is not assessed.

## Step 3a. Model development: quality and applicability

### Domain 1. Participants and data sources

| Question | Answer | Support |
|---|---|---|
| 1.1 Appropriate data sources? | Y | Routine electronic health record covering all encounters of the hospital over six years (§3.1). |
| 1.2 Appropriate study design? | Y | Retrospective cohort of all consecutive eligible encounters (§3.1). |
| 1.3 Inclusions and exclusions result in a representative dataset? | PY | Only newborn encounters and 25 encounters without an outcome record are excluded. Repeated encounters of the same woman are kept and their dependence is analysed (§3.1, §5.6). |

**Quality concern: low.**

**Applicability concern: unclear.** The intended population is not fixed in one place. §4.2 covers discharges from the emergency unit or a ward. §6 restricts use to antepartum women leaving the emergency unit and advises against use after delivery admissions. The development cohort includes both groups and a small share of outpatient encounters.

### Domain 2. Predictors

| Question | Answer | Support |
|---|---|---|
| 2.1 Defined and assessed in a similar way for all participants? | PN | The obstetric block can be read only where the triage fields exist, which is in emergency encounters. For inpatient and outpatient encounters it is absent and imputed, so the same predictor is measured in one sector and imputed in the other. The triage colour is recorded only in the emergency sector, and documentation coverage drifts over the years (§4.1, §4.3, §5.1). The extraction rules themselves are deterministic and identical for all encounters. |
| 2.2 Preprocessing similar for all participants? | Y | One pipeline, fitted on the training partition, is applied to all encounters (§4.3). |
| 2.3 Assessed without knowledge of outcome data? | PY | Predictors are recorded during the index encounter, before the follow-up window opens. The triage fields are not versioned, and 2.4% of triage texts contain a term describing a later event (§4.1). |
| 2.4 Available at the time the model is intended to be used? | NI | Age, sector, origin, month, triage colour, race, marital status and the triage text exist at admission. The text does not state when the principal ICD-10 code is recorded. If it is recorded at admission or at discharge, the answer is Y for use at discharge. If it is assigned after discharge, for example when the claim is closed, the answer is N. |

**Quality concern: unclear.** The rating rests on question 2.4, which is pending the principal-code timing, and on question 2.1, because the accuracy of the extractor was not measured.

**Applicability concern: unclear,** for the same reason as question 2.4.

### Domain 3. Outcome

| Question | Answer | Support |
|---|---|---|
| 3.1 Defined and assessed appropriately? | PN | The rule is objective and pre-specified. It also counts scheduled admissions and expected deliveries. Among visible returns, 49% include a delivery and 39% begin with a direct admission, and one third of positive returns carry no obstetric code (§3.2, Table 3). The outcome measures recontact with this hospital, not failed care transition. |
| 3.2 Defined and assessed in a similar way for all participants? | PN | The same rule applies to all, but returns to other services are invisible. Ascertainment therefore depends on residence, since 65% of encounters are of women living outside the host municipality (§3.1, §6). |
| 3.3 Assessed without use or knowledge of predictor data? | Y | The outcome is derived automatically from later encounter records. |
| 3.4 Appropriate interval between predictor and outcome assessment? | Y | 30 days after discharge. Index encounters with a truncated window are excluded from the test set (§3.2). |

**Quality concern: unclear.** The definition is transparent and decomposed, and narrower outcomes are refitted in Table 3. Misclassification of returns elsewhere is plausible and of unknown direction with respect to the predictors.

**Applicability concern: high** relative to the motivation in §1, a failed care transition, because the outcome includes planned returns and deliveries. Applicability is lower if the stated construct is recontact for emergency or inpatient care, as in §3.2.

### Domain 4. Analysis (development)

| Question | Answer | Support |
|---|---|---|
| 4.1 Evidence that the sample size was reasonable? | Y | 11,881 training events for 85 model columns (§4.5). The phase-specific delivery-admission model has far fewer events, and its training count is not reported. |
| 4.2 Continuous and categorical predictors handled appropriately? | Y | Continuous predictors are kept continuous; categorical predictors are one-hot encoded or handled natively by CatBoost (§4.3). |
| 4.3 Missing or censored data handled appropriately? | PY | Chained-equation imputation of predictors only, fitted on training, without the outcome. It is single imputation. Mode imputation assigns inpatients the emergency modal colour, and an explicit "not triaged" level is tested. Training windows are complete (§4.3, §5.6). |
| 4.4 If class-imbalance methods were used, was the model recalibrated? | NA | No imbalance methods were used (§4.3). |
| 4.5 Methods used to address overfitting? | Y | Regularisation, hyperparameter search with time-series cross-validation inside the training years, and model selection on the validation year (§4.3–4.4). |

**Quality concern: low.**

### Overall, development

| Judgement | Rating | Reason |
|---|---|---|
| Quality | **unclear** | Domains 2 and 3 are unclear. |
| Applicability | **high** | Domain 3 is high. |

## Step 3b. Model evaluation (temporal validation, 2022–2023): risk of bias and applicability

Domains 1–3 use the same data source, predictors and outcome, so their answers are carried over from Step 3a. The one addition is in domain 1: the 839 index encounters with a truncated window are excluded from the test set, and their effect is reported (§5.6). Question 1.3 stays PY.

| Domain | Risk of bias | Applicability |
|---|---|---|
| 1. Participants and data sources | low | unclear |
| 2. Predictors | unclear | unclear |
| 3. Outcome | unclear | high |

### Domain 4. Analysis (evaluation)

| Question | Answer | Support |
|---|---|---|
| 4.1 Evaluation based only on apparent performance avoided? | Y | Temporal hold-out. No modelling decision used the test partition (§4.2). |
| 4.2 Evidence that the sample size was reasonable? | Y | 2,481 events and 9,016 non-events (§4.5). The delivery-admission stratum has 210 events, and its intervals are wide (Table 4C). |
| 4.3 Missing or censored data handled appropriately? | PY | Test values are imputed with the training-fitted imputer. Truncated windows are excluded and analysed in a sensitivity analysis. |
| 4.4 If imbalance was corrected, evaluation done without correction? | NA | No correction. |
| 4.5 If data were split, evidence that data leakage was avoided? | PY | Partitions are temporal, and all preprocessing, tuning and calibration are fitted before the test years. 14% of test encounters are of women seen in training, and those encounters are ranked worse, not better (§5.6). The triage fields are not versioned (Domain 2). |
| 4.6 If resampling was used, were all development steps replicated? | NA for the primary estimate; PN for the rolling-origin analysis | The primary estimate is a hold-out, and bootstrap intervals resample a fixed model's predictions. The rolling-origin folds reuse hyperparameters selected on overlapping years, which §6 acknowledges. |
| 4.7 Performance evaluated appropriately (calibration, discrimination, net benefit)? | Y | AUROC, AUPRC, Brier, ECE, slope, intercept, calibration plot and decision curves (§4.4, Figure 1). Pooled discrimination is split into within-stratum and between-stratum pairs, and calibration is reported by phase (Table 4B, §5.4). |

**Risk of bias: low.**

### Overall, evaluation

| Judgement | Rating | Reason |
|---|---|---|
| Risk of bias | **unclear** | Domains 2 and 3 are unclear. |
| Applicability | **high** | Domain 3 is high. |

## What would change these ratings

| Domain | Change |
|---|---|
| 2 | Stating when the principal ICD-10 code is recorded resolves question 2.4. At admission or discharge it becomes Y; after discharge, the code must leave the model. |
| 2 | A manual annotation sample of the extractor, with precision and recall by sector, supports question 2.1. |
| 1 | Fixing a single intended population and reporting performance in it moves domain 1 applicability from unclear to low for that population. The antepartum emergency-unit subgroup is the candidate. |
| 3 | No analysis can remove planned returns from an administrative recontact outcome. Stating the outcome as recontact throughout, and keeping the narrower refits of Table 3, is what moves domain 3 applicability toward unclear. |
