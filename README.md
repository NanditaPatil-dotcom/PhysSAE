# PhysSAE
### Sparse Dictionary Learning of Physics-Aligned Features in Physics-Informed Neural Networks

> Understanding **what Physics-Informed Neural Networks actually learn** through mechanistic interpretability.

---

## Overview

Physics-Informed Neural Networks (PINNs) have become an important tool for solving differential equations by embedding physical laws directly into neural network training. Although they achieve impressive numerical accuracy, their internal representations remain largely unexplored.

**PhysSAE** introduces a mechanistic interpretability framework for PINNs using **Sparse Autoencoders (SAEs)**.

Instead of treating PINNs as black boxes, PhysSAE extracts hidden-layer activations, learns an overcomplete sparse dictionary of latent features, and identifies whether individual neurons correspond to meaningful physical concepts such as:

- Solution value
- Spatial gradients
- Temporal evolution
- Shock locations
- Interfaces
- Reaction terms

The framework further validates these learned concepts through **causal interventions**, demonstrating that individual sparse features have localized physical effects on the predicted PDE solution.

---

## Motivation

Traditional PINN evaluation focuses on:

- Prediction error
- PDE residual loss
- Boundary condition satisfaction

These metrics answer:

> **"Did the network solve the PDE?"**

PhysSAE instead asks:

> **"What physical concepts has the network actually learned?"**

Understanding the latent representation enables:

- Mechanistic interpretability
- Failure diagnosis
- Representation analysis
- Scientific insight beyond prediction accuracy

---

# Pipeline

```
PDE
 │
 ▼
Train PINN
 │
 ▼
Extract Penultimate Layer Activations
 │
 ▼
Normalize Activations
 │
 ▼
Train Sparse Autoencoder
 │
 ▼
Learn Sparse Dictionary Atoms
 │
 ├──────────────┐
 ▼              ▼
Concept Alignment
            Atom Ablation
 │              │
 └──────┬───────┘
        ▼
Interpret Physical Features
```

---

# Methodology

The complete workflow consists of six stages.

## 1. Train Physics-Informed Neural Network

A standard PINN is trained using:

- Initial condition loss
- Boundary condition loss
- PDE residual loss

The architecture uses a fully connected MLP with tanh activations.

---

## 2. Extract Latent Representations

After training, activations from the final hidden layer are collected over a dense spatio-temporal grid.

These activations represent the learned internal state of the PINN.

---

## 3. Sparse Dictionary Learning

A Sparse Autoencoder is trained on the extracted activations.

Objective:

- reconstruct latent vectors
- enforce sparsity through L1 regularization
- learn an overcomplete dictionary

The resulting sparse atoms ideally correspond to independent physical concepts.

---

## 4. Concept Alignment

Each learned atom is compared against physically meaningful fields.

Examples include

### Burgers Equation

- solution value
- gradient magnitude
- temporal derivative
- shock location
- spatial coordinate
- time

### Allen-Cahn Equation

- solution value
- interface indicator
- cubic reaction term
- gradient magnitude
- temporal derivative

Pearson correlation is used to determine which sparse atom best represents each concept.

---

## 5. Causal Validation

Correlation alone does not prove interpretability.

Each sparse atom is therefore individually removed from the latent representation.

The modified latent vector is passed through the decoder and frozen PINN readout.

The resulting prediction difference

```
Δu = u_original − u_modified
```

reveals the causal contribution of that sparse feature.

---

## 6. Compare Against PCA

Principal Component Analysis is trained on the same activations.

PhysSAE is compared with PCA to evaluate whether sparse dictionary learning produces more disentangled representations.

---


# Future Work

Planned extensions include:

- Pareto analysis between sparsity and reconstruction
- Monosemanticity metrics
- Two-sided concept alignment
- Additional PDE benchmarks
- ICA and k-means baselines
- Symbolic probing using SINDy and PySR
- Joint multi-PDE sparse dictionaries
- Co-trained PhysSAE-PINN architecture
- Comparison with Fourier Neural Operators
- Circuit-level analysis of PINNs

---

# Acknowledgements

This work explores the intersection of

- Physics-Informed Neural Networks
- Scientific Machine Learning
- Mechanistic Interpretability
- Sparse Dictionary Learning

and aims to provide interpretable representations for scientific neural networks.

---

## License

MIT License
