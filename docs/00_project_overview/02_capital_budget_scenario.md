# Project Scenario and Deterministic Cost Estimate

## 1. Purpose

Establish a transparent, reproducible baseline for a standalone Python Monte Carlo case study titled **What Is the Probability Our Capital Budget Will Be Enough?** This document defines the hypothetical infrastructure project, preliminary quantities and costs, capital funding constraint, and candidate uncertainty drivers. All costs are **illustrative planning assumptions**, not verified current market quotations or an engineer's opinion of probable construction cost for an actual project.

## 2. Decision Context

A municipal utility plans a new 16-inch water transmission main to improve system reliability and conveyance capacity. Decision-makers have a fixed capital appropriation and need to understand the likelihood that the total project cost will fit within that appropriation.

**Primary decision:** Is the authorized capital budget sufficient, and how much contingency would be required for specified confidence levels?

**Supporting decisions:** Which cost drivers create the greatest budget exposure? Would further geotechnical investigation, alignment changes, or early procurement materially reduce uncertainty?

## 3. Hypothetical Physical Scope

- **Facility:** 16-inch potable-water transmission main.
- **Length:** 15,000 linear feet (LF), including 14,200 LF open-cut and 800 LF horizontal directional drilling (HDD) beneath the Intracoastal Waterway (ICW). The 800 LF is an illustrative placeholder; actual crossing geometry, cover, entry/exit setbacks, and feasibility require engineering validation.
- **Construction setting:** Mixed urban/suburban roadway corridor with utility congestion, pavement restoration, and traffic management.
- **Major elements:** Main installation, one HDD crossing beneath the Intracoastal Waterway (ICW), valves and appurtenances, connections, testing/disinfection, restoration, and traffic control.
- **Schedule assumption:** One principal construction contract. The ICW crossing creates a permitting and notice-to-proceed dependency that may delay procurement and construction. Model schedule-driven escalation and time-related costs explicitly, not as an unrelated blanket premium.
- **Excluded unless explicitly added:** Land acquisition, major pump stations, treatment facilities, financing costs, taxes, and post-construction operating expenses.

## 4. Preliminary Deterministic Construction Estimate

The table below is a conceptual estimate for simulation development. Quantity and price assumptions should be reviewed and, where possible, replaced with documented public bid-tab or published cost data before the technical case study is finalized.

| ID | Cost component | Quantity | Unit | Unit cost ($) | Extended cost ($) |
|---|---|---:|---|---:|---:|
| C01 | 16-inch open-cut water main | 14,200 | LF | 340 | 4,828,000 |
| C02 | 16-inch HDD installation | 800 | LF | 1,350 | 1,080,000 |
| C03 | Isolation valves and appurtenances | 12 | EA | 28,000 | 336,000 |
| C04 | Connections and tie-ins | 4 | EA | 85,000 | 340,000 |
| C05 | Pavement, sidewalk, and surface restoration | 1 | LS | 1,150,000 | 1,150,000 |
| C06 | Traffic control and maintenance of traffic | 1 | LS | 420,000 | 420,000 |
| C07 | Testing, disinfection, and commissioning | 1 | LS | 180,000 | 180,000 |
| C08 | Mobilization and general conditions | 1 | LS | 650,000 | 650,000 |
| | **Base construction subtotal** | | | | **8,984,000** |

**Check:** Open-cut plus HDD length = 15,000 LF. The lump-sum items are placeholders for a conceptual model; avoid representing them as measured quantities.

## 5. Project-Level Costs and Approved Budget

| Item | Basis | Amount ($) |
|---|---|---:|
| Base construction subtotal | Sum of C01–C08 | 8,984,000 |
| Design, permitting, and engineering services (including ordinary ICW permit preparation) | Illustrative fixed allowance | 1,050,000 |
| Construction engineering and inspection | Illustrative fixed allowance | 700,000 |
| **Pre-contingency project baseline** | Sum of above | **10,734,000** |
| Owner-held contingency | Illustrative allocation | 1,610,100 |
| **Initial authorized capital budget** | Baseline + contingency | **12,344,100** |

### 5.1 Baseline-to-Budget Reconciliation

The `config/baseline_estimate.csv` file contains **ten cost items**: eight construction items (`C01`–`C08`) and two project-level soft-cost items (`S01`–`S02`). It does **not** include contingency. The approved budget is a separate funding limit in `config/simulation.yaml`.

