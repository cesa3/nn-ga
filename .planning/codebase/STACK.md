---
last_mapped_commit: 9edce5ffd6f0fe616ea1e9a5f662b991821e22a0
mapped_at: 2026-05-17
---

# Stack

## Language

- **Python 3.12** (Colab runtime)
- No `requirements.txt`, `pyproject.toml`, or `setup.py` — all dependencies are pre-installed in Colab

## Runtime

- **Google Colab** (primary execution environment)
- Jupyter Notebook kernel (`python3`)
- Notebook format: nbformat 4, nbformat_minor 0

## Core Dependencies

| Library | Version (Colab default) | Purpose |
|---------|------------------------|---------|
| `numpy` | latest | Array operations, matrix math, random number generation |
| `matplotlib` | latest | Plotting: decision boundaries, loss curves, bar charts |
| `scikit-learn` | latest | `make_moons` dataset, `train_test_split`, `StandardScaler`, `MLPClassifier` |
| `time` | stdlib | Runtime measurement for benchmark comparisons |

## No Build System

- No package manager configuration files
- No virtual environment setup
- All imports are standard Colab packages
- No transitive dependency management

## Configuration

- All hyperparameters are inline constants within notebook cells:
  - `SEED = 42`
  - `N_RUNS = 5`
  - `N_WEIGHTS = 337` (for 2→16→16→1 architecture)
  - `pop_size=80`, `generations=200`
  - `age_threshold=20`, `alpha=0.6`, `beta=0.2` (LAME-specific)

## Version Control

- Git repository on GitHub (`cesa3/nn-ga`)
- Single branch: `main`
