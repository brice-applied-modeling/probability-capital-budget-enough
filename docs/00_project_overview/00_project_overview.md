# What Is the Probability Our Capital Budget Will Be Enough?

## Project Overview

**Project type:** Python Monte Carlo simulation and infrastructure capital-budget risk analysis  
**Application:** Hypothetical municipal water transmission main  
**Primary output:** Reproducible analysis supporting a LinkedIn Engineering Judgment article  
**Status:** Project definition; assumptions and results not yet validated

## 1. Purpose

Estimate the probability that the total cost of a proposed municipal water transmission main will remain within an approved capital budget. Use Monte Carlo simulation to translate uncertainty in construction costs into decision-useful measures of budget sufficiency.

The analysis will demonstrate why a single deterministic estimate plus a flat contingency percentage does not, by itself, communicate the probability of a budget overrun.

## 2. Decision Question

> Given a defined project scope, an approved capital budget, and plausible uncertainty in project costs, what is the probability of completing the project within budget?

Supporting questions:

- What is the distribution of possible total project costs?
- What are the P50, P80, and P90 cost estimates?
- How much contingency is required to reach selected confidence levels?
- Which uncertain cost drivers contribute most to the range of outcomes?
- How does the probability of staying within budget change when the budget changes?

**Important:** A P80 cost is the simulated 80th percentile of total cost, not a guarantee of 80% confidence in real-world performance. Its usefulness depends on model quality and assumptions.

## 3. Illustrative Infrastructure Scenario

A municipality plans to construct a new water transmission main. The project includes pipe installation, excavation and backfill, pavement restoration, fittings and appurtenances, traffic control, and other construction costs. The owner must decide whether its proposed capital allocation is sufficient.

For the initial model, define a transparent baseline estimate and approved budget using **explicitly hypothetical values**. No results should be represented as observed municipal costs or calibrated industry benchmarks.

### Baseline cost structure

| Cost component | Baseline input | Potential uncertainty |
|---|---|---|
| Pipe material and installation | Quantity × unit price | Quantity changes, material pricing, productivity |
| Excavation and backfill | Quantity × unit price | Ground conditions, excavation depth, disposal |
| Pavement restoration | Area × unit price | Restoration limits, material prices |
| Fittings and appurtenances | Allowance or itemized quantities | Additional valves, fittings, field adjustments |
| Traffic control | Allowance | Duration and staging requirements |
| Other direct construction costs | Allowance | Miscellaneous construction scope |

**Cost boundaries:** Define whether the approved budget covers construction only or total capital expenditure. If total capital expenditure, include design, permitting, land/easements, construction administration, and other owner costs consistently in both simulated costs and budget. Do not mix cost bases.

## 4. Modeling Approach

### Phase 1: Deterministic estimate

Create a baseline estimate from quantities, unit prices, and allowances. Record assumptions, units, estimate date, scope exclusions, and contingency treatment. Set an illustrative approved budget separately from the baseline estimate.

### Phase 2: Uncertainty definitions

Assign defensible probability distributions to uncertain inputs. Candidate distributions include:

- **Triangular:** bounded minimum, most-likely, and maximum values when only structured engineering judgment is available.
- **PERT / beta-PERT:** bounded expert-elicited uncertainty with smoother tails, if justified.
- **Lognormal:** positive, right-skewed unit-price or cost factors where supported by evidence.
- **Discrete scenarios:** identifiable events such as unexpected utility conflicts or difficult subsurface conditions.

Start with a small set of high-impact variables. Distinguish ordinary estimating uncertainty from discrete risk events, and avoid double-counting the same risk in multiple cost categories.

### Phase 3: Monte Carlo simulation

Using Python and NumPy:

1. Draw one realization of each uncertain input per iteration.
2. Calculate the total project cost for that iteration.
3. Repeat for an initial **20,000 iterations**, using a fixed random seed for reproducibility.
4. Compare simulated total costs with the approved budget.
5. Calculate exceedance probability and cost percentiles.

Initial independence assumptions should be explicit. Later, test correlation between cost drivers sharing inflation, market conditions, or schedule exposure; independence can materially understate aggregate risk.

### Phase 4: Decision analysis

Report:

| Metric | Definition |
|---|---|
| Probability within budget | Fraction of simulated total costs ≤ approved budget |
| Probability of overrun | Fraction of simulated total costs > approved budget |
| Expected cost | Mean simulated total cost |
| P50 / P80 / P90 | 50th / 80th / 90th percentile total costs |
| Required contingency at P80 | P80 total cost minus baseline cost, with basis stated |
| Budget gap at P80 | P80 total cost minus approved budget |
| Expected overrun exposure | Mean of max(simulated cost − budget, 0) across all iterations |

Also evaluate sensitivity to key assumptions and alternative approved-budget levels.

## 5. Project Structure

