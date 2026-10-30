# SmartDispatch V3 — Explainable Dispatch

## Quick start on Windows PowerShell

1. Extract ZIP and enter its inner `SmartDispatch-Open-Source-Evaluation-main` folder.
2. `python -m pip install -r requirements-dev.txt`
3. `python -m pip install streamlit pandas`
4. `python -m pytest -q`
5. `python -m main`
6. `python -m streamlit run dashboard.py`
7. Open `http://localhost:8501` if the browser does not open automatically.

The dashboard's **Dispatch → Dispatch Decision Inspector** explains every candidate. Change route settings in the sidebar, toggle the travel record's Known checkbox, disable partners in the Partners tab, or fill a partner's capacity to see how candidate labels change.

## What changed in V3

- New: `explainer.py` candidate-by-candidate decision rationale (the single optional stretch feature).
- New: `tests/test_explainer.py` with six tests.
- Updated: `dashboard.py` with decision inspector and structured JSON output.
- Updated: TESTING, DISPATCH_NOTES, approach, challenges, optimizations.
- Preserved: existing service functions, fixture data, test suite, and original project layout.

The result is a deterministic offline simulation, not live delivery telemetry or real GPS. Session data resets when the Streamlit session is reset. Visible tests alone do not guarantee hidden-test success.
