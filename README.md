# Instructor Effectiveness Modeling

This project turns batch-level EdTech data into an instructor-level effectiveness analysis. The notebook does more than train a model: it explains how the effectiveness score is defined, why each metric is weighted, where the model performs well, and where the results should be treated carefully.

The core idea is simple: an instructor should not be judged from one noisy signal. The notebook combines learning outcomes, engagement, and learner feedback into a transparent score, converts that score into Low, Medium, and High tiers, and then trains a classical machine learning model to predict those tiers.

## What Is Inside

- Exploratory data analysis on 2,000 course batches
- A clearly defined Instructor Effectiveness Score
- Instructor-level aggregation from batch-level records
- Tiering into Low, Medium, and High effectiveness groups
- A Random Forest classifier for tier prediction
- Model evaluation with accuracy, macro-F1, confusion matrix, and cross-validation
- Feature importance analysis
- Plain-language recommendations and limitations

## Project Files

| File | Purpose |
|---|---|
| `Instructor_Effectiveness_Modeling.ipynb` | Main analysis notebook |

The notebook expects the source dataset file named:

```text
instructor_effectiveness_dataset_2000_rows.xlsx
```

That dataset is used locally by the notebook but is not currently included in this GitHub repository.

## Methodology

The analysis starts from batch-level data and aggregates it to the instructor level. Since the dataset does not include a ground-truth label for "good instructor", the notebook defines one explicitly instead of pretending it already exists.

The effectiveness score is built from three pillars:

| Pillar | Metrics | Weight |
|---|---|---|
| Learning outcomes | Completion rate, score improvement, quiz score | 50% |
| Engagement | Watch time, assignment submission, forum activity | 25% |
| Feedback | Feedback score, feedback response rate | 25% |

`dropout_rate` is intentionally excluded from the score because it is almost the inverse of `completion_rate`. Including both would double-count the same underlying behavior.

After scoring, instructors are grouped into three tiers using tertiles. A Random Forest classifier is then trained using aggregated instructor metrics to predict those tiers.

## Results

The notebook scores and tiers 120 instructors from 2,000 batches.

Model snapshot:

- Test accuracy: about 90%
- Macro-F1: about 0.90
- Validation: 5-fold cross-validated macro-F1 is included in the notebook

The strongest predictive signals are tied to outcomes and feedback behavior, especially completion/dropout-related features and feedback response patterns.

## Important Caveat

This model should not be used by itself for high-stakes instructor evaluation.

The score is useful as a coaching and triage tool, but it has real limitations:

- The model predicts a tier that was created from the same metrics used as features.
- Course difficulty is not controlled for.
- Learner background and cohort quality are not controlled for.
- Feedback data may be biased toward learners who stayed engaged.
- The sample size is small for a production performance system.

In practice, this kind of model is best used to surface patterns, guide investigation, and support human review. It should not replace qualitative evaluation or manager judgment.

## How To Run

1. Clone the repository.
2. Place `instructor_effectiveness_dataset_2000_rows.xlsx` in the project root.
3. Open `Instructor_Effectiveness_Modeling.ipynb` in Jupyter Notebook, JupyterLab, VS Code, or Google Colab.
4. Run the notebook from top to bottom.

Required Python packages:

```text
numpy
pandas
matplotlib
seaborn
scikit-learn
openpyxl
```

## Why This Project Matters

Instructor performance data can easily become unfair or misleading if it is reduced to a single metric. This notebook takes a more careful route: it makes the assumptions visible, separates scoring from modeling, evaluates the result honestly, and explains how the output should be used.

The goal is not to label instructors mechanically. The goal is to help an EdTech team understand which teaching patterns deserve attention, support, and deeper review.
