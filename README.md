# carlm451.github.io

Self-study notes at the intersection of physics, mathematics, and machine learning.

**Live site:** [https://carlm451.github.io](https://carlm451.github.io)

## What this is

These are my personal study notes, written while working through research papers and textbooks in computational physics and scientific machine learning. Each "course" is a guided breakdown of one or more papers, with worked numerical examples, SVG diagrams, and enough background math to follow the arguments from scratch.

**This is a work in progress.** Courses are added and revised as I study. Some are complete; others are in early stages or still planned. Errors are mine.

**Credit where it's due.** The core mathematics, algorithms, and scientific ideas in these notes belong to the authors of the cited papers. My contribution is the pedagogical scaffolding: the ordering, the worked examples, the diagrams, and the connecting narrative. Each course page cites its primary source paper.

## Live courses

### Topological Deep Learning

Background for reading Hajij et al., "Topological Deep Learning: Going Beyond Graph Data" (2022). Eight chapters in three parts, covering graphs through simplicial/cell/combinatorial complexes to higher-order message passing and the architecture zoo. Includes a running example (4-vertex graph with 2 triangular faces) carried through every chapter with fully verified matrices.

[Course home](https://carlm451.github.io/tdl/tdl-index.html)

### Variational Autoregressive Networks

Background for reading Wu, Wang & Zhang, "Solving Statistical Mechanics Using Variational Autoregressive Networks" (2019). Two chapters covering variational free energy, naive mean field theory on the 1D and 2D Ising model, autoregressive decomposition, and the one-layer VAN architecture with training loop.

[Course home](https://carlm451.github.io/van/van-index.html)

### Fourier Neural Operators

Background for reading Li et al., "Fourier Neural Operator for Parametric Partial Differential Equations" (2020). Seven chapters in four parts: operator learning and the neural operator architecture (Part I), the Fourier-space parameterization (Part II), benchmarks on Burgers/Darcy/Navier-Stokes (Part III), and an application to ultrasonic nondestructive testing with angle beam crack detection (Part IV). Darcy flow running example throughout Parts I-III; parametric crack geometry running example in Part IV.

[Course home](https://carlm451.github.io/fno/fno-index.html)

## Planned courses

- Physics-Informed Neural Networks (PINNs)
- DeepONet & Neural Operator Theory
- PDEs for Machine Learning
- Optimization for Deep Learning
- Autodiff & Backpropagation from Scratch
- Molecular Dynamics Simulations
- Computational Electromagnetics

## About

Static HTML site hosted on GitHub Pages. No build tools, no frameworks. MathJax for equations, inline SVG for diagrams, one shared CSS stylesheet.
