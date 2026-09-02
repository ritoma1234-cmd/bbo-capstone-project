# Model Card: Sequential Black-Box Optimisation Pipeline (Functions 1–8)

## 1. Overview
* Model Name: Hierarchical Evolving Surrogate Optimisation Pipeline (HESOP)
* Model Type: Sequential black-box surrogate optimiser integrating Gaussian Processes, Decision Trees, K-Means Clustering, PCA Subspace Projection, and Reinforcement Learning Upper Confidence Bound (UCB) Policy Search
* Version: 2.0 (Final Capstone Release)

## 2. Intended Use
* Primary Applications: Sample-efficient optimisation of expensive, unknown, non-convex objective functions bounded within the continuous unit hypercube [0.000000, 1.000000] across 2 to 8 dimensions.
* Appropriate Domains: Automated scientific experimentation, chemical reaction parameter tuning, aerodynamic wing profile design, and robotic policy parameterisation.
* Inappropriate Uses: Real-time safety-critical control loops requiring microsecond inference, or unbounded combinatorial optimisation problems.

## 3. Optimisation Architecture & Strategic Evolution (Weeks 12–24)
* Weeks 12 to 13 (Bayesian Baselines): Gaussian Process regression with RBF and Matern 5/2 covariance kernels paired with Upper Confidence Bound (UCB) local multi-start optimisation (L-BFGS-B) to map initial boundaries.
* Weeks 14 to 18 (Classification & Deep Ensembles): Support Vector Machine classifiers isolating top 20% performance regions, followed by Multi-Layer Perceptrons (MLPs) with L2 weight decay (alpha = 0.1) and hybrid Random Forest ensembles.
* Weeks 19 to 20 (Generative Attention Models): Transformer surrogates with multi-head self-attention and cross-attention memory conditioned on elite evaluation histories.
* Week 21 (Interpretable Decision Trees): Constrained Decision Tree Regressors (max_depth = 3) to extract human-readable decision rules and feature importance splits (e.g., establishing x1 as the primary driver for Function 2 with weight 1.0000).
* Weeks 22 to 23 (Clustering & PCA Subspace Search): Unsupervised K-Means clustering across top 30 to 35% evaluations to isolate core density basins, paired with Principal Component Analysis (PCA) to capture dominant variance directions and overcome sparsity in high dimensions (Function 8).
* Week 24 (Reinforcement Learning Policy Search): Continuous Multi-Armed Bandit policy optimisation using an Extra-Trees ensemble Q-value estimator with decaying exploration bonuses for maximum exploitation of verified peaks.

## 4. Performance Across the Eight Functions
* Metric: Cumulative scalar objective reward maximisation across sequential iterations.
* Peak Values & Observed Locations:
  * Function 1 (2D): Peak reward ≈ 7.710875e-16 at [0.731024, 0.733000]
  * Function 2 (2D): Peak reward = 0.611205 at [0.702637, 0.926564] (reinforced in Week 22 at 0.709246-0.926283)
  * Function 3 (3D): Peak reward = -0.034835 at [0.492581, 0.611593, 0.340176]
  * Function 4 (4D): Peak reward = -4.025542 at [0.577766, 0.428772, 0.425826, 0.249007]
  * Function 5 (4D): High-gain peak reward = 1088.859618 at [0.224189, 0.846480, 0.879484, 0.878516]
  * Function 6 (5D): Peak reward = -0.714265 at [0.728186, 0.154693, 0.732552, 0.693997, 0.056401]
  * Function 7 (6D): Peak reward = 1.364968 at [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970]
  * Function 8 (8D): Peak reward = 9.598482 at [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085]

## 5. Assumptions and Limitations
* Assumptions: Evaluation surfaces are locally continuous within the unit hypercube [0, 1] per dimension and exhibit localised reward basins.
* Limitations:
  * Decision tree surrogates produce flat step-wise plateaus that lack local gradient information, causing acquisition optimisers to default to midpoint values (0.500000).
  * High-dimensional functions (Functions 7 and 8) experience significant spatial sparsity that requires dimensional reduction (PCA) to prevent regression models from overfitting to isolated queries.

## 6. Ethical Considerations & Real-World Translation
* Reproducibility & Auditing: Comprehensive logging of pipeline architectures, hyperparameters, and query coordinates ensures that experimental trajectories can be fully audited and independently verified.
* Risk Mitigation in Physical Testing: Deploying sample-efficient surrogate models prevents testing destructive or hazardous parameter configurations in expensive physical hardware, reducing industrial waste and testing risks.
