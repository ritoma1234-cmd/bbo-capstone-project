# Datasheet: Black-Box Optimization Capstone Dataset (Modules 12 to 21)

## 1. Motivation
* Purpose: This dataset was generated to record and evaluate sequential query decisions across eight continuous black-box objective functions.
* Task Supported: It supports training machine learning surrogate models to locate global maximum values within bounded multi-dimensional search spaces under a strict query budget.
* Creators: Created as part of the Imperial College Executive Education Machine Learning and Artificial Intelligence Capstone Project.

## 2. Composition
* Dataset Contents: Input coordinate arrays and matching scalar objective evaluations across eight functions ranging from two to eight dimensions.
* Initial Shapes (Week 12 Baseline):
  * Function 1 (2D): Initial input shape (10, 2), output shape (10,)
  * Function 2 (2D): Initial input shape (10, 2), output shape (10,)
  * Function 3 (3D): Initial input shape (15, 3), output shape (15,)
  * Function 4 (4D): Initial input shape (30, 4), output shape (30,)
  * Function 5 (4D): Initial input shape (20, 4), output shape (20,)
  * Function 6 (5D): Initial input shape (20, 5), output shape (20,)
  * Function 7 (6D): Initial input shape (30, 6), output shape (30,)
  * Function 8 (8D): Initial input shape (40, 8), output shape (40,)
* Data Growth: Exactly ten sequential rounds of queries were executed between Week 12 and Week 21, adding ten new evaluation points per function.
* Completeness: No missing, corrupt, or null values exist in the logged arrays.

## 3. Collection Process
* Query Generation Across the 10 Rounds:
  * Week 12: Gaussian Process with RBF kernel and random sampling exploration
  * Week 13: Gaussian Process with Matern kernel and Upper Confidence Bound (UCB) optimization
  * Week 14: Support Vector Classification mapping top 20 percent performance boundaries
  * Week 15: Multi-Layer Perceptron neural network regression
  * Week 16: Advanced Multi-Layer Perceptron with L2 weight regularization to smooth edges
  * Week 17: CNN-parallel MLP regressor utilizing logistic sigmoid activation curves
  * Week 18: Automated hyperparameter-tuned hybrid ensemble (Gaussian Process plus Random Forest)
  * Week 19: Generative AI Transformer surrogate with multi-head self-attention
  * Week 20: Advanced Generative Transformer with cross-attention memory conditioned on elite points
  * Week 21: Intrinsically interpretable Decision Tree Regressors constrained to depth 3
* Time Frame: Collected weekly across Modules 12 to 21.

## 4. Preprocessing and Uses
* Coordinate Normalization: All input coordinates are strictly bounded inside the continuous unit hypercube between 0.000000 and 1.000000.
* Target Transformations: Target objective returns were flattened to one-dimensional vectors and normalized to a 0 to 1 range during deep learning rounds to stabilize gradients.
* Intended Uses: Benchmarking black-box optimization surrogates, evaluating model transparency, and studying exploration-exploitation tradeoffs.
* Inappropriate Uses: Using these synthetic outputs directly as ground-truth for physical systems without adjusting for real-world operational friction, wear, or noise.

## 5. Distribution and Maintenance
* Repository: Maintained publicly in this GitHub repository.
* License: Open-source educational use under the MIT License.
* Maintenance: Maintained by the repository owner for capstone portfolio evaluation.
