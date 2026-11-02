# Requirements Specification — NullStay

Version: 1.0 — Initial planning draft  
Project: NullStay  
Domain: Hospitality and Business Analytics

## 0. Project Context

### Problem

Hotel booking cancellations create uncertainty when hotel staff estimate occupancy and plan operations. Historical cancellation percentages can describe past patterns, but they do not distinguish the cancellation risk of individual bookings.

Research has investigated machine learning for predicting hotel booking cancellations and incorporating predictions into decision-support systems. This provides evidence for the problem and the feasibility of the approach, rather than a guarantee that NullStay will achieve similar results.

### Supporting Evidence

1. António, de Almeida, and Nunes (2017), *Predicting hotel booking cancellations to decrease uncertainty and increase revenue*, examines cancellation prediction using records from four resort hotels.  
   https://doi.org/10.18089/tms.2017.13203

2. António, de Almeida, and Nunes (2019), *An Automated Machine Learning Based Decision Support System to Predict Hotel Booking Cancellations*, investigates an operational decision-support approach.  
   https://doi.org/10.5334/dsj-2019-032

### Existing Approaches and Engineering Gap

| Approach | Strength | Limitation relevant to NullStay |
|---|---|---|
| Historical cancellation rate | Simple, inexpensive benchmark for estimating cancellation frequency. | An overall rate assigns the same estimate to bookings with different characteristics. |
| Supervised classification models | Can learn relationships between booking characteristics and cancellation outcomes. | Predictive performance alone does not establish whether probabilities are calibrated or useful to staff. |
| Automated cancellation decision-support systems | Demonstrate how predictions can support hotel operations. | Published performance and operational outcomes require validation before being applied to another dataset or setting. |

NullStay will combine historical exploration, booking-level predictions, and model explanations in a small dashboard. Its contribution is an implemented and evaluated student prototype, rather than a claim to invent a new prediction method.

### Objectives

- **O1:** Design a dashboard that helps hotel staff explore historical booking and cancellation patterns.
- **O2:** Develop a cancellation prediction workflow that provides estimated probabilities and understandable explanations.
- **O3:** Evaluate predictive performance, probability quality, usability, and processing speed against defined baselines and criteria.

### Intended Users

Hotel reservation staff and revenue-management analysts.

### Main User Tasks

- Explore cancellation patterns in historical bookings.
- Estimate the cancellation probability of a booking.
- Upload compatible booking records for batch prediction.
- Review model-influential inputs and export batch results.

### Data and Operating Context

The initial candidate is the Hotel Booking Demand dataset, containing historical records from two hotels in Portugal during 2015–2017.

Dataset publication: https://doi.org/10.1016/j.dib.2018.11.126  
Dataset access: https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand

Newer candidate sources are recorded separately in `data/README.md`. Their inclusion depends on access, provenance, compatibility, and data-quality checks.

The intended prediction point is when a booking is entered or confirmed. However, the historical dataset does not provide complete booking-time snapshots. Feature eligibility and the precise prediction claim must therefore be confirmed during the data audit.

### Constraints

- The project uses a limited historical dataset and student computing resources.
- The prototype must not claim that results generalise to current hotels or Malaysian hotels without further validation.
- Model inputs must be reviewed for whether they would be available at the intended prediction time.
- Data preparation and model selection must avoid using the final test set.
- Dataset access and redistribution must follow the applicable source terms.

### Out of Scope

- Automatically cancelling bookings or rejecting guests.
- Automatically changing room prices or making overbooking decisions.
- Guaranteeing revenue improvements or preventing cancellations.
- Live integration with hotel reservation systems.
- Treating model explanations as proof of why a guest will cancel.

### Evaluation Focus

Evaluation will cover predictive discrimination, calibration, dashboard task completion, processing speed, and compliance with data and decision boundaries.

## 1. Functional Requirements

| ID | Requirement | Acceptance check |
|---|---|---|
| FR1 | The system shall display historical booking counts, cancellation counts, and cancellation rates, with filters for hotel type and arrival period. | Compare displayed values with independently calculated values for selected filters. |
| FR2 | The system shall accept one booking through a form, validate the supported inputs, and return an estimated cancellation probability. | A valid booking produces a probability from 0 to 1; invalid inputs produce a clear validation message. |
| FR3 | The system shall display up to three model-influential inputs alongside a prediction and explain that they describe model associations. | Inspect example predictions for consistent inputs, readable labels, and an explanation of their limits. |
| FR4 | The system shall accept a CSV with the required booking fields and produce a probability for each valid row while identifying invalid rows. | Test a valid file, a file with missing required columns, and a file containing invalid rows. |
| FR5 | The system shall display batch booking counts and the sum of predicted probabilities as the estimated number of cancellations. | Compare the dashboard total with the sum of exported probabilities; confirm it is labelled as an estimate. |
| FR6 | The system shall allow users to download batch results containing a row identifier, prediction or validation status, and cancellation probability where available. | Download and inspect results for correct row alignment, values, and validation messages. |

