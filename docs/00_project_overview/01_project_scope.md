# Project Scope

## What Is the Probability Our Capital Budget Will Be Enough?

**Project type:** Standalone applied infrastructure analytics project  
**Primary method:** Monte Carlo simulation in Python  
**Application:** Municipal water transmission main capital budgeting  
**Final deliverables:** Reproducible analytical model, technical case study, and derivative LinkedIn article

## 1. Project Purpose

Evaluate how uncertainty in infrastructure project costs affects the probability that a proposed capital budget will be sufficient. Translate the simulation into defensible capital planning decisions rather than presenting a probability distribution without a decision context.

The project will use a **hypothetical municipal water transmission main** and clearly labeled illustrative inputs. It will not claim that assumed distributions or results represent a specific utility's historical experience.

## 2. Central Decision Question

> Given a defined project scope, a baseline cost estimate, an approved budget, and explicit uncertainty assumptions, what is the probability that the final project cost will remain within the available capital budget?

A related decision is how much funding should be reserved to reach a specified level of budget confidence, and which uncertainties merit the greatest management attention.

## 3. Research and Decision Questions

### A. Budget sufficiency

1. What is the probability that total project cost is at or below the approved capital budget?
2. What is the probability and potential magnitude of a budget overrun?
3. What is the distribution of simulated final costs, including median, mean, and upper-tail outcomes?
4. How does the answer change if the approved budget is increased or reduced?

### B. Contingency adequacy

5. Is a conventional fixed-percentage contingency sufficient under the stated assumptions?
6. What total budget corresponds to P50, P80, P90, and P95 cost outcomes?
7. What additional contingency would be needed to reach a selected confidence target?
8. How much additional funding is required when moving from one confidence target to another?

### C. Drivers of uncertainty

9. Which uncertain cost components contribute most to variation in total cost?
10. How do quantity uncertainty, unit-price uncertainty, differing construction conditions, and escalation affect outcomes?
11. How sensitive are conclusions to distribution choice, parameter ranges, and dependence between costs?
12. Are there concentrated downside risks that a single contingency percentage obscures?

### D. Scenarios and management decisions

13. How do base, favorable, and adverse market/construction scenarios change the probability of staying within budget?
14. Which risk-reduction actions could materially improve budget confidence, assuming their costs and effects can be reasonably estimated?
15. What are the tradeoffs between committing more contingency now, accepting overrun exposure, or refining the estimate before approval?
16. How should decision-makers communicate the difference between an expected cost and a confidence-based funding requirement?

## 4. Illustrative Project Scenario

A municipality is planning a water transmission main. The deterministic estimate will include defined quantities and unit costs for major construction items, plus appropriate project-level costs and allowances. The project will establish an **illustrative baseline estimate and capital budget**, with all values documented and internally consistent.

Potential cost elements include:

- Open-cut pipe installation and associated restoration
- Trenchless crossings, where included in the defined alignment
- Valves, fittings, appurtenances, and connections
- Traffic control, dewatering, and temporary construction measures
- Engineering, permitting, construction-phase services, and owner costs, if within the selected budget boundary
- Escalation and identified construction risk allowances, where applicable

**Budget boundary:** Before modeling, specify whether the analysis covers construction only or total project capital cost. The deterministic estimate, uncertainty model, and approved budget must use the same boundary.

## 5. Analytical Approach

### Phase 1: Define the decision and estimate

- Write the project scope, cost-estimate basis, budget boundary, and assumed delivery timing.
- Build an auditable deterministic estimate with quantities, unit costs, subtotals, and allowances.
- Establish the proposed budget and identify what contingency is already included.
- Create a data dictionary identifying every input, its units, and its source or rationale.

### Phase 2: Model uncertainty

- Identify uncertain quantities, unit costs, escalation, and discrete construction risks.
- Assign appropriate distributions and document parameter choices and rationale.
- Distinguish ordinary estimating uncertainty from discrete risk events and broader market scenarios.
- Consider correlation between related variables, avoiding unjustified independence.
- Prevent double counting between contingency allowances and modeled risk costs.

### Phase 3: Run and validate Monte Carlo simulation

- Implement a reproducible simulation in Python with a fixed random seed and configurable number of iterations.
- Generate component-level and total project cost outcomes.
- Confirm arithmetic consistency, plausible bounds, and reproducibility.
- Check convergence of key statistics and exceedance probabilities as simulation size increases.
- Test the model against deterministic and simplified limiting cases.

### Phase 4: Evaluate budget decisions

