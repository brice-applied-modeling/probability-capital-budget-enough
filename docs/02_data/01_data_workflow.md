# Data Workflow

## Project: Capital Budget Sufficiency Monte Carlo Case Study

**Runtime:** Python 3.12, managed with `uv`  
**Current phase:** Data discovery, ingestion, validation, and exploratory analysis  
**Boundary:** This workflow prepares evidence for the simulation; it does not predetermine model distributions or outcomes.

## 1. Workflow overview

```text
Define data needs and cost categories
             |
             v
Discover public sources and record provenance
             |
             v
Acquire and preserve raw data (immutable)
             |
             v
Ingest, inspect schema, and validate files
             |
             v
Clean, classify, normalize, and reconcile
             |
             v
EDA: comparability, distributions, trends, gaps
             |
             v
Select empirical/proxy/judgment assumptions
             |
             v
Review uncertainty and dependencies
             |
             v
Freeze model-input dataset and assumption version
             |
             v
Build and validate Monte Carlo model
```

## 2. Suggested repository structure

```text
capital-budget-sufficiency/
├── README.md
├── pyproject.toml
├── uv.lock
├── .python-version
├── data_requirements.md
├── data_workflow.md
├── data/
│   ├── raw/                  # Original downloaded source records
│   ├── interim/              # Parsed, not fully standardized
│   ├── processed/            # Validated, analysis-ready tables
│   └── source_register.csv
├── docs/
│   ├── assumptions_register.md
│   ├── data_quality_report.md
│   └── data_dictionary.md
├── notebooks/
│   ├── 01_data_ingestion.ipynb
│   ├── 02_data_cleaning.ipynb
│   └── 03_exploratory_data_analysis.ipynb
├── src/
│   └── capital_budget/
│       ├── __init__.py
│       ├── ingestion.py
│       ├── validation.py
│       ├── normalization.py
│       └── datasets.py
├── tests/
│   ├── test_validation.py
│   └── test_normalization.py
└── outputs/
    ├── figures/
    └── tables/
```

The notebooks are for investigation and explanation; reusable transformation logic belongs in `src/` and should have tests. Exclude large or restricted raw files from Git where necessary; commit source manifests and retrieval instructions instead.

## 3. Environment setup

From the repository root:

```bash
uv python install 3.12
uv init --python 3.12   # Only if pyproject.toml does not exist
uv add numpy pandas scipy matplotlib openpyxl jupyter ipykernel
uv add --dev pytest
uv sync
uv run python --version
uv run pytest
```

If the repository is already initialized, do not run `uv init` again. Use `uv lock` and commit `uv.lock` to preserve the resolved environment.

## 4. Phase 1: Source discovery and inventory

**Tasks**

1. Create the source register with source ID, publisher, reference, retrieval date, coverage, access constraints, and intended variables.
2. Identify public Florida municipal water utility bid tabulations and comparable HDD contract items.
3. Identify publicly accessible BLS construction-related indices and document the exact series IDs and coverage.
4. Identify USACE and Florida DEP permitting records or public examples with application and approval dates.
5. Identify public change orders and HDD case studies for discrete risk events.
6. Classify each source as A (direct), B (proxy), or C (judgment).

**Deliverable:** Source inventory and initial data gap register. Do not claim that a source exists or is publicly downloadable until verified.

## 5. Phase 2: Raw acquisition and ingestion

**Tasks**

1. Save downloaded data unchanged under `data/raw/` with descriptive, stable filenames.
2. Capture original URL/reference, access date, and source context; retain PDFs or scanned bid tabs as source documents when licensing allows.
3. Load structured CSV/XLSX records with explicit data types where possible.
4. If a PDF contains a bid table, manually verify extracted columns and values against the original document. Record any transcription method and spot-check results.
5. Inspect `shape`, `head`, `dtypes`, duplicate keys, and missing-value counts in `01_data_ingestion.ipynb`.
6. Output a parsed interim table without overwriting source files.

**Deliverable:** Traceable raw and interim records plus an ingestion log.

## 6. Phase 3: Cleaning, normalization, and QA

**Tasks**

1. Standardize units (LF, EA, SY, LS), currency, and dates.
2. Classify bid items (open-cut, HDD, valves, restoration, MOT, etc.).
3. Separate bids, engineer's estimates, awarded amounts, and final paid costs.
4. Reconcile extended prices and totals; flag mismatches rather than silently changing values.
5. Deduplicate within source and project context; do not remove legitimate competing bids.
6. Normalize nominal prices to a stated reference date using a documented index, retaining both original and normalized values.
7. For permitting records, calculate elapsed times only when start/end event definitions are consistent.
8. Log missing values, scope mismatches, and exclusions.