| Component | Source / calculation | Amount (USD) |
|---|---|---:|
| Base construction estimate | Sum of `C01`–`C08` in `baseline_estimate.csv` | $8,984,000 |
| Design, permitting, and engineering | `S01` in `baseline_estimate.csv` | $1,050,000 |
| Construction engineering and inspection (CEI) | `S02` in `baseline_estimate.csv` | $700,000 |
| **Pre-contingency baseline estimate** | **Sum of all ten CSV cost items** | **$10,734,000** |
| Owner-held contingency (15%) | $10,734,000 × 0.15 | $1,610,100 |
| **Authorized hypothetical capital budget** | **$10,734,000 + $1,610,100** | **$12,344,100** |

**Terminology:** The *baseline estimate* is the modeled project's preliminary pre-contingency cost. The *capital budget* is the authorized spending threshold including owner contingency. The phrase *reconciled estimate* means only that the cost components and funding calculation agree arithmetically; it does not describe a second estimate or independent market validation.

**Modeling rule:** Compare simulated total project cost to the $12,344,100 funding limit. Do not add the $1,610,100 contingency as a simulated cost item. All figures remain hypothetical, uncalibrated planning assumptions pending review against acquired evidence.

The contingency is **15% of the pre-contingency baseline**, not a modeled cost item. The budget is a funding threshold, not a simulation input that automatically increases project costs. This distinction prevents double counting.

For presentation, the municipality could instead authorize a rounded **$12.35 million** budget, but the initial model should use the exact $12,344,100 figure to preserve traceability.

## 6. Baseline Scope and Modeling Boundaries

The first simulation models **total project outturn cost** as:

`Total cost = uncertain construction component costs + project-level soft costs + separately modeled risk-event costs`

For Version 1, base design and construction engineering/inspection allowances are fixed, but incremental costs caused by ICW permitting delay may be simulated separately. Distinguish preconstruction delay (before notice to proceed) from active-construction delay: they do not necessarily incur the same costs.

The baseline estimate is **not** necessarily the expected value of the simulated distribution. Triangular and skewed inputs can produce a mean above or below their deterministic point estimates. Report both separately.

## 7. Candidate Uncertainty Register

These are *proposed* modeling choices, not calibrated probability distributions. Confirm parameter values and units before coding.

| Driver | Cost items affected | Initial treatment | Rationale / calibration need |
|---|---|---|---|
| Open-cut installed unit price | C01 | Bounded triangular or PERT | Bid-price variability, productivity, utility congestion |
| HDD installed unit price | C02 | Skewed bounded distribution | Ground conditions, bore complexity, subcontractor pricing |
| Open-cut installed quantity | C01 | Bounded quantity variation | Alignment changes, field measurement, design development |
| HDD length | C02 | Scenario or bounded variation | Crossing geometry and permitting changes |
| Restoration cost | C05 | Bounded triangular / PERT | Extent of pavement and surface replacement |
| Traffic-control cost | C06 | Bounded distribution | Phasing, traffic restrictions, duration |
| General conditions | C08 | Correlated with duration | Schedule extension can increase indirect costs |
| Market escalation | Multiple construction items | Common factor / scenario | Shared exposure across construction components |
| Unforeseen utility conflict | Selected items | Bernoulli event + severity | Discrete occurrence and conditional consequence |
| Adverse HDD subsurface condition | C02 | Bernoulli event + severity | Discrete geotechnical risk |
| ICW permit review duration | Procurement/construction start | Bounded duration or scenario | Agency review, completeness, resubmittals, and coordination |
| ICW permit-related redesign / mitigation | Soft costs / HDD scope | Bernoulli event + conditional cost | Additional studies, redesign, mitigation, or compliance requirements |
| Construction-window restriction | Start date / project duration | Scenario or discrete event | Seasonal restrictions or agency-imposed work windows, if applicable |
| Delay-driven escalation | Construction line items | Derived from simulated award/start date | Avoid independent escalation if schedule already drives it |
| Delay-related owner costs | Engineering, administration, inspection | Conditional duration and rate | Separate preconstruction support from active construction inspection |

### Correlation and double-counting rules

