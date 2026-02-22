# VAN Benchmark Paper Plan

**Variational Autoregressive Networks with Biases in External Fields: Benchmarking Against the Exact 1D Ising Solution**

---

## Source Material

### Primary reference paper
- **Wu, Wang & Zhang (2019)** — "Solving Statistical Mechanics Using Variational Autoregressive Networks," Phys. Rev. Lett. 122, 080602
- arXiv: https://arxiv.org/abs/1809.10606

### Course notes (this site)
- **Chapter 1** — `van/van-01-mean-field.html`: NMF background, variational free energy, 1D & 2D Ising NMF results
- **Chapter 2** — `van/van-02-from-nmf-to-van.html`: Autoregressive decomposition, one-layer VAN, training
  - **§6**: The bias + tanh parameterization using magnetization $\mu_i$ (Eq. 32) — motivated by closer connection to NMF where $m_i$ is used rather than $q_i(s_i)$
  - **§8**: Small-$W$ expansion showing pairwise correlations emerge at leading order
  - **§9**: Worked $N=3$ example with exact values ($Z = 12.606$, $f \approx -1.690J$ at $\beta J = 0.5$, $H=0$)
  - **§10**: REINFORCE training loop with running-mean baseline

---

## Context & Motivation

The VAN course notes introduce a **tanh + bias** formulation for the one-layer VAN:

$$\mu_i = \tanh\!\bigl(b_i + \sum_{j<i} W_{ij}\, s_j\bigr)$$

while the original Wu, Wang & Zhang paper uses a **sigmoid, no-bias** formulation. The key insight: the bias formulation **naturally accommodates nonzero external field** $H$, because setting $W = 0$ recovers general NMF with arbitrary magnetizations $m_i = \tanh(b_i)$, while the no-bias version collapses to the uniform distribution.

The 1D periodic Ising model has an **exact transfer-matrix solution at all $(T, H, N)$**, making it a perfect benchmark. This paper would be the first systematic study of VAN performance across the full $(T, H)$ phase space, and the first to demonstrate the role of biases in external fields.

**Goal:** A computational research paper comparing exact 1D Ising thermodynamics against a 1-layer VAN solver with biases, across the full $(T, H)$ parameter space, with theoretical analysis of why biases are essential when $H \neq 0$.

---

## Paper Structure

### Abstract (sketch)

The one-layer VAN with bias terms provides a minimal variational ansatz that (i) reduces to NMF when weights vanish, (ii) captures pairwise correlations to leading order, and (iii) maintains tractable likelihood and sampling. We benchmark this architecture against the exact transfer-matrix solution of the 1D periodic Ising model across the full $(T, H)$ plane, systematically quantifying the variational gap $\Delta F = F_q - F_{\text{exact}}$ as a function of temperature, field, and system size. We show analytically that biases are essential for $H \neq 0$ (the no-bias ansatz cannot represent the correct $W=0$ limit), that the 1-layer VAN is exact for $N=2$ at all $(T,H)$, and that the dominant error source is the autoregressive ordering artifact on the wrap-around bond. Finite-size scaling of the variational gap reveals $\Delta F / N \to 0$ as $N \to \infty$ for all $(T,H)$, with the slowest convergence near the $T=0$, $H=0$ crossover.

---

### Section 1 — Introduction

- VAN paper (Wu, Wang & Zhang 2019): demonstrated VANs for $H=0$ Ising and Potts models
- Gap in literature: no systematic study with external fields; no analysis of bias terms
- 1D Ising is the ideal testbed: exact at all $(T, H, N)$, nontrivial correlations, crossover physics
- Contributions: (1) bias formulation, (2) full $(T,H)$ benchmarking, (3) theoretical error analysis

### Section 2 — Background