- Estimate probability of remaining within the approved budget and the overrun distribution.
- Calculate P50/P80/P90/P95 total costs and implied contingencies.
- Compare alternative funding levels and scenarios.
- Perform sensitivity analysis to identify important uncertainty drivers.
- Where defensible, compare risk-reduction actions against their estimated cost and effect.

### Phase 5: Produce technical case study

- Explain project context, inputs, assumptions, model design, and validation.
- Present results with clear graphics and decision-focused interpretation.
- Discuss what the model does **not** establish and where better data would improve confidence.
- Summarize actionable implications for capital planning.

### Phase 6: Derivative LinkedIn article

- Develop *What Is the Probability Our Capital Budget Will Be Enough?* from the completed case study.
- Use a limited number of verified findings and approachable graphics.
- Avoid presenting illustrative probabilities as empirical industry benchmarks.

## 6. Key Outputs and Visualizations

| Output | Decision value |
|---|---|
| Total-cost histogram / density | Shows the range and shape of possible outcomes |
| Cumulative cost probability curve | Shows probability of staying within any candidate budget |
| Budget threshold on cost distribution | Makes sufficiency and exceedance risk visible |
| P50, P80, P90, P95 cost table | Connects confidence targets to funding requirements |
| Contingency-versus-confidence curve | Shows marginal funding needed for greater confidence |
| Overrun probability and severity summary | Distinguishes chance of overrun from potential impact |
| Sensitivity / driver ranking | Identifies uncertainties most worth investigating |
| Scenario comparison | Shows exposure to market and construction conditions |
| Simulation convergence check | Demonstrates numerical stability |

## 7. Technical Scope

**Language:** Python  
**Likely packages:** NumPy, pandas, SciPy, Matplotlib; additional packages only when justified  
**Inputs:** Version-controlled illustrative estimate and assumption tables (CSV or similar)  
**Workflow:** Separate input preparation, simulation, analysis, and reporting steps  
**Reproducibility:** Document environment, assumptions, random seed, and execution steps

The initial deliverable is an analysis, **not** a deployed application, dashboard service, or machine learning system.

## 8. Assumptions and Limitations

- The project is illustrative; no historical cost calibration is assumed at the outset.
- Simulation probabilities are **conditional on the specified input assumptions**, not objective guarantees.
- Tail results, especially P95, may be sensitive to distribution and correlation assumptions.
- Correlation, escalation, scope changes, and discrete risks must be addressed explicitly or disclosed as limitations.
- Risk reduction will be evaluated only when its cost and effect can be supported by reasonable assumptions.
- The model is not a replacement for estimating, engineering judgment, or project-specific risk review.

## 9. Out of Scope

- Predicting actual bid prices for a named municipality or contractor
- Calibrating distributions from proprietary historical cost databases
- Construction schedule simulation or a full integrated cost-schedule risk model
- Full portfolio-level capital improvement program optimization
- Machine learning prediction, web application development, or cloud deployment
- Formal investment appraisal, financing structure, or rate impact analysis

These may be considered future extensions, but should not dilute the initial case study.

## 10. Deliverables and Acceptance Criteria

| Deliverable | Completion standard |
|---|---|
| Scope and decision framework | Clear research questions, budget boundary, and exclusions |
| Deterministic cost estimate | Auditable quantities, unit costs, allowances, and total |
| Uncertainty register | Each variable/risk has a distribution or scenario, units, and rationale |
| Python simulation | Reproducible runs and saved results |
| Validation and sensitivity results | Numerical checks and material assumptions tested |
| Decision exhibits | Budget sufficiency, percentiles, contingency, overrun exposure, and drivers |
| Technical case study | Reproducible methodology, defensible interpretation, and limitations |
| LinkedIn article | Accurate, accessible synthesis based on completed analysis |

**Success criterion:** A reviewer can trace each major conclusion from the stated budget and estimate, through explicit uncertainty assumptions and Python outputs, to a capital funding decision. The result should answer **what budget confidence is being purchased, at what cost, and under which assumptions**.

## 11. First Implementation Milestone

Before coding the simulation:

1. Define a realistic hypothetical water main scope and estimate boundary.
2. Create the deterministic cost estimate with major bid items and allowances.
3. Choose the proposed budget and identify its embedded contingency.
4. Draft the uncertainty register, with reasons for each proposed distribution.
5. Define the first three required outputs: budget sufficiency probability, P80 funding requirement, and dominant uncertainty drivers.

**Milestone gate:** Do not interpret simulation outputs until the estimate basis, uncertainty definitions, and budget boundary are documented.
