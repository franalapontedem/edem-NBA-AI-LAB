# NBA AI LAB 🏀

A data and artificial intelligence lab for exploring and understanding NBA basketball.

This project is being developed throughout my master's program at EDEM. The goal is to learn by building: work through the full process from raw data to machine learning models, then turn the results into an application anyone can explore.

## First project: Win Probability Engine

**How likely is a team to win a game at a particular moment?**

The first project aims to answer this question throughout an NBA game. Using the score, remaining time and available context, the model will estimate the home team's probability of winning after each recorded event.

For example, a team trailing by ten points in the second quarter still has plenty of time to come back. The same deficit with thirty seconds left is a very different situation. The model will try to learn these relationships from historical games.

The idea is to display these estimates in an interactive website where visitors can select a game and explore how its win probabilities evolved.

## What will visitors be able to explore?

- **Historical game replay:** explore a timeline showing the score, game clock and win probability at each moment.
- **Major probability swings:** find the largest shifts in predicted win probability and inspect the associated events.
- **Model comparisons:** see how a simple baseline and a more advanced model differ.
- **Methodology and results:** understand the data, evaluation methods and limitations.

A probability swing describes how the prediction changes as the game state is updated. On its own, it does not establish that a particular play caused that change.

## How it will be built

1. **Prepare the data.** Collect results and play-by-play records, order events and validate scores, clocks and overtime periods.
2. **Reconstruct each game state.** Build features such as score differential, remaining time and pre-game team strength, using only information available at that moment.
3. **Train and compare models.** Start with logistic regression as a baseline, then evaluate gradient boosting with tools such as XGBoost or LightGBM.
4. **Evaluate the probabilities.** Split data chronologically, keeping every game entirely within a single training, validation or test partition.
5. **Build the web experience.** Present historical predictions through a simple, interactive interface.

Model quality will be assessed using log loss, Brier score and calibration curves. Good calibration means, for example, that roughly 70% of situations assigned a 70% win probability should end in a win. Metrics will account for the dependence between events within the same game, with uncertainty estimated by resampling whole games.

The project will also test whether recent scoring runs add predictive information once score, remaining time and team strength are taken into account.

## First release scope

The first release will focus on **historical games**, with event-level predictions and a minute-level summary. It will include a game selector, a win probability curve and the largest probability swings during the game.

Live tracking, sequence models and scenario simulators are possible extensions. The initial priority is a reproducible foundation with reliable data and carefully evaluated probabilities.

## Planned technologies and data sources

Specific tools will be confirmed during the initial experiments:

| Area | Planned options |
| --- | --- |
| Data | SportsDataverse / hoopR; supplementary sources if needed |
| Processing and storage | Python, Pandas or Polars, Parquet and DuckDB |
| Machine learning | scikit-learn and XGBoost or LightGBM |
| Web application | React, TypeScript and an interactive charting library |
| Prediction delivery | Precomputed data or a FastAPI service, depending on requirements |

Data availability, coverage and terms of use will be checked before deciding what can be published.

## Current status

**Project definition and feasibility stage.** No models have been trained, no evaluation results are available and there is no public demo yet.

The next milestone is to audit a data sample and verify that game states can be reconstructed correctly. Work will then expand to a complete pilot season and the first baseline model.

## Learning roadmap

The [project roadmap](ROADMAP.md) breaks the work into gradual milestones aligned with the master's curriculum, from Python and data engineering to machine learning and a public demo. Each step has concrete deliverables and completion criteria, with no fixed dates.

The [master's curriculum reference](MASTER_CURRICULUM.md) keeps the course topics available alongside the roadmap.

## Beyond the first project

In the longer term, NBA AI LAB could include projects on shot quality, roster analysis and tactical analysis. These are future research directions; the initial work will focus on the Win Probability Engine.

The goal is for any visitor to explore a game in seconds, while anyone interested in the technical details can understand how each prediction was produced and evaluated.
