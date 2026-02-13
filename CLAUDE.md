# CLAUDE.md

## Project Overview

Self-study course notes at **carlm451.github.io**, hosted on GitHub Pages. Static HTML site — no build tools, no frameworks, no Jekyll. Raw HTML files served directly.

## Site Structure

```
carlm451.github.io/
├── .nojekyll              # Prevents Jekyll processing (critical, do not remove)
├── CLAUDE.md              # This file
├── README.md              # Repo description
├── index.html             # Landing page — links to all courses
│
│   Topological Deep Learning Course (live)
├── tdl-index.html         # Course table of contents
├── tdl-01-graphs.html     # Ch 1: Graphs, Laplacian, eigendecomposition, spectral theory
├── tdl-02-gnn.html        # Ch 2: Feedforward NN review, GCN layer, full numerical walkthrough
├── tdl-03-edge-signals.html  # Ch 3: B₁₂, discrete curl, Hodge Laplacian L₁
├── tdl-04-face-signals.html  # Ch 4: L₂, Betti numbers, complete de Rham complex
├── tdl-05-complexes.html     # Ch 5: Simplicial → Cell → Combinatorial complexes
├── tdl-06-message-passing.html # Ch 6: Cochains, adjacency types, HOMP framework
├── tdl-07-architectures.html   # Ch 7: Hodge theory, architecture zoo (SNN, CWN, CAN, etc.)
├── tdl-08-applications.html    # Ch 8: Thermal physics connection, reading roadmap
│
│   Future courses (placeholders on landing page)
├── [pinns-*.html]         # Physics-Informed Neural Networks (planned)
├── [fno-*.html]           # Fourier Neural Operators (planned)
└── [pde-*.html]           # PDEs for ML (planned)
```

## Design System

All pages share inline CSS (no external stylesheet). Key design tokens:

- **Fonts:** Crimson Pro (body), Source Sans 3 (UI/labels), JetBrains Mono (code/math)
- **Colors:** `--accent: #b44a2f` (red), `--accent2: #2a6b5a` (green), `--accent3: #3a5a8c` (blue), `--accent4: #8b5a8c` (purple), `--bg: #faf8f5`
- **Math:** MathJax 3.2.2 (tex-svg-full), loaded per page via CDN
- **Diagrams:** Inline SVG throughout — no external images
- **Component classes:** `.def-box` (definitions, with `.green`/`.blue`/`.purple` variants), `.insight` (callout boxes with emoji), `.figure` (SVG diagrams with captions), `.math-block` (equations), `.comp-table` (comparison tables), `.steps` (numbered lists)

## The Running Example

The entire TDL course uses **one consistent example** across all chapters:

- **Graph:** 4 vertices (v₀–v₃), 5 edges (a–e), adjacency A is given in Ch 1
- **Simplicial complex:** Same graph + 2 triangular faces σ₁={0,1,2}, σ₂={0,2,3} (added in Ch 3)
- **Edge orientations:** a:0→1, b:0→2, c:1→2, d:0→3, e:2→3
- **Temperature signal:** T = (50, 20, 30, 10)ᵀ used for Laplacian/spectral examples
- **GNN features:** H⁰ = [[1.0,0.2],[0.4,0.8],[0.7,0.3],[0.2,0.9]] used in Ch 2

All matrices (B₁, B₁₂, L₀, L₁, L₂, eigenvalues, Â) have been computed and verified numerically. Maintain consistency if editing.

## Key Reference Papers

1. **Hajij et al. (2022)** — "Topological Deep Learning: Going Beyond Graph Data" — the primary paper this course provides background for
2. **Kipf & Welling (2017)** — "Semi-Supervised Classification with Graph Convolutional Networks" — GCN, covered in Ch 2
3. **Ebli, Defferrard & Spreemann (2020)** — Simplicial Neural Networks
4. **Bodnar et al. (2021)** — Weisfeiler and Leman Go Topological (CW Networks)
5. **Papillon et al. (2023)** — TopoX software suite / TopoModelX

## Navigation Pattern

Each chapter page has:
- Header with breadcrumb: Home / TDL / Chapter N
- Sticky top nav: ← Previous | Chapter N | Next →
- Footer nav: large prev/next cards
- Footer: links back to course home and landing page

The `make_page()` function in the original build script (not in repo) generated these. When adding new chapters, maintain the nav chain by updating prev/next links in adjacent chapters.

## Adding a New TDL Chapter

1. Copy an existing chapter file as template
2. Update: chapter number in header, breadcrumb, nav links
3. Update prev chapter's `next` links to point to new file
4. Update next chapter's `prev` links to point to new file
5. Add entry to `tdl-index.html`
6. Section numbering is globally sequential (Ch 1 = §1–2, Ch 2 = §3–8, Ch 3 = §9–14, etc.)

## Adding a New Course

1. Create `[topic]-index.html` (copy `tdl-index.html` as template)
2. Create chapter files: `[topic]-01-*.html`, `[topic]-02-*.html`, etc.
3. Add a card to `index.html` landing page (change placeholder from `.planned` to `.featured`)
4. Follow same design system — inline CSS, same fonts/colors, SVG diagrams, MathJax

## Planned Courses

From `index.html` placeholders (in rough priority order):

| Course | Prefix | Status |
|--------|--------|--------|
| Physics-Informed Neural Networks | `pinns-` | Planned |
| Fourier Neural Operators | `fno-` | Planned |
| DeepONet & Neural Operator Theory | `deeponet-` | Planned |
| PDEs for Machine Learning | `pde-` | Planned |
| Optimization for Deep Learning | `optim-` | Planned |
| Autodiff & Backprop from Scratch | `autodiff-` | Planned |
| Molecular Dynamics | `md-` | Planned |
| Computational Electromagnetics | `cem-` | Planned |

## Context

This site supports self-study at the intersection of physics, mathematics, and machine learning. The author has a Physics PhD (Brandeis) and works on AI-driven infrastructure inspection using UAV thermal imagery and spatiotemporal neural networks. Content should maintain physics intuition alongside mathematical rigor, with worked numerical examples wherever possible.

## Deployment

Push to `main` branch → GitHub Pages auto-deploys in ~60 seconds. No build step needed.