## 2. Non-Functional Requirements

The thresholds below are initial targets. They will be reviewed after the data audit and baseline evaluation. Any revision will be documented with its justification.

| ID | Quality attribute | Requirement and target | Measurement method |
|---|---|---|---|
| NFR1 | Predictive discrimination | On a held-out test set, the selected model shall achieve PR-AUC at least 0.05 above the test-set cancellation prevalence. | Calculate average precision as the defined PR-AUC measure. Also report precision, recall, F1, and the confusion matrix at a threshold selected using validation data. |
| NFR2 | Probability quality | The selected model shall achieve a lower Brier score than a constant-probability baseline using the training-set cancellation rate. | Compare both on the same held-out test records and inspect a calibration plot. |
| NFR3 | Response time | After application and model loading, the system shall return a single prediction within 2 seconds at the 90th percentile and process a valid 1,000-row CSV within 10 seconds at the 90th percentile. | Time 30 single predictions and 10 batch runs; record the device and test conditions. |
| NFR4 | Usability and clarity | At least 4 of 5 formative test participants shall complete the single-booking and batch-export tasks without assistance and correctly interpret a probability as uncertain. | Observe task completion and ask a short interpretation question. Record participants’ relevant experience. |
| NFR5 | Numerical correctness | Dashboard aggregates and exported predictions shall match reference calculations across at least five selected test cases. | Compare counts exactly, cancellation rates within 0.01 percentage points, and exported probabilities within 0.000001 of model outputs. |
| NFR6 | Reproducibility | Re-running the documented training workflow with the same data, configuration, seed, and environment shall reproduce the same test split and evaluation metrics within 0.0001. | Compare split identifiers and metrics from two runs. |

The usability study is a small formative evaluation. It does not establish usability for all hotel staff.

## 3. Responsible AI Requirements

| ID | Risk | Requirement | Evaluation method and acceptance target |
|---|---|---|---|
| RAI1 | Overreliance on predictions | The system shall present predictions as uncertain estimates and shall not automatically cancel, reject, or penalise bookings. | Inspect all prediction views for uncertainty wording and confirm that no automated booking-action function exists. |
| RAI2 | Leakage and misleading performance | The training workflow shall exclude the target and known post-outcome fields from model inputs. It shall document the eligibility of every retained feature. | Audit 100% of retained features. Exclude `reservation_status` and `reservation_status_date`; exclude other fields whose availability at the prediction point cannot be justified. Fit learned preprocessing only on training data. |
| RAI3 | Misleading explanations | The system shall describe explanations as model influences, rather than causes or guarantees about a guest’s behaviour. | Inspect at least 10 example explanations. All must include the explanation limitation and avoid causal or certain wording. |
| RAI4 | Uneven performance across hotel contexts | Evaluation shall report performance separately for City Hotel and Resort Hotel when each test subset contains at least 100 records and both outcomes. | Report subgroup sample sizes, precision, recall, PR-AUC, and Brier score. Flag a recall difference above 0.15 for investigation; do not describe smaller gaps as proof of fairness. |
| RAI5 | Exposure of uploaded booking data | The prototype shall not intentionally persist uploaded CSV contents or include guest-identifying fields in its required input schema. | Inspect storage and logging behaviour using a test upload. Confirm that no application-created upload file remains after processing and that required fields exclude names, emails, and phone numbers. |

Subgroup checks assess performance across the two available hotel contexts. They do not establish fairness across demographic groups.

## 4. Data Audit Decisions Still Required

Before modelling, confirm:

1. The target definition, including how no-shows are represented.
2. Missing-value meanings and valid input ranges.
3. Which fields are available at the intended prediction point.
4. Whether dates support an appropriate chronological evaluation.
5. How repeated anonymous rows should be handled without assuming they are duplicate bookings.
6. Whether source terms permit the planned storage and sharing.
7. Whether newer candidate data are suitable for any additional evaluation.

If genuine booking-time availability cannot be established, NullStay will explicitly describe its predictions as a retrospective historical prototype.

## 5. Initial Traceability

| Objective | Supporting requirements |
|---|---|
| O1: Explore historical patterns | FR1, NFR4, NFR5 |
| O2: Provide predictions and explanations | FR2–FR6, NFR2, RAI1, RAI3, RAI5 |
| O3: Evaluate the prototype | NFR1–NFR6, RAI2, RAI4 |

## 6. Current Status

This document specifies planned behaviour and proposed acceptance criteria. No model performance, processing-time result, or user-test outcome has been established yet.