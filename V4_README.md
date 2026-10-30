# SmartDispatch V4 — Operations Control Center

## Architecture

The evaluation engine is unchanged: `models.py` creates order/partner records, `service.py` calculates eligibility, pickup and ETA, assignments and idempotent cancellation, and `explainer.py` implements the sole optional stretch (candidate explanations). `dashboard.py` is a separate Streamlit presentation layer using these public functions.

## Start

From the directory containing `dashboard.py` and `service.py`:

```powershell
python -m pip install -r requirements-dev.txt
python -m pip install streamlit pandas
python -m pytest -q
python -m main
python -m streamlit run dashboard.py
```

Navigate to `http://localhost:8501` if Streamlit does not open automatically.

## Workspace

- **Overview:** operational metrics, capacity utilization, session orders, and recent activity.
- **Dispatch Studio:** order input, deterministic candidate explanation, selected ETA, and no-assignment reason.
- **Fleet Management:** compact partner directory, availability controls, and adding synthetic partners.
- **Operations & Audit:** order history, cancellation processing, audit entries, and CSV exports.

## Important limitations

- Data exists only for the active Streamlit session. Reload/reset can clear it.
- All travel values are synthetic; no map, GPS, or live API is used.
- Seeded partner workload is separate from session order counts.
- Reservation minutes in the UI are a simplified synthetic workload estimate, not a real routing schedule.
- The reliability panel gives a command to run tests; it does not claim to run tests from the browser.
- Existing engine return formats, ETA formulas and tie-breaking are unchanged.

## Verification (development environment)

- `python -m pytest -q`: 243 passed on Python 3.13.
- `python -m main`: `P-2`, pickup minute 13, ETA minute 35.
- `python -m py_compile dashboard.py`: passed.
- Full browser interaction on the user's Windows computer has not yet been verified.

## Submission scope

The official brief focuses on Python decision logic. V4's dashboard is a demonstration-only layer; the sole optional engine stretch remains candidate explanations. Review the repository's current contribution and AI-assistance policies before submitting.
