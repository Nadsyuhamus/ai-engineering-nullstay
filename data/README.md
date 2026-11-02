## Review of More Recent Data Sources

The  Hotel Booking Demand dataset is the provisional primary dataset. The alternatives below are recorded for comparison and possible future validation.

A dataset's upload date is not treated as its booking-record date.

| Candidate | Booking Period / Data Type | Access Status | Proposed Role |
| --- | --- | --- | --- |
| Hotel Reservations Classification Dataset | Reported booking years: 2017–2018; requires verification against the downloaded file | Kaggle listing available; file contents, provenance, and permissions not yet audited | Possible additional historical evaluation dataset |
| Lakeside Booking Cancellations | Synthetic dataset published as a 2026 competition; not verified real-world booking records | Competition listing available; access and rules require review | Optional pipeline or interface testing only |
| Data used in “The Effect of Pricing on Cancelations at a High-End U.S. Hotel” | The study reports real reservation records covering 2022–2023 | Public downloadable dataset not yet verified | Candidate for future validation if access and permission can be obtained |

### Source Links

- **Hotel Reservations Classification Dataset:**  
  https://www.kaggle.com/datasets/ahsan81/hotel-reservations-classification-dataset

- **Lakeside Booking Cancellations:**  
  https://www.kaggle.com/competitions/lakeside-booking-cancellations/overview

- **The Effect of Pricing on Cancelations at a High-End U.S. Hotel:**  
  https://doi.org/10.1177/19389655261455346

### Conditions for Adoption

Before using an alternative dataset:

1. Verify actual booking dates, provenance, and whether records are real or synthetic.
2. Check access, licensing, and redistribution conditions.
3. Inspect the cancellation-label definition and available input fields.
4. Check which fields would have been available at the intended prediction time.
5. Document schema differences before comparing or combining datasets.

Synthetic data will not be used to claim performance on contemporary real-world bookings. No alternative dataset has been adopted, and no cross-dataset validation has been completed.