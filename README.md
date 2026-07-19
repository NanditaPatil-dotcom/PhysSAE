# PhysSAE
### Physics-Aligned Sparse Autoencoders for Mechanistic Interpretability of Physics-Informed Neural Networks

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)]()
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)]()
[![License](https://img.shields.io/badge/License-MIT-green.svg)]()

**Interpreting the internal representations of Physics-Informed Neural Networks through Sparse Dictionary Learning**

</div>

---

## Overview

Physics-Informed Neural Networks (PINNs) have become one of the most influential methods in Scientific Machine Learning for solving ordinary and partial differential equations by embedding physical laws directly into the optimization process.

While PINNs achieve impressive predictive accuracy, **very little is understood about what they actually learn internally.**

**PhysSAE** introduces a mechanistic interpretability framework that discovers physically meaningful concepts directly from the latent space of trained PINNs using **Sparse Autoencoders (SAEs).**

Instead of treating PINNs as black boxes, PhysSAE answers questions such as:

- Which neuron represents shock formation?
- Does the network explicitly encode interfaces?
- Are temporal and spatial concepts disentangled?
- Which latent features are causally responsible for the final prediction?

---

# Motivation

Current PINN evaluation focuses almost exclusively on:

- PDE residual loss
- Boundary condition error
- Initial condition error
- Relative L2 solution accuracy

These metrics answer

> **Did the network solve the PDE?**

PhysSAE instead asks

> **What physical concepts has the network learned internally?**

Understanding internal representations enables

- Mechanistic Interpretability
- Failure diagnosis
- Representation analysis
- Scientific explainability
- Discovery of latent physical concepts

---

# Key Contributions

- Sparse Autoencoder framework for PINN latent representations
- Physics-aware concept alignment protocol
- Causal validation through latent feature ablation
- Comparison against PCA, ICA and clustering baselines
- Framework applicable to multiple PDE families
- Diagnostic tool for successful and failed PINNs

---

# Method Overview

```
          PDE
           │
           ▼
   Train Physics-Informed
      Neural Network
           │
           ▼
Extract Penultimate Activations
           │
           ▼
 Normalize Latent Features
           │
           ▼
 Train Sparse Autoencoder
           │
           ▼
Learn Sparse Dictionary Atoms
      ┌───────────────┐
      ▼               ▼
Concept Alignment  Atom Ablation
      │               │
      └──────┬────────┘
             ▼
 Mechanistic Interpretation
```

---

# Pipeline

## 1. Train PINN

A standard Physics-Informed Neural Network is trained using

- Initial condition loss
- Boundary condition loss
- PDE residual loss

The implementation uses fully-connected tanh networks.

---

## 2. Latent Activation Extraction

After convergence, activations from the final hidden layer are extracted over a dense spatio-temporal grid.

These activations form the representation space analyzed by PhysSAE.

---

## 3. Sparse Dictionary Learning

A Sparse Autoencoder is trained on the latent representations.

Objective:

- reconstruct latent vectors
- encourage sparse activation
- learn an overcomplete dictionary

The learned atoms ideally correspond to individual physical concepts.

---

## 4. Physics Concept Alignment

Each sparse atom is compared against handcrafted physical fields.

Examples include

### Burgers Equation

- solution value
- signed solution
- gradient magnitude
- curvature
- shock location
- spatial position
- time

### Allen-Cahn Equation

- interface indicator
- reaction term
- solution magnitude
- temporal derivative
- spatial gradient

### Heat Equation

- diffusion profile
- temporal evolution

### Schrödinger Equation

- amplitude
- phase
- real component
- imaginary component

### Convection Equation

- propagation front
- transport direction

Pearson correlation identifies which sparse atom best represents each physical concept.

---

## 5. Causal Validation

Correlation alone does not establish interpretability.

Each discovered atom is therefore individually ablated.

The modified latent representation is decoded and passed through the frozen PINN readout layer.

The prediction difference

```
Δu = uoriginal − umodified
```

reveals the causal influence of each learned feature.

Localized perturbations indicate that the discovered concept is genuinely used by the network rather than merely correlated.

---

## 6. Baseline Comparison

PhysSAE is evaluated against classical representation learning techniques:

- PCA
- ICA
- K-Means clustering

The comparison investigates

- concept disentanglement
- reconstruction quality
- monosemanticity
- causal localization

---

# Supported PDEs

Current experiments include

| PDE | Purpose |
|------|----------|
| Burgers | Shock formation |
| Allen-Cahn | Interface dynamics |
| Heat Equation | Diffusion |
| Schrödinger | Wave dynamics |
| Convection | PINN failure analysis |

---

# Experiments

The project investigates several questions in mechanistic interpretability for Scientific Machine Learning.

### Representation Learning

- Sparse dictionary learning
- Physics concept discovery
- PCA comparison
- ICA comparison
- K-Means baseline

### Interpretability

- Concept alignment
- Monosemanticity
- Spatial firing visualization
- Atom specialization
- Two-sided concept alignment

### Causal Analysis

- Single atom ablation
- Pairwise ablation
- Localized intervention
- Reconstruction analysis

### Robustness

- Successful PINNs
- Failed PINNs
- Cross-PDE evaluation
---

# Requirements

- Python 3.10+
- PyTorch
- NumPy
- SciPy
- scikit-learn
- Matplotlib

---

# Research Vision

PhysSAE lies at the intersection of

- Scientific Machine Learning
- Mechanistic Interpretability
- Sparse Representation Learning
- Physics-Informed Neural Networks
- Explainable AI

The long-term goal is to move beyond evaluating **whether** scientific neural networks work, toward understanding **how** they internally represent physical phenomena.

---

# License

Released under the MIT License.
