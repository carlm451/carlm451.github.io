# CLAUDE.md

## Project Overview

Self-study course notes at **carlm451.github.io**, hosted on GitHub Pages. Static HTML site — no build tools, no frameworks, no Jekyll. Raw HTML files served directly.

## Site Structure

```
carlm451.github.io/
├── .nojekyll              # Prevents Jekyll processing (critical, do not remove)
├── CLAUDE.md              # This file
├── README.md              # Repo description
├── index.html             # Landing page — links to all courses (body.page-landing)
├── _template-chapter.html # Template for new chapter pages — not served
│
├── assets/
│   └── style.css          # Shared stylesheet for all pages
│
├── tdl/                   # Topological Deep Learning course (live)
│   ├── tdl-index.html         # Course table of contents (body.page-index)
│   ├── tdl-01-graphs.html     # Ch 1: Graphs, Laplacian, eigendecomposition, spectral theory
│   ├── tdl-02-gnn.html        # Ch 2: Feedforward NN review, GCN layer, full numerical walkthrough
│   ├── tdl-03-edge-signals.html  # Ch 3: B₁₂, discrete curl, Hodge Laplacian L₁
│   ├── tdl-04-face-signals.html  # Ch 4: L₂, Betti numbers, complete de Rham complex
│   ├── tdl-05-complexes.html     # Ch 5: Simplicial → Cell → Combinatorial complexes
│   ├── tdl-06-message-passing.html # Ch 6: Cochains, adjacency types, HOMP framework
│   ├── tdl-07-architectures.html   # Ch 7: Hodge theory, architecture zoo (SNN, CWN, CAN, etc.)
│   └── tdl-08-applications.html    # Ch 8: Thermal physics connection, reading roadmap
│
├── van/                   # Variational Autoregressive Networks course (live)
│   ├── van-index.html         # Course table of contents (body.page-index)
│   ├── van-01-mean-field.html # Ch 1: Variational free energy, NMF ansatz, 1D & 2D Ising
│   └── van-02-from-nmf-to-van.html # Ch 2: Autoregressive decomposition, one-layer VAN, training
│
├── fno/                   # Fourier Neural Operators course (live)
│   ├── fno-index.html         # Course table of contents (body.page-index)
│   ├── fno-01-operator-learning.html  # Ch 1: Domain D, function spaces, operator G†, discretization
│   ├── fno-02-neural-operator.html    # Ch 2: Lift-iterate-project, kernel integral, Definitions 1-2
│   ├── fno-03-fourier-space.html      # Ch 3: Convolution theorem, Definition 3, R tensor, mode truncation
│   ├── fno-04-fno-layer.html          # Ch 4: Full FNO layer, two paths, FFT implementation, complexity
│   ├── fno-05-real-pdes.html          # Ch 5: Burgers, Darcy, Navier-Stokes, super-resolution, limitations
│   ├── fno-06-ultrasonic-ndt.html     # Ch 6: Elastic waves, P/S-waves, Snell's law, angle beam NDT setup
│   └── fno-07-fno-for-cracks.html     # Ch 7: Crack parameterization, FNO architecture, training, challenges
│
├── gcn/                   # Discovering Graph Convolutions course (live)
│   ├── gcn-index.html         # Course table of contents (body.page-index)
│   ├── gcn-01-circulants.html     # Ch 1: Z_N signals, shift operator S, circulant matrices, circular convolution
│   ├── gcn-02-discovering-dft.html # Ch 2: Shift invariance, simultaneous diagonalization, eigenvectors of S*, DFT emerges
│   ├── gcn-03-big-picture-zn.html # Ch 3: DFT matrix, spectral filtering, three isomorphic algebras, what breaks on graphs
│   ├── gcn-04-ring-to-graph.html  # Ch 4: A=S+S*, eigenvalue collapse, graph Laplacian, eigendecomposition, three chars revisited
│   ├── gcn-05-graph-convolutions.html # Ch 5: GFT, spectral convolutions, polynomial convolutions, lost characterization
│   ├── gcn-06-spectral-to-nn.html # Ch 6: ChebNet, GCN, message passing, oversmoothing, full GCN layer walkthrough
│   ├── gcn-07-analogies.html      # Ch 7: Complete analogy table, modern GNN variants, beyond graphs, further reading
│   └── eigenvisual_v7.html        # Interactive eigenvisual app (copied from graphconvolutions/, embedded in Ch 2)
│
├── [pinns/]               # Physics-Informed Neural Networks (planned)
└── [pde/]                 # PDEs for ML (planned)
```

