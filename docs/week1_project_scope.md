# Week 1 Project Scope — NullStay

## Project Name

NullStay: An Explainable Hotel Booking Cancellation Prediction Dashboard

## Domain

Hospitality / Business Analytics / Machine Learning

## Problem Statement

Hotel booking cancellations create uncertainty about how many reservations will become actual stays. This complicates demand forecasting and operational planning.

NullStay proposes a dashboard that estimates cancellation likelihood from selected booking-related information. It will display predictions, model explanations, and summaries across booking records to support human review.

The initial application will be developed and evaluated using historical data. It will not automatically cancel reservations, reject guests, or guarantee revenue improvements.

## Evidence of the Problem

### Evidence 1 — Cancellations affect demand forecasting

António, de Almeida, and Nunes (2017) explain that booking cancellations affect demand forecasts and hotel management decisions. Their study investigates cancellation prediction using data from four resort hotels.

This supports cancellation prediction as a practical problem. The study's reported performance does not establish what NullStay will achieve.

Reference:
António, N., de Almeida, A., & Nunes, L. (2017).
Predicting hotel booking cancellations to decrease uncertainty and increase revenue.
Tourism & Management Studies, 13(2), 25–39.

https://doi.org/10.18089/tms.2017.13203

### Evidence 2 — Cancellation prediction has been studied in operational decision support

António, de Almeida, and Nunes (2019) describe an automated machine-learning decision-support system for predicting hotel booking cancellations.

This supports the operational relevance of the task. Its outcomes cannot be assumed to apply to a new student prototype or different hotels.

Reference:
António, N., de Almeida, A., & Nunes, L. (2019).
An Automated Machine Learning Based Decision Support System to Predict Hotel Booking Cancellations.
Data Science Journal, 18, 32.

https://doi.org/10.5334/dsj-2019-032

## Initial Related Work / Existing Approaches

### 1. Historical cancellation-rate forecasting

Approach:
Estimate future cancellations using historical cancellation rates, either overall or for selected booking segments.

Key limitation:
A single average does not distinguish bookings with different characteristics. Segment averages can also be unreliable when few historical examples are available.

Relevance to NullStay:
Use historical cancellation rates as a simple baseline against which predictive models can be evaluated.

Source for the broader forecasting context:
https://doi.org/10.18089/tms.2017.13203

### 2. Booking-level machine-learning prediction

Study:
António et al. (2017) investigate hotel cancellation prediction as a classification task.

Key limitation:
Published performance depends on the hotels, available inputs, prediction timing, and evaluation procedure. It cannot be transferred directly to NullStay.

Relevance to NullStay:
Compare manageable models using a documented feature set and held-out data.

Source:
https://doi.org/10.18089/tms.2017.13203

### 3. Automated cancellation decision-support systems

Study:
António et al. (2019) describe an automated machine-learning decision-support system.

Key limitation:
An operational system requires integration, monitoring, and evaluation beyond an offline prototype. Predictive performance alone does not establish business benefit.

Relevance to NullStay:
Develop a bounded dashboard and evaluate its prediction quality, probability calibration, explanation presentation, and processing speed.

Source:
https://doi.org/10.5334/dsj-2019-032

## Engineering Gap / Limitation

Hotel cancellation prediction and decision-support systems already exist. NullStay does not claim a new algorithm or that existing products lack this capability.

The project will investigate whether a manageable dashboard can combine cancellation estimates, understandable model explanations, and aggregate booking summaries through a transparent and repeatable workflow.

The engineering challenges include validating inputs, avoiding future-information leakage, assessing probability quality, and communicating the limits of historical data.

## Initial Project Objectives

- O1: To design a hotel booking cancellation prediction dashboard with defined users, supported tasks, inputs, outputs, and decision-support boundaries.
- O2: To develop the dashboard with a validated tabular-data pipeline, cancellation prediction, model explanations, and batch results.
- O3: To evaluate predictive performance, probability calibration, and processing speed against simple baselines using held-out data.

## Target Users

Hotel reservation staff and revenue-management analysts.

