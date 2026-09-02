# Model Card: Sequential Black-Box Optimization Pipeline (Modules 12 to 21)

## 1. Overview
* Model Name: Sequential Surrogate Optimization Framework
* Model Type: Multi-model sequential surrogate pipeline evolving from Gaussian Processes to Deep Neural Networks, Transformers, and Interpretable Decision Trees
* Version: 21.0 (Module 21 Milestone)

## 2. Intended Use
* Primary Applications: Finding optimal parameters in unknown, expensive, continuous search spaces bounded within 0.0 to 1.0 across two to eight dimensions.
* Appropriate Domains: Hyperparameter optimization, chemical formulation, aerodynamic shape parameterization, and simulated industrial control.
* Inappropriate Uses: Real-time safety-critical control requiring microsecond guarantees, or high-frequency discrete combinatorial tasks.

## 3. Details: Strategy Evolution Across the 10 Rounds
* Week 12 (Gaussian Process Baseline): Used an RBF kernel to evaluate wide hypercube corners, querying points such as 0.003918-0.999461 (Function 1) and 0.838173-0.999697 (Function 2).
* Week 13 (Matern Kernel UCB): Swapped to a fixed Matern kernel with local L-BFGS-B search, focusing queries along early slopes such as 0.907033-0.806627 (Function 1) and 0.841847-1.000000 (Function 2).
* Week 14 (Support Vector Machines): Framed search as classification into the top 20 percent reward region, querying 0.845989-0.892633 (Function 1) and 0.492514-0.581258 (Function 2).
* Week 15 (MLP Neural Networks): Deployed multi-layer perceptrons with ReLU activation, which led several functions to saturate at boundary extremes like 0.806069-0.000000 (Function 1) and 0.000000-1.000000 (Function 2).
* Week 16 (L2-Regularized Neural Networks): Added L2 weight decay (alpha = 0.1) to test boundary stability, producing queries like 0.048485-0.137869 (Function 1) and 1.000000-1.000000 (Function 2).
* Week 17 (Sigmoid MLP Regressors): Introduced logistic activation functions to smooth non-linear valleys, retrieving interior points like 0.438768-0.086993 (Function 1) and 0.224206-0.264054 (Function 2).
* Week 18 (Hyperparameter-Tuned Hybrid Ensemble): Combined Gaussian Processes with Random Forests, producing high-confidence queries including 0.000000-1.000000 (Function 1) and 1.000000-0.000000 (Function 2).
* Week 19 (Generative AI Transformers): Formatted query history as sequence tokens into multi-head self-attention networks, querying 0.181825-1.000000 (Function 1) and 0.684233-0.440152 (Function 2).
* Week 20 (Agentic Cross-Attention Transformers): Guided search using cross-attention memory over elite historical evaluations, generating 1.000000-0.000000 (Function 1) and 1.000000-1.000000 (Function 2).
* Week 21 (Transparency and Interpretability): Trained shallow decision trees (depth 3) to extract clear rules and feature importance. For flat leaf spaces, continuous acquisition defaulted to midpoints (0.500000-0.500000 for Function 1, 0.500000-0.500001-0.499999 for Function 3, 0.500002-0.499998-0.499996-0.500000 for Function 4, and 0.493635-0.498692-0.498444-0.501292-0.498454-0.504360-0.495538-0.505885 for Function 8). For active functions, trees isolated key coordinate splits (0.621807-0.447875 for Function 2 and 0.562083-0.832273-0.276274-0.744630 for Function 5).

## 4. Performance Across the Eight Functions
* Metric: Maximizing cumulative scalar objective returns across iterations.
* Key Historical Peaks Observed:
  * Function 1 (2D): Initial baseline peak of 7.71e-16 at [0.731024, 0.733000].
  * Function 2 (2D): Peak reward of 0.611205 at [0.702637, 0.926564], closely targeted in Week 13 and Week 21.
  * Function 3 (3D): Peak reward of -0.034835 at [0.492581, 0.611593, 0.340176].
  * Function 4 (4D): Peak reward of -4.025542 at [0.577766, 0.428772, 0.425826, 0.249007].
  * Function 5 (4D): High-gain peak reward of 1088.859618 at [0.224189, 0.846480, 0.879484, 0.878516], with tree rules prioritizing x2 (weight 0.8638).
  * Function 6 (5D): Peak reward of -0.714265 at [0.728186, 0.154693, 0.732552, 0.693997, 0.056401], with tree rules highlighting x5 (weight 0.7453).
  * Function 7 (6D): Peak reward of 1.364968 at [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970], with tree rules highlighting x1 (weight 0.5981).
  * Function 8 (8D): Peak reward of 9.598482 at [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085].

## 5. Assumptions and Limitations
* Assumptions: Functions are bounded strictly between 0 and 1, continuous, and possess localized basins of attraction.
* Limitations:
  * Decision tree leaf functions lack continuous gradient information, causing local acquisition solvers to default to geometric center points (0.500000) when variance is low.
  * Deep neural networks and transformers risk overfitting to single extreme outliers in high dimensions unless smoothed with regularization or clustering.

## 6. Ethical Considerations and Governance
* Reproducibility: Fully logging dataset dimensions, model architectures, and weekly outputs ensures that experimental claims can be audited and reproduced by peers.
* Real-World Impact: Providing transparent model cards prevents deploying opaque models in high-risk engineering scenarios where untested parameter predictions could damage physical testing hardware.
