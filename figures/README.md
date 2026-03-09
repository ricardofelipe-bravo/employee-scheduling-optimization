# Figures

This folder contains visualizations generated from the optimization results of the workforce scheduling model.

## Description

The figures illustrate the demand coverage and staffing capacity across time slots for each day and each solution method. They allow direct visual interpretation of how well each algorithm satisfies the hourly demand and where shortages occur throughout the planning horizon.

These visualizations were generated directly from the MATLAB optimization scripts.

## Contents

- **Demand vs. Capacity vs. Shortage** — Three-bar plots showing hourly demand, assigned workforce capacity, and staffing shortage side by side, for Day 1 and Day 2 under both the Genetic Algorithm and intlinprog solutions.
- **Optimal Schedule** — Color-coded spreadsheet displaying the state of each employee (working, lunch, idle) per time slot, for both days and both methods.
