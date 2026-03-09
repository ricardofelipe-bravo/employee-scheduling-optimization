# employee-scheduling-optimization

Optimization model for employee shift scheduling in a banking branch, implemented using two mathematical optimization techniques in MATLAB.

The goal is to assign employees to hourly time slots across a planning horizon while minimizing service shortages, respecting labor contracts, lunch break constraints, and continuous shift requirements.

---

## Methods

Two approaches were implemented and compared:

- **Genetic Algorithm (`ga`)** — Heuristic metaheuristic that evolves a population of candidate schedules over multiple generations. Flexible and applicable to non-linear problems, but does not guarantee the global optimum.
- **Mixed Integer Linear Programming (`intlinprog`)** — Exact solver based on Branch & Bound. Guarantees the global optimum and is significantly faster for problems with linear structure.

Both methods converged to the same optimal solution, validating the model formulation.

---

## Repository Structure

```
employee-scheduling-optimization/
│
├── data/        # Input datasets used by the model
├── models/      # Mathematical formulation of the optimization problem
├── matlab/      # MATLAB implementations (GA and intlinprog)
├── figures/     # Visualizations of optimization results
├── results/     # Output schedules and shortage summaries
└── README.md
```

---

## Data Source

The dataset is based on **Datatón 2023 – Etapa 2** (Bancolombia), where service demand was originally recorded at 15-minute intervals and aggregated into hourly time slots for this study.

---

## Author

Ricardo Felipe Bravo Arevalo  
Departamento de Ingeniería Electrónica — Universidad de Nariño
