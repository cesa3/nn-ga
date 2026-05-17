---
last_mapped_commit: 9edce5ffd6f0fe616ea1e9a5f662b991821e22a0
mapped_at: 2026-05-17
---

# Conventions

## Code Style

- **Language**: Python with Thai comments (mixed language codebase)
- **Formatting**: No linter or formatter configured (no black, ruff, flake8)
- **Indentation**: 4 spaces (standard Python)
- **Line length**: No enforcement; some lines exceed 100 chars
- **Docstrings**: Used sparingly — only `lame_v1()` has a multi-line docstring explaining the algorithm
- **Comments**: Thai language comments dominate, explaining algorithmic concepts

## Naming Patterns

- **Functions**: `snake_case` — e.g. `compute_loss`, `nn_forward`, `gradient_informed_ga`
- **Variables**: `snake_case` — e.g. `best_w`, `pop_size`, `hist_train`
- **Constants**: `UPPER_SNAKE_CASE` — e.g. `SEED`, `N_RUNS`, `N_WEIGHTS`, `LAYER_BOUNDARIES`
- **Abbreviations**: Common ML abbreviations (`w` = weights, `acc` = accuracy, `te` = test, `tr` = train, `fn` = function)

## Function Patterns

- **No classes** — everything is pure functions and global state
- **Closures**: `tournament()` is defined as a closure inside GA loops, capturing `pop`, `fitness`, `pop_size`
- **Nested functions**: `multi_run(fn, name, n_runs)` takes a function as parameter
- **State mutation**: Population arrays (`pop`, `ages`) are mutated in-place per generation
- **Return tuples**: Functions return multiple values as tuples (unpacked at call site)

## Error Handling

- **Minimal**: No try/except blocks anywhere in the codebase
- **Clipping instead of handling**: `np.clip(pred, 1e-9, 1 - 1e-9)` prevents log(0) in loss computation
- **`np.clip` for sigmoid**: `np.clip(x, -500, 500)` prevents overflow in `np.exp`

## Numerical Patterns

- **Weight initialization**: `np.random.randn(pop_size, N_WEIGHTS) * 0.1` (small Gaussian)
- **Loss function**: Binary cross-entropy with epsilon clipping
- **Decay schedules**: Exponential decay using `(target/start) ** (gen/generations)` pattern
- **Random state**: `np.random.seed(SEED)` at function start for reproducibility

## Experiment Conventions

- **Benchmarking**: `N_RUNS=5` with different seeds, reporting mean±std
- **Logging**: Print statements every 50 generations (`if (gen+1) % 50 == 0`)
- **Plotting**: `matplotlib` with explicit color choices and 2×2 subplot grids

## Bilingual Code

- Thai comments are used for algorithmic explanations (especially in LAME v1)
- English for function names, variable names, and print output
- Section headers in comments use `# ====...====` divider style
