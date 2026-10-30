# Challenges and debugging record

## Challenge: reliable eligibility rather than a plausible ETA
The starter computed a numeric ETA even for inactive/full partners, and missing or malformed numeric inputs were not consistently rejected. The existing test suite exposed this: the untouched archive produced **65 failures and 164 passes**. The core issue was that the eligibility decision had been reduced to whether two route lookups returned non-None values.

## Investigation and fix
I read the implementation and test matrix, tracing `assign_order` into `estimate_eta` and identifying absent checks for partner activity, active-order capacity, finite nonnegative times, and required order identifiers. I added small validators and used them in ETA estimation so the assignment path shares exactly the same eligibility rules. I also ensured bad partner records are rejected instead of raising errors.

## Challenge: idempotent cancellation
The original cancellation function appended `event_id` unconditionally. The duplicate-event tests required a single occurrence, and duplicate events must not release capacity a second time. The fix checks `processed_cancellation_ids` before any workload or order mutation, and operates on a deep copy. A new regression also simulates an old event replayed after an order ID is reused, demonstrating that the repeated event cannot affect the new workload.

## Evidence and next steps
The updated suite ran with **237 passed in 0.15s**; the demo returned P-2 with ETA 35. New tests cover malformed partner entries, missing travel, invalid times, ties, full capacity and cancellation state consistency. No unresolved test failures were observed. A future investigation should focus on concurrent assignments and persisted cancellation deduplication across restarts, neither of which is implemented in this offline dictionary-only engine.

## V3 explainability tradeoff

Human-readable reasons must remain consistent with the underlying eligibility checks. `explainer.py` delegates numeric ETA computation to `service.py` and mirrors the rejection preconditions for labels. If the eligibility contract changes, both modules and the regression tests must be reviewed. Dashboard interactions remain session-local; concurrent or distributed event consistency is outside the offline challenge.
