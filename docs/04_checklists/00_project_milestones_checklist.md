# Capital Budget Monte Carlo — Project Milestones & Checklist

**Project:** What Is the Probability Our Capital Budget Will Be Enough?  
**Scenario:** Hypothetical 15,000 LF, 16-inch municipal water transmission main with an 800 LF HDD crossing beneath the Intracoastal Waterway (ICW)  
**Status as of:** October 26, 2026  
**Environment:** Python 3.12 · uv · pytest  

> **Tracking rule:** `[x]` means completed based on our planning discussions, not independently verified in the local repository. `[ ]` means pending or not yet confirmed. Update this file as work progresses.

## Project status

| Milestone | Description | Status | Exit criterion |
|---|---|---|---|
| M0 | Project definition and planning | Substantially complete | Research questions, scenario, and conceptual workflow documented |
| M1 | Repository and configuration foundation | In progress / verification needed | Repo, dependencies, configurations, and estimate reconciled |
| M2 | Data discovery and acquisition | Not started | Source register and initial credible datasets acquired |
| M3 | Data preparation and EDA | Not started | Cleaned comparable datasets and documented diagnostics |
| M4 | Baseline and uncertainty calibration | Not started | Baseline reviewed; distributions/scenarios justified |
| M5 | Monte Carlo implementation and verification | Not started | Reproducible, tested simulation including schedule risks |
| M6 | Risk analysis and capital decisions | Not started | Budget probabilities, contingency, sensitivity, scenarios |
| M7 | Technical case study and publication | Not started | Reproducible case study and separate LinkedIn article |

## M0 — Define project and analytical concept

**Milestone objective:** Agree on what decision the model supports and which costs and risks are included.

- [x] Define the core question: probability that the authorized capital budget is sufficient.
- [x] Choose hypothetical municipal water transmission main project.
- [x] Include 800 LF ICW HDD crossing and associated permitting/schedule risk.
- [x] Define preliminary pre-contingency estimate: **$10,734,000**.
- [x] Define illustrative 15% owner contingency: **$1,610,100**.
- [x] Define hypothetical authorized capital budget: **$12,344,100**.
- [x] Distinguish estimate, contingency, and budget in documentation.
- [x] Define intended outputs: technical case study, then LinkedIn article.
- [x] Outline data requirements, data workflow, acquisition strategy, and model workflow concept.
- [ ] Confirm all planning documents are present and internally consistent in the repository.

**Deliverables:** `docs/00_project_overview/` and `docs/02_data_management/` planning documents.

## M1 — Repository, inputs, and reproducibility

**Milestone objective:** Establish a working, auditable project before acquiring data.

- [x] Agree on `src/` package, `notebooks/`, `tests/`, `config/`, `docs/`, `data/`, and `outputs/` layout.
- [x] Decide to track `data/` metadata while ignoring `data/raw/`, `data/interim/`, and `data/processed/`.
- [x] Verify Python 3.12 and uv setup (`uv sync`).
- [x] Verify `pyproject.toml`, `uv.lock`, `.python-version`, and `.gitignore` in Git.
- [x] Verify `config/baseline_estimate.csv` exists with ten line items and sums to **$10,734,000**.
- [x] Verify `config/simulation.yaml` uses budget **$12,344,100** and a reproducible random seed.
- [ ] Create or verify `uncertainty_assumptions.yaml` and `schedule_assumptions.yaml`; leave unsupported parameters `null`.
- [ ] Create `data/source_register.csv` with documented columns and no private/raw datasets.
- [ ] Add baseline arithmetic and configuration validation tests.
- [ ] Confirm a clean clone can install dependencies and run tests without private datasets.

**Exit gate:** Baseline is machine-readable, arithmetic is tested, and no unvalidated uncertainty parameters masquerade as evidence.

## M2 — Data source discovery and acquisition

**Milestone objective:** Collect credible evidence for cost, escalation, permitting, and construction risk.

- [ ] Identify candidate Florida municipal water-main bid tabulations and engineer's estimates.
- [ ] Prioritize comparable open-cut and HDD line items; document scope differences.
- [ ] Acquire and register initial bid tabulations with source URLs, dates, units, and retrieval dates.
- [ ] Find appropriate public construction/material price indices and document their limitations.
- [ ] Research USACE/FDEP ICW permitting requirements and seek actual historical approval timelines.
- [ ] Search for construction change-order, differing-site-condition, utility-conflict, and HDD risk evidence.
- [ ] Separate procurement price evidence from post-award cost-growth evidence.
- [ ] Assign provisional evidence grades: A (direct), B (proxy), C (judgment).
- [ ] Record unavailable data and proposed fallback assumptions in an evidence-gap log.

**Exit gate:** At least one usable construction-price dataset and an escalation series are acquired, with documented plans for unresolved permit and construction risks. This is a workflow gate, not a claim of statistical sufficiency.

## M3 — Ingestion, cleaning, normalization, and EDA

**Milestone objective:** Produce comparable, traceable analytical datasets.

