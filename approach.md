# Engineering approach

## Baseline and trace
I extracted the provided archive, inspected `models.py`, `service.py`, `fixtures.py`, `main.py` and the existing tests. Before modifying the implementation, I ran `python -m pytest -q` on an untouched extraction: **65 failed, 164 passed in 0.45s**, using Python 3.13.5. The original `service.py` computed route legs but did not check active status, capacity or negative/invalid times, and recorded cancellation IDs without first checking for duplicates.

## Data flow
`new_order` and `new_partner` produce dictionaries. `estimate_eta` looks up `(partner zone, restaurant)` and `(restaurant, destination)` travel legs, computes pickup as `now + leg1 + workload`, and ETA as `pickup + prep + leg2`. `assign_order` evaluates partners, discards ineligible estimates and minimizes `(ETA, partner_id)` to guarantee a stable tie break. `process_cancellation` returns a deep-copied partner state and releases the recorded order's workload at most once per event ID.

## Fixes and decisions
I introduced shared validation helpers for non-negative finite numeric minutes and nonempty string identifiers. `bool` is rejected even though Python considers it an `int`. Capacity must be a positive integer, partner must be explicitly active, and active-order count must be below capacity. Missing routes and malformed partner records are ineligible. I kept the existing public function signatures and dictionary output shapes to preserve compatibility. Invalid orders return a specific reason, while exhausted candidate sets return the existing generic no-eligible reason. Duplicate cancellation IDs return unchanged copied state.

I chose simple O(n) candidate scanning and `min` over sorting all candidates (O(n log n)). No live route API, database or GUI was added because the contract is synthetic/offline.

## Verification and limits
I added eight tests in `tests/test_regressions_added.py`, preserving all existing tests. After changes `python -m pytest -q` reported **237 passed in 0.15s**. `python -m main` selected P-2 with pickup 13 and ETA 35. The implementation does not persist assignment reservations or handle concurrent writers; those belong to a future transactional layer.

## V3 design decision

Keep `estimate_eta`, `assign_order`, and `process_cancellation` contracts unchanged. Add `explainer.py` as a read-only adapter around them so explanations cannot silently change the assignment decision. The deterministic inspector order is based on candidate ID and does not claim that the incoming partner ordering represents priority.
