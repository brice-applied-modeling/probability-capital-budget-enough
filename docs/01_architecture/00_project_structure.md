# Project Structure

```text
capital-budget-monte-carlo/
│
├── README.md                         # Project introduction, setup, and usage
├── pyproject.toml                    # Python 3.12 dependencies and project configuration
├── uv.lock                           # Locked dependencies for reproducibility
├── .python-version                   # Python version used by uv
├── .gitignore                        # Excluded files, environments, and generated data
│
├── config/                           # Model inputs and simulation configuration
│   ├── baseline_estimate.csv         # Deterministic quantities, unit costs, and allowances
│   ├── uncertainty_assumptions.yaml  # Cost distributions, parameters, and correlations
│   ├── schedule_assumptions.yaml     # ICW permitting durations, delays, and escalation
│   └── simulation.yaml               # Iterations, random seed, scenarios, and budget
│
├── docs/                             # Technical documentation and case study
│   │
│   ├── 00_project_overview/          # Project definition and engineering assumptions
│   │   ├── 00_project_overview.md       # Purpose, objectives, and expected deliverables
│   │   ├── 01_project_scope.md          # Research questions, scope, and limitations
│   │   ├── 02_project_scenario_and_cost_estimate.md
│   │   │                             # Hypothetical water main project and cost estimate
│   │   ├── 03_model_workflow_concept.md     # Analytical methodology
│   │   └── 04_assumptions_register.md   # Assumptions, evidence quality, and justification
│   │
│   ├── 01_architecture/              # Software architecture and development standards
│   │   ├── 00_project_structure.md   # Repository organization and file responsibilities
│   │   ├── 01_data_management.md     # Data storage, provenance, and versioning policies
│   │   ├── 02_notebook_management.md # Notebook conventions and execution practices
│   │   └── 03_code_management.md     # Python modules, testing, and coding standards
│   │
│   ├── 02_data_management/           # Data acquisition and preparation methodology
│   │   ├── 00_data_requirements.md   # Required datasets, fields, sources, and priorities
│   │   ├── 01_data_workflow.md       # Ingestion, cleaning, validation, and EDA workflow
│   │   └── 02_notebook_organization.md
│   │                                 # Notebook purposes and expected deliverables
│   └── 03_case_study/                # Final technical case study and interpretation
│       └── case_study.md             # Methodology, findings, limitations, recommendations
│
├── data/                             # Research datasets and source metadata
│   ├── raw/                          # Original, unmodified downloaded source records
│   ├── interim/                      # Parsed data requiring further cleaning
│   ├── processed/                    # Validated and normalized analysis-ready datasets
│   ├── README.md                     # Data directory structure, provenance, and usage
│   └── source_register.csv           # Source URLs, dates, descriptions, and quality ratings
│
├── notebooks/                        # Exploratory analysis and reproducible demonstrations
│   ├── 01_data_ingestion.ipynb       # Inspect, import, and profile source datasets
│   ├── 02_data_cleaning.ipynb        # Examine cleaning and normalization decisions
│   ├── 03_exploratory_data_analysis.ipynb
│   │                                 # Explore historical costs, trends, and relationships
│   ├── 04_baseline_estimate.ipynb     # Review and validate deterministic cost estimates
│   ├── 05_uncertainty_modeling.ipynb  # Evaluate distributions and uncertainty assumptions
│   ├── 06_simulation.ipynb           # Execute Monte Carlo scenarios and inspect outputs
│   └── 07_results_analysis.ipynb     # Analyze budget risk, contingency, and sensitivity
│
├── src/                              # Reusable Python application source code
│   └── capital_budget_mc/            # Importable infrastructure analytics package
│       ├── __init__.py               # Package initialization
│       ├── ingestion.py              # Load CSV, Excel, and other source datasets
│       ├── validation.py             # Validate schemas, units, ranges, and missing values
│       ├── normalization.py          # Standardize prices, dates, units, and cost categories
│       ├── datasets.py               # Manage prepared datasets and source metadata
│       ├── estimate.py               # Calculate deterministic construction/project costs
│       ├── distributions.py          # Define and sample cost uncertainty distributions
│       ├── schedule.py               # Model permitting delays and schedule-driven costs
│       ├── risk.py                   # Model discrete HDD and construction risk events
│       ├── simulation.py             # Coordinate Monte Carlo iterations and scenarios
│       ├── sensitivity.py            # Evaluate key cost drivers and model sensitivities
│       └── metrics.py                # Calculate P50/P80/P90, exceedance, and contingency
│
├── tests/                            # Automated tests using pytest
│   ├── test_estimate.py              # Verify baseline and total cost calculations
│   ├── test_schedule.py              # Verify delay, float, and escalation calculations
│   ├── test_simulation.py            # Verify reproducibility and simulation behavior
│   ├── test_validation.py            # Verify data validation and error handling
│   ├── test_normalization.py         # Verify units, cost normalization, and adjustments
│   ├── test_distributions.py         # Verify distribution parameters and sampling
│   ├── test_risk.py                  # Verify event probabilities and cost consequences
│   └── test_metrics.py               # Verify percentiles, probabilities, and contingency
│
└── outputs/                          # Generated analytical results
    ├── figures/                      # Histograms, CDFs, sensitivity and risk charts
    └── tables/                       # Simulation summaries and scenario comparisons
```