**Deliverable:** Validated tables in `data/processed/`, a data dictionary, and a QA report.

**Suggested automated tests**

- Quantities and unit prices are nonnegative when required by the record definition.
- Extended prices reconcile within a documented rounding tolerance.
- Dates are valid and approval dates do not precede application dates.
- Price index series have valid dates, positive values, and unique date keys.
- Normalization reproduces known hand-calculated examples.
- Record IDs map back to source files.

## 7. Phase 4: Exploratory data analysis (EDA)

Use `03_exploratory_data_analysis.ipynb` for:

- Observation counts by category, project, year, and installation method.
- Price distributions, quantiles, and outliers for comparable bid items.
- Trends in historical cost indices and year-over-year changes.
- Relationships among bid year, quantity, diameter, method, and unit price.
- Permit elapsed-time distributions where enough consistent records exist.
- Data gaps and differences between directly comparable and proxy observations.

**Caution:** Multiple bid prices from the same project are not independent projects. Do not fit precise tails from a tiny or highly heterogeneous sample.

**Deliverable:** EDA figures and short evidence assessment by modeled variable.

## 8. Phase 5: Build the modeling assumptions register

For every sampled input, record:

| Field | Example / meaning |
|---|---|
| `variable_id` | `hdd_unit_price` |
| `baseline_value` | Deterministic estimate value |
| `units` | USD/LF |
| `distribution_or_scenario` | Candidate model, subject to review |
| `parameters` | Explicit values and their units |
| `source_ids` | Traceable supporting sources |
| `evidence_grade` | A, B, or C |
| `dependencies` | Common escalation driver, permit schedule, etc. |
| `limitations` | Small sample, proxy location, unknown scope |
| `sensitivity_case` | Low/base/high uncertainty assumption |

Choose distributions after inspecting data. Where evidence is weak, prefer clearly labeled judgment-based ranges and multiple scenarios over unjustified statistical precision.

**Deliverable:** Versioned assumptions register, reviewed before Monte Carlo implementation.

## 9. Phase 6: ICW permit and schedule-risk integration

Construct a small dependency model before cost simulation:

1. Define the required permit set for the hypothetical crossing as a scenario assumption, subject to jurisdictional review.
2. Specify baseline application timing, approval milestone, bid/award and construction start, and available float.
3. Represent possible requests for additional information and approval-duration uncertainty without treating published agency review clocks as empirical durations.
4. Calculate critical-path delay, if any.
5. Apply delay-driven escalation only to affected procurement costs and dates.
6. Add separately supported incremental engineering, administration, or remobilization costs when triggered.
7. Keep direct HDD construction-event costs distinct from permit delays, unless an explicitly modeled common cause links them.

**Deliverable:** Documented input/output specification for the permit-to-schedule-to-cost module.

## 10. Phase 7: Freeze inputs and hand off to simulation

Before simulation:

- Assign a dataset version and assumptions version.
- Record random seed policy and target simulation size (to be selected during modeling).
- Reconcile the deterministic cost estimate to the authorized budget.
- Define primary output: `Pr(total_project_cost <= authorized_budget)`.
- Define secondary outputs: P50/P80/P90/P95 costs, required contingency, expected shortfall, cost-driver sensitivity, and with/without-permit-risk scenarios.
- Confirm no double-counting of contingency, escalation, or HDD risk.

**Deliverable:** An auditable, reproducible input package ready for the Monte Carlo engine.

## 11. First practical work session

**Assignment A: Source inventory**

- Create `data/source_register.csv` with column headers.
- Find and document 3–5 candidate public bid tabulations, at least one relevant escalation series, and candidate permit-timeline sources.
- Record what each source can and cannot support. Do not download or scrape restricted materials.

**Assignment B: Baseline estimate reconciliation**

- Build or import the detailed estimate line items.
- Confirm that open-cut and HDD lengths total 15,000 LF.
- Verify all cost category subtotals and contingency treatment.

**Assignment C: Data gap memo**

- Identify the three most consequential inputs that still require engineering judgment.
- Propose reasonable sensitivity scenarios for those inputs, without presenting them as observed data.

**Completion standard:** We know what we have, what is missing, and what we can responsibly model. Only then move to distribution selection and simulation code.
