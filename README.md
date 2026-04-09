# MATH70113 — Simulation Methods for Finance

Group coursework — Imperial College London.

**Group members**
- Paul Archer (06054057)
- Hamza El Arji (06020926)
- Chedi Mnif (06064588)
- Diane Murzi (06048769)

This repository contains the two Jupyter notebooks that implement the numerical experiments for the group assignment, together with the source paper for Part 2 and the original assignment brief.

## Repository contents

| File | Description |
|---|---|
| `MATH70113_part_1.ipynb` | Part 1 — Pathwise Delta and Vega for a down-and-out barrier call with Brownian-bridge correction. |
| `MATH70113_part_2.ipynb` | Part 2 — Numerical study of Glasserman & Yu (2004), *Number of paths versus number of basis functions in American option pricing*. |

## Part 1 — Barrier option Greeks (`MATH70113_part_1.ipynb`)

Estimates the price, Delta and Vega of a down-and-out barrier call under Geometric Brownian Motion using the **pathwise sensitivity method**, combined with the **Brownian-bridge barrier-crossing correction** from Section 6.4 of Glasserman (2004) to recover O(h) weak convergence (instead of the O(√h) of the naive discretisation).

Parameters follow the brief: r = 0.05, σ = 0.5, T = 1, S₀ = 100, K = 110, B = 90.

**Main outputs**
1. Monte Carlo estimates of price, Delta and Vega.
2. Comparison against the closed-form benchmark (Reiner–Rubinstein) and 4th-order central finite differences.
3. Sensitivity sweeps with respect to S₀, σ and the barrier level B.
4. Weak convergence study in the time step h, showing the O(h) rate of the Brownian-bridge estimator versus O(√h) of the naive scheme, with explicit MC noise floor lines.
5. Statistical convergence study in the number of paths N (O(N^(−1/2)) reference).
6. Optional brute-force N = 10⁹ run and Richardson extrapolation to further verify the O(h) rate.

## Part 2 — Glasserman & Yu (2004) (`MATH70113_part_2.ipynb`)

Reproduces and extends the worst-case MSE analysis of regression-based American option pricing (the LSM algorithm of Longstaff & Schwartz). The central question is how the number of basis functions K and the number of simulated paths N must be jointly scaled to control the regression error.

**Notebook sections**
- **2.1 — MSE table, Brownian setting** (t₁ = 1, t₂ = 2). Reproduces Table 1 of the paper using Hermite polynomials, illustrating the critical threshold K* = log(N)/c_ρ.
- **2.2 — Verification of Lemmas 1–2.** Numerical checks of the moment identities underlying the Brownian-case bounds.
- **2.3 — MSE table, lognormal setting.** Reproduces Table 3 of the paper, showing the slower O(√log N) critical rate for geometric Brownian motion. Includes a heatmap (Figure 5), the Gram matrix conditioning remark, and Table 4.
- **2.4 — Practical LSM: American put RMSE grid.** Applies the LSM algorithm to a Bermudan put following Longstaff & Schwartz (2001) and measures RMSE on a (K, N) grid to compare practice with the worst-case theory.
- **2.5 — Theory vs practice.** Overlays the critical K*(N) curves on the empirical RMSE landscape to visualise where overfitting kicks in.
- **2.6 — Critical assessment.** Discussion of the numerical evidence, the gap between worst-case bounds and observed LSM behaviour, and the role of Gram matrix conditioning.


## Running the notebooks

**Requirements**
- Python ≥ 3.10
- `numpy`, `scipy`, `matplotlib`, `joblib`
- Optional: `cupy` for GPU acceleration (Part 1 auto-detects and falls back to NumPy on CPU)

**Reproducibility.** All random seeds are fixed. Shared Gaussian draws are reused across parameter sweeps to reduce variance and runtime. Figures are saved into a local `figures/` directory created at runtime.

To run, simply open either notebook in Jupyter and execute cells top-to-bottom. The Part 1 notebook will print whether it is running on GPU or CPU at startup; the Part 2 notebook is CPU-only and the heaviest cells (Table 3 and the LSM RMSE grid) take a few minutes on a laptop.