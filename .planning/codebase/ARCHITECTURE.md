---
last_mapped_commit: 9edce5ffd6f0fe616ea1e9a5f662b991821e22a0
mapped_at: 2026-05-17
---

# Architecture

## Pattern

**Single-file experimental notebook** — all code lives in `NN_+GA.ipynb`. No modular separation between components.

## System Overview

The project implements and compares 4 methods for training a neural network on a binary classification task:

```
make_moons dataset
       │
       ├──► Backprop (MLPClassifier) ─── baseline
       ├──► Standard GA ──────────────── evolutionary baseline
       ├──► GI-GA (Gradient-Informed) ── hybrid GA + numerical gradient
       └──► LAME v1 ──────────────────── Lifespan-Aware Memory Evolution (novel)
```

## Neural Network Architecture

Two architectures are implemented:

### Simple (early cells): 2 → 4 → 1
- 17 total weights
- Sigmoid activation (both layers)
- Single hidden layer

### Full (main experiment): 2 → 16 → 16 → 1
- 337 total weights
- ReLU activation (hidden layers), Sigmoid (output)
- Two hidden layers
- Weight layout: `W1(2×16) + b1(16) + W2(16×16) + b2(16) + W3(16×1) + b3(1)`

## Data Flow

```
make_moons(500, noise=0.2)
    │
    ▼
train_test_split (80/20)
    │
    ▼
StandardScaler.fit_transform(train)
StandardScaler.transform(test)
    │
    ▼
nn_forward(X, weights) ──► predictions ──► loss / accuracy
```

## Algorithm Architectures

### Standard GA
```
Init random population
  └──► Per generation:
       ├── Evaluate fitness (negative loss)
       ├── Tournament selection (k=5)
       ├── Single-point crossover
       ├── Gaussian mutation (decaying scale)
       └── Elitism (keep best)
```

### GI-GA (Gradient-Informed GA)
```
Standard GA
  + Numerical gradient (finite difference) every 10 generations
  + Top 25% of children get gradient-informed update:
    child += -α·∇loss + β·noise
```

### LAME v1 (Lifespan-Aware Memory Evolution)
```
Standard GA base
  + Per-weight age tracking
  + Population-guided repair (instead of random reset):
    direction = top_mean - current
    old_weights += α·direction + β·noise
  + Layer-wise crossover (respects layer boundaries)
  + Alpha/Beta decay schedule
```

## Entry Points

1. Early cells: Simple 2→4→1 demo (GA vs Backprop)
2. Main cell: Full 2→16→16→1 experiment with 4-way comparison + plots

## Abstractions

- `unpack(w)` — Reshapes flat weight vector into layer matrices
- `nn_forward(X, w)` — Forward pass through the network
- `compute_loss(w, X, y)` — Binary cross-entropy loss
- `compute_acc(w, X, y)` — Classification accuracy
- `compute_gradient(w, X, y)` — Numerical gradient via finite differences
- `plot_boundary(ax, X, y, predict_fn, title)` — Decision boundary visualization
