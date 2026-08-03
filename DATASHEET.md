# bbo-capstone-project
Black-Box Optimization Capstone Project featuring Datasheet and Model Card documentation.
Part 1: Datasheet for the BBO Capstone Project Dataset
1. Motivation
Creation Purpose: I created this dataset to track, benchmark, and optimise 8 unknown black-box objective functions across an iterative 10-round search process.

Supported Task: It supports continuous global optimisation within a bounded unit hypercube [0, 1]^d, facilitating the evaluation of surrogate model predictions, acquisition strategies, and decision complexity.

2. Composition
Contents: The dataset consists of continuous coordinate query vectors (2D to 8D) paired with their evaluated scalar objective outputs.

Size and Format: Contains 10 sequential query rounds across 8 functions, stored as standardised float strings (formatted to 6 decimal places) in Python dictionaries (inputs_dict and outputs_dict).

Gaps: No records are missing, though early rounds feature sparse coverage in higher dimensions (for example, Function 8 in 8D space), leaving spatial uncertainty gaps.

3. Collection Process
Query Generation Strategy: Queries were generated sequentially using evolving surrogate strategies: progressing from linear baselines to neural networks, ensemble tuning, generative LLM transformers, agentic RAG cross-attention models, and finally shallow optimal decision trees.

Timeframe: Data collection occurred across 10 sequential operational cycles (Weeks 11 through 21).

4. Preprocessing and Uses
Transformations: All inputs were normalised to [0, 1]^d. In later rounds, RAG memory retrieval (filtering the top 25th percentile) was applied to isolate elite trajectory contexts.

Intended Uses: Benchmarking black-box optimisation routines, testing surrogate model fidelity, and analysing trade-offs between model complexity, transparency, and accuracy.

Inappropriate Uses: Direct deployment in real-time safety-critical control systems without empirical variance estimation or domain recalibration.

5. Distribution and Maintenance
Availability: Stored locally in Google Colab notebook environments and archived in the Capstone Project Portal repository.

Maintenance and Terms: Maintained solely by the primary researcher (myself) as an academic project. Updates occur sequentially after each portal submission round.
