# Data Requirements

## Project: What Is the Probability Our Capital Budget Will Be Enough?

**Status:** Data discovery and acquisition  
**Project:** Hypothetical 15,000-LF, 16-inch municipal potable-water transmission main, including an approximately 800-LF HDD crossing of the Intracoastal Waterway (ICW).  
**Primary outcome:** Probability that total project cost exceeds the authorized capital budget, with contingency and schedule-risk analyses.

## 1. Purpose and data principles

Gather evidence to support a reproducible, transparent Monte Carlo cost model. The project scenario is hypothetical; historical data may inform assumptions but must not be presented as data from this specific project.

- Preserve **raw source records** and document provenance, units, dates, and limitations.
- Distinguish **observed historical variation**, **forecast uncertainty**, and **expert judgment**.
- Keep baseline estimate, contingency, and authorized budget separate; avoid double-counting risk allowances.
- Distinguish **base permitting effort** from **incremental permitting delays and costs**.
- Never imply that a fitted distribution is validated solely because a statistical fitting routine returns parameters.
- Use only public, authorized, or appropriately anonymized data; do not include confidential client records.

## 2. Reference project and baseline

| Parameter | Working assumption | Treatment |
|---|---|---|
| Main diameter | 16 inches | Fixed scope descriptor |
| Total alignment | 15,000 LF | Baseline quantity |
| Open-cut installation | 14,200 LF | Baseline quantity, subject to design changes |
| ICW HDD crossing | 800 LF | Baseline quantity, subject to design changes |
| Base construction estimate | ~$8.98 million | Illustrative, to be reconciled with detailed estimate |
| Pre-contingency project cost | ~$10.73 million | Illustrative, to be reconciled |
| Owner contingency | 15% (~$1.61 million) | Budget provision, not a second simulated cost |
| Authorized capital budget | ~$12.34 million | Fixed decision threshold for baseline scenario |

**Reconciliation gate:** Before simulations, recover the line-item estimate underlying these rounded figures, verify extensions and totals, and document what is included in soft costs, permitting, escalation, and contingency. Do not use rounded figures as the authoritative calculation table.

## 3. Data inventory and modeling requirements

| ID | Variable / dataset | Preferred observations | Unit / grain | Candidate sources | Modeling use | Priority | Evidence target |
|---|---|---|---|---|---|---|---|
| C01 | Open-cut main installation bid prices | Bid tabs with pipe material, diameter, depth, location, date, quantities | USD/LF per bid item | Florida municipal and utility procurement portals, bid tabulations | Unit-price uncertainty | Critical | A/B |
| C02 | HDD installation bid prices | HDD bid items with diameter, casing/carrier, length, ground conditions | USD/LF per crossing | Public bid tabs and awarded contracts | HDD baseline and variability | Critical | A/B |
| C03 | Valves, fittings, connections | Bid items and quantities | USD/EA or LS | Utility bid tabs | Component costs | High | A/B |
| C04 | Pavement and site restoration | Bid items by surface and extent | USD/SY, USD/LF, or LS | Bid tabs | Restoration variability | High | A/B |
| C05 | MOT, mobilization, testing | Comparable bid items | USD/LS or item | Bid tabs | Other direct construction costs | Medium | B |
| Q01 | Quantity changes | Original estimated versus final installed quantities | % deviation by item/project | Public change orders, closeout reports | Quantity uncertainty | High | B/C |
| R01 | Utility conflicts / differing conditions | Event, consequence, mitigation, extra cost | Event and USD/event | Change orders, claims, case studies | Discrete risk events | High | B/C |
| R02 | HDD geotechnical/construction risks | Inadvertent returns, tooling, bore modifications, obstruction events | Event and USD/event | Public HDD reports, change orders, engineering case studies | HDD event likelihood and severity | High | B/C |
| E01 | Construction escalation | Dated cost index observations | Index/month or quarter | BLS PPI series, other documented indices | Time-adjust historical costs; future escalation | Critical | A/B |
| P01 | ICW permitting timeline | Application, completeness, RAI, approval dates, permit type | Calendar days per permit | USACE district public records, Florida DEP public records, permit files | Approval-duration uncertainty | Critical | B/C |
| P02 | Permitting direct costs | Application fees, studies, consultant effort | USD/permit or hours | Agency schedules, public contracts, scope documents | Base/incremental permitting cost | Medium | B/C |
| S01 | Project schedule logic | Permitting start, design milestones, float, bid/award, NTP | Dates, months, dependencies | Hypothetical baseline schedule; documented project examples | Map approval time to critical-path delay | Critical | B/C |
| S02 | Delay cost consequences | Staff extension, remobilization, bid-date shifts | USD/month, %/year, event cost | Public contracts, indices, judgment | Translate delay into cost | High | B/C |
| F01 | Soft costs | Engineering, construction administration, testing, owner costs | USD or % by category | Public capital program estimates | Complete project cost | High | B/C |

### Evidence classifications

