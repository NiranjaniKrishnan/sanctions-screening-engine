# Sanctions & Adverse Media Screening Engine — Project Log

## Objective

Build an end-to-end fincrime screening tool that combines SQL/dbt, Python,
and generative AI to solve a real industry pain point: sanctions screening
tools generate huge volumes of false-positive alerts, and analysts spend
most of their time manually ruling out matches rather than investigating
genuine risk.

This project screens a synthetic customer base against real, public
sanctions data using layered fuzzy-matching techniques (exact, phonetic,
token-based, edit-distance), then uses the Claude API to generate a
plain-language, evidence-grounded rationale for each flagged match —
turning a raw match score into something an analyst could actually action.

**Why this project:** it demonstrates data engineering (SQL/dbt), applied
Python (fuzzy matching, data generation), applied AI (LLM-grounded
reasoning, not just a chatbot wrapper), and domain fluency in fincrime
(understanding *why* false positives happen and what a real screening
workflow looks like) — all in one coherent, demoable pipeline.

**Target audience for this doc:** future-me revisiting the project, and
interviewers who want to understand design decisions, not just see a
finished repo.

---

## Architecture Overview

```
Raw data sources (OFAC sanctions list, synthetic customers)
        |
   DuckDB (local file-based database)
        |
   dbt (raw -> staging -> intermediate -> marts, tested & documented)
        |
   Python (layered fuzzy matching logic)
        |
   Claude API (generates grounded rationale for flagged matches)
        |
   Streamlit (interactive front end / demo)
        |
   GitHub (version control, portfolio visibility)
```

**Tech stack:** Python 3.11, dbt-core + dbt-duckdb, DuckDB, Streamlit,
Anthropic Claude API, rapidfuzz/jellyfish for matching, Faker for
synthetic data generation.

---

## Phase 0 — Environment Setup

**Goal:** a fully isolated, reproducible local environment with secrets
management and version control in place *before* writing any project
logic.

### Steps taken

1. **Python** — installed Python 3.11.9 via `pyenv` (version manager,
   avoids relying on the Mac's system Python), pinned this project to it
   with `pyenv local 3.11.9`, then created an isolated `venv` so
   dependencies never leak into or clash with other projects.
2. **Dependencies** — defined all required packages in `requirements.txt`
   (pandas, polars, rapidfuzz, jellyfish, duckdb, dbt-core, dbt-duckdb,
   streamlit, anthropic, python-dotenv, requests, faker, pytest) and
   installed via `pip install -r requirements.txt`.
3. **dbt + DuckDB** — initialized a dbt project (`dbt init`) pointed at
   DuckDB, a lightweight file-based analytical database requiring no
   server. Configured the connection in `~/.dbt/profiles.yml` (kept
   outside the repo, since it's environment/machine-specific, not
   project code). Verified with `dbt debug` and a full `dbt run` +
   `dbt docs generate` on the scaffolded example models.
4. **Claude API** — created an API key via console.anthropic.com, stored
   it in a local `.env` file (never committed), loaded via
   `python-dotenv`. Verified connectivity with a minimal test script
   calling `client.messages.create(...)`.
5. **Streamlit** — confirmed a minimal app renders correctly at
   `localhost:8501` before building any real UI logic.
6. **Git + GitHub** — initialized a local repo, configured git identity,
   wrote a `.gitignore` scoped to exclude the virtual environment,
   secrets (`.env`), and dbt-generated artifacts (`target/`, `logs/`,
   `*.duckdb`) since these are outputs, not source. Authenticated to
   GitHub via a scoped Personal Access Token (password auth is
   deprecated), pushed the initial scaffold to a public repo.

### How it was validated

| Component      | Validation step                                            | Result |
|-----------------|-------------------------------------------------------------|--------|
| Python/venv     | `python --version` inside activated venv                    | 3.11.9 confirmed |
| Dependencies    | `pip list` cross-checked against `requirements.txt`          | All packages present |
| dbt + DuckDB    | `dbt debug`                                                  | "All checks passed!" |
| dbt pipeline    | `dbt run` on scaffolded example models                       | 2/2 models built successfully |
| Claude API      | Test script round-trip call                                  | Returned live model response |
| Streamlit       | Manual run of test app                                       | Rendered correctly in browser |
| Git/GitHub      | `git status` (secrets excluded), `git log`, GitHub repo page | Files pushed and visible, `.env`/`venv/` absent from repo |

### Key design decisions

- **DuckDB over a hosted warehouse** — zero infrastructure cost/setup,
  fully sufficient for a portfolio-scale synthetic dataset, and the dbt
  workflow transfers directly to Snowflake/BigQuery/Postgres later if
  needed.
- **Secrets never touch git** — `.env` for the API key, `profiles.yml`
  outside the repo for the database connection, both excluded via
  `.gitignore` from the first commit, not retrofitted later.
- **Isolated Python environment** — avoids "works on my machine" issues
  and mirrors how real teams manage dependencies per-project.

### Outcome

A working, version-controlled scaffold with every layer of the stack
(data layer, transformation layer, AI layer, front end, source control)
verified independently before any business logic was written.

---

## Phase 1 — Data Sourcing

*(To be filled in as this phase is completed)*

**Goal:**

**Steps taken:**

**How it was validated:**

**Key design decisions:**

**Outcome:**

---

## Phase 2 — dbt Modeling

*(To be filled in)*

---

## Phase 3 — Fuzzy Matching Engine

*(To be filled in)*

---

## Phase 4 — Adverse Media + LLM Rationale Layer

*(To be filled in)*

---

## Phase 5 — Front End

*(To be filled in)*

---

## Phase 6 — Testing, Docs, Polish

*(To be filled in)*

---

## Overall Project Summary

*(To be written once the project is complete — pull the "Outcome" section
from each phase into a 3-4 sentence narrative, plus final quantified
results, e.g. match recall/precision on seeded ground truth.)*
