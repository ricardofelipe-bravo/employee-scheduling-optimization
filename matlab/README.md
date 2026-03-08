# MATLAB Implementations

This folder contains the MATLAB scripts used to solve the workforce scheduling optimization problem using two different approaches: a heuristic method and an exact optimization method.

## Implemented Methods

### Genetic Algorithm — `Optimizacion_turnos_ga.mlx`

A metaheuristic inspired by biological evolution. The algorithm operates on a population of **300 individuals** evolving for up to **400 generations** through tournament selection, Laplace crossover, and mutation.

The objective function evaluates the **service shortage** directly as a nonlinear function over **296 binary/integer decision variables**, avoiding the need for a reformulation of the model.

Since this is a heuristic approach, it **does not guarantee the global optimum**, but it is capable of exploring high-dimensional discrete search spaces with considerable flexibility.

### Mixed Integer Linear Programming — `Optimizacion_turnos_intlinprog.mlx`

An exact optimization method based on **linear relaxation and branch-and-bound**, solved using the **HiGHS solver** through MATLAB's `intlinprog`.

To express the objective function in linear form, **16 auxiliary shortage variables** (sh(t,d)) are introduced, satisfying:

sh(t,d) ≥ demand(t,d) − Σ x(e,t,d)

This reformulation converts the problem into a **Mixed Integer Linear Programming (MILP)** model with **312 variables**.

Unlike the genetic algorithm, this approach **guarantees the global optimal solution** and typically finds it in a significantly shorter computation time.

## How to Run

1. Open MATLAB.
2. Load the desired script.
3. Run the optimization model.

Available scripts:

* `Optimizacion_turnos_ga.mlx`
* `Optimizacion_turnos_intlinprog.mlx`

## Requirements

* MATLAB **R2021a or later**
* **Optimization Toolbox**
* **Global Optimization Toolbox** (required for the Genetic Algorithm)
