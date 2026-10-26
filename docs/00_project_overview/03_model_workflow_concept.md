# Capital Budget Monte Carlo: Modeling Workflow Concept

**Project:** What Is the Probability Our Capital Budget Will Be Enough?  
**Status:** Conceptual methodology, prior to data acquisition and model calibration  
**Case:** Hypothetical municipal 16-inch water transmission main, including an 800 LF HDD crossing beneath the Intracoastal Waterway (ICW)

## 1. Purpose

This document explains how the hypothetical project estimate, historical data, uncertainty modeling, and Monte Carlo simulation fit together. The objective is to estimate the probability that the total project cost will remain within the authorized capital budget and to quantify contingency requirements at selected confidence levels.

The hypothetical estimate defines **the project being evaluated**. Historical evidence helps establish **what costs and delays are plausible**. The Monte Carlo model combines these inputs to generate a distribution of possible total project costs.

## 2. Starting Point: Hypothetical Project and Budget

| Component | Preliminary amount |
|---|---:|
| Base construction | $8,984,000 |
| Design and permitting | $1,050,000 |
| Construction engineering and inspection (CEI) | $700,000 |
| **Pre-contingency baseline estimate** | **$10,734,000** |
| Owner contingency (15% of baseline) | $1,610,100 |
| **Authorized hypothetical capital budget** | **$12,344,100** |

These figures are illustrative assumptions, **not validated historical bid prices**. The baseline estimate is maintained in `config/baseline_estimate.csv`; the authorized budget is maintained in `config/simulation.yaml`. Contingency is a funding allowance, not an extra cost added to each simulation result.

The budget is held fixed for the initial sufficiency test. If evidence suggests the baseline unit prices are unrealistic, revise the estimate transparently and document both the original and calibrated cases. Do not quietly change the authorized budget to preserve a desired probability.

## 3. End-to-End Workflow

```text
Hypothetical project scope and baseline estimate
                     |
                     v
Identify and acquire relevant historical evidence
                     |
                     v
Ingest, clean, normalize, and evaluate comparability
                     |
                     v
Calibrate baseline assumptions and model uncertainties
                     |
                     v
Simulate costs, discrete risks, and schedule effects
                     |
                     v
Compare simulated total costs with authorized budget
                     |
                     v
Report probabilities, contingency needs, sensitivities
                     |
                     v
Document findings in technical case study
```

### Phase 1 — Define the decision

**Question:** Is the authorized $12,344,100 budget likely to be sufficient for this hypothetical project?

Define scope, quantities, estimate basis, pricing date, assumed procurement date, construction schedule, ICW crossing, and the treatment of owner costs. Establish the distinction between pre-contingency estimate, contingency allowance, and total authorized budget.

**Output:** Versioned baseline estimate and project assumptions register.

### Phase 2 — Acquire supporting data

Collect, where accessible:

- Comparable water-main bid tabulations, with open-cut and HDD items separated.
- Engineer's estimates and awarded bid prices, where available.
- Construction and material price indices for escalation.
- Actual permit application, information-request, and approval dates for relevant waterway crossings.
- Change orders, differing-site-condition records, and HDD construction-risk evidence.
- Estimate-to-actual quantities and project costs, where available.

Record source, retrieval date, project characteristics, units, price year, and limitations. Track source metadata in `data/source_register.csv`; keep raw, interim, and processed datasets out of Git.

**Output:** Source inventory and local raw datasets.

### Phase 3 — Prepare and assess evidence

Normalize unit prices to a consistent basis where defensible. Examine bid year, geography, diameter, installation method, project size, soil conditions, and included scope. Keep original values and document adjustments. Inspect missing data, outliers, sample sizes, and selection bias.

Do **not** assume that variation among historical bids directly represents the uncertainty of this specific future project. Different scopes and market conditions can create variation unrelated to the risk being modeled.

**Output:** Validated analysis-ready datasets, exploratory analysis, and an evidence-gap log.

### Phase 4 — Develop the uncertainty model

Use the evidence to select and justify plausible distributions or scenarios for:

1. **Bid-price uncertainty:** Future open-cut and HDD unit prices and other construction components.
2. **Quantity uncertainty:** Changes in installed quantities, where applicable.
3. **Post-award risk events:** Utility conflicts, differing site conditions, HDD complications, and associated cost consequences.
4. **Schedule uncertainty:** ICW permitting duration, approval delays, and critical-path impacts.
5. **Escalation and extended services:** Later procurement prices, additional engineering/CEI effort, or other justified delay costs.