- **2.1**: 1D periodic Ising Hamiltonian: $\mathcal{H} = -J \sum_{\langle ij \rangle} s_i s_j - h \sum_i s_i$
- **2.2**: Transfer matrix $\mathbf{T} = \begin{pmatrix} e^{\beta J + \beta h} & e^{-\beta J} \\ e^{-\beta J} & e^{\beta J - \beta h} \end{pmatrix}$, exact $Z = \lambda_+^N + \lambda_-^N$
- **2.3**: Exact thermodynamics: $f$, $\langle m \rangle$, $\chi$, $C_v$, $\langle s_i s_j \rangle$ all in closed form
- **2.4**: Variational principle: $F_q \geq F$, $D_{KL}(q \| p) = \beta(F_q - F)$

### Section 3 — The One-Layer VAN with Biases

- **3.1**: Autoregressive factorization $q_\theta(\mathbf{s}) = \prod_i q(s_i \mid s_{<i})$
- **3.2**: Bernoulli conditionals with tanh parameterization (Eq. 32 from notes):
  $$\mu_i = \tanh\!\bigl(b_i + \sum_{j<i} W_{ij}\, s_j\bigr)$$
- **3.3**: Parameter count: $N$ biases + $N(N-1)/2$ weights = $N(N+1)/2$ total
- **3.4**: **The bias question** — systematic comparison:
  - $W=0$ with biases $\to$ NMF with $m_i = \tanh(b_i)$ (correct limit)
  - $W=0$ without biases $\to$ uniform distribution (wrong for $H \neq 0$)
  - Table: side-by-side conventions (original paper vs. this work)
- **3.5**: Variational free energy and REINFORCE gradient

### Section 4 — Theoretical Analysis

- **4.1**: **Small-$W$ expansion with biases** — generalize the expansion from §8 of the notes to include biases:
  $$\ln q_\theta(\mathbf{s}) \approx \text{const} + \sum_i \alpha_i s_i + \sum_{i>j} \tilde{W}_{ij}\, s_i s_j$$
  where $\alpha_i$ depends on $b_i$ and $\tilde{W}_{ij}$ depends on $W_{ij}$ and $\{b_k\}$. Show this is a general pairwise model with fields.
- **4.2**: **$N=2$ exactness proof** — for 2 spins with periodic BCs, the 1-layer VAN has 2 biases + 1 weight = 3 parameters, matching the 3 independent Boltzmann weights $\to$ exact at all $(T,H)$.
- **4.3**: **Ordering artifact analysis** — the periodic bond $s_N \to s_1$ is not directly representable. Quantify this error:
  - For $N$ spins with PBC, one bond is "frustrated" by the autoregressive ordering
  - The error is $O(1/N)$ per spin, vanishing in the thermodynamic limit
  - Worst case: low $T$, $H=0$ (strong correlations, bond matters most)
- **4.4**: **Converged parameter structure** — by symmetry arguments:
  - For $H=0$: all $b_i = 0$, all $W_{ij} = w > 0$, with $w \sim \beta J$
  - For $H \neq 0$: $b_i \neq 0$, breaking of translational symmetry due to ordering
  - The ordering artifact means the optimal parameters depend on position in the autoregressive chain

### Section 5 — Computational Experiments

- **5.1**: **Setup**
  - System sizes: $N \in \{2, 4, 8, 16, 32, 64\}$
  - $(T, H)$ grid: $T/J \in [0.1, 5.0]$ (40 points, log-spaced near $T=0$), $H/J \in [-2.0, 2.0]$ (20 points) $\to$ 800 grid points
  - 5 random seeds per point for error bars
  - Optimizer: Adam, lr=0.01, 5000 steps (sufficient for 1-layer)
  - Baseline: NMF (same architecture with $W=0$ frozen)
- **5.2**: **Metrics** at each $(T, H, N)$:
  - Variational gap: $\Delta F = F_q - F_{\text{exact}}$
  - Magnetization error: $|\langle m \rangle_q - \langle m \rangle_{\text{exact}}|$
  - Susceptibility error (from finite-$H$ derivative)
  - Nearest-neighbor correlation error: $|\langle s_i s_{i+1} \rangle_q - \langle s_i s_{i+1} \rangle_{\text{exact}}|$
