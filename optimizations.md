# Optimizations and trade-offs

## Objective
Correctness and maintainability were the priorities. The starter had incomplete eligibility and cancellation handling, evidenced by 65 failing tests in the untouched archive.

## Options considered
- **Scan and choose minimum:** linear in partner count and straightforward for offline synthetic data; implemented.
- **Sort all candidates:** would add O(n log n) sorting without changing the chosen result; rejected.
- **Cache route legs:** could reduce repeated lookup overhead for huge batches, but Python dictionary lookup is already efficient and cache invalidation would add complexity; not implemented.
- **Persist reservations/events:** needed for concurrent production systems but outside the supplied stateless interface; not implemented.

## Implemented improvements
Shared `_minutes`, `_identifier`, `_valid_order` and `_valid_partner` helpers centralize checks and reduce disagreement between direct ETA calculation and assignment. A single `min` over eligible candidates provides deterministic selection. Cancellation copies state and short-circuits duplicate events. Eight regression tests guard the behaviours.

## Evidence and trade-offs
The untouched archive ran **164 passes and 65 failures in 0.45s**; after changes, the suite ran **237 passes in 0.15s** on Python 3.13.5. These are different test outcomes and workloads, **not a controlled performance benchmark**, so no speedup is claimed. Deep copying cancellation state costs memory but prevents unexpected caller-side mutation. Keeping processed event IDs forever is simple but would require bounded or durable storage in a long-running system.

## Next steps
Add transactional assignment reservations, a durable idempotency store, and benchmark large candidate sets before introducing caches or indexing. Verify correctness under concurrent workloads separately.

## V3 optimization scope

Prefer deterministic, testable explanations over additional routing heuristics. ETA and tie-breaking are unchanged. Candidate explanations add one ETA evaluation per candidate for the inspector, yielding linear additional work in the partner count, plus sorting for stable presentation. No map, live GPS, batch optimizer, or preparation-time recalculation feature was added.
