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

## Project Structure

```text
project-root/
├── core/
│   ├── boundaryCondition.py
│   ├── grid.py
│   ├── material.py
│   ├── simulationConfig.py
│   └── simulationResult.py
├── solvers/
│   ├── backward.py
│   ├── crankNicolson.py
│   ├── dispatchSolver.py
│   ├── dufortFrankel.py
│   └── forward.py
├── extension/
│   ├── evaluateDesign.py
│   ├── gradCalculator.py
│   └── optimiseThicknesses.py
├── data/
│   └── temp597.npy
├── runSimulation.py
├── runExtension.py
├── thicknessStudy.py
└── README.md
