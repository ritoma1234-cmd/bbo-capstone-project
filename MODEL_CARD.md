Part 2: Model Card for the BBO Optimisation Approach
1. Overview
Approach Name: Hybrid Evolutionary-Surrogate Optimisation Pipeline (HESOP)

Type: Multi-Stage Black-Box Optimiser (Sequential Surrogate-Guided Search)

Version: 10.0 (Final Iteration: Intrinsically Interpretable Tree Surrogate)

2. Intended Use
Suitable Tasks: Global continuous optimisation of expensive, non-convex black-box functions under strict sample budgets.

Avoided Use Cases: Real-time applications requiring sub-millisecond query execution, or unconstrained unbounded continuous search spaces.

3. Details and Strategy Evolution
My optimisation strategy evolved across distinct technical phases:

Rounds 1–3 (Linear Surrogates): Established baseline global boundaries using standard regression.

Rounds 4–6 (Deep Networks and Ensembles): Leveraged non-linear function approximators and hyperparameter tuning to explore complex spatial valleys.

Rounds 7–8 (Generative Transformers and RAG): Implemented multi-head self-attention and cross-attention memory buffers to capture long-range spatial context.

Rounds 9–10 (Interpretable Decision Trees): Replaced opaque networks with constrained decision trees (depth=3) to extract transparent, human-inspectable decision rules and feature importances.

4. Performance
Results Summary: Evaluated across 8 functions (2D to 8D). In Week 20 (Adv-GenAI), Functions 1 and 2 converged on boundary corners (1.000000, 0.000000), while complex functions like Function 8 explored interior vectors (0.000000, 0.269686, ...). In Week 21 (Interpretable-Tree), shallow decision trees centred flat leaf regions at interior centroids (0.500000, 0.500000) while discovering clear axis-aligned splits for Functions 2, 5, 6, and 7 (e.g., Function 5: 0.562083, 0.832273, 0.276274, 0.744630).

Metrics Used: Scalar objective value maximisation, feature importance weights, and decision complexity (tree depth and node count).

5. Assumptions and Limitations
Assumptions: Objective functions are continuous over [0, 1]^d; historical evaluations accurately reflect local spatial topology.

Limitations and Failure Modes: Constrained shallow decision trees produce step-wise output discretisation, causing gradient optimisers to plateau near region centroids (0.5) when no strong axis-aligned splits exist.

6. Ethical Considerations and Reflection
Transparency and Reproducibility: Documenting data lineage via datasheets and model architecture via model cards ensures that optimisation choices remain fully auditable. Switching to interpretable decision trees provides explicit decision rules, allowing stakeholders to understand why specific spatial regions were queried.

Model Card Structure: The current card structure effectively balances technical rigour with governance clarity. Adding further complexity would reduce readability without offering additional diagnostic value, as the decision rules are already intrinsically transparent.