- **5.3**: **Training diagnostics**: convergence speed, parameter trajectories, loss landscape for small $N$

### Section 6 — Results

- **6.1**: **$(T,H)$ heatmaps** of $\Delta F / N$ — the main result figures
  - Side-by-side: NMF vs 1-layer VAN
  - Show VAN dramatically improves over NMF everywhere, especially low $T$
- **6.2**: **$H=0$ slice** — reproduce/extend Wu et al. results, compare with and without biases
  - Without biases: VAN works fine (biases converge to ~0 anyway)
  - With biases: identical performance, but biases provide faster convergence
- **6.3**: **$H \neq 0$ slice** — the new regime
  - Without biases: VAN struggles, especially at low $T$ and moderate $H$
  - With biases: VAN captures the magnetization curve accurately
  - **This is the paper's central empirical result**
- **6.4**: **Finite-size scaling** — $\Delta F / N$ vs $N$ at fixed $(T, H)$ points
  - Confirm $O(1/N)$ scaling from the ordering artifact analysis
  - Identify the $(T, H)$ regions with slowest convergence
- **6.5**: **$N=2$ verification** — confirm numerical exactness (machine precision gap)
- **6.6**: **Parameter analysis** — visualize converged $\{b_i, W_{ij}\}$ across $(T, H)$
  - At $H=0$: confirm $b_i \approx 0$, $W_{ij} \approx w$
  - At $H \neq 0$: show biases track the external field, $b_i \sim \beta h$

### Section 7 — Discussion

- The bias formulation is a strict generalization of the no-bias version (extra $N$ parameters, negligible cost)
- For symmetric models ($H=0$), biases are optional but harmless
- For models with external fields, biases are essential — they provide the correct $W=0$ limit
- The ordering artifact is the dominant error; possible mitigation: average over random orderings, or use MADE-style multiple orderings
- Connection to inverse Ising problem: converged VAN parameters approximate the interaction matrix
- Limitations: 1D only (no phase transition), 1-layer only (pairwise correlations), periodic BCs only

### Section 8 — Conclusion

- First systematic VAN benchmark across the full $(T, H)$ phase space
- Bias terms essential for external fields, cost-free for symmetric models
- 1-layer VAN with biases provides an excellent variational ansatz for pairwise models
- Natural extension: 2D Ising (where exact Onsager solution exists for $H=0$) and $q$-state Potts

---

## Key Figures (10 total)

| # | Description | Type |
|---|---|---|
| 1 | Architecture diagram: 1-layer VAN with bias terms, lower-triangular mask | Schematic SVG |
| 2 | Exact 1D Ising phase diagram: $\langle m \rangle$ heatmap in $(T/J, H/J)$ plane | Colormesh |
| 3 | $\Delta F / N$ heatmap: NMF (left) vs 1-layer VAN (right), $N=16$ | Side-by-side colormesh |
| 4 | $\Delta F / N$ heatmap: VAN without biases (left) vs with biases (right), $N=16$ | Side-by-side colormesh |
| 5 | $H=0$ slice: $\Delta F/N$ vs $T/J$ for multiple $N$, VAN vs NMF | Line plot |
| 6 | $T/J = 0.5$ slice: $\langle m \rangle$ vs $H/J$, exact vs VAN vs NMF, $N=16$ | Line plot |
| 7 | Finite-size scaling: $\Delta F / N$ vs $1/N$ at selected $(T, H)$ points | Log-log plot |
| 8 | $N=2$ verification: $\Delta F$ vs $(T, H)$ at machine precision | Colormesh |
| 9 | Converged parameters: $b_i$ and $W_{ij}$ vs site index, several $(T,H)$ | Multi-panel line |
| 10 | Training convergence: $F_q$ vs step for representative $(T,H)$ points | Line plot |

---

## Implementation Plan

### Repository structure

