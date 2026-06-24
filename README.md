# Game Engagement Prioritisation Analytics

An evidence-led product analytics notebook for prioritising high-engagement comparable games under limited analyst capacity. The project uses observable game metadata to help an indie or AA studio decide which titles deserve deeper human review before product positioning, platform, and marketing decisions.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/chrisnch/game-engagement-prioritisation-analytics/blob/main/notebooks/final_notebook.ipynb)

## Key Result

In a fixed 600-game review queue, random screening would identify about 150 high-engagement games. The final machine-learning screening model identifies 413, making it useful as a prioritisation aid rather than an automatic success predictor.

## Dataset

The analysis uses Rudra Kumar Gupta's Kaggle dataset, **Ultimate Games Dataset | 15K Games | 43 Features**:

https://www.kaggle.com/datasets/rudrakumargupta/ultimate-games-dataset-15k-games-43-features

The submitted raw CSV is stored at:

```text
data/raw/Ultimate_Games_Dataset.csv
```

Before making this repository public, re-check the Kaggle licence and remove the CSV if redistribution is not allowed.

## Notebook

Open the final notebook:

```text
notebooks/final_notebook.ipynb
```

## Methods

- Data quality and schema audit for the 15,000-game snapshot.
- Top-quartile `engagement_score` target definition.
- Leakage-controlled preprocessing and train/holdout split.
- Cross-validated model comparison focused on top-20% review-queue lift.
- Locked holdout evaluation with ROC, precision-recall, and decision simulation.
- Feature importance and SHAP interpretation for business-facing recommendations.

## Reproduce

Create an environment, install dependencies, and run the notebook from the repository root:

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
jupyter lab notebooks/final_notebook.ipynb
```

Running the notebook regenerates `figures/` and `outputs/`. The committed figures and canonical CSV outputs come from the latest executed notebook run.

## Repository Contents

```text
notebooks/final_notebook.ipynb
data/raw/Ultimate_Games_Dataset.csv
figures/
outputs/
requirements.txt
```

Only final notebook artifacts are included. Old notebook versions, build scripts, cache folders, processed datasets, Steam/IGDB enrichment files, and scratch outputs are intentionally excluded.
