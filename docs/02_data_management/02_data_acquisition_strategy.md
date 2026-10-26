# Data Acquisition Strategy

**Project:** What Is the Probability Our Capital Budget Will Be Enough?  
**Case:** Hypothetical 16-inch municipal water transmission main, including an 800-LF HDD crossing of the Intracoastal Waterway (ICW)  
**Status:** Research plan; sources and assumptions are not yet validated  
**Companion documents:** `00_data_requirements.md`, `01_data_workflow.md`, `project_scenario_and_cost_estimate.md`

## 1. Purpose

Acquire, document, and assess evidence to support a reproducible Monte Carlo model of capital budget sufficiency. The objective is **not** to collect the largest possible dataset; it is to obtain relevant observations that justify the baseline estimate, uncertainty ranges, dependence assumptions, schedule-risk mechanisms, and sensitivity cases.

The model must distinguish uncertainty in the cost of procuring construction from changes after award. Historical bid dispersion alone does not measure final-cost overrun risk.

## 2. Questions the Evidence Must Support

1. What is a defensible baseline cost for each major work item in the conceptual estimate?
2. How much can unit prices and installed quantities vary for comparable work?
3. How do construction costs change between estimate date, bid date, and construction?
4. What is the likely range of ICW crossing permit-review durations and how often might reviews require additional information?
5. Under what circumstances would permitting delay procurement or the critical path, and what costs would that cause?
6. How often might discrete HDD or utility-conflict events occur, and what could they cost?
7. How do correlations and limited evidence affect the probability that the capital budget is sufficient?
8. Which additional data would most improve confidence in a capital funding decision?

## 3. Scope and Acquisition Priorities

| Priority | Evidence category | Target records | Use in model | Main limitations |
|---|---|---|---|---|
| P0 | Comparable construction bids | Bid tabulations, bid schedules, engineer's estimates, bid dates, item descriptions | Baseline and pre-award unit-price variability | Scope, location, quantity, and date differences |
| P0 | Construction escalation | BLS PPI series, other documented indices relevant to pipeline materials and construction | Cost movement between estimate and procurement | Indices are proxies, not local bid-price forecasts |
| P0 | ICW permitting history | Public permit files, submittal and decision dates, RAI dates, conditions | Permit-duration scenarios and possible delay drivers | Sparse comparable cases; publication dates may differ from actual milestones |
| P1 | HDD crossing costs and conditions | Comparable HDD bid items, crossing geometry, geotechnical context, change orders | HDD unit-price uncertainty and discrete risk | Small sample; work scopes rarely identical |
| P1 | Quantity and post-award changes | Original vs final quantities, change orders, final contract amounts | Quantity variation and construction cost growth | Public records may not itemize root causes |
| P1 | Delay cost mechanisms | Contract provisions, documented time-related costs, professional service extensions | Translate critical-path delays to dollars | Highly contract-specific |
| P2 | Utility conflicts and restoration | Utility adjustment records, bid items, change orders | Scenario and discrete event inputs | Inconsistent coding across projects |

**Milestone rule:** Begin with P0 categories; do not postpone the entire project waiting for a perfect HDD risk dataset.

## 4. Candidate Sources and Search Paths

These are **research targets, not verified datasets or endorsements**. Record the actual document and URL when acquired.

### 4.1 Florida utility construction bids

Search public procurement and agenda archives from Florida utilities, municipalities, counties, and water management entities for water transmission main and force main bid tabulations. Candidate owners include JEA, St. Johns County, Jacksonville-area municipalities, and other Florida public utilities. Expand geographically if comparable local records are scarce.

Search terms:
- `water transmission main bid tabulation 16 inch Florida PDF`
- `water main HDD bid schedule unit price Florida`
- `utility construction bid tabulation horizontal directional drill`
- `water main engineer's estimate bid results Florida`

For each project, request or download the bid schedule, award recommendation, engineer's estimate if public, addenda affecting scope, and relevant contract summary. Use public-record requests when online files are incomplete. Avoid employer or client confidential records unless expressly authorized for public reuse.

### 4.2 Escalation and market conditions

