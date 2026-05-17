---
last_mapped_commit: 9edce5ffd6f0fe616ea1e9a5f662b991821e22a0
mapped_at: 2026-05-17
---

# Integrations

## External APIs

None. The project is fully self-contained with no external API calls.

## Databases

None. All data is generated in-memory via `sklearn.datasets.make_moons`.

## Auth Providers

None.

## Webhooks / Event Systems

None.

## File System

- **Output path**: `/mnt/user-data/outputs/` (Colab-specific, currently broken — `FileNotFoundError`)
  - `LAME_results.png` — comparison plots
  - `LAME_boundaries.png` — decision boundary visualizations

## Colab Integration

- Notebook includes a "Open in Colab" badge linking to `github.com/cesa3/nn-ga/blob/main/NN_%2BGA.ipynb`
- Colab metadata in notebook cells (`colab.base_uri`, `outputId`)

## Third-Party Libraries (Detailed)

### scikit-learn (`sklearn`)

- **`make_moons`**: Generates nonlinear 2-class moon-shaped dataset (500 samples, noise=0.2)
- **`train_test_split`**: 80/20 train-test split
- **`StandardScaler`**: Feature normalization (zero mean, unit variance)
- **`MLPClassifier`**: Backpropagation baseline with Adam optimizer

### matplotlib

- Used for 4 types of plots:
  1. Dataset scatter plot
  2. Decision boundary contour plots
  3. Loss convergence curves
  4. Bar charts (mean±std accuracy comparison)

## No CI/CD

No continuous integration, deployment, or automated testing pipelines.
