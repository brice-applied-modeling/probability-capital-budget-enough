# What Is the Probability Our Capital Budget Will Be Enough?

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![uv](https://img.shields.io/badge/Package_Manager-uv-DE5FE9?logo=astral&logoColor=white)
![Monte Carlo](https://img.shields.io/badge/Method-Monte%20Carlo-166534)
![Infrastructure](https://img.shields.io/badge/Domain-Infrastructure%20Analytics-0F766E)
![Status](https://img.shields.io/badge/Status-Planning-F59E0B)
![License](https://img.shields.io/badge/License-TBD-lightgrey)

## Overview

This standalone Python project evaluates whether a hypothetical municipality's authorized capital budget is sufficient to deliver a water transmission main project under uncertainty. It uses Monte Carlo simulation to translate uncertain construction quantities, unit prices, subsurface conditions, permitting duration, and schedule-related escalation into probabilities and actionable capital planning metrics.

The completed analysis will become a **technical case study**, with a separate LinkedIn article explaining its findings for infrastructure decision-makers. Results are not predetermined.

> **Important:** The project scenario, costs, and distributions are illustrative. They are not current bid prices, agency permitting forecasts, or engineering recommendations for a real project.

## Project scenario

| Item | Working assumption |
|---|---|
| Asset | 16-inch potable water transmission main |
| Total alignment | 15,000 linear feet |
| Open-cut installation | 14,200 linear feet |
| Horizontal directional drilling (HDD) | 800 linear feet beneath the Intracoastal Waterway |
| Setting | Hypothetical coastal municipality, mixed urban/suburban corridor |
| Base construction estimate | Approximately $8.98 million |
| Pre-contingency project estimate | Approximately $10.73 million |
| Owner contingency | 15%, approximately $1.61 million |
| Authorized capital budget | Approximately $12.34 million |

The baseline values are provisional and will be reconciled to a detailed quantity-and-unit-price estimate before simulation.

### Why the Intracoastal Waterway matters

The HDD crossing introduces both **direct construction risk** and **permitting-driven schedule risk**. Permit review, requests for additional information, and potential work restrictions may affect the critical path. A delay can increase engineering or administration costs, defer procurement, and expose construction prices to further escalation.

The model will distinguish **permit duration** from **actual project delay**: only delays that consume available float and affect critical activities should drive time-related costs. It will also avoid double-counting escalation or contingency already included elsewhere.

## Questions the analysis will answer

1. **Budget sufficiency:** What is the probability that final project cost will remain at or below the authorized capital budget?
2. **Contingency adequacy:** What funding levels correspond to P50, P80, P90, and P95 outcomes? How do they compare with the current contingency?
3. **Cost drivers:** Which quantities, prices, and discrete risks have the largest influence on cost uncertainty?
4. **ICW permitting and schedule:** How likely is permitting to delay the critical path, and how much does that delay contribute to total project cost?
5. **Scenario comparison:** How do outcomes change under alternative escalation, subsurface, and permitting scenarios?
6. **Decision support:** Would increasing contingency, advancing permitting, or funding additional investigation materially improve the project's risk position?

## Modeling approach

### Deterministic baseline

Develop and verify a bottom-up cost estimate with quantities, unit prices, allowances, and explicit contingency. Separate construction cost, soft costs, and owner-held contingency.

### Probabilistic inputs

Represent uncertainty using documented distributions supported by engineering judgment, historical information where available, and scenario assumptions. Candidate inputs include:

- Open-cut installation quantities and unit costs
- HDD installation and unforeseen subsurface conditions
- Utility conflicts, restoration, and traffic control
- Construction price escalation and procurement timing
- ICW permitting review duration and discrete permitting complications
- Schedule float, delay exposure, and time-dependent soft costs

Distribution types, bounds, correlations, and discrete-event probabilities will be chosen and justified during model development, **not assumed to be validated today**.

### Simulation and analysis

Use a reproducible random seed and configurable iteration count. For each iteration, sample cost and schedule drivers, calculate total project cost, and compare it with the authorized budget.

Key outputs:

- Probability of staying within budget and probability of exceedance
- Total-cost distribution and P50/P80/P90/P95 percentiles
- Required contingency at selected confidence levels
- Expected budget overrun and conditional overrun severity
- Cost and schedule sensitivity analysis
- Incremental impact of ICW permitting and schedule risk

## Technology

- **Python:** 3.12
- **Environment and dependencies:** [uv](https://docs.astral.sh/uv/)
- **Numerical analysis:** NumPy, pandas, SciPy
- **Visualization:** Matplotlib (and optionally Seaborn or Plotly later)
- **Testing:** pytest
- **Notebooks:** Jupyter, for exploratory analysis and presentation; reusable simulation logic belongs in `src/`

## Getting started

Install [uv](https://docs.astral.sh/uv/getting-started/installation/), then initialize the repository if it does not yet have a `pyproject.toml`:

```bash
uv init --python 3.12
uv python install 3.12
uv add numpy pandas scipy matplotlib jupyter
uv add --dev pytest
uv sync
```

If the repository already contains a `pyproject.toml` and `uv.lock`, simply run:

```bash
uv sync
```

Run tests with:

```bash
uv run pytest
```

Start Jupyter with:

```bash
uv run jupyter lab
```

Commit `pyproject.toml` and `uv.lock` for reproducibility. Set `requires-python = ">=3.12,<3.13"` in `pyproject.toml` if the project is intentionally limited to Python 3.12.

## Proposed repository structure

For the project structure, see the following location in the repository:

[project structure](docs/01_architecture/00_project_structure.md)

*This is the target structure; directories and modules will be created as work progresses.*

## Development roadmap

| Phase | Deliverable | Acceptance criterion |
|---|---|---|
| 1. Baseline estimate | Cost breakdown and assumption register | Quantities, extended costs, allowances, and budget reconcile |
| 2. Uncertainty specification | Input distributions and risk dependencies | Every input has units, rationale, and treatment of correlation |
| 3. Monte Carlo engine | Reproducible simulation and tests | Results reproduce with fixed seed; deterministic limits behave correctly |
| 4. Risk analysis | Probability metrics, sensitivity, scenario comparisons | No double-counted cost or schedule exposure; results are interpretable |
| 5. Case study | Technical methodology, findings, and limitations | All figures trace to model outputs and stated assumptions |
| 6. Communication | LinkedIn article based on the case study | Claims reflect computed results without overstating precision |

## Model quality and limitations

The simulation is a **decision-support model**, not a prediction of the exact final cost. Confidence levels describe outcomes conditional on the modeled assumptions, not guaranteed performance. Sensitivity tests will explore the effect of alternative distribution shapes, correlations, escalation rates, and permitting timelines.

For a credible case study, the project will clearly separate illustrative inputs from empirically supported data, distinguish risk allowances from contingency, and disclose major exclusions.

## Current status

**Planning / baseline definition.** The scenario and research questions are defined. Detailed cost reconciliation, probability distributions, coding, and results remain to be completed.

## Intended publications

1. **Technical case study:** Complete methodology, assumptions, validation, analysis, and findings.
2. **LinkedIn article:** *What Is the Probability Our Capital Budget Will Be Enough?* A decision-maker-focused explanation derived from the completed analysis.
