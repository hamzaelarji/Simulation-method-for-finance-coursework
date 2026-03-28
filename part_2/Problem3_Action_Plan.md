# Problem 3 — Action Plan: Review of Glasserman & Yu (2004)
## "Number of Paths Versus Number of Basis Functions in American Option Pricing"

---

## What the Markers Want (from the assignment brief)

1. **Summarise** the problem and solution methodology
2. **Reproduce numerical results** from the paper
3. **Validate theoretical results through numerical experiments** (not just restate theory)
4. **Bonus points** for:
   - Numerical tests **beyond** those in the paper
   - Comparing with **related literature**
   - Discussion of **pros and cons**

---

## The Plan — 6 Blocks of Work

### Block 1: Implement the Longstaff–Schwartz (LSM) Algorithm
> *Foundation — everything else builds on this*

**What to code:**
- Standard LSM for pricing a **Bermudan put option** on GBM
  - Parameters from Longstaff–Schwartz Table 1: `S₀ = 36,38,40,42,44`, `K = 40`, `r = 0.06`, `σ = 0.2`, `T = 1` or `2`, `m = 50` exercise dates
- Use **Laguerre polynomials** (as in LS original) and also plain **power monomials** as basis functions
- Two-pass procedure: backward induction to learn stopping rule, then forward pass on **independent** paths for unbiased price estimate

**Deliverables:**
- Table reproducing LS original results (validates your LSM code)
- Convergence plot: price vs N (number of paths) for fixed K

---

### Block 2: Reproduce the Single-Period Normal Setting (Theorem 1 & Table 1 of G–Y)
> *This is the centrepiece of the paper*

**What the paper shows:**
- Brownian motion, Hermite polynomial basis
- Critical relationship: `K* = log(N) / c_ρ` where `c_ρ = 2·log(2 + √ρ)`, `ρ = t₂/t₁`
- Below `K*`: MSE → 0; above `K*`: MSE → ∞

**What to code:**
1. For each `(N, K)` pair in Table 1 of the paper (`N ∈ {500, ..., 128000}`, `K ∈ {1,...,12}`):
   - Generate `B = 5000` batches, each of `N` paths
   - For each batch, compute the regression coefficient estimate `β̃`
   - Estimate `MSE(β̃) = (1/B) Σ |β̃ - β|²`
   - Use `Y* = ρ^{K/2} · HeK(S₂/√t₂) / √(K!)` with `t₁=1, t₂=2`
2. Reproduce **Table 1** of Glasserman–Yu
3. Plot a **heatmap** of log(MSE) over the (N, K) grid with the theoretical `K* = log(N)/c_ρ` line overlaid — this is a very strong visual

**Deliverables:**
- Reproduced Table 1 (or close match)
- Heatmap showing the phase transition at the critical boundary
- Commentary on how fast the transition is

---

### Block 3: Reproduce the Lognormal Setting (Theorem 2)
> *GBM case — the practically relevant one*

**What the paper shows:**
- Geometric Brownian motion, power-function basis `ψ_k(S(t)) = exp(kW(t) - k²t/2)`
- Critical relationship: `K* = √(log(N) / (3t₁ + t₂))` (lower bound), `K* = √(log(N) / (5t₁ + t₂))` (upper bound)
- Convergence is **much slower** — N must grow as `exp(K²)` instead of `exp(K)`