1. Do not independently escalate costs already expressed in escalated outturn dollars.
2. Do not model the same utility conflict as both a high unit price and a separate full-cost event without separating their effects.
3. Treat common market factors as correlated across affected cost components, rather than drawing an independent escalation rate for each line item.
4. If HDD adverse-ground events are modeled separately, distinguish ordinary HDD unit-price uncertainty from exceptional subsurface events.
5. Document whether uncertainty applies to quantities, unit prices, or both.
6. Do not add an ICW permitting premium to the baseline if the existing $1,050,000 soft-cost allowance already includes ordinary permit preparation. Only incremental review, mitigation, redesign, or support costs belong in the risk model.
7. Do not charge full construction general conditions for a delay before mobilization. Contract terms, procurement timing, and the stage of delay govern exposure.
8. Avoid independently sampling a market escalation percentage and also applying escalation derived from the same simulated delay.

## 8. Intracoastal Waterway Permitting and Schedule Risk Module

**New research hypothesis:** For this crossing, permitting may create more capital-budget exposure through schedule uncertainty and cost escalation than through direct permit fees.

### Permitting and schedule logic

Build a simplified milestone model:

`design complete → permit application complete → agency review / information requests → authorization → bid or award → notice to proceed → HDD construction → completion`

Define a baseline planned authorization and construction-start date, then simulate permit-review and resubmittal durations. The schedule effect depends on whether permitting lies on the **critical path**. If design, procurement, or other work has available float, some permit delay may not change the construction start. A simple first-pass formula is:

`start_delay_months = max(0, simulated_permit_completion - latest_permit_date_without_start_delay)`

Evaluate construction-window restrictions separately if applicable. Document calendar assumptions, including when bids are priced and when escalation stops being an owner exposure.

### Cost pathways (avoid double counting)

1. **Direct incremental permitting cost:** supplemental studies, agency responses, redesign, mitigation, or other requirements *beyond* the ordinary permitting allowance.
2. **Pre-award schedule escalation:** if permit delay pushes bidding/award or procurement later, estimate the cost increase for affected work using a common annual escalation assumption and elapsed time (for example, `adjusted_cost = reference_cost × (1 + annual_rate) ** (delay_months / 12)`). This is a modeling convention, not a claim about actual bid behavior.
3. **Preconstruction owner support:** extended consultant or program-management effort attributable to delay, using an appropriate monthly cost if supported.
4. **Construction-period extension:** only if permit conditions or restrictions affect active construction; model incremental general conditions, inspection, and other time-dependent costs at that stage.

**Important:** The existing deterministic baseline remains **$10,734,000**, with a **$12,344,100** authorized budget. For the initial version, ordinary permit preparation is assumed included in the $1,050,000 design/permitting allowance. Do not automatically add a new permit fee or increase the budget. Any baseline revision must be explicit and reconciled.

### Required scenarios

| Scenario | Permitting assumption | Analytical purpose |
|---|---|---|
| A: On-time | Approval before the latest non-delaying date | Reference budget sufficiency |
| B: Moderate delay | Review or resubmittal extends beyond schedule float | Measure incremental escalation and owner support |
| C: Extended delay | Longer approval process and possible construction-window conflict | Examine upper-tail budget exposure |
| D: Mitigation / early action | Earlier submittal, targeted investigation, or added review resources | Test whether a plausible intervention reduces exposure; do not assume it guarantees faster agency approval |

Scenario durations, probabilities, and monthly cost rates are **to be researched or explicitly labeled hypothetical**, not invented as empirical facts. The model should be able to compare a discrete scenario approach with continuous duration sampling.

### Additional questions to answer

- What is the probability the ICW permit becomes critical to the construction start?
- How many months of schedule delay occur at P50, P80, and P90?
- What share of total expected cost and budget-exceedance risk is attributable to permitting **direct costs** versus **delay-driven costs**?
- How much does the probability of staying within budget change with and without the ICW schedule risk?
- What is the incremental contingency needed to maintain P80 or P90 confidence after accounting for permit delays?
- How sensitive are results to escalation rates, available schedule float, and restrictions on construction windows?
- Under what assumptions would earlier permitting effort or design investigation justify its cost?

### Implementation requirements

Create a schedule-risk input table containing the reference milestone dates, float, review-duration assumptions, possible information-request cycles, escalation convention, and stage-specific monthly costs. Use the same random seed for paired comparisons when practical. Report both cost and schedule outcomes; include a schedule-risk contribution chart and scenario comparison. Do not claim that simulated review times represent any agency's actual performance without documented evidence.

---

## 9. Questions the Model Must Answer

### Budget sufficiency

- What proportion of simulated outcomes are at or below the authorized $12,344,100 budget?
- What is the probability of exceeding the budget, and by how much when an overrun occurs?
- What are the expected, median, P80, P90, and P95 total project costs?

### Contingency adequacy

