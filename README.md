# Risk-Based Loan Pricing Engine

This project estimates probability of default (PD) for consumer loans and converts that risk estimate into a risk-adjusted annual percentage rate (APR). It is structured to show the full credit-risk workflow: data preparation, PD modeling, expected-loss pricing, and profit-based approval decisions.

## Project goals

- Train and compare probability-of-default models on Lending Club loan data.
- Evaluate discrimination with AUC and Kolmogorov-Smirnov (KS) statistics.
- Translate PD into an APR using expected loss, funding costs, operating costs, and a capital charge.
- Set the approval cutoff using expected portfolio profit rather than classification accuracy alone.

## Expected results

The completed project should include a score-band pricing table, calibration and discrimination charts, a mispricing analysis, and a profit-versus-risk curve for the approval strategy.

## Repository layout

```text
.
├── README.md
├── requirements.txt                 # Python package versions
├── .gitignore                       # Ignore raw data, artifacts, and secrets
├── configs/
│   ├── data_paths.yaml              # Dataset locations and time windows
│   ├── model_config.yaml            # Model, split, and tuning parameters
│   └── pricing_assumptions.yaml     # LGD, capital charge, costs, and margin inputs
├── data/
│   ├── raw/                         # Original Lending Club files; never committed
│   ├── interim/                     # Cleaned and joined temporary tables
│   └── processed/                   # Final feature and scoring datasets
├── docs/
│   ├── data_dictionary.md           # Definitions, targets, and feature provenance
│   ├── methodology.md               # Modeling, pricing, and validation approach
│   ├── model_card.md                # Intended use, limits, and fairness notes
│   └── results.md                   # Final findings and tables
├── notebooks/
│   ├── 01_eda.ipynb                 # Portfolio and default-rate exploration
│   ├── 02_feature_engineering.ipynb # Feature and missing-data analysis
│   ├── 03_model_validation.ipynb    # ROC, KS, calibration, and comparison
│   └── 04_pricing_analysis.ipynb    # APR bands and approval economics
├── src/
│   ├── data/
│   │   ├── preprocess.py            # Cleaning, target construction, and splits
│   │   └── features.py              # Reusable feature transformations
│   ├── models/
│   │   ├── train.py                 # Baseline and LightGBM training
│   │   ├── evaluate.py              # AUC, KS, calibration, and reporting
│   │   └── score.py                 # PD scoring for a prepared application set
│   ├── pricing/
│   │   ├── expected_loss.py         # PD, LGD, and EAD calculations
│   │   ├── apr.py                   # Risk-adjusted APR calculation
│   │   └── approval_policy.py       # Expected-profit cutoff optimization
│   └── visualization/
│       └── plots.py                 # Risk, pricing, and profit charts
├── models/                          # Serialized models; excluded from Git
├── outputs/
│   ├── figures/                     # Validation and portfolio charts
│   ├── tables/                      # Score bands and pricing output tables
│   └── scored_loans/                # Generated scored-loan data; excluded if sensitive
└── tests/
    ├── test_features.py             # Transformation and leakage checks
    ├── test_pricing.py              # APR and expected-loss formula checks
    └── test_approval_policy.py      # Cutoff and constraint checks
```

## Data and compliance

Use only a lawfully obtained public Lending Club dataset. Store original files in `data/raw/`, do not upload them to GitHub, and document the data version, source, license, target definition, and any filtering decisions in `docs/data_dictionary.md`.

This is an educational project, not lending advice or a production credit-decision system. The final `docs/model_card.md` should address sampling bias, data leakage prevention, calibration, fairness testing, and the limits of using historical lending outcomes.

## Pricing framework

For each application, derive an illustrative APR from modeled PD and documented assumptions for loss given default (LGD), exposure at default (EAD), funding cost, operating cost, capital charge, and target margin. Keep every assumption in `configs/pricing_assumptions.yaml` so the calculation is auditable.

## Reproducibility checklist

- Use a chronological or out-of-time validation split where possible.
- Record random seeds, excluded columns, and feature versions.
- Check for target leakage before every training run.
- Separate model artifacts and scored output from version-controlled source files.
- Publish aggregate results only; do not expose borrower-level sensitive data.

## Future deliverables

- A model-validation report and model card under `docs/`.
- A score-band pricing table in `outputs/tables/`.
- An optional Streamlit interface in `app.py` after the pricing logic is tested.