```text
capital-budget-monte-carlo/
├── README.md
├── project_overview.md
├── pyproject.toml
├── data/
│   ├── raw/                  # Source assumptions and supporting inputs
│   └── processed/            # Validated modeling inputs
├── notebooks/
│   ├── 01_input_review.ipynb
│   ├── 02_monte_carlo_analysis.ipynb
│   └── 03_sensitivity_and_scenarios.ipynb
├── src/
│   └── capital_budget_mc/
│       ├── __init__.py
│       ├── inputs.py
│       ├── simulation.py
│       └── metrics.py
├── tests/
│   ├── test_simulation.py
│   └── test_metrics.py
├── outputs/
│   ├── figures/
│   └── tables/
└── docs/
    ├── assumptions.md
    └── article_findings.md
```

## 6. Technology

- **Python:** modeling and analysis
- **NumPy:** random sampling and vectorized simulation
- **pandas:** structured inputs and results
- **Matplotlib:** publication-ready charts
- **pytest:** validation and reproducibility tests
- **uv:** project and dependency management
- **Spyder or Jupyter:** exploratory analysis and model development

Keep core simulation logic in reusable Python modules rather than embedding everything in notebooks.

## 7. Planned Visualizations

1. **Total-cost distribution:** histogram with baseline estimate, approved budget, P50, P80, and P90 marked.
2. **Cost exceedance curve:** probability that total cost exceeds a given budget level.
3. **Budget sufficiency curve:** probability of staying within budget over a range of possible allocations.
4. **Sensitivity chart:** impact of uncertain inputs on total-cost outcomes, using an appropriate sensitivity measure.

Clearly label all values as hypothetical and avoid implying precision beyond the quality of assumptions.

## 8. Validation and Quality Control

- Confirm units and arithmetic in the deterministic baseline.
- Ensure all sampled costs and quantities respect realistic constraints.
- Test that fixed-seed runs are reproducible.
- Verify probability within budget plus probability of overrun equals 1.
- Check that percentiles increase monotonically.
- Compare simulated results with simple hand-calculated or deterministic cases.
- Check stability of key percentiles and budget probabilities across sample sizes and seeds.
- Run sensitivity cases for distribution choice, correlation, and major risk-event assumptions.
- Document what the model does **not** capture, including scope changes, systemic market shocks, and schedule escalation unless modeled explicitly.

## 9. Scope Boundaries

### Included in the initial version

- Single-project capital-budget analysis
- Conceptual cost estimate with hypothetical inputs
- Construction cost uncertainty
- Reproducible Monte Carlo simulation
- Budget sufficiency and contingency analysis
- Sensitivity testing and decision-focused charts

### Deferred

- Multi-project capital improvement program optimization
- Detailed construction schedule risk analysis
- Formal probabilistic calibration to historical bid data
- Discounted cash-flow and financing analysis
- Portfolio-level prioritization
- Web application or API

These extensions should not distract from the central decision question.

## 10. Deliverables and Acceptance Criteria

**Deliverable A: Baseline estimate and assumption register**  
A transparent estimate with cost categories, units, baseline quantities, baseline prices, budget basis, distribution parameters, and rationale.

**Deliverable B: Reproducible Python model**  
A simulation that runs from a clean environment, uses a documented random seed, and passes basic tests.

**Deliverable C: Decision analysis**  
A table reporting budget sufficiency, exceedance risk, cost percentiles, contingency requirements, and sensitivity findings.

**Deliverable D: Figures**  
Clear, labeled plots suitable for adaptation into a LinkedIn article.

**Deliverable E: Engineering Judgment article**  
A plain-language explanation of how Monte Carlo simulation helps municipal owners distinguish a budget allowance from a quantified probability of sufficiency. Include caveats about assumption quality and avoid presenting hypothetical model outputs as real project evidence.

## 11. Recommended Work Sequence

| Step | Assignment | Completion test |
|---|---|---|
| 1 | Define project scope and cost boundary | Scope and budget cover the same items |
| 2 | Build baseline cost estimate | Costs reconcile and assumptions are documented |
| 3 | Select uncertainty distributions | Parameters and reasoning recorded |
| 4 | Implement and test simulation | Reproducible results and passing tests |
| 5 | Calculate budget risk metrics | Probabilities and percentiles validated |
| 6 | Run sensitivity and scenario tests | Key conclusions tested for robustness |
| 7 | Produce article charts and findings | Decision narrative supported by results |
| 8 | Draft LinkedIn article | Accessible explanation with defensible conclusions |

## 12. Editorial Thesis

> A capital budget is not simply sufficient or insufficient because an estimate falls below it. Budget sufficiency is a probability, conditional on the risks and uncertainties represented in the estimate.

The article should emphasize that Monte Carlo simulation does not eliminate uncertainty. It makes assumptions visible, translates them into possible outcomes, and helps decision-makers ask better questions about risk tolerance and contingency.