- What contingency is required to achieve P80, P90, and P95 funding confidence relative to the $10,734,000 pre-contingency baseline?
- How does the initial 15% contingency compare with the simulated requirement?
- How much additional authorization would be needed at each selected confidence level?

### Cost drivers and decisions

- Which input uncertainties explain the greatest variation in final cost?
- How do outcomes change with market escalation, HDD conditions, or utility-conflict assumptions?
- What is the modeled value of reducing selected uncertainties through investigation or design refinement?

## 10. Python Model Architecture

Suggested repository structure:

```text
capital-budget-sufficiency/
├── README.md
├── project_overview.md
├── project_scope.md
├── docs/
│   ├── project_scenario_and_cost_estimate.md
│   ├── assumptions_register.md
│   ├── methodology.md
│   └── case_study.md
├── data/
│   ├── inputs/
│   │   ├── baseline_cost_estimate.csv
│   │   ├── uncertainty_assumptions.csv
│   │   └── schedule_risk_assumptions.csv
│   └── outputs/
├── notebooks/
│   ├── 01_baseline_estimate.ipynb
│   ├── 02_uncertainty_exploration.ipynb
│   └── 03_results_and_sensitivity.ipynb
├── src/
│   └── capital_budget/
│       ├── __init__.py
│       ├── baseline.py
│       ├── distributions.py
│       ├── schedule_risk.py
│       ├── simulation.py
│       ├── metrics.py
│       └── plots.py
├── tests/
│   ├── test_baseline.py
│   └── test_simulation.py
└── pyproject.toml
```

Use `numpy`, `pandas`, `scipy`, `matplotlib`, and `pytest`. Keep reusable modeling logic in `src/`; notebooks should support exploration and communication rather than contain the only implementation.

## 11. Development Phases and Acceptance Criteria

### Phase 1: Deterministic estimate

- Create `baseline_cost_estimate.csv` with IDs, descriptions, quantities, units, unit prices, and extensions.
- Write Python functions that recompute all extended costs and subtotals.
- Verify 14,200 + 800 = 15,000 LF, construction subtotal = $8,984,000, and pre-contingency baseline = $10,734,000.
- Verify owner contingency = $1,610,100 and authorized budget = $12,344,100.

### Phase 2: Assumptions and distributions

- Develop a parameter register with low, most-likely, high (or alternative parameterization), evidence source, and justification.
- Define discrete events separately from ordinary estimating variability.
- Document the ICW permitting milestone logic, available schedule float, and delay-cost pathways.
- Document shared market factors, correlations, and potential overlap.

### Phase 3: Monte Carlo engine

- Implement reproducible random sampling using an explicit seed.
- Start with 10,000 simulations, then assess convergence at larger counts.
- Verify simulated costs remain physically meaningful and formulas preserve cost-component traceability.

### Phase 4: Risk analysis and validation

- Calculate budget sufficiency, exceedance magnitude, and P50/P80/P90/P95.
- Report simulated ICW permitting delay, schedule criticality, and incremental delay-driven cost. Compare scenarios with and without this risk.
- Estimate contingency and incremental funding needed at target confidence levels.
- Conduct sensitivity and scenario analyses; distinguish input-output association from causal attribution.
- Test baseline reconciliation, reproducibility, edge cases, and simulation convergence.

### Phase 5: Technical case study

- Document inputs, evidence quality, methodology, charts, findings, limitations, and decision implications.
- Label all synthetic assumptions and avoid claiming that illustrative outputs predict an actual municipal project.
- Draft the LinkedIn article **only after** the analytical findings are finalized.

## 12. Immediate Next Assignment

1. Review the hypothetical project configuration and baseline line items for engineering plausibility.
2. Build `data/inputs/baseline_cost_estimate.csv` from Section 4.
3. Implement a deterministic estimator that reproduces the totals in Section 5.
4. Write unit tests for quantity extensions, subtotals, contingency, and budget reconciliation.
5. Define a first-pass ICW permitting milestone timeline and identify which delay costs occur before versus during construction.
6. Begin an assumptions register. Do **not** assign probability distributions simply to make the simulation run; first identify credible ranges and how they could be substantiated.

## 13. Limitations

This is a **synthetic teaching and decision-support case study**. The preliminary unit costs and allowances are deliberately illustrative and must not be used for procurement, design authorization, or real-world municipal budgeting without appropriate estimating and market validation. Confidence levels are conditional on the selected assumptions and are not guarantees.
