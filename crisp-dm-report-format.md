---
title: CRISP-DM for structuring analytics reports
---

CRISP-DM provides a useful structure for analytics writeups, whether those
writeups be short (e.g., a blog, medium, or Kaggle post) or long (e.g., an academic paper).

Study the circular graphic on [the wikipedia page for CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining).
Note its stages. These stages become report sections. While the process is circular,
the report describes a snapshot in time, so the report can flow somewhat linearly,
without looping back.

If the report is written as a jupyter notebook, then outputs etc can easily be
integrated into the report, interspersed with markdown.

# 1. Business Undestanding
Describes the general business problem that the analyst, and that the model,
set out to address.

# 2. Data Understanding
Describes the source dataset.

* level of analysis of raw data
* meaning of each column
* descriptives, such as datatypes, number of unique values, modes, averages, etc.
* optionally, descriptive-analytics-type plots of features from the raw data.

# 3. Data Preparation
Describes all work done by the analyst to the data before the modeling step.

* all cleaning steps taken
* aggregation -- level of analysis?
* engineered features -- justification, description sufficiently detailed
  that someone else could replicate. Repeat of descriptives as for raw features

# 4. Modeling
Describes the selection of modeling algorithms. This section does _not_ evaluate
the performance of the models.

Describes all modeling algorithm settings, including hyperparameterization, if any.

Describes training approach -- cross-validation? Splitting strategy?

# 5. Evaluation
Evaluates the predictive performance of each of the models described in the previous step.

* f1-score
* accuracy

Include plots! For each type of figure, plot all models on one figure.

## 5.x Select a model to recommend for deployment

Explore Model Interpretation -- for the selected model, which features were the most
predictive? Partial dependencies, etc.

# 6. Deployment

Make it possible for someone else to _use_ the model without having to train it themselves.

Options include:
* putting a pickled pipeline on github, and add a Usage README at a minimum.
* uploading it as an ML endpoint to a service such as AWS Sagemaker
