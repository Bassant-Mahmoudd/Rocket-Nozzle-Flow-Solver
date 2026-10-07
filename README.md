# 1D Compressible Flow Solver for Convergent-Divergent Nozzles

## Overview
This repository contains a Python-based computational tool for analyzing 1D isentropic, choked flow through a convergent-divergent rocket nozzle. Rather than relying on pre-built optimization libraries, this project implements a custom **Newton-Raphson root-finding algorithm** to iteratively solve the highly non-linear Area-Mach relation. 

The solver calculates the local Mach number, static pressure, and static temperature distributions across the nozzle's axial length and visualizes the aerodynamic gradients alongside the physical nozzle contour. 

## Mathematical Formulation & Numerical Methods

### 1. Nozzle Geometry
The internal aerodynamic contour is defined by the following polynomial, extending 10 inches upstream and downstream of the throat ($x=0$):
$$r(x) = 1 + 0.435\vert{}x\vert{} - 0.00365x^2 - 0.000659\vert{}x\vert{}^3$$

### 2. The Newton-Raphson Mach Solver
For choked, isentropic flow, the local Mach number ($M$) is implicitly coupled to the geometric area ratio ($A/A^*$) via:
$$\left(\frac{A}{A^*}\right)^2 = \frac{1}{M^2} \left[ \frac{2}{\gamma+1} \left( 1 + \frac{\gamma-1}{2} M^2 \right) \right]^{\frac{\gamma+1}{\gamma-1}}$$

Because this equation cannot be solved analytically for $M$, the script defines an objective function $F(M) - (A/A^*)^2 = 0$. A custom Newton-Raphson numerical loop iteratively updates the Mach guess using the analytical derivative $F'(M)$ until the convergence tolerance ($10^{-8}$) is met. The algorithm dynamically branches its initial guess depending on whether the flow is in the subsonic (convergent) or supersonic (divergent) regime.

### 3. Isentropic Flow Properties
Once the Mach profile is established, local static temperature ($T$) and static pressure ($P$) are resolved using isentropic stagnation relations driven by chamber conditions ($P_c = 500 \text{ psi}$, $T_c = 5000^\circ\text{R}$)[cite: 28, 32]:
$$T = T_c \left(1 + \frac{\gamma-1}{2}M^2\right)^{-1} \quad \text{and} \quad P = P_c \left(1 + \frac{\gamma-1}{2}M^2\right)^{-\frac{\gamma}{\gamma-1}}$$

## Repository Contents
* `Nozzle_Flow_Solver.ipynb`: The main Jupyter Notebook containing the mathematical documentation, the Newton-Raphson solver, and the `matplotlib` visualization architecture.

## Flow Visualization Dashboard

