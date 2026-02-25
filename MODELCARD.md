Here is the expanded and detailed version of your **Model Card**, reformatted to match the clean, standard Markdown style of the previous files. You can copy and paste this directly into your `model_card.md` file.

```markdown
# Model Card: Multi-Surrogate Bayesian Optimisation Strategy

## Overview
This model card details the ensemble of machine learning surrogate models used to sequentially optimise eight distinct, high-dimensional black-box functions for the Imperial College ML/AI Capstone Project. The goal of these models is not to deploy a traditional predictive system, but to act as mathematical proxies to efficiently find global maxima while minimising the number of expensive evaluations.

## Model Details
The architecture relies on an Iterative Sequential Model-Based Optimisation (SMBO) approach. To mitigate the bias of any single algorithm, a tri-model ensemble approach was used:
* **Gaussian Process Regressor (GP):** The primary probabilistic model, using a Matérn ($\nu=2.5$) or RBF kernel combined with a WhiteKernel to account for noise. This was particularly effective for smooth, continuous landscapes.
* **Random Forest Regressor (RF):** A tree-based ensemble used to capture non-linear interactions, rugged topologies, and sudden jumps in the data. Uncertainty is derived from the variance across the individual decision trees.
* **Gradient Boosting Machine (GBM):** An auxiliary ensemble of independently seeded HistGradientBoosting models. It provides a strong mean prediction on structured response surfaces and approximates epistemic uncertainty via model-to-model disagreement.

## Intended Use
This framework is designed for sample-efficient optimisation of expensive-to-evaluate, noisy, and hidden mathematical functions where trial-and-error is cost-prohibitive. Specific simulated applications include:
* **Function 1 (2D):** Detecting radiation or contamination sources where signals are sparse and weak.
* **Function 2 (2D):** Optimising a noisy black-box ML likelihood score.
* **Function 3 (3D):** Testing drug compound combinations to minimise adverse side effects.
* **Function 4 (4D):** Dynamic warehouse product placement with multiple local optima.
* **Function 5 (4D):** Optimising a chemical process factory yield with a steep, unimodal peak.
* **Function 6 (5D):** Formulating a consumer product (e.g., a cake recipe) balancing flavour against cost and waste.
* **Functions 7 & 8 (6D, 8D):** Tuning hyperparameters (e.g., learning rate, hidden layers) for complex machine learning models.

## Implementation Strategy
To balance the need to explore unknown areas and exploit known high-yield areas, the models use specific acquisition strategies:
1. **Candidate Generation:** A large pool of potential query points is generated each round using a mix of global uniform/Sobol sampling and local Gaussian perturbations (jitter) around the current best observation. 
2. **Acquisition Scoring:** Candidates are scored using Expected Improvement (EI) or Upper Confidence Bound (UCB). Occasionally, scores from the GP, RF, and GBM are blended to ensure consensus.
3. **Diversity Filtering:** Minimum Euclidean distance constraints are applied to prevent the optimiser from repeatedly querying the exact same coordinates and getting stuck in a local region.

## Training Data
The models were trained sequentially on small datasets provided via the Capstone Portal. Starting with initial batches of 10 to 42 points (depending on dimensionality), the datasets grew iteratively by exactly one carefully chosen data point per week. The inputs are purely continuous numerical values constrained to the $$ interval, mapping to a single continuous scalar target to be maximised.

## Performance and Evaluation Tracking
Because traditional train/test splits are impossible in black-box optimisation with such small datasets, model performance was rigorously tracked via continuous weekly diagnostics:
* **In-Sample Fit:** $R^2$ and RMSE were evaluated weekly to ensure the surrogate correctly mapped the known topological landscape without severe underfitting.
* **Residual Analysis:** Scatter and histogram plots of residuals were checked to ensure zero-mean distribution and to spot heteroscedasticity or bias.
* **Feature Importance:** RF and GBM feature importances were monitored to confirm the models were consistently identifying the true dominant features across weeks.

## Limitations and Trade-offs
* **Scalability Bottlenecks:** Gaussian Processes scale cubically with the number of observations. While highly accurate for the small sample sizes in this project, performance degrades drastically if scaled to thousands of experiments.
* **Boundary Trapping:** In higher dimensions, acquisition functions can aggressively push candidates to the absolute boundaries of the domain. Without strict distance penalties, the model risks getting trapped against a boundary wall.
* **Epistemic vs. Aleatoric Confusion:** While the ensemble approximates uncertainty well, it can occasionally struggle to differentiate between a genuinely noisy region and a region it simply hasn't explored yet.
```