## Design System

All pages share a single external stylesheet at `assets/style.css`. Three page types are distinguished by body class:

- **`body.page-landing`** — Landing page (`index.html`): wider header, card grid
- **`body.page-index`** — Course index pages (`tdl/tdl-index.html`, etc.): chapter card list
- **`body.page-chapter`** — Chapter pages: sticky nav, progress bar, TOC, back-to-top

### Design Tokens

- **Fonts:** Crimson Pro (body), Source Sans 3 (UI/labels), JetBrains Mono (code/math)
- **Colors:** `--accent: #b44a2f` (red), `--accent2: #2a6b5a` (green), `--accent3: #3a5a8c` (blue), `--accent4: #8b5a8c` (purple), `--bg: #faf8f5`
- **Math:** MathJax 3.2.2 (tex-svg-full), loaded per page via CDN
- **Diagrams:** Inline SVG throughout — no external images

### Component Classes

| Class | Purpose | Variants |
|-------|---------|----------|
| `.def-box` | Definition/theorem boxes | `.green`, `.blue`, `.purple` (border color) |
| `.insight` | Callout boxes with emoji | `.physics` (atom), `.warning`, `.key` |
| `.figure` | SVG diagrams with captions | — |
| `.math-block` | Display equations | `.math-label` for titles |
| `.comp-table` | Comparison tables | — |
| `.steps` | Numbered step lists | — |
| `.toc` | In-page table of contents | — |
| `pre > code` | Code blocks (dark theme) | `.kw`, `.fn`, `.str`, `.num`, `.cmt` for syntax tokens |

### SVG Diagram Classes

| Class | Element | Colors |
|-------|---------|--------|
| `.node-v` | Vertices (rank 0) | fill: #e8a88a, stroke: #b44a2f |
| `.node-e` | Edges (rank 1) | fill: #8ec5b6, stroke: #2a6b5a |
| `.node-f` | Faces (rank 2) | fill: #a8c0e0, stroke: #3a5a8c |
| `.edge-line` | Connecting lines | stroke: #6b6560, width: 1.5 |
| `.label-text` | Primary labels | 11px, fill: #2a2520 |
| `.label-sm` | Secondary labels | 10px, fill: #6b6560 |

### Chapter Page Features

Each chapter page includes:
- **Sticky nav** with prev/next links and chapter indicator
- **Progress bar** (gradient fill, updates on scroll via JS)
- **In-page TOC** linking to section anchors
- **Back-to-top button** (appears after 600px scroll)
- **Print stylesheet** (hides nav, expands content, shows URLs)

## The Running Example

The entire TDL course uses **one consistent example** across all chapters:

- **Graph:** 4 vertices (v₀–v₃), 5 edges (a–e), adjacency A is given in Ch 1
- **Simplicial complex:** Same graph + 2 triangular faces σ₁={0,1,2}, σ₂={0,2,3} (added in Ch 3)
- **Edge orientations:** a:0→1, b:0→2, c:1→2, d:0→3, e:2→3
- **Temperature signal:** T = (50, 20, 30, 10)ᵀ used for Laplacian/spectral examples
- **GNN features:** H⁰ = [[1.0,0.2],[0.4,0.8],[0.7,0.3],[0.2,0.9]] used in Ch 2

All matrices (B₁, B₁₂, L₀, L₁, L₂, eigenvalues, Â) have been computed and verified numerically. Maintain consistency if editing.

## VAN Course — Key Facts

The VAN course covers variational mean field theory and its generalization to autoregressive networks. Section numbering is globally sequential within the VAN course (independent from TDL):
- **Chapter 1 (§1–§4):** Variational free energy, NMF ansatz, 1D Ising (spurious transition at kT_c=2J), 2D Ising (NMF kT_c=4J vs Onsager exact kT_c≈2.269J)
- **Chapter 2 (§5–§10):** Autoregressive decomposition, Bernoulli conditionals, lower-triangular weight matrix, small-W expansion (pairwise correlations), N=3 worked example, VAN training loop

