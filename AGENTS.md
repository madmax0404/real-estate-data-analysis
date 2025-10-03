# Repository Guidelines

## Project Structure & Module Organization
Analytic work centers on root-level Jupyter notebooks named by district, asset type, and snapshot date (e.g., `집값분석 - 강동구 천호동 연립다세대 20210603.ipynb`). Data drops and reference tables sit in domain folders such as `한국부동산원`, `좌표데이터`, and `토지이용계획정보`; use the same pattern for new sources and keep raw files read-only. Derived tables and exports belong in `추출한_건물정보` or scoped subdirectories. The lightweight `main.py` is available as an entry point for scripted workflows and should remain a thin orchestrator around the notebook pipeline.

## Build, Test, and Development Commands
Create an isolated environment before touching notebooks:
- `python3 -m venv .venv && source .venv/bin/activate`
- `pip install -U pip pandas numpy geopandas lightgbm scikit-learn folium shapely`
Launch notebooks with `jupyter lab` (preferred) or `jupyter notebook`. For quick smoke checks, run `python main.py` to confirm the environment is wired correctly.

## Coding Style & Naming Conventions
Follow PEP 8 in code cells (4-space indentation, descriptive snake_case variables). Keep import/setup cells at the top, consolidate configuration constants in one cell, and mark expensive operations with markdown headers. Name new notebooks with the existing convention: `{분석유형} - {지역/데이터셋} {세부설명} {YYYYMMDD}.ipynb`. When committing scripts, stick to lowercase module names and avoid hard-coded local paths—use `Path` relative to the repo root.

## Testing Guidelines
Before pushing, restart and run notebooks end-to-end to ensure deterministic outputs and no hidden state. Validate joins and aggregations by printing row counts or sample slices, and snapshot key metrics in markdown. If a notebook produces reusable datasets, add a final cell that asserts expected column sets and value ranges. Capture any regression checks (e.g., LightGBM CV scores) in the notebook output so reviewers can diff them.

## Commit & Pull Request Guidelines
Git history favors short imperative summaries (e.g., "Update README.md"); follow the same format while adding a brief scope suffix when data changes, such as "Update dataset — 송파구 오피스텔". For pull requests, include: purpose and dataset scope, notebooks or scripts touched, data sources updated, and before/after screenshots or metrics for visualizations. Link to tracking issues when available and call out any notebooks that must be re-run downstream.

## Data Handling Notes
Many inputs originate from public agencies; verify license terms before redistribution and redact personal identifiers. Large raw files should stay zipped when feasible, and anything exceeding 100 MB should rely on external storage with download instructions captured in the README.
