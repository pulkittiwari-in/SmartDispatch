# SmartDispatch V5 — Intelligent Dispatch Control Tower

## Scope
This is a **presentation-only** enhancement to the original offline Python evaluation engine. `service.py`, `models.py`, and `explainer.py` retain their public interfaces. The sole optional engine stretch feature is candidate-by-candidate explanation.

## Run
From the inner folder containing `dashboard.py`:

```powershell
python -m pip install -r requirements-dev.txt
python -m pip install streamlit pandas
python -m pytest -q
python -m streamlit run dashboard.py
```

Visit http://localhost:8501 (or the URL shown in the terminal).

## Navigation
- **Command Center**: session-derived fleet KPIs, workload, activity, and unassigned decisions.
- **Dispatch Studio**: synthetic order input, assignment, candidate explanations and recorded decision snapshot.
- **Fleet Management**: compact partner directory, availability, capacity, and synthetic partner creation.
- **Synthetic Zone Network**: read-only schematic of known travel legs from the most recently recorded decision. Node placement is not geography.
- **Operations & Audit**: order history, cancellation controls, session audit records and CSV export.
- **Analytics & Reliability**: session-derived utilization, dispatch success rate, ETA comparisons, and recorded test baseline.

## Verification
- Baseline V4 before V5 changes: 243 pytest tests passed with Python 3.13.
- After V5 presentation changes: run `python -m pytest -q` to verify engine compatibility.
- `dashboard.py` Python syntax checked; interactive Streamlit browser testing must be performed on the user's machine.
- Session state is ephemeral. Restart/reset clears demo activity.

## Limitations
The schematic network is **not** GPS, a real map, or a routing algorithm. Dashboard is a simulator, not a persistent production dispatch service. Displayed ETA is an absolute simulation minute offset, not a wall-clock arrival time. This dashboard should be kept separate from the narrowly scoped evaluation pull request if organizers enforce the one-stretch-item rule.

## V5 verified maintenance update
- `decision_snapshot.py` captures a pre-assignment snapshot for historical replay.
- `tests/test_decision_snapshot.py` checks nonmutation, capacity-change resilience, and reproducibility.
- `.streamlit/config.toml` applies a native dark theme to tables and widgets.
- The synthetic zone diagram renders through a local HTML component, without external maps.
- `verification.json` is a timestamped **recorded** pytest result, not an always-live health check.
- **Current local verification:** 246 pytest tests passed; CLI demo passed; Python dashboard compiled.
- **Not verified here:** Interactive Streamlit browser behavior. Validate locally before evaluation.