### The Bias Question
The VAN paper (Eq. 8) uses **no biases**: σ(∑W_ij s_j), N(N-1)/2 params. The notes (Eq. 32) include biases: tanh(b_i + ∑W_ij s_j), N(N+1)/2 params. Setting W=0: with biases → general NMF; without biases → uniform distribution. The paper omits biases because for the symmetric Ising model (h=0), optimal NMF has m_i=0 above T_c.

### Equation Numbering
Equations are numbered sequentially across both chapters: Ch 1 uses \tag{1}–\tag{31}, Ch 2 uses \tag{32}–\tag{33}+.

## FNO Course — Key Facts

The FNO course breaks down Li et al. (arXiv:2010.08895) with a 2D Darcy flow running example. Section numbering is globally sequential within the FNO course:
- **Chapter 1 (§1–§6):** Darcy flow PDE, domain D=(0,1)², function spaces A and U, operator G†, training objective (Eq. 1), discretization, resolution invariance
- **Chapter 2 (§7–§12):** Lift-iterate-project pipeline, P (pointwise lift), Definition 1 (iterative update, Eq. 2), Definition 2 (kernel integral, Eq. 3), Q (projection), forward pass dimensions
- **Chapter 3 (§13–§17):** Fourier refresher, convolution theorem, translation-invariant kernels, Definition 3 (Fourier integral operator, Eq. 4), R tensor shape (C^{12×12×32×32}), mode truncation
- **Chapter 4 (§18–§22):** Full FNO layer (Eq. 5), two parallel paths (Fourier global + W local), discrete FFT implementation (Eq. 6), complete forward pass with tensor shapes, complexity O(n log n)
- **Chapter 5 (§23–§27):** Burgers (Eq. 7), Darcy with experimental results, Navier-Stokes (Eqs. 8-10), zero-shot super-resolution, FNO vs PINNs vs DeepONet comparison
- **Chapter 6 (§28–§32):** Elastic wave equation (Eq. 11), P/S-wave speeds (Eq. 12), Snell's law (Eq. 13), angle beam NDT setup, transducer pulse (Eq. 14), fracture BC (Eq. 15), NDT operator
- **Chapter 7 (§33–§37):** Crack parameterization (Eq. 16), signal-level FNO-1D (Eq. 17), full-field FNO-3D (Eq. 18), training data from COMSOL, loss function (Eq. 19), resolution transfer (Eq. 20), Bayesian inversion (Eq. 21), FNO variant comparison (PINO, Geo-FNO, U-FNO, FFNO)

### Running Example (Parts I–III)
- **PDE:** −∇·(a(x)∇u(x)) = 1 on D=(0,1)², u=0 on boundary
- **Input:** Permeability a(x) ∈ {3, 12} (piecewise constant)
- **Output:** Pressure field u(x)
- **Architecture:** d_a=1, d_v=32, d_u=1, k_max=12, T=4 layers, ~1.19M params

### Running Example (Part IV)
- **PDE:** Elastic wave equation (velocity-strain form) in 2D aluminum + acrylic wedge
- **Geometry:** Aluminum specimen ~35mm × 32mm, acrylic wedge, PZT-5H transducer at 1.5 MHz
- **Physics:** P-wave in wedge → mode conversion → S-wave at 45° in aluminum → crack scattering
- **Input:** Crack parameters (x_c, y_c, L, θ) or binary field encoding
- **Output:** Velocity wavefield v(x,t) or transducer signal V(t)
- **Materials:** Aluminum (ρ=2700, cp=6200, cs=3120), Acrylic (ρ=1190, cp=2080, cs=1000)

### Equation Numbering
Equations numbered sequentially across all chapters: Eq. 1 (training objective) through Eq. 21 (Bayesian inversion posterior).

## GCN Course — Key Facts