```
van-1d-ising-benchmark/
├── src/
│   ├── exact/
│   │   └── transfer_matrix.py      # Exact Z, F, ⟨m⟩, χ, C_v, ⟨s_i s_j⟩
│   ├── nmf/
│   │   └── mean_field.py           # NMF solver (fixed-point iteration)
│   └── van/
│       ├── model.py                # 1-layer VAN (PyTorch): W lower-triangular, optional biases
│       ├── train.py                # REINFORCE training loop with baseline
│       └── sample.py               # Autoregressive sampling
├── experiments/
│   ├── sweep_TH.py                 # Main (T,H) grid sweep
│   ├── finite_size.py              # N-scaling experiments
│   └── n2_exact.py                 # N=2 verification
├── figures/
│   └── plot_*.py                   # One script per figure
├── tests/
│   ├── test_transfer_matrix.py     # Verify exact solution against known results
│   ├── test_van_n2.py              # Verify N=2 exactness
│   └── test_nmf_consistency.py     # Verify NMF matches W=0 VAN
└── paper/
    └── main.tex                    # LaTeX manuscript
```

### Tech stack

- **PyTorch** for VAN model and training (GPU-acceleratable)
- **NumPy/SciPy** for exact transfer matrix (eigenvalues of 2x2 matrix)
- **Matplotlib** for all figures
- **pytest** for verification tests

### Key implementation details

#### 1. Transfer matrix (`src/exact/transfer_matrix.py`)

- Input: $(\beta, J, h, N)$
- Compute $\lambda_{\pm}$ analytically:
  $$\lambda_{\pm} = e^{\beta J} \cosh(\beta h) \pm \sqrt{e^{2\beta J} \sinh^2(\beta h) + e^{-2\beta J}}$$
- $Z = \lambda_+^N + \lambda_-^N$, $f = -(1/N\beta) \ln Z$
- $\langle m \rangle$ and $\langle s_i s_j \rangle$ from derivatives or explicit trace formulas

#### 2. VAN model (`src/van/model.py`)

- `nn.Parameter` for $W$ (full $N \times N$, masked to lower-triangular during forward)
- `nn.Parameter` for $b$ (length $N$), with flag `use_bias=True/False`
- Forward: sequential pass computing $\mu_i = \tanh(b_i + \sum_{j<i} W_{ij} s_j)$
- `log_prob(s)`: sum of log Bernoulli probabilities
- `sample(batch_size)`: autoregressive ancestral sampling

#### 3. Training (`src/van/train.py`)

- Loss = $\langle \beta E(\mathbf{s}) + \ln q_\theta(\mathbf{s}) \rangle_q$ (REINFORCE)
- Baseline: running mean of $\beta E(\mathbf{s}) + \ln q_\theta(\mathbf{s})$
- Adam optimizer, batch size 1000, 5000 steps
- Convergence check: loss change < $10^{-6}$ over 100 steps

---

## Verification Checklist

1. **Unit tests**: `pytest tests/` — verify transfer matrix against known values (e.g., $N=2$ by hand, $H=0$ against textbook), verify $N=2$ VAN exactness, verify NMF$\leftrightarrow$VAN consistency
2. **Small-$N$ spot checks**: Compare VAN results at $N=3$ against the worked example in `van-02` (§9) — must match $Z=12.606$, $f \approx -1.690J$ at $\beta J = 0.5$, $H=0$
3. **Symmetry checks**: At $H=0$, verify $\langle m \rangle = 0$ and $b_i \approx 0$ after training
4. **Scaling check**: Confirm $\Delta F / N \propto 1/N$ at large $N$
5. **Bias ablation**: Confirm biases help at $H \neq 0$ and are neutral at $H=0$

---

## Novel Contributions

1. **First VAN study with external fields** — all prior work uses $H=0$
2. **Bias formulation** — explicit tanh+bias parameterization with theoretical justification
3. **Complete $(T, H)$ benchmarking** — 800-point grid x 6 system sizes x with/without biases
4. **Analytical results** — $N=2$ exactness proof, ordering artifact $O(1/N)$ scaling, small-$W$ expansion with biases
5. **Practical recommendation** — always include biases (cost-free, essential for fields)
