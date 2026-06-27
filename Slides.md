---
theme: seriph
title: Wave Dark Matter Simulations with Physics-Informed Machine Learning
info: |
  European Astronomical Society 2026
class: text-center
transition: fade
mdc: true
---

# Wave Dark Matter Simulations
## with Physics-Informed Machine Learning

**Ashutosh K. Mishra**  
PhD Student, EPFL

Advisor: **Emma Tolley**

European Astronomical Society Meeting  
July 2026

---

# Motivation

<div class="grid grid-cols-2 gap-6">

<div>

## Small-scale challenges of CDM

- Missing satellites
- Core–cusp problem
- Too-big-to-fail
- Diversity problem

</div>

<div>

Questions:

- Missing baryonic physics?
- Or is dark matter fundamentally different?

</div>

</div>

<v-click>

The nature of dark matter remains an open question.

</v-click>

---

# Beyond Cold Dark Matter

Several alternatives have been proposed:

- Warm Dark Matter (WDM)
- Self-Interacting Dark Matter (SIDM)
- Fuzzy Dark Matter (FDM)

<v-click>

Current observational constraints strongly restrict the viable parameter space.

</v-click>

---

# Fuzzy Dark Matter

Ultra-light scalar particles

$$
m \sim 10^{-22}\ \mathrm{eV}
$$

Key properties

- Non-thermal production
- Bose–Einstein condensate
- Macroscopic de Broglie wavelength
- Quantum pressure suppresses small-scale structure

<v-click>

Quantum effects become important on galactic scales.

</v-click>

---

# Why FDM?

<div class="grid grid-cols-2 gap-8">

<div>

Possible signatures

- Suppressed matter power spectrum
- Solitonic cores
- Wave interference
- Granular density fluctuations

</div>

<div>

Open questions

- Current observational limits
- High-redshift constraints
- Ultra-dwarf galaxies
- Two-field models

</div>

</div>

---

# Governing Equations

The dynamics are described by the

## Schrödinger–Poisson equations

$$
i\hbar\frac{\partial\psi}{\partial t}
=
-\frac{\hbar^2}{2m}\nabla^2\psi
+
m\Phi\psi
$$

$$
\nabla^2\Phi
=
4\pi G(\rho-\bar{\rho})
$$

Alternative formulation:

- Madelung fluid equations
- Quantum pressure term

---

# Why Simulations Are Hard

Requirements

- Resolve Mpc cosmological scales
- Resolve kpc wave structure
- Preserve interference patterns
- Extremely small timesteps

Traditional fluid solvers

❌ Lose interference

Traditional N-body methods

❌ Cannot model wave dynamics

---

# Current State of FDM Simulations

Existing Schrödinger–Poisson solvers

- Accurate
- Extremely expensive
- Limited to small simulation boxes

Typical simulations

- 1–10 Mpc/h
- High computational cost
- Limited parameter exploration

---

# Physics-Informed Machine Learning

Idea

Instead of learning only from data,

**teach the neural network the governing physics.**

Loss function combines

- Data fidelity
- Schrödinger equation
- Poisson equation
- Boundary conditions

---

# Outline

## Part I

Coordinate-based PINNs

- Solve Schrödinger–Poisson directly

## Part II

Physics-informed generative models

- Super-resolution
- Cosmological evolution
- Neural Operators