**What to code:**
1. Same Monte Carlo experiment as Block 2 but now with GBM and the power basis
2. Produce a table analogous to Table 1 for the lognormal case (the paper doesn't give one explicitly — **this goes beyond the paper**)
3. Overlay both critical boundaries (upper and lower from Theorem 2) on a heatmap

**Deliverables:**
- Table of MSE values for lognormal case
- Heatmap with dual critical boundaries
- Direct comparison plot: normal vs lognormal critical K* as a function of N

---

### Block 4: Numerical Experiments on the Full LSM Algorithm
> *Bridge theory to practice — this is where bonus points come from*

**Experiments to run:**

#### 4a. Paths vs Basis Functions in Practice
- Fix the American put problem from Block 1
- Vary `K ∈ {2, 3, 4, ..., 15}` (number of basis functions) and `N ∈ {1000, 2000, ..., 500000}`
- For each (N, K): run 100 independent trials, record mean price and RMSE vs benchmark (e.g. binomial tree with 10000 steps)
- Plot: **contour plot** of pricing error over the (N, K) plane
- Show that in practice, the theoretical warnings about K growing too fast relative to N **do materialise** as pricing bias or instability

#### 4b. Choice of Basis Functions
- Compare at least 3 basis families on the same problem:
  1. **Monomials**: `1, S, S², S³, ...`
  2. **Laguerre polynomials** (weighted, as in LS original)
  3. **Hermite polynomials** (as in G–Y theory)
  4. *(Optional)* Chebyshev polynomials
- For each family, plot price convergence as K increases (fixed large N)
- Show that some bases are **better conditioned** than others (plot condition number of the Gram matrix Ψ̂ vs K)

#### 4c. Overfitting Demonstration
- Pick a moderately large K (e.g. K=15) and small N (e.g. N=500)
- Show that the in-sample continuation value fits look good but the out-of-sample (forward pass) price is **biased high**
- Compare with a small K (e.g. K=3) — show it's more robust
- This directly validates the paper's warning

**Deliverables:**
- Contour/heatmap of pricing error in (N, K) plane
- Basis function comparison table and condition number plots
- Overfitting demonstration with in-sample vs out-of-sample comparison

---

### Block 5: Related Literature Comparison
> *Shows breadth of understanding — markers love this*

**Papers to reference and discuss:**

| Paper | Key Result | How it Relates |
|-------|-----------|----------------|
| **Clément, Lamberton & Protter (2002)** | Proved convergence of LS for fixed K as N→∞ | G–Y extends this by letting K→∞ too |
| **Stentoft (2004)** | Convergence if K³/N → 0 (shifted Legendre polynomials) | Alternative sufficient condition; compare with G–Y |
| **Tsitsiklis & Van Roy (2001)** | Alternative regression-based method (regress on value, not continuation) | G–Y algorithm is closer to LS but results apply broadly |
| **Broadie & Glasserman (2004)** | Stochastic mesh method | Alternative MC approach — no basis function issue |
| **Belomestny (2011) / Zanger (2013, 2018)** | Sharper non-asymptotic error bounds | Improvements on G–Y theory |

**What to write (1–2 paragraphs each):**
- How G–Y fits into the theoretical landscape
- The practical rule of thumb: keep K small relative to N (K³/N → 0 from Stentoft)
- Whether the exponential growth warning is conservative in practice

---

### Block 6: Report Write-Up
> *Structure for the Part 2 section of the report (aim for ~7–8 pages)*

**Suggested structure:**

1. **Introduction & Problem Statement** (~0.5 page)
   - American option pricing as optimal stopping
   - Why simulation + regression (LS method) is needed for high dimensions
   - What question G–Y addresses

2. **The Longstaff–Schwartz Method** (~1 page)
   - Dynamic programming formulation: V*, C*, the projection operator Πₙ
   - The sample-based algorithm (Steps 1–3 from G–Y §2.2)
   - Role of basis functions

3. **Theoretical Results from Glasserman–Yu** (~2 pages)
   - Single-period analysis setup: MSE(β̃) and its bounds
   - **Theorem 1** (Normal/Hermite): K = O(log N), with proof sketch
   - **Theorem 2** (Lognormal/power): K = O(√log N)
   - **Theorem 3** (Multiperiod bound)
   - Intuition: fourth moments of basis functions drive the required sample size

4. **Numerical Reproduction of Paper Results** (~1.5 pages)
   - Reproduced Table 1 + heatmap
   - Lognormal experiments
   - Discussion of match with theory

5. **Extended Numerical Experiments** (~2 pages)
   - Full LSM pricing experiments (Block 4a–4c)
   - Basis function comparison
   - Overfitting demonstration
   - Practical implications

6. **Related Literature & Discussion** (~1 page)
   - Comparison with Clément et al., Stentoft, Zanger
   - Pros and cons of the G–Y analysis
   - Practical guidelines for practitioners

---

## Implementation Order (Suggested Timeline)

| Step | Task | Priority |
|------|------|----------|
| 1 | Implement LSM algorithm + validate against LS Table 1 | 🔴 Critical |
| 2 | Reproduce G–Y Table 1 (normal setting, Hermite polynomials) | 🔴 Critical |
| 3 | Heatmap visualisation with critical boundary | 🔴 Critical |
| 4 | Lognormal setting experiments (Theorem 2) | 🟡 High |
| 5 | Full LSM pricing experiments (Block 4a: error contours) | 🟡 High |
| 6 | Basis function comparison (Block 4b) | 🟡 High |
| 7 | Overfitting demo (Block 4c) | 🟢 Medium |
| 8 | Multi-dimensional extension (e.g. max-call on basket) | 🟢 Bonus |
| 9 | Write report | 🔴 Critical |

---

## Key Formulas to Implement

```
Hermite polynomials:  He_n(x) = Σ (-1)^i · n! · x^{n-2i} / ((n-2i)! · i! · 2^i)

Basis (normal):       ψ_{nk}(x) = He_k(x/√tₙ) / √(k!)

Critical K (normal):  K* = log(N) / c_ρ,   c_ρ = 2·log(2 + √ρ),   ρ = t₂/t₁

Critical K (lognormal, upper bound): K* = √((1-δ)·log(N) / (5t₁+t₂))
Critical K (lognormal, lower bound): K* = √((1+δ)·log(N) / (3t₁+t₂))

MSE bound (multiperiod): E[‖Ĉₙ - Cₙ‖²] ≤ (2^{m-n} - 1)·(K+1)²/N · B_K · A_K^{m-n} · (E[ψ²_{mK}])²
```

---

## What Will Make This "Excellent"

✅ Clean, correct LSM implementation  
✅ Faithful reproduction of G–Y Table 1 with matching numbers  
✅ Beautiful heatmap showing the phase transition  
✅ Going beyond the paper: lognormal table, practical LSM experiments  
✅ Basis function comparison showing condition number issues  
✅ Overfitting demo connecting theory to practice  
✅ Thoughtful literature comparison (not just listing papers)  
✅ Clear, well-structured LaTeX report with proper mathematical typesetting  
✅ All code in a single reproducible Jupyter notebook with seeds set  
