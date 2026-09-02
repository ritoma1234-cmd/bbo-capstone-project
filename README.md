# BLACK-BOX OPTIMISATION CAPSTONE CHALLENGE

PROJECT MOTIVATION AND REAL-WORLD FRAMING
Real-world experimentation in areas such as chemical formulation, pharmaceutical drug discovery, and aerodynamic design is expensive, time-consuming, and resource-capped. In these domains, practitioners cannot afford exhaustive grid or brute-force searches. This capstone project addresses that challenge through sequential Black-Box Optimisation (BBO).
The objective is to discover the global maximum reward values across eight unknown, continuous, non-convex objective functions bounded within the unit hypercube from 0.000000 to 1.000000, spanning two to eight dimensions. Because the mathematical formulas of the underlying surfaces are withheld, each evaluation query must be treated as a valuable measurement.
The goal is to maximise performance returns while operating under a strict sample budget over 13 consecutive optimisation rounds.

CORE OPTIMISATION CHALLENGES
1. The Curse of Dimensionality:
In lower-dimensional functions (Functions 1 and 2 in 2D, Function 3 in 3D), evaluations rapidly map local peak structures. In contrast, higher-dimensional spaces, most notably Function 8 (8D) with 40 initial evaluations, suffer from severe spatial sparsity where points become isolated in the volume of the hypercube.

2. Non-Convexity and Local Traps:
The surfaces feature multimodal landscapes with misleading local peaks, steep valleys, and periodic ripples. Unconstrained local search routines risk premature convergence in suboptimal basins.

3. Overfitting versus Generalisation:
Early experiments demonstrated that flexible, high-capacity regressors (such as unregularized deep neural networks) easily overfit to isolated high-reward outliers at extreme boundary coordinates (such as all 1.0 or 0.0 values).

THE EVOLVING SURROGATE PIPELINE (HESOP)
Rather than relying on a static algorithm, our approach developed an evolving sequential surrogate pipeline called HESOP (Hierarchical Evolving Surrogate Optimisation Pipeline):

- Global Boundary Exploration (Weeks 12 to 13):
Deployed Gaussian Process regression using Radial Basis Function (RBF) and Matern 5/2 covariance kernels paired with Upper Confidence Bound (UCB) multi-start local optimisation (L-BFGS-B) to map initial landscape gradients.

- Boundary Classification and Neural Ensembles (Weeks 14 to 18):
Introduced Support Vector Machine classifiers to identify high-probability elite zones (top 20th percentile), followed by Multi-Layer Perceptrons (MLPs) smoothed with L2 weight decay (alpha = 0.1) and hybrid Random Forest ensembles to capture non-linear curvatures.

- Generative Modelling and Attention Mechanisms (Weeks 19 to 20):
Formatted past evaluation trajectories as sequential tokens using multi-head self-attention and cross-attention memory conditioned on elite historical observations.

- Transparency and Interpretability (Week 21):
Implemented intrinsically interpretable Decision Tree Regressors constrained to a maximum depth of 3. This provided rule transparency and verified feature importance splits (for example, establishing x1 as the dominant driver in Function 2 with an importance weight of 1.0000).

- Unsupervised Clustering and PCA Subspace Search (Weeks 22 to 23):
Applied K-Means clustering across elite points (top 30 to 35%) to filter unrepeatable noise spikes and calculate multi-point centroids. For Function 8, Principal Component Analysis (PCA) captured over 56% of reward variance in the first two principal components, enabling efficient latent-space sampling that counteracted dimensional sparsity.

- Reinforcement Learning Policy Search (Week 24):
Formulated final candidate selection as continuous Multi-Armed Bandit policy optimisation using an Extra-Trees ensemble Q-value estimator with decaying exploration bonuses to ensure maximum exploitation inside verified optimal basins.


FLOWCHART OVERVIEW
[Raw Historical Evaluation History: Inputs 0 to 1, Outputs y]
                            |
                            v
[Preprocessing and Elite Data Filtering: Min-Max Target Scaling, Top 30% Elite Masking]
                            |
           +----------------+----------------+
           |                                 |
           v                                 v