- **A — Direct:** Relevant, traceable historical observations closely matching the modeled quantity or mechanism.
- **B — Proxy:** Related data requiring adjustments or extrapolation; explain the differences.
- **C — Judgment:** Explicitly documented engineering assumptions without sufficient empirical observations.

These are **evidence-strength labels**, not statistical confidence intervals. Reassess classifications after data acquisition.

## 4. Minimum fields for source records

### Bid-item records

`source_id`, `source_url_or_reference`, `agency`, `project_id`, `project_name`, `bid_date`, `award_status`, `bidder`, `item_description`, `item_category`, `installation_method`, `pipe_diameter_in`, `pipe_material`, `quantity`, `unit`, `unit_price_usd`, `extended_price_usd`, `location`, `notes`.

Record whether a price is a bid, engineer's estimate, awarded contract price, or final paid amount. Do not pool them without labeling.

### Permit timeline records

`source_id`, `agency`, `permit_type`, `project_id`, `crossing_type`, `application_date`, `complete_application_date`, `rai_dates`, `approval_date`, `conditions`, `notes`.

A missing approval date may represent an open application, not a zero-day review. Distinguish elapsed calendar time from statutory review clocks and applicant-response time.

### Escalation records

`series_id`, `publisher`, `geography`, `industry_or_material`, `observation_date`, `index_value`, `base_period`, `retrieved_date`.

## 5. Data comparability and normalization

1. Classify open-cut and HDD separately. Do not use open-cut pricing as an HDD proxy without a documented adjustment.
2. Normalize units and check that item descriptions have comparable scope inclusions.
3. Preserve original nominal prices; create separate inflation-adjusted analytical fields using a declared base date and index.
4. Track bid year, location, size, quantity, material, and delivery context to assess comparability.
5. Avoid treating multiple bids on one project as independent realized construction outcomes.
6. Record exclusions and outliers with reasons, retaining the original records.
7. Flag sparse samples and uncertainty from project-to-project differences.

## 6. Specific ICW HDD and permitting requirements

The HDD crossing is a distinct subsystem. Model three linked mechanisms rather than a single generic risk percentage:

- **Direct HDD cost uncertainty:** bore length, geotechnical conditions, contractor pricing, and discrete construction events.
- **Permit approval uncertainty:** time to obtain all required authorizations for the *hypothetical* crossing. Determine likely permitting pathways only after specifying location, jurisdiction, and environmental impacts; do not assume a particular permit or duration applies universally.
- **Schedule-to-cost propagation:** only the portion of approval delay that affects critical-path milestones produces schedule-related cost. Separate escalation caused by delayed procurement from prolonged engineering/administration or remobilization costs.

Candidate logic:

`critical_path_delay_months = max(0, permit_completion_month - required_permit_month - available_float_months)`

The actual schedule model may need multiple permit dependencies and calendar conversion. Apply escalation only to cost categories and procurement timing genuinely affected by the delay. Account for possible correlation between environmental complexity, permit duration, and HDD scope.

## 7. Preliminary modeling decisions to research, not assume

| Question | Required evidence / decision |
|---|---|
| How should unit prices vary? | Comparable bid-price distributions or defensible bounded assumptions |
| Are cost drivers correlated? | Common market factors, joint bid behavior, and sensitivity scenarios |
| How likely are HDD adverse events? | Documented events or explicitly judgment-based probability scenarios |
| What determines permit duration? | Actual application-to-approval histories, completeness and RAI effects |
| How does delay change cost? | Bid/award date logic, index behavior, monthly extension costs |
| What does contingency cover? | Clear mapping to modeled cost items and excluded exposures |
| Which percentiles matter? | P50/P80/P90/P95 total cost and required contingency |

## 8. Source register and quality checks

Maintain `data/source_register.csv` or equivalent with source ID, publisher, URL/path, retrieved date, coverage, licensing/access, evidence grade, intended use, and caveats.

For each dataset, check: provenance; unit consistency; duplicates; missingness; date validity; quantity × unit price reconciliation; item-category mapping; comparable scope; and sample size. Never silently replace missing values with zero.

## 9. Data acquisition acceptance criteria

- [ ] Detailed deterministic estimate reconciles to baseline and budget.
- [ ] Each model variable has a source, proxy, or explicit judgment rationale.
- [ ] Bid-price data distinguish installation methods and bid/award/final cost status.
- [ ] Escalation series and price normalization methodology are documented.
- [ ] Permit timeline assumptions distinguish agency time, applicant response, and schedule float.
- [ ] Discrete HDD risk and permit delay are not counted again in generic contingency or unit-price variability.
- [ ] Missing or weak evidence is visible in an assumptions and limitations register.
- [ ] Raw data and transformations are reproducible without confidential information.

## 10. Deliverables

- `data_requirements.md` (this document)
- `data/source_register.csv`
- `data/raw/` preserved source data and source notes
- `data/processed/` standardized analysis-ready datasets
- `docs/assumptions_register.md` with evidence grade, units, bounds, and rationale
- `docs/data_quality_report.md` with limitations and data gaps

**Exit criterion:** The team can explain the empirical or judgmental basis of every sampled model input before implementing the full Monte Carlo engine.
