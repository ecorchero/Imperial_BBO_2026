This file serves as the project overview and satisfies the requirement for a non-technical write-up to help decision-makers understand the model's value and how to use it.
# Black-Box Optimisation (BBO) Challenge - Capstone Project

## Project Overview
This repository contains the code, data, and documentation for Phase 2 of the Imperial College ML/AI Capstone Project: The Black-Box Optimisation Challenge [1, 6]. The goal of this project is to iteratively find the maximum outputs for eight distinct "black-box" functions, where the internal equations are unknown [2]. 

Each of the eight functions simulates a complex, real-world scenario where conducting experiments is expensive or time-consuming, such as radiation detection, drug discovery, and machine learning hyperparameter tuning [2, 7, 8]. Because we cannot evaluate every possible combination, we must use machine learning to make intelligent, data-driven decisions about where to sample next [9].

## Non-Technical Summary
In real-world business and scientific applications, we often need to find the "best" configuration for a system—like the optimal mix of ingredients for a recipe or the best stock distribution across warehouses—without knowing the exact mathematical relationship between our choices and the final outcome [10, 11]. Trial-and-error is too costly. 

Instead of guessing randomly, this project uses a strategy called **Bayesian Optimisation** [8]. 
1. **Learn:** We use the small amount of historical data we have to train a machine learning model (a "surrogate"). This model acts as a proxy, guessing how the system will react to new configurations [12].
2. **Predict:** The model predicts not only the expected outcome of a new configuration but also how *uncertain* it is about that guess [13].
3. **Act:** We use these predictions to choose our next experiment, carefully balancing the need to *exploit* known good areas and *explore* highly uncertain areas to avoid missing hidden improvements [8].

By repeating this loop, we efficiently zero in on the optimal solutions using a fraction of the time and resources that a brute-force search would require.

## Repository Structure
* `/data`: Contains the initial and weekly aggregated `.npy` datasets for Functions 1-8 [14, 15].
* `/notebooks`: Contains the weekly Jupyter notebooks used for data exploration, surrogate training, and candidate generation [16, 17].
* `datasheet.md`: Documents the characteristics, origin, and limitations of the datasets [3].
* `model_card.md`: Details the machine learning surrogate models used in the optimisation loop [3].
* `methodology.md`: Explains the technical pipeline and acquisition strategies employed.

## Usage
To run the weekly optimisation loops, navigate to the `notebooks` directory and execute the Jupyter notebooks sequentially. Ensure that the paths to the `data` folder are correctly configured in the `CONFIG` section of each notebook [18].