- [ ] Ingest original files without altering raw copies.
- [ ] Validate schemas, units, bid dates, project identifiers, and missing fields.
- [ ] Standardize item descriptions and distinguish open-cut from HDD work.
- [ ] Normalize prices to a common date, recording the selected index and assumptions.
- [ ] Investigate outliers, duplicates, project-size effects, and scope differences.
- [ ] Summarize sample sizes, dispersion, temporal patterns, and representativeness.
- [ ] Save processed datasets locally; keep provenance in tracked metadata.
- [ ] Document EDA findings and limitations in notebooks and source notes.

**Exit gate:** Cleaned datasets are reproducible from raw inputs, and limitations are clearly documented.

## M4 — Baseline review and uncertainty specification

**Milestone objective:** Translate evidence into defensible project-specific uncertainty inputs.

- [ ] Compare preliminary unit prices and allowances against normalized historical evidence.
- [ ] Decide whether and how to recalibrate the $10.734M baseline; document any revision.
- [ ] Identify uncertain quantities versus uncertain unit prices.
- [ ] Select candidate distributions or scenario ranges with evidence-based justification.
- [ ] Avoid treating all historical between-project variation as uncertainty in this one project.
- [ ] Evaluate dependencies and correlations, including escalation and schedule timing.
- [ ] Define discrete HDD and utility-conflict events separately from continuous price variation.
- [ ] Define ICW permit-duration scenarios, float, critical-path effects, and delay-driven cost mechanisms.
- [ ] Explicitly avoid double counting escalation, risk allowances, and contingency.
- [ ] Record every assumption, evidence grade, source, units, and sensitivity range.
- [ ] Review distribution diagnostics and stress-test judgment-based parameters.

**Exit gate:** Model inputs are traceable, internally consistent, and clearly marked as empirical, proxy, or judgment-based.

## M5 — Monte Carlo engine and validation

**Milestone objective:** Produce tested, repeatable simulated total project costs.

- [ ] Implement deterministic estimate calculations in `estimate.py`.
- [ ] Implement distribution sampling and parameter checks.
- [ ] Implement permitting schedule, float, and escalation logic.
- [ ] Implement discrete construction risk events.
- [ ] Integrate model components in `simulation.py` with seeded random number generation.
- [ ] Add tests for arithmetic, probabilities, boundary cases, and schedule logic.
- [ ] Test that zero-uncertainty inputs reproduce the deterministic baseline.
- [ ] Test sensitivity to simulation count and random seed; assess convergence.
- [ ] Run a documented baseline scenario and store reproducible summary outputs.

**Exit gate:** Simulation is reproducible, passes tests, and has no unexplained cost duplication.

## M6 — Budget sufficiency and decision analysis

**Milestone objective:** Convert simulation outputs into capital-planning decisions.

- [ ] Calculate probability of staying within / exceeding the authorized budget.
- [ ] Calculate P50, P80, P90, and P95 total project costs.
- [ ] Calculate contingency needed for selected confidence levels.
- [ ] Quantify the additional funding required relative to the authorized budget.
- [ ] Evaluate influential drivers using defensible sensitivity methods.
- [ ] Compare with/without ICW permitting delay and adverse HDD scenarios.
- [ ] Assess alternative procurement timing, early permitting, or risk mitigation scenarios.
- [ ] Explain assumptions, limitations, and consequences of model misspecification.
- [ ] Prepare charts and concise decision tables suitable for the case study.

**Exit gate:** Results directly answer the project research questions and clearly distinguish modeled probability from guaranteed outcomes.

## M7 — Technical case study and LinkedIn article

**Milestone objective:** Publish transparent findings without letting the article dictate the analysis.

- [ ] Write the technical case study: project, data, methods, calibration, findings, limitations, and decisions.
- [ ] Include a baseline-to-budget reconciliation and source/evidence summary.
- [ ] Document model reproducibility, dependencies, and assumptions.
- [ ] Review technical claims and remove unsupported precision.
- [ ] Select and commit only approved final figures/tables to `docs/03_case_study/assets/`.
- [ ] Draft LinkedIn article: *What Is the Probability Our Capital Budget Will Be Enough?*
- [ ] Ensure article figures and statements match the validated case study.
- [ ] Finalize repository README with instructions and links to case study.

**Exit gate:** Standalone case study is complete; LinkedIn article accurately summarizes its results.

## Next actions (current sprint)

- [ ] **1. Verify the configuration files and reconcile the CSV line items to $10,734,000.**
- [ ] **2. Verify the source register schema and local-only dataset ignore rules.**
- [ ] **3. Identify and log the first comparable Florida water-main bid tabulations.**
- [ ] **4. Select an initial cost escalation series and document its suitability.**
- [ ] **5. Start an evidence-gap log for ICW permit durations and HDD risk events.**

## Milestone review log

| Date | Milestone | Decision / evidence | Next action |
|---|---|---|---|
| 2026-10-26 | M0 | Scope and conceptual workflow discussed; files require repository verification | Verify M1 configuration and documentation |

---

**Maintenance:** Check off tasks only after reviewing the corresponding file, dataset, test result, or analytical output. Record material assumption changes and scope changes in the review log.
