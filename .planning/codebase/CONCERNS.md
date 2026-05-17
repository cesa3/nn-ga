---
last_mapped_commit: 9edce5ffd6f0fe616ea1e9a5f662b991821e22a0
mapped_at: 2026-05-17
---

# Concerns

## Bugs

### CRITICAL: Broken cell — `compute_acc` not defined

Cell 8 (early LAME v1 prototype) crashes with `NameError: name 'compute_acc' is not defined` at line 44. This cell uses `compute_acc()` which is only defined later in cell 9. The function in cell 3 is named `compute_accuracy()` (different name). This cell is dead code but indicates the notebook was developed incrementally without cell re-execution.

### HIGH: `savefig` path doesn't exist

`plt.savefig('/mnt/user-data/outputs/LAME_results.png')` and `LAME_boundaries.png` fail with `FileNotFoundError`. This path is Colab-specific and the directory doesn't exist. The plots still display inline but can't be saved.

## Technical Debt

### Monolithic notebook

All code lives in a single notebook with no modularity. The same functions are defined twice (cells 3 and 9) with different implementations and names:
- Cell 3: `compute_accuracy()` (2→4→1 network)
- Cell 9: `compute_acc()` (2→16→16→1 network)

### Duplicated implementations

- `sigmoid()` defined twice (cells 3 and 9)
- `nn_forward()` defined twice with different architectures
- `compute_loss()` defined twice
- `genetic_algorithm()` (cell 5) vs `standard_ga()` (cell 9) — same algorithm, different implementations

### Global state leakage

- `X_test`, `y_test` are global variables used inside `lame_v1()` and `standard_ga()` without being passed as parameters
- `N_WEIGHTS` is a global constant used in function bodies without being a parameter
- `LAYER_BOUNDARIES` is global, used inside `lame_v1()` without passing

### Performance issues

- `compute_loss()` is called per-individual in a Python list comprehension: `[compute_loss(ind, X, y) for ind in pop]` — O(pop_size) serial evaluations, no vectorization
- `compute_gradient()` uses a Python loop over all 337 weights with finite differences — extremely slow (2×337 forward passes per gradient computation)
- `multi_run()` re-runs `standard_ga()` and `gradient_informed_ga()` twice per seed (once for accuracy, once for time) — wasteful

### No hyperparameter tuning

All hyperparameters are hardcoded with no systematic search or validation.

## Security

No security concerns — the project has no user input, no network requests, no secrets, and no authentication.

## Fragile Areas

### Thai font rendering in matplotlib

Multiple `UserWarning: Glyph missing from font(s) DejaVu Sans` warnings for Thai characters in plot labels. The Thai text in plot titles won't render correctly in Colab.

### Reproducibility

- `np.random.seed()` is called inside GA functions, which can affect global random state
- Tournament selection uses `np.random.choice()` without isolated RNG
- Multi-run benchmarks may not be fully independent due to global state

### Numerical stability

- `np.clip(x, -500, 500)` in sigmoid is a band-aid; potential for gradient issues with large weights
- No gradient clipping in the GA update step (could produce NaN weights)

## Suggested Improvements

1. Extract common code into a Python module (`.py` file) imported by the notebook
2. Fix or remove the broken cell 8
3. Create output directory before `savefig` or use `plt.savefig()` without path
4. Vectorize fitness evaluation across the population
5. Pass all dependencies as function parameters (no global state)
6. Add Thai font support for matplotlib or use English labels