[Low Dimensions 2D: K-Means]      [High Dimensions 8D: PCA Subspace]
- Density Basins                  - Dominant Eigenaxes
- Outlier Filtering               - Latent K-Means
           |                                 |
           +----------------+----------------+
                            |
                            v
[Surrogate and Acquisition Engine: GP / Extra-Trees Q-Estimator with UCB Policy]
                            |
                            v
[Optimal Candidate Coordinate: Example 0.702637-0.926564 for Function 2]

## NON-TECHNICAL EXPLANATION OF YOUR PROJECT
Imagine trying to find the highest peak in a vast, fog-covered mountain range where every step costs significant time and resources. Rather than wandering blindly, this project builds an intelligent mapping system. Using machine learning, the system analyses past measurements to model the terrain's hidden structure, discard misleading dead ends, and pinpoint the highest elevations. Across 13 sequential iterative rounds, our pipeline transitioned from broad exploration to focused exploitation, discovering optimal values across eight complex, multi-dimensional systems while strictly conserving evaluation resources.

## DATA
The dataset consists of sequential evaluation queries across eight black-box objective functions ranging from 2D to 8D. All input parameters are strictly continuous and bounded within the unit hypercube [0.000000, 1.000000]. Initial datasets included 10 to 40 starting evaluations per function, with one new evaluation point added per function across 13 weekly optimisation rounds (Modules 12 to 24).

## MODEL
Our optimisation architecture deploys a sequential surrogate pipeline called HESOP (Hierarchical Evolving Surrogate Optimisation Pipeline):
* **Early Rounds (Weeks 12–13):** Gaussian Processes with RBF and Matern kernels paired with Upper Confidence Bound (UCB) acquisition for initial boundary exploration.
* **Intermediate Rounds (Weeks 14–20):** Support Vector Machines, L2-regularised Multi-Layer Perceptrons, and Generative Transformers with cross-attention memory conditioned on elite points.
* **Interpretable Phase (Week 21):** Intrinsically interpretable Decision Trees (depth 3) to extract explicit decision rules and coordinate importance splits.
* **Final Rounds (Weeks 22–24):** K-Means density clustering to filter spatial noise, Principal Component Analysis (PCA) to counteract 8D sparsity, and Extra-Trees Q-value policy search for final exploitation.

## HYPERPARAMETER OPTIMISATION
Surrogate models and acquisition routines were tuned by the following:
* 5-fold cross-validation and grid search for neural network layer configurations (64, 32) and L2 regularisation weight decay (alpha = 0.1) to smooth sharp boundaries.
* Matern kernel length-scale bounds and an adaptive Upper Confidence Bound schedule where exploration coefficients decayed from 3.0 down to near zero as query confidence increased[cite: 5].
* Constrained tree depth (max_depth = 3, min_samples_leaf = 2) to maintain human interpretability and prevent overfitting to spatial outliers.

## RESULTS
The pipeline successfully isolated high-reward basins across all eight functions
* Function 1 (2D): Peak reward ≈ 7.71e-16 at [0.731024, 0.733000]
* Function 2 (2D): Peak reward = 0.611205 at [0.702637, 0.926564] (reinforced by Week 22 cluster query at 0.709246-0.926283)
* Function 3 (3D): Peak reward = -0.034835 at [0.492581, 0.611593, 0.340176]
* Function 4 (4D): Peak reward = -4.025542 at [0.577766, 0.428772, 0.425826, 0.249007]
* Function 5 (4D): Peak reward = 1088.859618 at [0.224189, 0.846480, 0.879484, 0.878516]
* Function 6 (5D): Peak reward = -0.714265 at [0.728186, 0.154693, 0.732552, 0.693997, 0.056401]
* Function 7 (6D): Peak reward = 1.364968 at [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970]
* Function 8 (8D): Peak reward = 9.598482 at [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085]

## PROJECT DOCUMENTATION
* [Comprehensive Datasheet](DATASHEET.md)
* [Model Card](MODEL_CARD.md)
* [Module 21 Datasheet](DATASHEET_MODULE_21.md)
* [Module 21 Model Card](MODEL_CARD_MODULE_21.md)
