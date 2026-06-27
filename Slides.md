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

---

layout: section

# Part I
## Schrödinger–Poisson Informed Neural Networks (SPINN)

Learning FDM directly from the governing equations.

---

# Coordinate-Based PINNs

<div class="grid grid-cols-2 gap-8">

<div>

Input coordinates

- $x$
- $y$
- $z$
- $t$

</div>

<div>

Neural network predicts

- Real part $\Re(\psi)$
- Imaginary part $\Im(\psi)$
- Gravitational potential $\Phi$

</div>

</div>

<v-click>

The network represents the continuous solution of the Schrödinger–Poisson system.

</v-click>

---

# Automatic Differentiation

Instead of finite differences,

the derivatives are computed analytically through the computational graph.

$$
\frac{\partial\psi}{\partial t},
\quad
\nabla\psi,
\quad
\nabla^2\psi
$$

are obtained using automatic differentiation.

Advantages

- No numerical stencils
- Continuous representation
- Machine precision gradients

---

# Physics-Informed Loss

The training objective combines several constraints.

$$
\mathcal{L}
=
\mathcal{L}_{SP}
+
\mathcal{L}_{IC}
+
\mathcal{L}_{BC}
$$

where

- Schrödinger residual
- Poisson residual
- Initial conditions
- Boundary conditions

All residuals are minimized simultaneously during training.

---

# Training Procedure

<div class="grid grid-cols-2 gap-8">

<div>

Network

- MLP
- Coordinate input
- Continuous prediction

</div>

<div>

Optimization

- Automatic differentiation
- Backpropagation
- Adam optimizer
- Physics-informed loss

</div>

</div>

<v-click>

No simulation snapshots are required beyond the initial conditions.

</v-click>

---

# Density Reconstruction

The physical density follows directly from

$$
\rho = |\psi|^2
$$

The learned wavefunction naturally captures

- interference
- diffraction
- wave-like evolution

without explicitly programming these effects.

---

# SPINN Results

The network accurately reproduces

- one-dimensional simulations
- three-dimensional simulations

Agreement is obtained using only

- governing equations
- initial conditions

No supervised density labels are required.

---

# Key Result

The network learns FDM dynamics

without simulation targets.

Instead,

the governing physics acts as the supervision.

<v-click>

This demonstrates that PINNs can recover non-linear wave dynamics from first principles.

</v-click>

---

layout: section

# From Idealized Systems
## to Cosmological Simulations

---

# The Cosmological Challenge

Real cosmological simulations introduce

- expanding background
- evolving density field
- many realizations
- large computational domains

Direct PINN training becomes significantly more difficult.

---

# Two Complementary Directions

Instead of relying on a single model,

we explore two approaches.

### Super-resolution

Recover unresolved small-scale structure.

### Evolution model

Predict future cosmological fields directly.

Both approaches enforce physical consistency during learning.

---

# Super-Resolution

Goal

Learn

$$
\mathrm{Low\ Resolution}
\rightarrow
\mathrm{High\ Resolution}
$$

Applications

- Recover wave interference
- Sharpen filaments
- Increase effective simulation resolution

---

# Cosmological Training Data

Training simulations are generated using **Jaxion**.

Characteristics

- Box size: **1 Mpc/h**
- Multiple realizations
- Redshifts from

$$
z = 127
\rightarrow
z = 0
$$

Fine timesteps are essential for preserving wave dynamics.

---

# Data Preparation

For different tasks

### Evolution

$$
128^3
\rightarrow
64^3
$$

### Super-resolution

$$
32^3
\rightarrow
128^3
$$

The dataset spans many independent cosmological realizations.

---

# Cosmo-SPINN

Cosmological equations are rewritten using

- comoving coordinates
- super-comoving time

This improves numerical stability while preserving the physical evolution.

The neural network now predicts

- wavefunction
- gravitational potential

across cosmic time.

---

layout: section

# Part II
## Physics-Informed Generative Models

Learning cosmological structure beyond the simulation resolution.

---

# Motivation

Direct Schrödinger–Poisson simulations remain computationally expensive.

Instead, learn the mapping

$$
\text{Low Resolution}
\longrightarrow
\text{High Resolution}
$$

while enforcing the governing physics.

<v-click>

Goal:

Generate physically consistent FDM realizations at a fraction of the computational cost.

</v-click>

---

# Physics-Informed UNet

<div class="grid grid-cols-2 gap-8">

<div>

Input

- Low-resolution density
- Wave function
- Cosmological time

</div>

<div>

Output

- High-resolution density
- Fine-scale interference
- Filaments
- Solitonic cores

</div>

</div>

Training is guided by both

- simulation data
- physical residuals

---

# Physics Constraints

Instead of minimizing only reconstruction error,

the network is penalized whenever it violates

- Schrödinger equation
- Poisson equation

Loss

$$
\mathcal{L}
=
\mathcal{L}_{data}
+
\lambda
\mathcal{L}_{physics}
$$

Physics acts as a regularizer.

---

# Two Physics Objectives

We investigate two formulations.

### SP Residuals

Directly minimize the Schrödinger–Poisson equations.

Advantages

- Strong physical consistency
- Better recovery of fine structures

---

### DKD Residuals

Physics based on

Drift → Kick → Drift

time integration.

Advantages

- Stable optimization
- Better long-term behaviour
- Easier convergence

---

layout: image-right

image: /figures/super_resolution_example.png

# Super-Resolution Results

Recovered structures show

- sharper filaments
- denser halo cores
- richer interference patterns

The reconstructed fields closely match the high-resolution simulations.

---

layout: image-right

image: /figures/sp_residual_results.png