The GCN course tells the mathematical story of how graph convolutions emerge from the same ideas that produce the DFT, following Bamieh (2018)'s "discovery" approach. Section numbering is globally sequential within the GCN course:
- **Chapter 1 (§1–§4):** Z_N signals on a ring, shift operator S (8×8), circulant matrices as polynomials in S, circular convolution = polynomial in S
- **Chapter 2 (§5–§8):** Shift invariance as commutativity (SM=MS), simultaneous diagonalization theorem, constructive derivation of eigenvectors of S*, DFT forced by diagonalization
- **Chapter 3 (§9–§12):** DFT matrix W, spectral filtering recipe (DFT→multiply→IDFT), three isomorphic algebras (Bamieh Thm 5.1), three pillars, preview of what breaks on graphs
- **Chapter 4 (§13–§17):** A=S+S* (undirected ring), eigenvalue collapse λ_k=2cos(2πk/N), 6-vertex running graph, graph Laplacian L=D−A, full eigendecomposition, three characterizations revisited
- **Chapter 5 (§18–§22):** Graph Fourier Transform (x̂=U^Tx), spectral graph convolutions (U diag(ĥ) U^T x), polynomial convolutions (p_θ(L)), connecting both views, lost group-theoretic characterization
- **Chapter 6 (§23–§27):** Computational bottleneck (O(N³) vs O(d|E|)), ChebNet, GCN (Kipf & Welling degree-1), message passing as consequence, oversmoothing, full GCN layer worked example
- **Chapter 7 (§28–§32):** Complete Z_N↔graph analogy table, what we discovered, modern GNN variants (GAT, GraphSAGE, GIN), beyond graphs (Hodge Laplacian), further reading

### Running Examples
- **Part I (Z_8):** Shift operator S on Z_8, kernel h = [3,1,0,0,0,0,0,2], circulant matrix C_h, DFT verification
- **Part II (6-vertex graph):** Two triangles {0,1,2} and {3,4,5} connected by bridge edge {2,3}
  - Adjacency A, Degree D = diag(2,2,3,3,2,2), Laplacian L = D−A
  - Eigenvalues: λ₀=0, λ₁=(5−√17)/2≈0.44, λ₂=λ₃=λ₄=3 (triple!), λ₅=(5+√17)/2≈4.56
  - The triple eigenvalue at 3 demonstrates non-unique eigenbasis on graphs

### Interactive Elements
- **eigenvisual_v7.html** — copied from graphconvolutions/ into gcn/, embedded via iframe in Ch 2 §7

### Equation Numbering
Equations numbered sequentially across all chapters: Eq. 1 (signal as vector) through Eq. 48 (Hodge Laplacian).

## Key Reference Papers

### TDL Course
1. **Hajij et al. (2022)** — "Topological Deep Learning: Going Beyond Graph Data" — the primary paper this course provides background for
2. **Kipf & Welling (2017)** — "Semi-Supervised Classification with Graph Convolutional Networks" — GCN, covered in Ch 2
3. **Ebli, Defferrard & Spreemann (2020)** — Simplicial Neural Networks
4. **Bodnar et al. (2021)** — Weisfeiler and Leman Go Topological (CW Networks)
5. **Papillon et al. (2023)** — TopoX software suite / TopoModelX

### VAN Course
1. **Wu, Wang & Zhang (2019)** — "Solving Statistical Mechanics Using Variational Autoregressive Networks," Phys. Rev. Lett. 122, 080602 (arXiv:1809.10606) — the primary paper this course provides background for
2. **Onsager (1944)** — Exact solution of the 2D Ising model — comparison target for NMF

### FNO Course
1. **Li et al. (2020)** — "Fourier Neural Operator for Parametric Partial Differential Equations" (arXiv:2010.08895) — the primary paper this course provides background for
2. **COMSOL tutorial** — "Angle Beam Nondestructive Testing" (models.aco.angle_beam_ndt) — source material for Part IV NDT application
3. **Mehtaj & Banerjee (2025)** — "SciML for Elastic and Acoustic Wave Propagation" (Sensors, 25, 3588) — review of FNO/DeepONet/PINNs for wave problems

