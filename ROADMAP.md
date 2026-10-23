# Project Roadmap

NBA AI LAB is a side project for reinforcing concepts learned during the EDEM master's program. Progress follows practical milestones, not dates or weekly commitments.

The first destination is a **Win Probability Engine**: a reproducible model and a public application for exploring how win probabilities evolve during historical NBA games.

## How to use this roadmap

- Work on one milestone at a time and move on when its completion criteria are met.
- Revisit earlier work when a class introduces a useful technique.
- Keep each step small enough to pause and resume. End each work session with a short note on what changed and the next action.
- Treat the curriculum links as opportunities to practice, not requirements to use every technology.
- Keep public documentation and application content in English.

The curriculum references below come from the [master's curriculum reference](MASTER_CURRICULUM.md): B1 Foundations, B2 Cloud Data, B3 Cloud AI, and the communication and project modules. They describe learning connections, not official course deadlines or assessment requirements.

## Milestones at a glance

| Step | Goal | Concrete result | Main learning connection |
| --- | --- | --- | --- |
| 1 | Make the project easy to run | Documented local setup and a small Python command | B1: Linux, Git, Python |
| 2 | Reconstruct one historical game | Ordered events, score timeline and data audit | B1: Python libraries, APIs, SQL |
| 3 | Build a trustworthy pilot dataset | Repeatable ingestion and validated game-state tables | B1/B2: SQL, Docker, data quality |
| 4 | Understand the games | Exploratory report and descriptive charts | B2: SQL, visualization, Looker |
| 5 | Make the pipeline reproducible | Versioned inputs, repeatable transformations and automated checks | B1/B2: Git Actions, dbt, cloud storage |
| 6 | Establish a prediction baseline | Logistic regression and an honest evaluation report | B3: machine learning |
| 7 | Test whether complexity helps | Gradient boosting comparison and feature ablations | B3: machine learning, experimentation |
| 8 | Make predictions explorable | Local interactive historical replay | B1/B3: application development, APIs |
| 9 | Publish a reliable first release | Public demo, reproducible workflow and project write-up | B2/B3: Cloud Run, security, FinOps, MLOps |
| 10 | Choose one research extension | A focused experiment with a clear conclusion | B3: neural networks, RNNs, GenAI or agents |

Steps 1–5 already form a useful data engineering portfolio project. Steps 6–7 add the machine learning study. Steps 8–9 turn it into a public product. Step 10 is optional.

## 1. Make the project easy to run

**Goal:** become comfortable working in the repository before building a pipeline.

Practice Git commits and branches, terminal commands, Python environments and dependency management. Add a minimal project structure when there is code to put in it.

**Deliverables**

- Setup instructions and a dependency definition.
- A small Python command that reads a local sample and prints a summary.
- Ignore rules for environments, generated data and credentials.

**Done when:** the documented setup works from a clean environment and the sample command runs successfully.

## 2. Reconstruct one historical game

**Goal:** prove that the source data can support the idea before downloading seasons.

Inspect a candidate source such as SportsDataverse / hoopR. Obtain the result and play-by-play for one completed game, record its provenance and terms of use, and inspect the actual fields. Add a separate overtime example to validate clock handling.

**Deliverables**

- A small ingestion script and a source audit describing coverage, access and usage constraints.
- An ordered event table with game ID, event ID, period, clock and home/away score.
- A simple score timeline and notes on missing, duplicated or ambiguous events.

**Done when:** the reconstructed final score agrees with the recorded result, event ordering is deterministic, overtime is handled, and unresolved data limitations are documented.

This step produces descriptive game states, not model predictions.

## 3. Build a trustworthy pilot dataset

**Goal:** move from isolated examples to one complete pilot season.

Separate raw inputs from normalized tables. Define the meaning of each game-state field, including clocks, period boundaries, score corrections and the eventual winner. Use Python and SQL to validate the data; use Parquet and DuckDB locally unless a demonstrated need justifies more infrastructure.

**Deliverables**

- Repeatable ingestion for a pilot season and a manifest of source versions or retrieval dates.
- Game and event tables with documented identifiers and field definitions.
- A completeness report covering expected versus available games, missing events, duplicates, final-score mismatches and overtime coverage.
- Explicit rules for excluding or quarantining invalid games.

**Done when:** every expected game is accounted for as accepted, missing or excluded with a reason, accepted games pass the documented checks, and rerunning ingestion does not duplicate records.

The pilot season validates engineering. Later modeling will require multiple chronological periods, ideally multiple seasons, to support training, validation and a final held-out test.

## 4. Understand the games

**Goal:** learn what the dataset contains before asking a model to learn from it.

Explore scoring patterns, lead changes, overtime, event counts and missingness. Compare distributions across periods and games. Use SQL and Python charts; optionally reproduce selected descriptive views in Looker when studying visualization.

**Deliverables**

- A concise exploratory report with readable charts and reproducible queries.
- A data dictionary refined using the findings.
- A list of candidate features, each with its definition and when its inputs become available.

**Done when:** the report explains the dataset's main patterns and limitations, and no proposed feature depends on information from the future.

## 5. Make the pipeline reproducible

**Goal:** turn the exploratory work into a workflow that can be rerun and checked.

Separate ingestion, normalization, validation and feature preparation. Add automated checks through GitHub Actions and containerize the workflow when practicing Docker. Apply dbt where SQL transformations benefit from it.

**Deliverables**

- Documented commands to rebuild the pilot tables from the recorded source snapshot.
- Automated checks for schema, score integrity, ordering and duplicate handling.
- Dataset and transformation version information.
- An optional cloud storage exercise with documented access controls and cost limits.

**Done when:** the same inputs and code reproduce the same logical tables, checks run automatically, and secrets and raw datasets are kept out of Git.

Cloud practice can begin here, but local execution remains sufficient to continue. Add Terraform if infrastructure is introduced and needs to be recreated.

## 6. Establish a prediction baseline

**Goal:** produce the first defensible win probabilities.

Expand the audited data to support chronological train, validation and test partitions. Reserve the final test before model tuning. Start with logistic regression using score differential, remaining time and suitable interactions. Add pre-game team strength only when it can be computed using earlier games.

**Deliverables**

- A documented sampling rule for event states and a split manifest with no game appearing in multiple partitions.
- A reproducible training command and saved baseline model.
- Validation results for log loss, Brier score and calibration, including breakdowns by period or time remaining.
- Game-balanced metrics and uncertainty estimates based on resampling whole games.
- Example probability timelines, including overtime games.

**Done when:** features pass a point-in-time audit, the model can be retrained, and its probability quality and limitations are documented. Use validation data for iteration; keep the final test untouched until the release evaluation.

A modest baseline with sound evaluation is a successful milestone. There is no predetermined accuracy target.

## 7. Test whether complexity helps

**Goal:** find out which additions improve predictions on unseen games.

Compare gradient boosting with the baseline using the same data splits and evaluation rules. Add feature groups separately: pre-game strength, rest, recent scoring dynamics and possession only if it can be reconstructed reliably. Fit any calibration step without using the final test.

**Deliverables**

- A model comparison using validation data.
- Ablation experiments showing the contribution of each added feature group.
- Experiment records with configurations, data versions and results.
- A written choice of model for the first release, including any trade-offs.

**Done when:** the choice is supported by probability metrics, calibration and temporal performance. If boosting or momentum features do not improve the baseline, record that conclusion and retain the simpler approach.

## 8. Make predictions explorable

**Goal:** let someone understand the project by exploring a real game.

Build a local web application using precomputed historical predictions first. Develop the interface alongside selected example games; wait for the model study before presenting predictions as validated results.

**Deliverables**

- A game selector and interactive win probability timeline.
- Score, clock and event details synchronized with the selected moment.
- Event-level inspection and a documented minute-summary rule.
- Clickable markers for the largest probability swings.
- A short methodology view with model version and limitations.

**Done when:** a visitor can select a game, inspect its timeline and understand the displayed probability on both desktop and mobile. Swing descriptions use predictive, non-causal language.

An API is optional: precomputed files may be sufficient for historical replay.

## 9. Publish a reliable first release

**Goal:** deliver a public demo and a study someone else can inspect.

Freeze the chosen model and evaluation procedure, then evaluate on the held-out test. Publish the results even if they are less favorable than validation results. If they motivate further tuning, treat the old test as development data and reserve a new untouched test for later claims.

**Deliverables**

- A public, responsive historical replay application.
- Final log loss, Brier score, calibration plots and time-based breakdowns.
- Setup and reproduction instructions, an architecture diagram and documented data restrictions.
- A deployment process, basic failure checks and a rollback procedure.
- A short written or recorded walkthrough explaining the problem, approach, findings and limitations.

**Done when:** the public demo works, the published results trace back to versioned data and code, and a reviewer can follow the workflow without private credentials or undocumented manual steps.

Apply cloud learning proportionally: Cloud Run if a service is needed, minimum necessary permissions, secret management, and cost monitoring for any cloud resources. Provision only what the application uses.

## 10. Choose one research extension

**Goal:** deepen one question after the first release works.

Choose one extension, define a hypothesis and completion criteria, and compare it with the existing system:

| Extension | Learning connection | Evidence to look for |
| --- | --- | --- |
| Event-sequence model | ANN, RNN, sequence modeling | Better held-out probability quality or useful insight compared with the tabular model |
| Model explanation panel | Interpretability | Clear local explanations with explicit limits; no causal claims |
| Comeback explorer | SQL, visualization, product design | Reliable retrieval and explanation of historical low-probability wins |
| Grounded research assistant | GenAI, LangChain/LangGraph or Google ADK | Answers grounded in project documentation and measured with a small evaluation set |
| Pipeline monitoring | MLOps | Detection of schema changes, data failures or model drift |

Live prediction and scenario simulation need separate scoping. Neither is required to complete the first release.

## Where the rest of the curriculum fits

Some classes may be better reinforced through a small, separate exercise rather than permanent project infrastructure.

| Topic | Appropriate use in this project |
| --- | --- |
| NoSQL | Optional serving experiment if access patterns justify it; relational tables remain sufficient initially |
| Pub/Sub and Dataflow | Optional simulated event-stream exercise after the batch workflow works |
| PySpark / Dataproc | Optional comparison on a representative dataset; keep only if scale warrants it |
| Airflow / Composer | Optional orchestration exercise when there are enough dependent jobs to benefit |
| CNNs | Better suited to a separate shot-image or court-vision project than the initial tabular win model |
| AgentOps | Relevant only if an agent extension is actually built |
| Communication and critical thinking | Short milestone write-ups, clear charts, explicit assumptions and honest interpretation |
| Data Project, AI Project and TFM | Potential opportunities to reuse or extend work, subject to the actual assessment requirements |

## Progress tracker

- [ ] 1. Local setup and sample command
- [ ] 2. Single-game reconstruction and overtime audit
- [ ] 3. Validated pilot season
- [ ] 4. Exploratory report
- [ ] 5. Reproducible pipeline
- [ ] 6. Baseline model and validation report
- [ ] 7. Model comparison and feature ablations
- [ ] 8. Local historical replay
- [ ] 9. Public release and final evaluation
- [ ] 10. Optional research extension

**Next action:** begin Step 1 by choosing the Python environment and dependency workflow, then document and run a minimal local sample command.
