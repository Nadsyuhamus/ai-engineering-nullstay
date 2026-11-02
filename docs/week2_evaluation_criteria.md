# Evaluation Criteria — NullStay

Version: 1.0 — Initial planning draft

## 1. Evaluation Purpose

NullStay will be evaluated as a historical hotel booking cancellation prediction dashboard.

Evaluation will examine whether its predictions are useful, its probabilities are reliable, its interface is understandable, and its processing is sufficiently fast.

All thresholds are proposed acceptance targets. Results remain pending until the prototype is implemented and tested.

## 2. Evaluation Criteria

| ID | Criterion | Objective / requirement mapping | Evaluation category | Method | Metric and acceptance target | Current outcome |
|---|---|---|---|---|---|---|
| EC1 | Cancellation prediction performance | O2, O3 / FR2, FR4, NFR1 | Domain / predictive quality | Evaluate the selected model on a held-out test set. Select any classification threshold using validation data only. | Average precision, used as the defined PR-AUC measure, must exceed test-set cancellation prevalence by at least 0.05. Also report precision, recall, F1, and a confusion matrix. | Pending |
| EC2 | Probability reliability | O2, O3 / FR2, FR5, NFR2 | Domain / predictive quality | Compare model probabilities with a constant-probability baseline using the training-set cancellation rate. Inspect a calibration plot. | Model Brier score must be lower than the baseline Brier score on the same test records. | Pending |
| EC3 | Explanation clarity and user understanding | O1, O2, O3 / FR3, NFR4, RAI1, RAI3 | User experience / responsible use | Ask five participants to complete the single-booking and batch-export tasks and interpret a prediction. Separately inspect at least 10 explanations. | At least 4 of 5 participants complete both tasks without assistance and correctly explain that a probability is uncertain. All inspected explanations include their limitation and avoid causal or certain wording. | Pending |
| EC4 | Processing speed | O2, O3 / FR2, FR4, NFR3 | Cost / latency — latency component | Measure 30 single predictions and 10 runs using a valid 1,000-row CSV after application and model loading. Document the device and conditions. | Single prediction p90 ≤ 2 seconds; batch processing p90 ≤ 10 seconds. | Pending |
| EC5 | Dashboard and export correctness | O1, O2, O3 / FR1, FR4–FR6, NFR5 | Functional correctness | Compare dashboard values and exports with independent reference calculations across at least five selected cases. Include invalid input cases. | Counts match exactly; cancellation rates differ by no more than 0.01 percentage points; probabilities differ by no more than 0.000001. Invalid inputs receive clear validation messages. | Pending |
| EC6 | Leakage prevention and reproducibility | O3 / NFR6, RAI2 | Engineering quality / responsible evaluation | Audit feature eligibility and preprocessing boundaries. Repeat the documented training workflow twice. | Every retained feature has a documented eligibility decision; no target or known post-outcome fields enter the model. Repeated runs reproduce test identifiers and metrics within 0.0001. | Pending |
| EC7 | Performance across hotel contexts | O3 / RAI4 | Responsible AI / subgroup assessment | Evaluate City Hotel and Resort Hotel separately when each test subset contains at least 100 records and both outcomes. | Report subgroup sample sizes, precision, recall, PR-AUC, and Brier score. A recall difference above 0.15 triggers investigation and documentation. | Pending |
| EC8 | Decision boundaries and upload handling | O2, O3 / RAI1, RAI5 | Responsible AI / privacy | Inspect prediction views, available actions, application storage, and logging using a test upload. | All prediction views communicate uncertainty. No automatic booking cancellation, rejection, or penalty function exists. No application-created uploaded CSV remains after processing; required inputs exclude names, emails, and phone numbers. | Pending |

## 3. Why These Categories Apply

NullStay uses structured booking records and supervised prediction. Its main evaluation categories are predictive quality, functional correctness, user understanding, latency, and Responsible AI.

Generation quality and language-model instruction-following are not applicable to the current design because NullStay does not require a chatbot or generated answers.

Latency is measured directly. Paid model API cost is not currently an acceptance criterion because the planned design uses locally executed classification models. Deployment resource requirements will still be documented.

## 4. Evaluation Protocol

### 4.1 Data and Feature Audit

Before training:

- Confirm the cancellation label and treatment of no-shows.
- Inspect missing values, date coverage, and repeated anonymous rows.
- Document whether each proposed input is available at the intended prediction point.
- Exclude the target and known post-outcome information.
- Confirm dataset access and sharing terms.

### 4.2 Data Separation

Use separate training, validation, and test partitions.

- Use training data to fit preprocessing and models.
- Use validation data to select models, tune settings, and choose a classification threshold.
- Keep the test set untouched until the final evaluation.

Prefer a chronological split when the audited dates and intended prediction point support it. Document the final split and its limitations. An arrival-date split alone does not prove that all outcomes were available when a real model would have been trained.

### 4.3 Model Comparison

Compare:

1. A constant-probability baseline using the training-set cancellation rate.
2. Logistic regression.
3. One suitable tree-based model.

Use the same partitions and eligible information for comparable evaluation. Select the final model using validation evidence.

### 4.4 Final Evaluation

Evaluate the selected model once on the held-out test set for the reported final comparison.

Record:

- Dataset version and partition sizes.
- Cancellation prevalence in each partition.
- Feature list and preprocessing configuration.
- Model settings and classification threshold.
- Predictive, calibration, subgroup, and processing-time results.
- Failed criteria and their implications.

If test results lead to further development, disclose that the test set has influenced development. Do not continue describing repeated test-based adjustments as an untouched final evaluation.

### 4.5 Interface Evaluation

Use predefined tasks:

1. Filter historical bookings and identify a cancellation rate.
2. Enter a valid booking and interpret its predicted probability.
3. Explain what a model-influential input means.
4. Upload a compatible CSV and export results.
5. Identify and correct an invalid input.

Record task completion, assistance required, misunderstandings, and participant experience.

The formal NFR4 acceptance target applies to the single-booking and batch-export tasks and the probability interpretation question. The other tasks provide additional formative feedback.

## 5. Interpreting Outcomes

Each criterion will receive a status:

- **Pass:** Its defined acceptance conditions are met.
- **Fail:** One or more acceptance conditions are not met.
- **Not evaluated:** Required implementation or evidence is unavailable.

For EC7, completing the subgroup assessment does not establish fairness. A recall gap above 0.15 requires investigation; a smaller gap does not prove the absence of bias.

A model that fails predictive or calibration targets will be reported honestly. The dashboard may still demonstrate a working workflow, but the project must not claim that its predictions are suitable for operational use.

## 6. Reflection: Most Important Criterion

Probability reliability is especially important for NullStay because the dashboard displays cancellation probabilities and adds them to estimate batch cancellations.

A model can rank bookings reasonably well while producing misleading probabilities. For example, consistently overstating risk could inflate the estimated number of cancellations.

Therefore, EC2 checks whether model probabilities improve on a simple historical-rate baseline. Explanation clarity remains essential so users understand that these probabilities are uncertain estimates and do not justify automatically cancelling or rejecting bookings.

## 7. Evaluation Limitations

- Historical Portuguese hotel data may not represent current hotels or Malaysian operations.
- A retrospective dataset may not establish genuine booking-time prediction performance.
- Five participants provide formative usability feedback, not population-wide evidence.
- Hotel-type comparisons do not establish demographic fairness.
- Meeting these criteria does not demonstrate increased revenue or reduced cancellations.