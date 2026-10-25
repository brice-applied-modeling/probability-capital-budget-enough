# Project Structure

```text
capital-budget-monte-carlo/
├── README.md
├── pyproject.toml
├── uv.lock
├── .python-version
├── .gitignore
├── docs/
│   ├── project_overview.md
│   ├── project_scope.md
│   ├── project_scenario_and_cost_estimate.md
│   ├── assumptions_register.md
│   └── case_study.md
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_baseline_estimate.ipynb
│   ├── 02_uncertainty_eda.ipynb
│   └── 03_results_analysis.ipynb
├── src/
│   └── capital_budget_mc/
│       ├── __init__.py
│       ├── estimate.py
│       ├── distributions.py
│       ├── schedule.py
│       ├── simulation.py
│       └── metrics.py
├── tests/
│   ├── test_estimate.py
│   ├── test_schedule.py
│   └── test_simulation.py
└── outputs/
    ├── figures/
    └── tables/
```
