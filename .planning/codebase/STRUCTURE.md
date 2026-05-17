---
last_mapped_commit: 9edce5ffd6f0fe616ea1e9a5f662b991821e22a0
mapped_at: 2026-05-17
---

# Structure

## Directory Layout

```
nn-ga/
├── NN_+GA.ipynb    # Single Jupyter notebook — ALL source code
└── README.md        # Project title only: "# nn-ga"
```

That's it. No `src/`, `tests/`, `config/`, or `scripts/` directories.

## Notebook Cell Organization (`NN_+GA.ipynb`)

The notebook has 9 code cells + 1 markdown cell, organized into two logical sections:

### Section 1: Simple Demo (2→4→1 network)

| Cell | Purpose |
|------|---------|
| 1 | Imports: numpy, matplotlib, sklearn |
| 2 | Dataset generation: make_moons, split, scale, plot |
| 3 | NN definition: sigmoid, nn_forward, compute_accuracy, compute_loss, N_WEIGHTS=17 |
| 4 | Backprop baseline: MLPClassifier (4 hidden units, logistic activation) |
| 5 | Standard GA: tournament selection, single-point crossover, mutation |
| 6 | Results comparison: Backprop vs GA |
| 7 | Decision boundary plots |

### Section 2: Full Experiment (2→16→16→1 network)

| Cell | Purpose |
|------|---------|
| 8 | Early LAME v1 prototype (BROKEN — uses `compute_acc` before definition) |
| 9 | Complete experiment: dataset, NN, backprop, standard GA, GI-GA, LAME v1, multi-run benchmark, all plots |

### Empty Cell

| Cell | Purpose |
|------|---------|
| 10 | Empty placeholder |

## Key Locations

| What | Where |
|------|-------|
| Dataset generation | `NN_+GA.ipynb` cell 2 (simple) / cell 9 (full) |
| Neural network forward pass | `NN_+GA.ipynb` cell 3 / cell 9 |
| Standard GA | `NN_+GA.ipynb` cell 5 / cell 9 |
| GI-GA implementation | `NN_+GA.ipynb` cell 9 |
| LAME v1 implementation | `NN_+GA.ipynb` cell 9 |
| Results & plots | `NN_+GA.ipynb` cell 9 |
| Broken LAME prototype | `NN_+GA.ipynb` cell 8 |

## Naming Conventions

- Functions: `snake_case` (`compute_loss`, `nn_forward`, `lame_v1`)
- Variables: `snake_case` (`best_w`, `pop_size`, `hist_train`)
- Constants: `UPPER_SNAKE_CASE` (`SEED`, `N_RUNS`, `N_WEIGHTS`, `LAYER_BOUNDARIES`)
- Abbreviations: `w` for weights, `acc` for accuracy, `te` for test, `tr` for train
- Result variables: `{method}_{metric}` pattern (`ga_tr`, `lame_te`, `bp_loss`)

## No Module Boundaries

All code is in global notebook scope. No imports between cells for project code. No class definitions. Everything is standalone functions and global variables.
