# Monte Carlo Methods in Finance

Group coursework for MATH70113 — Simulation Methods for Finance (Imperial College London, 2026).

**Authors:** Paul Archer, Hamza El Arji, Chedi Mnif, Diane Murzi

## Contents

- `report.pdf` — full write-up with derivations, results and analysis *(start here)*
- `MATH70113_NB_Final.ipynb` — all numerical experiments and figures

## Summary

**Part I:** Pathwise Delta and Vega estimation for a down-and-out barrier call under Black--Scholes, with Brownian bridge barrier correction (O(h) weak convergence).

**Part II:** Review and extension of Glasserman & Yu (2004) on regression-based American option pricing — MSE phase transition, worst-case vs typical targets, quasi-regression vs full OLS, and a Longstaff--Schwartz pricing experiment with Laguerre polynomials.

## Running the notebook

Designed for Google Colab (T4 GPU). Falls back to CPU automatically.
```bash
pip install cupy-cuda12x  # optional, for GPU acceleration
jupyter notebook MATH70113_NB_Final.ipynb
```

Total runtime: ~20 min on GPU, ~2h min on CPU.