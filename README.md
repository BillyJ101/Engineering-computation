# Thermal Protection System Simulation & Optimisation

A modular finite-difference solver for transient heat conduction, extended to optimise multi-layer thermal protection systems under damaged and undamaged conditions.

---

## Key Features

- Modular solver architecture supporting multiple differencing methods:
  - Forward Euler
  - Backward Euler
  - Crank–Nicolson
  - Dufort–Frankel
- Time-dependent boundary condition handling via interpolation
- Config-driven simulation pipeline
- Multi-layer material modelling with spatially varying properties
- Gradient-based optimisation of layer thickness under constraints
- Scenario-based evaluation
- Visualisation tools

---

## Architecture Overview

The project follows a modular, extensible architecture separating configuration, physics, and optimisation logic.

### Core Components

- `SimulationConfig`  
  Defines all simulation parameters

- `dispatchSolver`  
  Routes execution to the selected numerical method and decouples solver selection from implementation

- Solver implementations  
  Independent modules for each scheme with shared input/output structure

- `BoundaryCondition`  
  Handles time-dependent temperature data via interpolation

- Material system  
  Supports both homogeneous and layered materials and maps physical properties onto the spatial grid

- Extension module  
  - `evaluateDesign`: computes objective across scenarios  
  - `computeGradient`: numerical gradient estimation  
  - `optimiseThicknesses`: constrained gradient descent loop  

---

## Methodology

The system solves the 1D transient heat equation using finite difference methods:

- Spatial discretisation over the tile thickness  
- Time integration via selectable numerical schemes  
- Stability and accuracy vary by method  

Material properties may vary spatially, allowing layered composites to be modelled.

Boundary conditions are time-dependent and interpolated onto the simulation time grid.

---

## Optimisation Extension

The tile is modelled as a three-layer composite with fixed total thickness.

### Goal

Minimise peak inner surface temperature across different damage scenarios.

### Approach

- Map layer thicknesses onto the simulation grid  
- Evaluate performance using full transient simulation  
- Estimate gradients via finite differences  
- Apply gradient descent with constraints:
  - Fixed total thickness  
  - Minimum manufacturable layer thickness  

This transforms the problem into a constrained design optimisation task.