Review official Bureau of Labor Statistics Producer Price Index (PPI) series and their definitions, including pipeline-relevant materials or construction inputs. Consider ENR indices only if accessible and permitted for the intended use. Record series IDs, base periods, release dates, frequency, and limitations. Do not mix indices without documenting why.

### 4.3 ICW HDD permitting

Identify the actual jurisdictional and permitting pathway applicable to the **hypothetical** crossing before collecting durations. Investigate USACE Jacksonville District records and relevant Florida DEP or delegated permitting records; evaluate navigation, submerged lands, environmental resources, and other approvals as applicable. Do not assume every ICW HDD requires the same permits.

For comparable crossing permits, collect dates for initial submittal, completeness determination, requests for additional information (RAIs), responses, public notice if applicable, permit issuance, and material revisions. Distinguish elapsed calendar time from agency review time and applicant response time. Record whether work restrictions or seasonal windows could affect construction.

### 4.4 Construction change orders and HDD risk

Seek original and final contract values, change-order logs, causes, and time extensions for similar pipeline/HDD work. Classify whether an event was caused by subsurface conditions, utility conflicts, design changes, scope changes, market escalation, or administrative delay. Do not infer an HDD event probability from a few newsworthy failures.

## 5. Comparability and Inclusion Rules

A record is not automatically comparable because it mentions a water main.

For every bid item, capture at minimum:

- Project ID, owner, location, source document, and bid date.
- Item description, diameter, material, installation method, quantity, unit, and bid unit price.
- Whether the price includes pipe, fittings, drilling fluid management, mobilization, restoration, traffic control, or other bundled work.
- HDD-specific context when known: bore length, diameter, casing/carrier arrangement, waterway crossing, ground conditions, entry/exit constraints, and risk allocation.
- Bidder identity and award status, where public; multiple bids on one project are **not independent projects**.
- Confidence in comparability and reasons for inclusion or exclusion.

Prefer direct comparables; retain less-comparable observations as proxy evidence, clearly tagged. Never silently pool open-cut and HDD prices. Normalize units before analysis. Adjust historical costs to a common reference date only using a documented index and formula; preserve original prices and dates.

## 6. Data Source Register

Keep `data/source_register.csv` tracked in Git, containing **metadata only**. Suggested fields:

```csv
source_id,category,source_title,owner_or_publisher,source_url,document_date,access_date,geography,record_type,local_raw_path,license_or_access_notes,evidence_grade,comparability_notes,quality_flags,status
```

- Use stable `source_id` values (e.g., `BID-001`, `PPI-001`, `PERMIT-001`).
- Store local paths as relative paths, not machine-specific absolute paths.
- `evidence_grade`: **A** = relevant direct records; **B** = proxy/adjusted evidence; **C** = documented engineering judgment (with a supporting assumptions-register entry).
- Grades describe relevance and strength for a **specific use**, not a universal rating of a publisher.
- `status`: candidate, requested, acquired, reviewed, accepted, or excluded.
- Add a separate item-level table under ignored `data/interim/` or `data/processed/` for individual bid items and permit milestones.

## 7. Acquisition Procedure

1. **Create an inventory:** List model inputs and required evidence in `00_data_requirements.md`; map each input to at least one acquisition target.
2. **Discover sources:** Search official procurement portals, meeting packets, agency databases, and public permit repositories; log candidates immediately.
3. **Screen relevance:** Check diameter, installation method, date, scope, geographic setting, and source completeness before downloading in bulk.
4. **Acquire originals:** Save untouched files under `data/raw/<category>/<source_id>/`; preserve filenames and optionally record SHA-256 hashes.
5. **Register provenance:** Complete the source register, including document version, access restrictions, and whether public redistribution is permitted.
6. **Extract and standardize:** Parse relevant bid items, dates, and events into `data/interim/`; document manual extraction decisions.
7. **Validate:** Check units, dates, duplicate bid items, missing fields, inconsistent totals, and scope bundling.
8. **Normalize:** Produce analysis-ready records in `data/processed/`, retaining source IDs and original values alongside derived values.
9. **Review sufficiency:** Assess sample size, coverage, outliers, and whether evidence supports a distribution, bounded scenario, or only judgment.
10. **Freeze an analysis snapshot:** Record input dataset versions, configuration versions, retrieval cutoff date, and random seed before published simulation runs.

