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
├── [pinns/]               # Physics-Informed Neural Networks (planned)
├── [fno/]                 # Fourier Neural Operators (planned)
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
| Physics-Informed Neural Networks | `pinns/` | `pinns-` | Planned |
| Fourier Neural Operators | `fno/` | `fno-` | Planned |
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
