This file is required to explain the provenance, shape, and characteristics of the data you are working with.
# Datasheet for Black-Box Optimisation Datasets

## Motivation and Provenance
The datasets used in this project were provided as part of the Imperial College ML/AI Capstone Project [1, 39]. They represent evaluations of eight synthetic, deterministic, or noisy "black-box" functions simulating real-world objectives [2]. 

## Dataset Composition
The data is structured into 8 distinct functions, starting with a small initial batch of experiments (10 to 30 points) provided as `.npy` arrays [7, 15, 40]. The datasets grew iteratively by one observation per week via queries submitted to a closed portal oracle [41]. All input variables are continuous and constrained to the $[42]$ interval [43, 44]. The output is a single scalar target to be maximised [40].

### Function Summaries
* **Function 1 (2D):** Simulates a radiation field. Mostly flat, sparse landscape with rare, weak, localised signals [7, 45].
* **Function 2 (2D):** Simulates black-box ML likelihood optimisation. Highly noisy landscape requiring cautious Bayesian exploration [46, 47].
* **Function 3 (3D):** Simulates drug discovery compound combinations. Optimising for minimal side effects (maximising a transformed negative score). Smooth landscape requiring fine local tuning [8, 48].
* **Function 4 (4D):** Simulates dynamic warehouse placement. Features a highly non-linear, rugged landscape with a large spread of negative outputs and multiple local optima [10, 49, 50].
* **Function 5 (4D):** Simulates chemical process yield. Features a steep, unimodal peak with extreme dynamic output ranges (from $0.1$ to $>4000$) requiring log-transformations [11, 51, 52].
* **Function 6 (5D):** Simulates a cake recipe optimisation penalised for cost and waste. Exhibits strong linear trends on specific dimensions (e.g., $x_4, x_5$) forming a high-performance ridge [11, 43, 53].
* **Function 7 (6D):** Simulates tuning hyperparameters for an ML model. Moderate to high signal-to-noise ratio where 2-3 dimensions heavily dominate performance [54-56].
* **Function 8 (8D):** Simulates complex ML hyperparameter tuning. High-dimensional landscape where global optimisation is difficult, requiring local basin refinement [56-58].

## Limitations and Biases
Because the underlying physical or mathematical models are hidden, the datasets are purely observational [59]. The primary limitation is data sparsity: particularly in higher dimensions (e.g., F7 and F8), 30-40 observations are insufficient to map the global topology, meaning the datasets are heavily biased toward the specific local basins chosen during the weekly acquisition process [58, 60]. 

