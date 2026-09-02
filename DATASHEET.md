# Comprehensive Datasheet: Black-Box Optimisation (Functions 1–8)

This datasheet documents the optimisation decisions, dataset characteristics, and performance outcomes across all eight black-box functions for the capstone project.

---

## 1. Function Overview & Baseline Summary

1. Which functions does this datasheet describe?
* Functions 1 through 8 across two to eight continuous dimensions.

2. What real-world scenarios do these functions simulate?
* Function 1 (2D): Environmental contamination detection and sensor array placement.
* Function 2 (2D): Chemical reaction yield optimisation under pressure and temperature constraints.
* Function 3 (3D): Multi-stage industrial manufacturing throughput.
* Function 4 (4D): Thermal and energy distribution balancing in composite structures.
* Function 5 (4D): High-gain mechanical or acoustic resonance tuning.
* Function 6 (5D): Aerodynamic wing profile drag minimisation.
* Function 7 (6D): Multi-variable logistics and supply chain routing throughput.
* Function 8 (8D): High-dimensional algorithmic and robotic policy optimisation.

3. Dimensionality, Initial Dataset Shapes, and Outputs:

| Function | Dimension | Input Shape | Output Shape | Initial Best Reward (y) | Best Observed Coordinates |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Function 1 | 2D | (10, 2) | (10,) | 7.710875e-16 | [0.731024, 0.733000] |
| Function 2 | 2D | (10, 2) | (10,) | 0.611205 | [0.702637, 0.926564] |
| Function 3 | 3D | (15, 3) | (15,) | -0.034835 | [0.492581, 0.611593, 0.340176] |
| Function 4 | 4D | (30, 4) | (30,) | -4.025542 | [0.577766, 0.428772, 0.425826, 0.249007] |
| Function 5 | 4D | (20, 4) | (20,) | 1088.859618 | [0.224189, 0.846480, 0.879484, 0.878516] |
| Function 6 | 5D | (20, 5) | (20,) | -0.714265 | [0.728186, 0.154693, 0.732552, 0.693997, 0.056401] |
| Function 7 | 6D | (30, 6) | (30,) | 1.364968 | [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970] |
| Function 8 | 8D | (40, 8) | (40,) | 9.598482 | [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085] |

4. What does the output represent?
* A scalar continuous objective reward value, where higher values correspond to improved system performance, minimum loss, or maximum yield.

---

## 2. Nature of the Data

1. Structure of the Dataset:
* The initial datasets varied in scale proportionally to dimension, ranging from 10 points in 2D up to 40 points in 8D. All input features are bounded within the continuous unit hypercube from 0.000000 to 1.000000.

2. Dataset Evolution Across Iterations:
* The dataset grew by exactly one evaluated coordinate vector per function across each sequential round (Weeks 12 to 24), adding 13 new high-value evaluations per function.
* Early queries explored perimeter boundaries, while later queries focused on localised cluster neighbourhoods.

3. Function Noise and Stochasticity:
* Low-dimensional surfaces (Functions 1, 2, and 3) behaved deterministically with minimal measurement noise.
* Function 5 exhibited extreme output sensitivity and scale (peaking over 1088.85), indicating steep, sharp resonance ridges.
* High-dimensional spaces (Functions 7 and 8) exhibited apparent variance driven primarily by extreme spatial sparsity rather than pure random noise.

4. Landscape Topology (Modality and Smoothness):
* Multimodal and Smooth: Functions 2, 3, 5, and 6 showed clear multimodal surfaces with multiple competing local peaks.
* Complex Coupled Surfaces: Functions 4, 7, and 8 exhibited non-convex behavior where individual parameters had low isolated impact, requiring subspace combinations to navigate.

---

## 3. Optimization Strategy

1. Optimization Methods Used:
* Weeks 12 to 13: Gaussian Process regression (RBF and Matern 5/2 kernels) with Upper Confidence Bound (UCB) multi-start local optimisation.
* Weeks 14 to 18: Support Vector Machine boundary classification, L2-regularized Multi-Layer Perceptrons (MLPs), and hybrid Random Forest plus Gaussian Process ensembles.
* Weeks 19 to 20: Generative Transformer architectures featuring multi-head self-attention and cross-attention memory conditioned on elite points.
* Weeks 21 to 24: Constrained Decision Trees (depth 3) for rule transparency, K-Means density clustering, Principal Component Analysis (PCA) subspace reduction, and Extra-Trees Q-value policy search.

