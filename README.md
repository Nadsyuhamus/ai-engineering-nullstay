# NullStay

### An Explainable Hotel Booking Cancellation Prediction Dashboard

## Overview

NullStay is a proposed dashboard for hotel reservation staff and revenue-management analysts.

It will estimate cancellation likelihood from selected booking-related information, display model explanations, and summarise predictions across booking records.

## Project Status

Planning stage. Dataset inspection, implementation, and evaluation have not yet been completed.

## Problem Statement

Booking cancellations create uncertainty about how many reservations will become actual stays. This complicates demand forecasting and operational planning.

NullStay will investigate whether a bounded dashboard can provide useful cancellation estimates and understandable outputs using historical booking data.

## Evidence of the Problem

- António et al. (2017) investigate cancellation-related forecasting uncertainty using data from four resort hotels.  
  [Read the paper](https://doi.org/10.18089/tms.2017.13203)

- António et al. (2019) describe an automated cancellation-prediction decision-support system. Its outcomes are not assumed to apply to NullStay.  
  [Read the paper](https://doi.org/10.5334/dsj-2019-032)

## Existing Approaches

1. **Historical cancellation rates:** Provide a simple planning baseline, but do not distinguish individual bookings.
2. **Booking-level predictive models:** Estimate outcomes using booking characteristics; performance depends on data and evaluation conditions.
3. **Automated decision-support systems:** Integrate predictions into operational workflows; their business impact requires operational evaluation.

## Engineering Gap

NullStay will investigate a manageable workflow combining validated booking inputs, cancellation estimates, model explanations, and aggregate summaries.

It does not claim algorithmic novelty or superiority over existing commercial products.

## Project Objectives

- O1: Design the dashboard, supported tasks, and decision-support boundaries.
- O2: Develop the data pipeline, prediction capability, explanations, and batch outputs.
- O3: Evaluate predictive performance, probability calibration, and processing speed against baselines.

## Target Users

Hotel reservation staff and revenue-management analysts.

## Planned Features

- Historical booking exploration.
- Single-booking cancellation estimates.
- Compatible CSV batch scoring.
- Model explanations and aggregate summaries.
- Exportable predictions for human review.

## Boundaries

The application will not automatically cancel reservations, reject guests, impose penalties, set prices, or determine overbooking levels.

Model explanations describe associations, not proven causes.

The final feature set and prediction timing remain subject to data inspection.

## Data Sources

- [Hotel Booking Demand — Kaggle](https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand)
- [Original dataset publication](https://doi.org/10.1016/j.dib.2018.11.126)
- [Supporting data dictionary](https://github.com/rfordatascience/tidytuesday/tree/main/data/2020/2020-02-11)

These references describe the same underlying dataset. NullStay will follow the tabular-data preparation pathway.

## Week 1 Deliverables

- [Group observation sheet](docs/week1_observation_sheet.md)
- [Group conceptual system map](docs/week1_system_map.png)
- [Individual project scope](docs/week1_project_scope.md)

The observation sheet and system map analyse the group's assigned existing system. They are separate from the individual NullStay project.

## Repository Structure

| Folder | Purpose |
| --- | --- |
| `docs/` | Scope, requirements, architecture, and engineering evidence |
| `data/` | Dataset documentation and permitted data artefacts |
| `src/` | Application source code |
| `notebooks/` | Data-processing and development notebooks |
| `evaluation/` | Evaluation procedures and results |
| `prompts/` | Reserved folder from the lab structure; no chatbot is planned |

## Weekly Progress

| Week | Focus | Deliverable | Status |
| --- | --- | --- | --- |
| 1 | Group evidence and individual project framing | Observation sheet, system map, scope, README | Initial documentation prepared; data audit pending |
| 2 | Requirements, evaluation, and V1 architecture | Requirements, criteria, diagram, decisions | Not started |
| 3 | Tabular data engineering | Notebook, inventory, pipeline, prepared outputs | Not started |