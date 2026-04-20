# The Milk Powder Project

**Authors:** Bea Cooke, Wenting Gu, Holly Sutcliffe, Christopher Won

**Institution:** University of Auckland — MATHS 399

**Reference:** MINZ Paper (Austral. Mathematical Soc., 2018)

## Overview

This project models how heat transfers through a single 25 kg bag of milk powder under
various temperature conditions, with the goal of predicting optimal storage temperatures
to prevent degradation.

Inspired by the question: *"Can we predict how long we can store milk powders, especially
in elevated temperatures and humidities?"*

The baseline from the MINZ paper: milk powder stored at 25°C with water activity
a_w = 0.225 and 60% relative humidity has a predicted shelf life of **770 days**.
Accounting for up to one year of supply chain transit, the consumer retains roughly
**405 days** to use the product.

## Problem

Temperature is the primary focus — specifically, how it drives:
- **Lactose crystallisation** and browning above a critical threshold
- **Glass transition** (at ~50°C with a_w ≈ 0.22), causing complex inter-particle
  bridging and accelerated degradation
- Heat gradients between the surface and interior of the bag

## Mathematical Model

The project uses the **1D homogeneous heat diffusion equation** (Olver, 2014):

```
∂u/∂t = γ · ∂²u/∂x²
```

where:
- `u(x, t)` = temperature at position x and time t
- `γ` = thermal diffusivity ≈ 0.7 mm²/s (for milk powder)
- `x` ∈ [0, 130 mm] — the width dimension of a standard 25 kg bag (830 × 430 × 130 mm)

## Models & Scenarios

### 1. Neumann Boundary Conditions (Insulated Packaging)
- Zero heat flux at both ends: `u_x(t,0) = u_x(t,130) = 0`
- Temperature converges to equilibrium at **23°C**
- Represents the theoretically optimal storage scenario

### 2. Inhomogeneous Boundary Conditions (Direct Heat Exposure)
- One end held at 40°C (face-down on a heat source), other at 35°C
- Models a surface-exposed bag; converges to a linear equilibrium profile

### 3. Pragmatic Scenario — Auckland to Africa Shipment (~90 days)
- Initial steady-state distribution derived from Auckland storage (outside: 10.5°C, inside: 5°C)
- Heat flux boundary conditions applied using packaging thermal resistance
- Temperature profile generalised as: `u(x) = (ω/δ)(0.130 − x) + p`
- Models the glass transition event at **50°C** (first reached at ~day 36.5)
- Models a **sudden 30°C spike** (as seen at ~day 77 in MINZ data)

## Key Findings

| Condition | Outcome |
|---|---|
| Neumann (insulated) BCs | Optimal — maintains shelf life ≥ 770 days if a_w and humidity held |
| Thickened packaging | Practical — delays glass transition by increasing heat loss through walls |
| Uninsulated / thin packaging | Risk of glass transition at 50°C; accelerated degradation |

**Optimal Condition 1:** Full insulation (Neumann BCs) — theoretically ideal but not
currently achievable with standard multi-wall paper + LDPE bags.

**Optimal Condition 2:** Thickened packaging (especially on sun-facing sides) to
increase heat loss and delay the glass transition temperature — more practical and
manufacturable.

## Implementation

All models were implemented numerically using **R** (Fourier series solutions, 50-term
approximations) and **MATLAB** (3D surface plots via `pdepe`).

## Repository Contents

| File | Description |
|---|---|
| `The Milk Powder Project.pdf` | Full project report with derivations, figures, and R/MATLAB code |

## References

- Chopovda et al. (2018). *Predicting the shelf life of milk powder.* ANZIAM Journal.
- Olver, P.J. (2014). *Introduction to Partial Differential Equations.* Springer.
- Mueller, B. (2010). *Package Optimisation Model.* Massey University.