Classify evidence as **A: directly relevant historical observations**, **B: related/proxy evidence**, or **C: documented engineering judgment**. Use sensitivity or scenario analysis when observations are too sparse for defensible statistical fitting. Identify correlations and avoid double-counting escalation or risks already embedded in unit prices.

**Output:** Documented and version-controlled uncertainty assumptions with source references.

### Phase 5 — Run the Monte Carlo simulation

For each iteration, sample uncertain quantities, unit prices, risk events, and schedule outcomes; calculate the resulting **total project cost before contingency**; then compare it against the fixed authorized capital budget.

Conceptually:

\[
C_{\mathrm{total}} = C_{\mathrm{construction}} + C_{\mathrm{design/permit}} + C_{\mathrm{CEI}} + C_{\mathrm{risk}} + C_{\mathrm{delay}}
\]

Only include distinct cost effects once. For example, if a delay escalates a future bid price, do not also apply an independent escalation allowance to that same cost. Account for available schedule float before treating a permitting delay as a critical-path delay.

The initial planned run is 100,000 iterations with a fixed random seed, subject to convergence and reproducibility checks.

**Output:** Simulated project-cost distribution and scenario results.

### Phase 6 — Evaluate the capital decision

Calculate:

- **Probability of budget sufficiency:** `P(total_cost <= authorized_budget)`.
- **Probability of overrun:** `P(total_cost > authorized_budget)`.
- **P50/P80/P90 costs:** Total-cost percentiles from the simulated distribution.
- **Required contingency at confidence level p:** `max(0, Pp_total_cost - baseline_cost)`.
- **Additional funding for confidence level p:** `max(0, Pp_total_cost - authorized_budget)`.
- **Sensitivity:** Which assumptions most affect cost variability and budget-exceedance risk.
- **Scenario comparisons:** With/without selected ICW permitting or HDD risks; alternative permitting and mitigation strategies.

The confidence levels reflect the assumptions and evidence used in the model. They are **conditional model estimates**, not guarantees.

**Output:** Budget sufficiency analysis, contingency recommendations, risk-driver charts, and scenario comparisons.

### Phase 7 — Publish the technical case study

Describe the hypothetical project, data sources, assumptions, model logic, diagnostics, findings, limitations, and capital planning implications. The LinkedIn article is a later, accessible explanation of the completed analysis, not the basis for selecting assumptions or results.

**Output:** Standalone technical case study with reproducible code and clearly labeled illustrative inputs.

## 4. Example: How Bid Data Enters the Model

Suppose the preliminary estimate assumes **$340/LF** for an open-cut water-main item. That figure is a starting assumption, not a statistically established market price.

After obtaining comparable historical bids, normalize prices where justified and evaluate whether $340/LF is credible for the defined project and pricing date. Then estimate an appropriate future-price distribution or scenario range. In each simulation iteration, sample a possible unit price and multiply by the modeled quantity. Repeat for other relevant cost components.

**Important:** Historical bid data primarily informs procurement-price uncertainty. It does not, by itself, establish the probability or magnitude of construction change orders.

## 5. Special Treatment of the ICW HDD Crossing

The ICW crossing introduces two related but distinct exposures:

- **Direct construction exposure:** HDD bid prices, geology, drilling conditions, and discrete construction complications.
- **Schedule exposure:** Permitting duration, information requests, approval restrictions, and any resulting critical-path delay.

Schedule delays create additional costs only through a specified mechanism, such as delayed procurement escalation, extended owner/engineering services, remobilization, or contractor delay costs supported by the assumed contract and schedule. A long permit review does not automatically mean an equal construction delay or an overrun.

Compare at least a baseline scenario and an ICW-risk scenario, while documenting whether the baseline already includes normal permitting effort and expected schedule duration.

## 6. What This Project Is—and Is Not

**This project is:** A decision-support analysis of the probability that an authorized capital budget will cover a hypothetical infrastructure project's eventual costs.

**This project is not:** A test that proves the preliminary estimate is accurate, a prediction of the exact final cost, or a guarantee that historical prices will recur.

The baseline can be recalibrated as evidence improves. The approved hypothetical budget remains an explicit decision threshold, and changes to it must be shown as separate funding scenarios.

## 7. Immediate Next Steps

1. Verify that `baseline_estimate.csv` reproduces the documented $10,734,000 pre-contingency baseline.
2. Confirm that `simulation.yaml` uses the exact $12,344,100 hypothetical budget.
3. Begin historical bid-data acquisition and complete the source register.
4. Analyze comparability and identify evidence gaps before assigning probability distributions.
5. Document any baseline recalibration before implementing the simulation.

**Working principle:** Project estimate → historical evidence → defensible uncertainty assumptions → Monte Carlo simulation → capital budget decision → technical case study.
