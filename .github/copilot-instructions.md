# AI coding agent instructions for this repo

This repository is a set of teaching notebooks for Economic Dynamics and Complexity (discrete/continuous dynamics, plotting, interactives). The primary task is to complete placeholders in notebooks and produce interactive, reproducible visualizations. There is no traditional build/test pipeline; the main workflow is Jupyter in VS Code.

## Repo layout and entry points
- Root notebooks (typical workflow):
  - `SS_5.ipynb` — Logistic map and chaos (logistic_map, cobweb plots, bifurcation diagram)
  - `SS2_logistic_growth.ipynb` / `SS2_logistic_growth_solution.ipynb`
  - `SS3_Diffusion.ipynb`
  - `SS4_coupledDynamics.ipynb` / `SS4_coupledDynamics_solution.ipynb`
  - `sympy_basics.ipynb`
  - `Jupyter Setup and Python Intro-20250919/` — getting started material

## Notebook editing rules (critical)
- Preserve notebook JSON: keep existing cells, metadata, and cell IDs intact; append new cells instead of replacing unless explicitly required.
- Keep answers in the provided markdown “Answer” areas. Implement code only where “YOUR CODE HERE” appears or in new cells clearly labeled.
- Use lightweight dependencies only: `numpy`, `matplotlib`, `ipywidgets` (already used). Avoid adding new libraries unless necessary.
- Prefer interactive sliders with `ipywidgets.interact` as shown in notebooks.
- Avoid generating huge outputs or long prints; prefer concise plots and small iteration counts for demos.

## Conventions and patterns in this codebase
- Logistic map assumes carrying capacity K=1.
  - Function signature used across notebooks: `logistic_map(x, r)` should support both scalar and numpy array `x`.
  - Discrete dynamics helper: `logistic_dynamics(P0, r, n)` returns the first `n` iterations as a sequence (include `P0`), computed via a simple Python loop (no `odeint`).
  - For bifurcation: `logistic_dynamics(P0, r, n, m)` should return the last `m` values (discard transients); used in loops over `r`.
- Cobweb plots are built with a generic utility:
  - `plot_cobweb(f, x0, n, **f_kwargs)` expects `f(x, **f_kwargs)` and draws vertical then horizontal moves each iteration.
  - Identity line: dashed gray; function curve: blue, thin linewidth; annotations optional via `annotate_point` provided in the notebook.
- Plotting style: optional `plt.style.use('seaborn-v0_8-whitegrid')` is commented; keep as-is unless instructed.

## Examples from the notebooks
- Logistic map definition:
  - `def logistic_map(x, r): return r * x * (1 - x)` (should broadcast over numpy arrays)
- Discrete iterations (shape):
  - Initialize `vals = np.empty(n+1); vals[0] = P0;` then loop `for i in range(n): vals[i+1] = logistic_map(vals[i], r)`.
- Bifurcation data collection:
  - For each `r` in `np.linspace(2.5, 4.0, 4000)`, call the `logistic_dynamics(..., n, m)` that returns last `m`; then `extend` `r_points` with `[r]*m` and `P_points` with the returned values.

## Developer workflow (VS Code + Jupyter)
- Open a notebook, select a Python kernel, ensure `ipywidgets` is enabled so `@interact` renders sliders.
- Dependencies commonly used here: `numpy`, `matplotlib`, `ipywidgets`, `sympy` (in the intro notebook). Use these; avoid adding others.
- Keep computations bounded (e.g., `n <= 300`, `m <= 100`, `r in [0.5, 4.1]`) to maintain responsiveness.

## What to avoid
- Don’t refactor across notebooks into modules unless asked; notebooks are self-contained teaching artifacts.
- Don’t alter problem statements or remove placeholders. Don’t change variable names/signatures specified by the tasks.
- Don’t introduce randomness without a fixed seed.

## PR/commit etiquette for notebooks
- Minimize unrelated cell execution count churn. Keep diffs focused on changed cells.
- Prefer adding short helper cells over large rewrites. Preserve outputs that illustrate results; avoid embedding very large binary outputs.

## Pointers to key files
- See `SS_5.ipynb` for the logistic map tasks: function signatures, cobweb utility, and bifurcation scaffold are already defined and should be completed rather than replaced.
- Setup material is in `Jupyter Setup and Python Intro-20250919/` if environment hints are needed.

Questions for maintainers
- Confirm preferred Python version and exact package versions (consider adding a lightweight `requirements.txt`).
- Should outputs be kept in git for solutions, or cleared before committing? If there’s a policy, we can codify it here.
