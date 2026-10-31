# SmartDispatch — Submission Guide

## Run the application

Install dependencies:
python -m pip install -r requirements-dev.txt

Start the dashboard:
python -m streamlit run dashboard.py

## Run automated tests

python -m pytest -q

## Judge demonstration

1. Demonstrate a successful dispatch.
2. Explain partner eligibility and ETA calculations.
3. Demonstrate an unassigned order.
4. Demonstrate lexicographic tie-breaking.
5. Demonstrate duplicate cancellation handling.

## Scope

This document provides submission and demonstration instructions.
It does not change the dispatch algorithm or dashboard.