2. Strategy Justification by Function Characteristics:
* Low Dimensions (2D to 3D): Gaussian Processes and K-Means clustering efficiently mapped local peaks without dimensional overhead.
* High Dimensions (4D to 8D): PCA dimensionality reduction and transformer attention were essential to eliminate uninformative orthogonal directions and combat the curse of dimensionality.

3. Balancing Exploration and Exploitation:
* In Rounds 12 through 16, 70 percent of the query budget was dedicated to broad exploration along hypercube boundaries using high UCB exploration coefficients.
* From Round 21 onward, exploration parameters decayed to near zero, allocating 85 to 90 percent of the budget to local exploitation around verified cluster centroids.

4. Strategy Adaptations:
* Deep neural networks overfitted to boundary edges in Weeks 15 and 16 (hitting boundary 1.0 or 0.0 values). This prompted the shift toward regularised decision trees, density clustering, and PCA to enforce multi-point spatial support before querying.

---

## 4. Data Handling and Preprocessing

1. Rescaling and Normalization:
* Input coordinates were strictly mapped to the 0.0 to 1.0 hypercube. Target outputs were min-max normalised during neural network and reinforcement learning phases to prevent gradient explosion on high-gain surfaces (Function 5).

2. Surrogate Models Trained:
* GaussianProcessRegressor, Support Vector Classifiers, Multi-Layer Perceptrons, Transformer neural networks, DecisionTreeRegressor, KMeans, PCA, and ExtraTreesRegressor.

3. Preprocessing Requirements:
* L2 weight decay (alpha = 0.1) on neural layers to smooth steep boundaries, tree depth truncation to 3 to prevent leaf memorisation, and flattening 1D arrays for standard estimator pipelines.

4. Outlier Handling:
* In Weeks 22 to 24, elite percentile filtering (top 30 to 35 percent) was applied to isolate core reward clusters and filter out unrepeatable, isolated high-noise queries.

---

## 5. Performance and Results

1. Best Output Values and Coordinates Achieved:
* Function 1 (2D): y ≈ 7.710875e-16 at [0.731024, 0.733000]
* Function 2 (2D): y = 0.611205 at [0.702637, 0.926564] (reinforced in Week 22 at 0.709246-0.926283)
* Function 3 (3D): y = -0.034835 at [0.492581, 0.611593, 0.340176]
* Function 4 (4D): y = -4.025542 at [0.577766, 0.428772, 0.425826, 0.249007]
* Function 5 (4D): y = 1088.859618 at [0.224189, 0.846480, 0.879484, 0.878516]
* Function 6 (5D): y = -0.714265 at [0.728186, 0.154693, 0.732552, 0.693997, 0.056401]
* Function 7 (6D): y = 1.364968 at [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970]
* Function 8 (8D): y = 9.598482 at [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085]

2. Confidence in Global Optimality:
* High Confidence (Functions 1, 2, 5): Evaluations formed dense spatial clusters where variance dropped significantly around centroids.
* Moderate Confidence (Functions 4, 7, 8): In 6D and 8D spaces, although strong directional gradients were discovered along Principal Component 1, complete global exploration remains challenging due to dimensional volume.

---

## 6. Ethical, Practical, and General Considerations

1. Real-World Applications:
* The iterative sequential surrogate framework directly mirrors industrial lab-in-the-loop workflows such as pharmaceutical drug candidate formulation, battery cell testing, and aerodynamic prototype optimisation where real-world evaluations are expensive.

2. Limitations of Synthetic Benchmarks:
* Synthetic functions evaluate instantaneously without mechanical delays, whereas real-world machinery experiences sensor drift, thermal degradation, and physical fatigue.

3. Scalability to Expensive Systems:
* Yes. The methodology prioritises sample efficiency through noise filtering, clustering, and PCA reduction, minimising wasteful queries and reducing the risk of testing hazardous parameter combinations.

4. Future Risks and Pitfalls:
* Users must avoid unconstrained shallow decision trees in continuous spaces without smoothing, as flat step-wise plateaus cause optimisers to stall at midpoint defaults (0.500000).
