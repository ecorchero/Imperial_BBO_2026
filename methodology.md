This document outlines the technical pipeline, surrogate model choices, and acquisition functions used during your weekly iterations.
# Methodology: Bayesian Optimisation Pipeline

This document outlines the machine learning techniques applied to optimise the eight black-box functions over the course of the competition [21].

## 1. The Weekly Optimisation Loop
For each function, the optimisation followed a strict iterative loop [12, 19]:
1. **Load & Clean:** Aggregate all historical `(X, y)` data, standardise features, and apply transformations (e.g., `log1p`) for functions with extreme output ranges (e.g., F5) [12, 22].
2. **Surrogate Fitting:** Train machine learning models to approximate the unknown black-box function [12].
3. **Candidate Generation:** Generate a large pool of potential query points using uniform random sampling, Sobol sequences, and local Gaussian perturbations around the current best observation [23, 24].
4. **Acquisition Scoring:** Score candidates using an acquisition function (Expected Improvement or Upper Confidence Bound) [20].
5. **Diversity Filtering:** Apply a distance-based penalty or nearest-neighbour filter to prevent the model from proposing near-duplicates of previously evaluated points [20, 25, 26].

## 2. Surrogate Models
To ensure robust predictions and reliable uncertainty estimates, a multi-model ensemble approach was heavily utilised, especially in higher dimensions [27]:
* **Gaussian Process Regression (GP):** The primary surrogate for low-dimensional or smooth functions (e.g., F1, F3, F5). GPs were configured with a Matérn ($\nu=2.5$) or RBF kernel with Automatic Relevance Determination (ARD) and a WhiteKernel to account for noise [20, 28-30].
* **Random Forest (RF):** Used for higher-dimensional, bumpy, or noisy landscapes (e.g., F4, F6, F7, F8). Uncertainty was estimated via the variance across the individual decision trees [12, 20, 27, 31].
* **Gradient Boosting Machines (GBM):** Used as an auxiliary cross-check. Uncertainty was captured by training an ensemble of GBMs across different random seeds [20, 31, 32].

## 3. Acquisition Strategies
To balance exploration (searching uncertain regions) and exploitation (refining known high-yield regions), two main acquisition functions were employed [20]:
* **Expected Improvement (EI):** Used heavily in early and mid-stages to mathematically quantify the expected magnitude of improvement over the current best observation [33-35].
* **Upper Confidence Bound (UCB):** Used to explicitly tune the exploration-exploitation trade-off using the $\kappa$ parameter. $\kappa$ was gradually decayed in later weeks (e.g., Weeks 5 and 6) to transition the optimiser into a pure exploitation and micro-tuning phase [36-38].

