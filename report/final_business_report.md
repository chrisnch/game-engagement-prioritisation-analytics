# Prioritising High-Engagement Game Review: An Evidence-Led Product Analytics Study

## 1. Business Problem

- This study addresses a resource-allocation problem: helping a hypothetical indie/AA game studio prioritise comparable games for scarce product research, marketing and platform-porting review.
- The business decision is which comparable games deserve analyst attention before positioning and platform discussions.
- This matters because smaller studios often operate with tight budgets, short deadlines, limited user data and constrained experimentation or playtesting capacity (Linaker, Bjarnason and Fagerholm, 2024).
- Stakeholders include product leads, producers, researchers and platform/marketing managers who need evidence before committing review time.
- The analysis asks whether market metadata can rank comparable games more efficiently than random screening or a simple rule.
- The model is a human-in-the-loop screening tool. It is not a revenue model, causal model, greenlight system, investment rule or replacement for playtesting and producer judgement.

## 2. Dataset

- The Kaggle Ultimate Games Dataset (Gupta, n.d.) is suitable for market screening because it contains 15,000 games and 43 fields. No external dataset is merged.
- The variables cover release context, genre, design metadata, platform/store coverage, franchise depth, text fields and post-launch reception indicators.
- The target is `high_engagement`, defined as `engagement_score >= 4.25`. This threshold represents the top quartile of the supplied engagement proxy and gives a positive rate of about 25.01%.
- The engagement score is an opaque proxy. It should not be interpreted as revenue, profit, retention, sales volume or overall commercial success.

## 3. Data Pre-Processing

- Pre-processing preserves the full dataset while separating quality issues from long-tail market variation.
- Missing values, blanks and explicit `Not Specified` placeholders are audited before treatment rather than blindly deleted.
- All 15,000 rows are retained because the audit finds no major corruption, including duplicate IDs, invalid dates, negative counts or rating-range violations.
- Outliers are retained because game markets are long-tailed and extreme engagement may be commercially meaningful rather than erroneous.
- Numeric imputation, transformations, encoding and text vocabularies are handled inside fitted modelling pipelines, which prevents holdout information leaking into training.
- Outcome-like variables, including `engagement_score`, `high_engagement`, popularity, library and status fields, are excluded from predictors.
- Post-launch variables such as ratings, reviews and playtime are used as diagnostics only, not as planning-safe main predictors.

## 4. Analysis

- Classification supports prioritisation because the output is a ranked review queue, not an automatic success prediction or a response to skewness alone.
- The data are split into an 80/20 stratified train/holdout split. The holdout set contains 3,000 games, including 750 high-engagement positives.
- Model comparison progresses from baseline/simple models to a flexible random forest. The final selected model is `RandomForest_flexible`.
- ROC-AUC and PR-AUC are used as ranking evidence, but the main business evidence is the fixed-capacity decision simulation.
- The fixed queue reflects a realistic analyst-capacity constraint: the studio can review only a limited number of games, so the relevant question is which 600 games should be reviewed first.
- A training-selected franchise-depth heuristic is included because a studio could use a simple rule without machine learning. The machine-learning queue must therefore beat a plausible operational benchmark, not only random selection.

## 5. Results And Interpretation

- The main result is that the ML review queue identifies more high-engagement comparable games than both random screening and the best simple rule at the same workload.
- The final model achieves holdout ROC-AUC of 0.869, with a 95% bootstrap confidence interval of 0.855-0.881, and PR-AUC of 0.715. These results indicate useful ranking performance within the same market snapshot.

| Review strategy | High-engagement games found in 600-game queue | Business interpretation |
|---|---:|---|
| Random screening | 150 | Expected result from reviewing 20% of the 3,000-game holdout without prioritisation. |
| Franchise-depth heuristic | 331 | Credible non-ML benchmark that materially improves on random screening. |
| Final ML model | 413 | Identifies the most high-engagement games at the same analyst workload. |

- The decision simulation is the most important result. Within a 600-game review queue, the final model finds 263 more high-engagement comparable games than random review and 82 more than the heuristic, without increasing analyst workload.
- The top-20% ML queue has 68.8% precision, captures 55.1% of holdout positives and delivers 2.75x lift over random screening.
- The same queue still creates 187 false reviews and misses 337 high-engagement games, so model rank should support, not replace, expert review.
- Platform, design text and franchise signals are predictive screening signals, but they may also reflect visibility, publisher support, existing audience, later porting, metadata coverage or broader market context. They should not be converted into causal design rules.
- Chronological ROC-AUC falls to 0.686, showing temporal fragility. The model should be refreshed and recalibrated before live use and should not be treated as a future launch forecasting system.
- Target sensitivity remains directionally robust across target definitions, with top-20% lift roughly 2.62x-2.87x. This supports the ranking conclusion but does not validate the engagement proxy as a commercial outcome.

## 6. Business Insights And Recommendations

- Use the model as a ranked review queue for comparable-game review, while keeping final decisions human-in-the-loop.
- Producer override, playtesting and audience research should remain central because the model cannot assess creative fit, novelty, community response or strategic risk.
- The studio should not automatically greenlight, cancel, fund or reject game concepts based on the model score.
- False positives should be reviewed to understand why some highly ranked games fell below the engagement threshold, and missed positives should be reviewed to identify under-detected niches or metadata gaps.
- The model should be refreshed and recalibrated before operational deployment because chronological performance falls materially below the random holdout result.
- Future improvement should collect stronger deployment-grade data, including timestamped launch platforms, marketing spend, price, user acquisition, retention and financial outcomes.

## References

Gupta, R.K. (n.d.) *Ultimate Games Dataset | 15K Games | 43 Features*. Kaggle. Available at: https://www.kaggle.com/datasets/rudrakumargupta/ultimate-games-dataset-15k-games-43-features

Linaker, J., Bjarnason, E. and Fagerholm, F. (2024) *Pre-Release Experimentation in Indie Game Development: An Interview Survey*. Available at: https://arxiv.org/abs/2411.17183