These are intended users. Their needs and the prototype's usability have not yet been validated through user testing.

## Initial Supported Tasks

- Explore historical booking and cancellation patterns.
- Estimate cancellation likelihood for a compatible booking record.
- Score multiple booking records uploaded as a compatible CSV.
- Display inputs that influenced the model's prediction.
- Summarise estimated cancellations across a supplied batch.
- Export predictions for human review.

Model explanations will describe associations within the fitted model, not proven causes of cancellation.

## Initial Prediction Task

Predict the outcome represented by the dataset's `is_canceled` label using a justified subset of booking-related fields.

The intended interface will use information available when a reservation is entered or confirmed. However, the historical dataset is not a complete booking-time event log.

The final prediction point, feature set, and treatment of no-show records will be determined through inspection of the downloaded dataset and its documentation.

Fields revealing the final outcome or later events will not be used as prediction inputs. Any remaining uncertainty about feature availability will be recorded.

## Data Sources

### Primary Application Dataset

Hotel Booking Demand, published by António, de Almeida, and Nunes (2019), accessed through the Kaggle distribution.

The original publication describes 40,060 resort-hotel bookings and 79,330 city-hotel bookings. These are commonly distributed as one combined CSV with a hotel identifier.

These are published counts, not locally verified inspection results.

Kaggle:
https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand

Original dataset publication:
António, N., de Almeida, A., & Nunes, L. (2019).
Hotel booking demand datasets.
Data in Brief, 22, 41–49.

https://doi.org/10.1016/j.dib.2018.11.126

### Supporting Data Dictionary

The published field definitions and distribution documentation will support schema interpretation and feature-availability review.

https://github.com/rfordatascience/tidytuesday/tree/main/data/2020/2020-02-11

### Data-Source Clarification

The Kaggle CSV, original publication, and data dictionary refer to the same underlying application dataset. They are not three independent training datasets.

NullStay follows Topic 3's tabular-data pathway, which permits one justified dataset. This differs from the original Topic 1 document-based project framing and will be identified transparently.

Dataset version, access date, licence, file fingerprint, missing values, repeated rows, and field definitions will be recorded during the data audit.

## Similar to EduAdvisor

Both applications require clear requirements, traceable data, data preparation, evaluation, and Responsible AI controls.

Both must communicate limitations and be evaluated against defined acceptance criteria.

## Different from EduAdvisor

NullStay uses structured hotel booking records to predict cancellation outcomes and display results through a dashboard.

It does not provide conversational answers from retrieved documents. Evaluation will focus on classification performance, probability calibration, processing speed, and understandable presentation.

## Responsible AI Risks

### Risk 1 — Incorrect predictions and overreliance

Potential harm:
Staff may overestimate or underestimate cancellation risk and make unsuitable planning decisions. A genuine guest could be treated unfairly after a false high-risk prediction.

Proposed control:
Retain human review and communicate limitations. Do not automatically cancel bookings, reject guests, impose penalties, or determine overbooking levels.

### Risk 2 — Information leakage

Potential harm:
Using final reservation status or later information could produce misleadingly strong evaluation results.

Proposed control:
Audit feature availability, document excluded inputs, and keep final test data separate from model development.

### Risk 3 — Unequal model performance

Potential harm:
Errors may differ between hotel types or booking segments.

Proposed control:
Evaluate sufficiently represented segments separately, report sample sizes and uncertainty, and avoid using predictions as justification for discriminatory treatment.

### Risk 4 — Privacy in uploaded files

Potential harm:
User-uploaded booking records may contain names, contact details, or other unnecessary personal information.

Proposed control:
Define an accepted input schema, reject unnecessary identifying fields, and avoid retaining uploaded records by default.

### Risk 5 — Historical-data limitations

Potential harm:
Users may assume that performance on older records from two hotels applies to current hotels in other locations.

Proposed control:
Describe NullStay as a historical-data prototype. Require new validation before operational use.

## Current Status

Initial project framing completed.

Dataset inspection, feature confirmation, model development, and evaluation are pending. No predictive performance or operational benefit is claimed.