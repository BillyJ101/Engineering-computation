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
- Config-driven simulation 
- Multi-layer material modelling with spatially varying properties
- Gradient-based optimisation of layer thickness under constraints
- Scenario-based evaluation
- Visualisation tools
- Image-based data extraction pipeline for generating boundary condition inputs
- Constraint-based thickness search using a secant method with manufacturability limits

---

## Architecture Overview

The project follows a modular, extensible architecture separating configuration, physics, and optimisation logic.

### Core Components

- `SimulationConfig`  
  Defines all simulation parameters

- `dispatchSolver`  
  Finds and executes selected numerical method and decouples solver selection from implementation

- Solver implementations  
  Independent modules for each scheme with shared i/o structure

- `BoundaryCondition`  
  Handles time-dependent temperature data via interpolation

- Material system  
  Supports both homogeneous and layered materials and maps physical properties onto the spatial grid

- Extension module  
  - `evaluateDesign`: computes objective across scenarios  
  - `computeGradient`: numerical gradient estimation  
  - `optimiseThicknesses`: constrained gradient descent loop  

- Data extraction pipeline (`dataExtraction/`)  
  Converts temperature–time data from images into numerical input:
  - User-defined axis calibration via point selection  
  - Colour-based filtering to isolate data curve 
  - Pixel-to-physical coordinate static transformation

- Thickness search tool  
  A standalone script that determines the minimum tile thickness satisfying a temperature constraint:
  - Uses a secant method with midpoint fallback for robustness  
  - Evaluates peak inner surface temperature via full simulation  
  - Iteratively converges to the minimum feasible thickness  

---

## Project Structure

```text
└── group75/
    ├── main.py
    ├── runExtension.py
    ├── runSpatialStepStudy.py
    ├── runThicknessStudy.py
    ├── runTimeStepStudy.py
    ├── findMinimumThickness.py
    │
    ├── core/
    │   ├── boundaryCondition.py
    │   ├── grid.py
    │   ├── material.py
    │   ├── simulationConfig.py
    │   └── simulationResult.py
    │
    ├── solvers/
    │   ├── backwardSolver.py
    │   ├── crankNicolsonSolver.py
    │   ├── dispatchSolver.py
    │   ├── dufortFrankelSolver.py
    │   ├── forwardSolver.py
    │   └── tdmSolver.py
    │
    ├── extension/
    │   ├── damageScenarios.py
    │   ├── evaluateDesign.py
    │   ├── gradCalculator.py
    │   ├── materialPropertyBuilder.py
    │   ├── objectiveFunction.py
    │   └── optimiser.py
    │
    ├── plotting/
    │   └── tempuraturePlots.py
    │
    ├── inputOutput/
    │   └── loadBoundaryData.py
    │
    ├── dataExtraction/
    │   └── extractBoundaryData.py
    │
    └── data/
        ├── temp597.npy
        └── temp597.jpg
