# Evaluating Defensive Value in Center Field with Public Statcast Data

## Overview
This project builds a lightweight public-data model to evaluate MLB center field defense. Using Statcast play-level data, it estimates the probability that an airborne ball in an approximate center-field zone becomes an out, then compares expected outs to actual outs at the player level.

The goal was not to replicate an internal club system, but to show a credible baseball R&D workflow: define opportunities, engineer features, fit an interpretable model, aggregate to a decision-friendly metric, and communicate limitations clearly.

## Question
Who appears to add the most defensive value in center field, after accounting for play difficulty?

## Data
- Play-level Statcast data pulled with `pybaseball`
- Season-level fielding data used to keep only players who actually logged center-field time
- Season used in the notebook: update the `season` variable in one place so the analysis stays consistent

## Method
1. Pull Statcast data for one season.
2. Keep balls in play classified as fly balls or line drives.
3. Create a binary outcome for whether the play resulted in an out.
4. Approximate center-field opportunities using hit-location coordinates (`hc_x`, `hc_y`).
5. Engineer a simple distance proxy from home plate.
6. Train a logistic regression model using:
   - `launch_speed`
   - `launch_angle`
   - `distance`
7. Score each play with `catch_prob`.
8. Aggregate to the player level:
   - `actual_outs`
   - `expected_outs`
   - `OAE = actual_outs - expected_outs`
9. Join season-level fielding data to keep only players listed as CF.

## Outputs
- Top player leaderboard by Outs Above Expectation (OAE)
- Scatter plot of actual outs vs expected outs
- Simple model diagnostics (accuracy and ROC AUC)

## Why this is useful
This MVP mirrors a practical R&D pattern:
- estimate play difficulty at the event level
- separate results from opportunity mix
- translate model output into a ranking that can support evaluation and discussion

## Limitations
- Center-field opportunities are approximated with spray-location boundaries rather than exact defender responsibility.
- The feature set is intentionally simple.
- The model is an MVP and is meant to demonstrate reasoning, workflow, and communication more than final precision.

## Natural next steps
- Use a richer set of Statcast features, such as hang time, direction, and fielder movement proxies
- Test non-linear models (XGBoost / LightGBM)
- Add uncertainty intervals to player rankings
- Compare multiple seasons for stability
- Expand the method to all outfield positions

## Suggested repo structure
```text
phillies-cf-defense-mvp/
├── center_field_defense_mvp.ipynb
├── README.md
├── requirements.txt
└── project_summary_tim_ponden.docx
```

## Resume line
Built a Statcast-based defensive modeling project to evaluate MLB center fielders, developing a play-level catch probability model and aggregating results into player-level Outs Above Expectation metrics for decision-oriented player evaluation.