### GCN Course
1. **Bamieh (2018)** — "A Tutorial on Matrix Functions and their use in the Discovery of the DFT" — the primary reference for Part I (circulant matrices, simultaneous diagonalization, discovering the DFT)
2. **Hammond, Vandergheynst & Gribonval (2011)** — "Wavelets on Graphs via Spectral Graph Theory" — foundational graph signal processing, ancestor of ChebNet
3. **Defferrard, Bresson & Vandergheynst (2016)** — "Convolutional Neural Networks on Graphs with Fast Localized Spectral Filtering" — ChebNet, bridge from spectral theory to neural networks
4. **Kipf & Welling (2017)** — "Semi-Supervised Classification with Graph Convolutional Networks" — the GCN degree-1 simplification
5. **Bronstein, Bruna, Cohen & Veličković (2021)** — "Geometric Deep Learning: Grids, Groups, Graphs, Geodesics, and Gauges" — the "5G" blueprint paper
6. **Distill.pub (2021)** — "Understanding Convolutions on Graphs" — complementary visual introduction (this course goes deeper mathematically)

## Navigation Pattern

Each chapter page has:
- Header with breadcrumb: Home / TDL / Chapter N
- Sticky top nav: ← Previous | Chapter N | Next →
- Progress bar: gradient fill showing scroll position
- In-page TOC: links to section anchors within the chapter
- Footer nav: large prev/next cards
- Footer: links back to course home and landing page
- Back-to-top button: fixed bottom-right, appears on scroll

## Adding a New TDL Chapter

1. Copy `_template-chapter.html` into the `tdl/` directory as starting point
2. Replace all `PLACEHOLDER` values: CHAPTER_NUM, CHAPTER_TITLE, COURSE_NAME, etc.
3. Ensure paths use `../` prefix: `../assets/style.css`, `../index.html`
4. Add section IDs matching the TOC links (e.g., `id="secNN"`)
5. Update prev chapter's `next` links to point to new file
6. Update next chapter's `prev` links to point to new file
7. Add entry to `tdl/tdl-index.html`
8. Section numbering is globally sequential (Ch 1 = §1–2, Ch 2 = §3–8, etc.)

## Adding a New Course

1. Create a subdirectory for the course: `[topic]/`
2. Create `[topic]/[topic]-index.html` (copy `tdl/tdl-index.html` as template, use `body.page-index`)
3. Create chapter files from `_template-chapter.html`: `[topic]/[topic]-01-*.html`, `[topic]/[topic]-02-*.html`, etc.
4. All course files use `../` prefix for shared assets: `../assets/style.css`, `../index.html`
5. Inter-chapter links within a course use relative paths (no prefix needed, same directory)
6. Add a card to `index.html` landing page pointing to `[topic]/[topic]-index.html` (change `.planned` to `.featured`)
7. Follow same design system — shared `assets/style.css`, same fonts/colors, SVG diagrams, MathJax

## Planned Courses

From `index.html` placeholders (in rough priority order):

| Course | Directory | File prefix | Status |
|--------|-----------|-------------|--------|
| Discovering Graph Convolutions | `gcn/` | `gcn-` | Live |
| Physics-Informed Neural Networks | `pinns/` | `pinns-` | Planned |
| Fourier Neural Operators | `fno/` | `fno-` | Live |
| DeepONet & Neural Operator Theory | `deeponet/` | `deeponet-` | Planned |
| PDEs for Machine Learning | `pde/` | `pde-` | Planned |
| Optimization for Deep Learning | `optim/` | `optim-` | Planned |
| Autodiff & Backprop from Scratch | `autodiff/` | `autodiff-` | Planned |
| Molecular Dynamics | `md/` | `md-` | Planned |
| Computational Electromagnetics | `cem/` | `cem-` | Planned |

## Context

This site supports self-study at the intersection of physics, mathematics, and machine learning. The author has a Physics PhD (Brandeis) and works on AI-driven infrastructure inspection using UAV thermal imagery and spatiotemporal neural networks. Content should maintain physics intuition alongside mathematical rigor, with worked numerical examples wherever possible.

## Deployment

Push to `main` branch → GitHub Pages auto-deploys in ~60 seconds. No build step needed.