# SP Residual Training

Characteristics

✔ Sharp small-scale features

✔ Accurate high-$k$ power

✔ Closest agreement with target simulation

Trade-off

Higher prediction variance.

---

layout: image-right

image: /figures/dkd_results.png

# DKD Residual Training

Characteristics

✔ Stable optimization

✔ Smooth density field

✔ Excellent large-scale agreement

Trade-off

Slight suppression of the smallest scales.

---

# Quantitative Comparison

<div class="grid grid-cols-2 gap-8">

<div>

### SP Residual

- Better small-scale recovery
- Higher-frequency structures
- Larger variance

</div>

<div>

### DKD Residual

- Lower variance
- More stable
- Better global reconstruction

</div>

</div>

---

layout: image-right

image: /figures/power_spectrum.png

# Matter Power Spectrum

Across unseen realizations,

both approaches recover

- large-scale power
- non-linear evolution
- physically consistent spectra

SP performs best at

high spatial frequencies.

---

# Main Result

Physics-informed supervision substantially improves

- reconstruction quality
- physical consistency
- generalization

without requiring substantially larger models.

---

layout: section

# Neural Operators

Learning the evolution operator itself.

---

# From Neural Networks

Neural networks learn

$$
f(x)
$$

a mapping between vectors.

---

# To Neural Operators

Neural operators learn

$$
\mathcal{G}(u)
$$

a mapping

between entire functions.

This enables

- arbitrary spatial resolution
- mesh-independent inference
- direct solution operators

---

# Fourier Neural Operators

Use Fourier transforms to learn

the evolution operator efficiently.

Pipeline

Field

↓

FFT

↓

Spectral Convolution

↓

Inverse FFT

↓

Predicted Next State

---

# Why FNO?

Advantages

- Resolution independent
- Fast inference
- Long-range interactions
- Naturally suited for PDEs

Perfect for cosmological simulations.

---

layout: image-right

image: /figures/fno_prediction.png

# DKD Predictor

Traditional simulations evolve

Kick

↓

Drift

↓

Kick

The FNO learns this operator directly.

Instead of integrating every timestep,

the network predicts the evolution.

---

layout: image-right

image: /figures/fno_results.png

# Autoregressive Prediction

The network predicts

multiple future timesteps

from previous outputs.

Tested on completely unseen realizations,

prediction errors remain below

approximately **6%** over hundreds of timesteps.

---

# Why This Matters

The learned operator provides

- orders-of-magnitude faster inference
- new cosmological realizations
- differentiable simulations
- scalable surrogate models

for future FDM studies.

---

layout: section

# Conclusions

## Physics-Informed Machine Learning for Fuzzy Dark Matter

---

# Main Contributions

<div class="grid grid-cols-2 gap-8">

<div>

### Physics-Informed PINNs

- Solve Schrödinger–Poisson directly
- Continuous wavefunction representation
- No simulation labels required
- Accurate 1D and 3D FDM dynamics

</div>

<div>

### Physics-Informed Generative Models

- High-quality super-resolution
- Physically consistent reconstructions
- Improved small-scale structure
- Generalize across realizations

</div>

</div>

---

# Neural Operators

Fourier Neural Operators provide

- Fast surrogate simulations
- Resolution-independent inference
- Long-term autoregressive prediction
- Efficient cosmological evolution

<v-click>

Towards practical machine-learning cosmological solvers.

</v-click>

---

# Take Home Message

Physics should not simply generate training data.

<v-click>

Physics should become part of the learning algorithm.

</v-click>

<br>

This enables machine learning models that are

- more accurate
- more data efficient
- more physically reliable

for cosmological simulations.

---

layout: center

# Current Status

| Method | Status |
|:-------|:------:|
| PINNs for Schrödinger–Poisson | ✅ Working |
| Cosmo-SPINN | ✅ Working |
| Physics-informed UNet | ✅ Working |
| Fourier Neural Operators | ✅ Working |
| Large cosmological boxes | 🚧 In Progress |

---

# Ongoing Work

Current directions

- Scale PINNs to larger cosmological volumes
- Longer temporal evolution
- Physics-informed Neural Operators
- Multi-resolution cosmological simulations
- Recover core–halo relations
- Extend to multi-field FDM

---

# Long-Term Vision

Develop machine-learning solvers capable of

- replacing expensive Schrödinger–Poisson simulations
- generating cosmological FDM boxes on demand
- accelerating Bayesian inference
- enabling parameter exploration

without sacrificing physical fidelity.

---

layout: statement

# Thank You

## Questions?

Ashutosh Kumar Mishra

EPFL

📧 ashutosh.mishra@epfl.ch

---

layout: center

# Resources

### Related Work

- Mishra & Tolley (ApJ, 2025)
- Cosmo-SPINN
- Physics-Informed UNet
- Fourier Neural Operators
- Jaxion
- JaxPM

QR code to arXiv

---

layout: section

# Appendix

---

# Multiple Initial Conditions

- Training across multiple realizations
- Generalization performance
- Remaining small-scale hallucinations

---

# Data Generation Pipeline

AxionCAMB

↓

Linear Matter Power Spectrum

↓

JaxPM

↓

LPT Displacements

↓

Wavefunction Construction

↓

Jaxion

↓

Training Dataset

---

# Numerical Schrödinger–Poisson Solver

Second-order spectral method

Kick

↓

Drift

↓

Kick

Key properties

- Unitary evolution
- Spectral derivatives
- Stable long integrations

---

# Additional Results

- Extra super-resolution examples
- Additional FNO rollouts
- Failure cases
- Ablation studies
- Hyperparameter sensitivity



