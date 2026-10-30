# V5 targeted contrast repair

## Changes
- Replaced the older Streamlit theme with the requested dark palette in `.streamlit/config.toml`.
- Removed the old broad CSS selectors targeting native Streamlit widgets, paragraphs, sidebar labels, metrics, tables, and alerts. These competed with Streamlit's own theme and caused inconsistent contrast.
- Kept app-owned `.sd-*` presentation rules, with full-opacity text. The sidebar controls, forms, tables, charts, alerts, and button states now rely on Streamlit native theme rendering.
- Tightened the page header/layout spacing and removed unnecessary scrollbars in the synthetic network embedding. The synthetic SVG's labels and surfaces use the new palette.
- Did not change any dispatch, model, cancellation or explanation engine code.

## Verified
- `python -m pytest -q`: 246 passed.
- `python -m py_compile dashboard.py`: passed.
- `python -m main`: expected P-2, pickup 13, ETA 35.
- Contrast (calculated): #CBD5E1 on #101827 > 12:1; #A8B7CC on #1E2D46 > 7:1; #FFFFFF on #1E2D46 > 13:1.

## Needs visual validation on Windows
Streamlit is not installed in the preparation environment. After extracting the ZIP, run `python -m streamlit run dashboard.py`, then inspect all six pages on your Streamlit version. Check sidebar radio selections, input focus and disabled states, dataframe headers, chart tooltips, alert colors, narrow laptop width and the top-of-page white strip. If any issue remains, capture a screenshot and report the Streamlit version using `python -m streamlit version`.

No additional features were introduced.
