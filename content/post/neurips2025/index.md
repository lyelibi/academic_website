---
title: NeurIPS 2025
subtitle: 

# Summary for listings and search engines
summary: Curated list of research posted from the 39th Annual Conference on Neural Information Processing Systems (NeurIPS 2025).

# Link this post with a project
projects: []

# Date published
date: "2025-12-13T00:00:00Z"

# Date updated (optional)
lastmod: "2025-12-13T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
# image:
#   caption: 'Image credit: [**Unsplash**](https://unsplash.com/)'
#   focal_point: ""
#   placement: 2
#   preview_only: false

authors:
- admin

tags:
- Research
- Analysis

categories:
- Blog
---

# NeurIPS 2025: Research Highlights & Commentary

**Conference Overview**
The 39th Annual Conference on Neural Information Processing Systems (NeurIPS 2025) in San Diego highlighted a distinct shift from purely generative tasks toward "AI for Science" and industrial-grade engineering. Key themes (I was interested in) included scaling graph analytics to billion-node networks, improving the interpretability of financial models, and the rigorous benchmarking of tabular data.

Below is a curated categorization of research posters, with a specific focus on their applicability to industrial contexts, financial services, and high-dimensional data environments.

---

## 1. Next-Gen Tree Models & Tabular Data
**Category Summary:**
While Deep Learning dominates perception tasks (vision, NLP), tree-based models (XGBoost, Random Forests) remain the gold standard in industry for tabular data. This year's research focuses on hybridizing the two: identifying ways to make trees differentiable, more expressive via non-linear splits, or interpretable via graph theory. The goal is to handle the "big data" regime—millions of rows and thousands of columns—typical of banking and insurance.

### Learning Gradient Boosted Decision Trees with Algorithmic Recourse