## 8. Modeling Rules for Limited Evidence

- Do not equate variation between unrelated project bids with uncertainty in this project's final cost.
- Distinguish **pre-award price uncertainty**, **post-award quantity/change-order risk**, **discrete construction events**, and **schedule-driven escalation**.
- For sparse samples, use transparent bounded scenarios or elicited distributions, not automatically fitted parametric distributions.
- Track uncertainty in assumptions themselves through sensitivity analysis and alternative scenarios.
- Model dependence where justified, including common escalation factors across materials and possible coupling between permit delays, bid timing, and soft costs.
- Prevent double counting: a delay-driven escalation allowance must not also be embedded unchanged in the baseline price distribution.
- A P80/P90 result is conditional on model assumptions and must not be represented as a guaranteed budget confidence level.

## 9. Specific ICW Schedule-to-Cost Logic

Collect evidence to represent the following chain:

`Permit application → review/RAI cycles → permit issuance → available float → critical-path delay → financial consequences`

Define the planned permit-needed date and planned procurement/construction milestones. Compute effective delay only after accounting for available float and schedule logic. Separate possible financial consequences:

1. **Pre-award escalation:** Later bid date or construction procurement can change prices.
2. **Extended owner/consultant services:** Additional design, administration, or permit-response effort.
3. **Post-award delay:** Contractor standby, remobilization, time extensions, or changed sequencing **only when contract timing and risk allocation support it**.
4. **Construction windows:** Restrictions that shift the work season or construction sequence.

Do **not** automatically charge contractor standby for a permit delay that occurs before contract award. Keep direct permit fees distinct from indirect schedule consequences.

## 10. Repository and Data Handling Policy

```text
data/
├── source_register.csv    # Tracked: metadata only
├── raw/                   # Ignored: unmodified source files
├── interim/               # Ignored: extracted and partially cleaned data
└── processed/             # Ignored: validated analysis-ready datasets
```

Recommended `.gitignore` entries:

```gitignore
/data/raw/
/data/interim/
/data/processed/
```

Source code, notebooks, configuration, methodology, and publishable case-study exhibits remain eligible for version control. Do not commit confidential documents, restricted third-party datasets, credentials, or raw records merely because they are publicly viewable. Keep a documented re-acquisition process and source links for reproducibility.

## 11. Initial Acquisition Sprint

**Sprint goal:** Determine whether the model's major inputs can be grounded in usable public records.

| Task | Minimum deliverable | Completion test |
|---|---|---|
| Construction bid discovery | Shortlist of comparable utility projects and source links | At least several credible candidates screened for scope |
| Bid extraction pilot | One fully documented bid schedule with item-level provenance | Quantities, units, and prices reconcile to source |
| Escalation index selection | One or more justified series and a normalization example | Series metadata and formula documented |
| ICW permit pilot | One or more comparable permit records or documented evidence gap | Milestone dates and jurisdictional differences logged |
| Risk evidence review | Change-order or HDD risk sources, or explicit gap statement | No unsupported event-frequency claim |
| Evidence-gap assessment | Input-by-input A/B/C classification and collection plan | Every major model input has a source or flagged assumption |

**No arbitrary sample-count gate:** A larger sample can still be unrepresentative. Assess coverage and suitability before fitting distributions.

## 12. Exit Criteria Before Probabilistic Modeling

Proceed to the first Monte Carlo prototype when:

- The baseline estimate is itemized and its illustrative or evidence-based status is explicit.
- Each material stochastic input has a documented source or judgment rationale.
- Bid timing, permitting duration, schedule float, and cost mechanisms are clearly distinguished.
- At least one feasible source or defensible fallback is identified for each critical input.
- Missing evidence and major correlations are listed as model limitations.
- Data provenance is traceable from cleaned record to original source.

A prototype may proceed with **labeled assumptions**; a published case study must clearly disclose which findings are evidence-based versus scenario-dependent.

## 13. Immediate Next Action

Begin with a source-discovery inventory for **Florida water-main bid tabulations**, **construction escalation indices**, and **ICW-related HDD permit histories**. Record candidates in `data/source_register.csv` before extracting or fitting anything. Then complete one end-to-end ingestion and normalization pilot using a single bid tabulation.