[Read the Paper (OpenReview)](https://openreview.net/forum?id=r9zzTQLnxw)

* **Key Finding:** The method incorporates a "recourse loss" into GBDT training, ensuring that executable actions exist for users to overturn unfavorable predictions.
* **Commentary:** The idea here is clever: train a model with an additional recourse loss to ensure an actionable variable exists to overturn a prediction. Having worked in financial services, where models must be explainable (often via Shapley values), this paper is puzzling but promising. In the US, an adverse credit decision requires a "reason," usually linked to model features via Shapley values. However, important features aren't always actionable (e.g., a customer cannot instantly change their age or credit history length). This model goes further than policy requires by learning a "causal" intervention—essentially guaranteeing we can supply an action for the customer to improve their odds. The main risk is whether this makes the model prone to "gaming," but it represents a more proactive approach to ethical lending.

{{< figure src="/uploads/posters/recourse_poster.jpg" title="Learning Gradient Boosted Decision Trees with Algorithmic Recourse — Poster" >}}

### Empowering Decision Trees Via Shape Function Branching

[Read the Paper (arXiv:2510.19040)](https://arxiv.org/abs/2510.19040)

* **Key Finding:** "Shape Generalized Trees" (SGT) split data based on learned univariate shape functions rather than axis-aligned thresholds, achieving higher accuracy with more compact trees.
* **Commentary:** Decision trees do not need to be overly large if we re-imagine how splitting happens. While the classical CART algorithm builds axis-aligned splits, "ShapeCART" allows splitting on one variable with multiple thresholds, introducing non-linear decision boundaries. This aligns with recent research on oblique trees (which split using multiple variables) but keeps the trees more compact—an interesting property when considering embeddings from random forests. While ShapeCART performs significantly better in benchmarks, the real challenge is industrial relevance. In financial services, "big data" means 5–10 million rows and thousands of columns. New methods must be evaluated in that high-dimensionality regime to be truly viable.

{{< figure src="/uploads/posters/shapecart_poster.jpg" title="Empowering Decision Trees Via Shape Function Branching — Poster" >}}

### TabSTAR: A Tabular Foundation Model for Tabular Data with Text Fields

[Read the Paper (arXiv:2505.18125)](https://arxiv.org/abs/2505.18125)

* **Key Finding:** TabSTAR fuses numerical encoders with text embedding models end-to-end, achieving SOTA results on tabular data mixed with unstructured text.
* **Commentary:** There is a lot to like here. Historically, handling categorical variables involved simple one-hot encoding, often ignoring the rich semantics of text fields because integrating them was too labor-intensive. Tabular Foundation Models propose using Transformers to generalize feature associations across diverse datasets. A key innovation in TabSTAR is embedding not just features but also targets (dependent variables), using an interaction encoder that acts similarly to JEPA with an alignment loss. This takes advantage of the dependent variable during the training process, rather than just at the loss level. However, the evaluation is limited to 10,000 observations. This is a recurring theme with tabular deep learning: if they cannot scale to the massive datasets typical in banking, their industrial usefulness is severely limited.

{{< figure src="/uploads/posters/tabstar_poster.jpg" title="TabSTAR — Poster" >}}

### A Faster Training Algorithm for Regression Trees with Linear Leaves

[Read the Paper (OpenReview)](https://openreview.net/forum?id=urDdBuhbLx)

* **Key Finding:** An adaptive optimization strategy switches between matrix inversion methods based on sample-to-dimension ratios, speeding up training for oblique trees.
* **Commentary:** Optimizing oblique trees (which split on multiple variables to learn non-linear boundaries) usually requires the Tree Alternating Optimization (TAO) method, involving the inversion of correlation matrices. By utilizing the Sherman-Morrison formula, this work allows us to invert a potentially massive correlation matrix by handling a smaller one—a very useful mathematical identity. The method effectively manages the tradeoff between the number of samples at a node and the dimensionality of the dataset. Interestingly, complexity analysis reveals it is preferable to grow larger (deeper) trees for scalability using this method. The speedups are significant as dimensionality increases, and it accelerates classical CART training as well.

{{< figure src="/uploads/posters/fast_tree_poster.jpg" title="A Faster Training Algorithm for Regression Trees with Linear Leaves — Poster" >}}

### Hybrid Autoencoders for Tabular Data (TANDEM)

[Read the Paper (arXiv:2511.06961)](https://arxiv.org/abs/2511.06961)

* **Key Finding:** TANDEM combines a tree-based encoder (high-frequency patterns) with a neural network encoder (low-frequency patterns) to outperform SOTA in low-label settings.
* **Commentary:** Spectral analysis reveals that neural networks favor low-frequency components, while tree models are better at capturing high-frequency components (which are significantly more prevalent in tabular data than in vision data). This architecture is clever because it stacks a neural network encoder with a tree-based encoder (OSDT) to leverage the representation power of both. Since tabular data lacks the spatial structure of images, finding methods that respect its unique "frequency" characteristics is a smart direction, particularly for synthetic data generation.

{{< figure src="/uploads/posters/hybridautoencoder_poster.jpg" title="Hybrid Autoencoders for Tabular Data (TANDEM) — Poster" >}}

### TabArena: A Living Benchmark for Machine Learning on Tabular Data

[Read the Paper (OpenReview)](https://openreview.net/forum?id=urDdBuhbLx)

* **Key Finding:** An open-source benchmark with 51 curated datasets to accurately compare Deep Learning methods against tree-based models.
* **Commentary:** Similar to TabSTAR, the evaluation here is limited to small datasets with relatively low dimensionality. While it aims to be the "ImageNet of tables," it does not challenge the status of tree models as the standard for massive datasets. Until benchmarks incorporate the "huge" scale data found in banking and tech (millions of rows, thousands of columns), their utility for industrial decision-making remains limited.

{{< figure src="/uploads/posters/tabarena_poster.jpg" title="TabArena — Poster" >}}


### SpEx: A Spectral Approach to Explainable Clustering

[Read the Paper (arXiv:2511.00885)](https://arxiv.org/abs/2511.00885)

* **Key Finding:** SpEx approximates complex reference clustering using an interpretable decision tree by optimizing a multi-way cut objective.
* **Commentary:** It turns out that trees grown by decision tree models can be mapped to graphs—a simple realization with profound implications. The paper argues that classical CART trees are incentivized to split samples from separate classes but not penalized for splitting samples from the *same* class, which fragments clusters. SpEx generates a graph of the data, learns its clustering, and trains a decision tree using this graph. This effectively produces a tree model that respects the underlying cluster structure better than standard methods, validating the study of tree models as graphs.

{{< figure src="/uploads/posters/spex_poster.jpg" title="SpEx: A Spectral Approach to Explainable Clustering — Poster" >}}

### Autoencoding Random Forests (RFAE)

[Read the Paper (arXiv:2505.21441)](https://arxiv.org/abs/2505.21441)

* **Key Finding:** RFAE introduces a method to use Random Forests as autoencoders by utilizing the "Breiman kernel" for encoding and a constrained optimization process for decoding, effectively handling mixed-type tabular data.
* **Commentary:** This is arguably one of the most exciting developments of the conference. Since we can map decision trees to graphs, the entire graph analytics toolset becomes available to better understand the latent features induced by tree models. If you can extract a graph from an XGBoost model (technically multiple graphs), you can calculate adjacency matrices and use spectral tools (e.g., decompositions) or graph embedding techniques to understand the expressive power of decision trees. This approach allows us to investigate axis-aligned trees, oblique trees, and SGT trees through a graph-theoretical lens. For industrial practitioners relying on tree ensembles, this offers a major leap in interpretability and feature extraction.

{{< figure src="/uploads/posters/rfae_poster.jpg" title="Autoencoding Random Forests (RFAE) — Poster" >}}

---

## 2. Graph Intelligence at Scale
**Category Summary:**
Graph Neural Networks (GNNs) are moving beyond small academic datasets. The focus is now on processing billion-scale networks (like blockchains), solving topological bottlenecks like "oversquashing," and understanding the privacy implications of how these models memorize data.

### The Temporal Graph of Bitcoin Transactions

[Read the Paper (arXiv:2510.20028)](https://arxiv.org/abs/2510.20028)

* **Key Finding:** The authors created a comprehensive ETL pipeline modeling the Bitcoin blockchain as a temporal heterogeneous graph to analyze evolving trading behaviors.
* **Commentary:** The primary challenge with this kind of graph is scale. With 2 billion nodes, running graph analytics becomes a massive hurdle due to memory constraints and the computing costs of GNN algorithms. The authors suggest sampling methods adapted to downstream tasks, such as the forest fire model. Being able to run analytics on the Bitcoin transaction network is incredibly useful for detecting co-payment or co-trade patterns and identifying subgraphs linked to money laundering. Additionally, it should be possible to learn risk patterns: classical risk models often use engineered features that map customer-merchant interactions over time. We should be able to develop similar models on a sample of the Bitcoin network—provided the network is actually used as intended (a payment network backed by a stable currency).

{{< figure src="/uploads/posters/bitcoin_graph_poster.jpg" title="The Temporal Graph of Bitcoin Transactions — Poster" >}}

### Memorization in Graph Neural Networks

[Read the Paper (arXiv:2508.19352)](https://arxiv.org/abs/2508.19352)

* **Key Finding:** There is an inverse relationship between graph homophily and memorization; GNNs are forced to "memorize" heterophilic nodes (those connected to different labels) when the structure is uninformative.
* **Commentary:** It is crucial to distinguish between overfitting (model-specific) and memorization (data-specific). The authors explain that GNNs struggle to predict labels for heterophilic connectivity (nodes connected to unlike labels) unless they have seen them during training—essentially forcing the model to memorize. This has significant implications for data selection. We might need scenarios where we learn models with high accuracy but low memorization, perhaps followed by post-training treatments. Furthermore, the difficulty GNNs have with heterophilic nodes may be connected to "oversquashing," a topological bottleneck addressed by other research at this conference.

{{< figure src="/uploads/posters/memorization_gnn_poster.jpg" title="Memorization in Graph Neural Networks — Poster" >}}

### Taxonomy of Reduction Matrices for Graph Coarsening

[Read the Paper (arXiv:2506.11743)](https://arxiv.org/abs/2506.11743)

* **Key Finding:** The authors establish a taxonomy proving that structural information in graph coarsening is contained in the "lifting matrix," allowing for optimized reduction matrices.
* **Commentary:** Given that graphs like the Bitcoin network (2 billion nodes) are too large for most infrastructure, we need methods to aggregate nodes into "super nodes" without losing critical information. This process involves "lifting" (mapping many nodes to few) and "reduction" (mapping few back to many). One can deduce that information loss primarily occurs during reduction. Traditionally, the reduction matrix was just the inverse of the lifting matrix. This work proposes learning them separately, prioritizing the reduction matrix to minimize information loss. This flexibility could also help mitigate GNN issues like oversmoothing and oversquashing by imposing specific constraints on the learned matrices.

{{< figure src="/uploads/posters/coarsening_poster.jpg" title="Taxonomy of Reduction Matrices for Graph Coarsening — Poster" >}}

---

## 3. Agentic AI & Financial Simulation
**Category Summary:**
The intersection of Large Language Models (LLMs) and Agent-Based Modeling (ABM) is creating a new frontier in financial simulation. Instead of static rules, agents now have "beliefs" and "intentions." However, a recurring critique is whether these general-purpose simulators can accurately replicate the specific market-clearing mechanisms required for rigorous economic analysis.

### Learning Individual Behavior in Agent-Based Models with Graph Diffusion Networks

[Read the Paper (arXiv:2505.21426)](https://arxiv.org/abs/2505.21426)

* **Key Finding:** A framework that replaces hand-coded agent rules with Graph Diffusion Networks, allowing simulations to be differentiable and automatically calibrated.
* **Commentary:** This is a fascinating approach to scalability. Instead of running computationally expensive ABMs fully, the authors simulate a small number of steps and use the output to train a diffusion model. This diffusion model, which is cheaper to run, learns the internal dynamics and generates plausible next steps. Crucially, they associate a GNN with the diffusion model to incorporate agent connectivity. This allows the model to capture complex patterns (tested here on Predator-Prey models). It raises an interesting possibility: perhaps we only need to calibrate ABMs for short time series and let diffusion models take over to produce high-fidelity synthetic data.

{{< figure src="/uploads/posters/abmdiffusion_poster.jpg" title="Learning Individual Behavior in Agent-Based Models with Graph Diffusion Networks — Poster" >}}

### TwinMarket: Scalable Behavioral and Social Simulation for Financial Markets

[Read the Paper (TwinMarket - related methodology)](https://arxiv.org/abs/2505.15155)

* **Key Finding:** TwinMarket uses LLM-driven agents with cognitive processes (Belief, Desire, Intention) interacting in a dynamic social network to reproduce market phenomena.
* **Commentary:** Unlike classical Ising trading models that use a lattice structure, this model constructs a similarity graph based on trading allocations over time. Agents are fed social media posts from their most similar neighbors, making the interaction process implicit. We hope LLM agents can extract richer information than random agents, but the evaluation here is unclear. There is a comparison with empirical data for a 1,000-agent simulation, but it is ambiguous what is actually being evaluated. My criticism remains that the market clearing mechanism—the core of how agents interact and prices form—seems neglected in favor of focusing on the novelty of LLM agents.

{{< figure src="/uploads/posters/twinmarket_poster.jpg" title="TwinMarket: Scalable Behavioral and Social Simulation for Financial Markets — Poster" >}}

### EconGym: A Scalable AI Testbed with Diverse Economic Tasks

[Read the Paper (OpenReview)](https://openreview.net/forum?id=qPrBgHgsmP)

* **Key Finding:** A unified simulation platform benchmarking AI agents across diverse economic scenarios and heterogeneous roles.
* **Commentary:** The authors attempt to create a general-purpose economic simulator, but this generality is its weakness. Economic modeling requires carefully designed market clearing mechanisms specific to the problem being solved. Markets are where agents interact, and that interaction process must be rigorous. It is unlikely one can make this "general" without taking shortcuts on the interaction mechanisms. While the goal of a standardized testbed is noble, the complexity of correctly modeling diverse economic interactions likely makes a "one-size-fits-all" software solution infeasible.

{{< figure src="/uploads/posters/econgym_poster.jpg" title="EconGym: A Scalable AI Testbed with Diverse Economic Tasks — Poster" >}}

### R&D-Agent-Quant: A Multi-Agent Framework for Data-Centric Factors and Model

[Read the Paper (arXiv:2505.15155)](https://arxiv.org/abs/2505.15155)

* **Key Finding:** An end-to-end framework using LLM agents to automate quantitative finance R&D, from factor discovery to model design.
* **Commentary:** The authors present a table of results where their factor-based models outperform classical auto-regressive time-series models. However, it is unclear exactly what ideas they are testing. Are these factors derived from legitimate financial logic, or is the system training "out of thin air"? While using agents to build factors is a compelling concept, one must scrutinize the paper to ensure the improvements are robust and not artifacts of overfitting or look-ahead bias, common pitfalls in automated trading research.

{{< figure src="/uploads/posters/rdagentquant_poster.jpg" title="R&D-Agent-Quant: A Multi-Agent Framework for Data-Centric Factors and Model — Poster" >}}

---

## 4. Scientific ML & Theoretical Foundations
**Category Summary:**
This section covers the mathematical backbone of the conference: new correlation coefficients, physics-informed operators, and regularization techniques. These contributions are less about immediate application and more about fixing the fundamental flaws in how models measure dependence or solve differential equations.

### SHGR: A Generalized Maximal Correlation Coefficient

[Read the Paper (OpenReview)](https://openreview.net/forum?id=c1snZ9dvCR)

* **Key Finding:** SHGR is a differentiable estimator for maximal correlation, robust to noise and capable of capturing non-linear dependencies.
* **Commentary:** New correlation coefficients appear every few years, but often their benefits are unclear. However, in industrial contexts (e.g., using Mutual Information for feature grouping), there is a clear need for non-linear dependency measures. What distinguishes SHGR is the rigorous protocol established to test it: capturing non-linear patterns, computing cost, and robustness to noise/extreme values. It appears to outperform most existing metrics on these criteria. This is exciting, but like all theoretical metrics, it needs to be implemented and evaluated in a production environment to prove it offers a tangible performance improvement.

{{< figure src="/uploads/posters/shgr_poster.jpg" title="SHGR: A Generalized Maximal Correlation Coefficient — Poster" >}}

### STNet: Spectral Transformation Network for Solving Operator Eigenvalue Problem

[Read the Paper (arXiv:2510.23986)](https://arxiv.org/abs/2510.23986)

* **Key Finding:** STNet accelerates high-dimensional eigenvalue problems using deep learning to dynamically reshape the operator's spectrum.
* **Commentary:** This is an eigenfunction solver that attempts to gain stability and efficiency via neural networks. However, the critical question is scalability. The poster does not explicitly discuss whether the problems tackled are large-scale, which is the primary bottleneck for these solvers. While the method tackles the "curse of dimensionality," verification via the full paper is needed to see if it holds up against industrial-scale physics problems.

{{< figure src="/uploads/posters/stnet_poster.jpg" title="STNet: Spectral Transformation Network for Solving Operator Eigenvalue Problem — Poster" >}}

---

## Additional Research
*The following posters were also reviewed by me during the conference but did not receive specific commentary during the review session. They are categorized here for completeness.*

**Return of ChebNet (Stable-ChebNet)**
[Read the Paper](https://openreview.net/forum?id=oLyfML1Qze)
* *Category:* Graph Neural Networks
* *Key Finding:* Revives ChebNet by modifying it into a stable dynamical system, allowing it to scale to large receptive fields without oversmoothing.

**Best of Both Worlds: Bridging Laplace and Fourier**
[Read the Paper](https://openreview.net/forum?id=wq5kBGP1wx)
* *Category:* Scientific Machine Learning
* *Key Finding:* Separates differential equations into transient (Laplace) and steady-state (Fourier) parts for better accuracy in operator learning.

**Improving Decision Trees through the Lens of Parameterized Local Search**
[Read the Paper](https://openreview.net/forum?id=FB5nWEQV7K)
* *Category:* Theoretical Computer Science
* *Key Finding:* Identifies specific conditions (fixed-parameter tractability) where local search operations for decision trees can be efficiently optimized.

**Differentiable Decision Tree via "ReLU+Argmin" Reformulation**
[Read the Paper](https://openreview.net/forum?id=F11iEhKoYp)
* *Category:* Differentiable Programming
* *Key Finding:* Reformulates decision tree splitting as a differentiable "ReLU + Argmin" task, enabling gradient-based training.

**Regression Trees Know Calculus**
[Read the Paper](https://openreview.net/forum?id=wFXvD83mhb)
* *Category:* Interpretable ML
* *Key Finding:* Demonstrates that regression trees can estimate gradients via finite differences, enabling gradient-based interpretation methods.

**What We Don’t C: Representations beyond VAEs**
[Read the Paper (arXiv:2511.09433)](https://openreview.net/forum?id=F2USOqa0La)
* *Category:* Representation Learning / Astrophysics
* *Key Finding:* Uses "Latent Guided Flow Matching" to disentangle and discover hidden factors (like anomalies) that were not present in the original conditioning labels.