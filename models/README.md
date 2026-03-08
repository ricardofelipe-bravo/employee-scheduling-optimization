# Optimization Model (MILP)

This section presents the mathematical formulation of the **Mixed Integer Linear Programming (MILP)** model used to optimize workforce scheduling.

## 1. Problem Description

The project addresses a workforce shift-scheduling problem where the objective is to assign work states (Working, Lunch, or Off) to a set of employees while satisfying operational demand and labor regulations.

The model ensures coverage of service demand while respecting constraints such as contract duration, lunch breaks, and scheduling continuity.

## 2. Sets

* (E): set of employees
* (T): set of time slots
* (D): set of days in the planning horizon

## 3. Decision Variables

Binary variables are used to represent the state of each employee (e) at time slot (t) on day (d):

* (x_{e,t,d} = 1) if employee (e) is **working** at time (t) on day (d); 0 otherwise.
* (y_{e,t,d} = 1) if employee (e) is **at lunch** at time (t) on day (d); 0 otherwise.
* (z_{e,t,d} = 1) if employee (e) is **off (idle)** at time (t) on day (d); 0 otherwise.

Additional variable:

* (s_{t,d}^+): integer variable representing the **understaffing shortage** at time (t) on day (d).

## 4. Objective Function

The objective is to **minimize the total operational shortage** during the scheduling horizon:

[
\min Z = \sum_{d \in D} \sum_{t \in T} s_{t,d}^+
]

## 5. Constraints

### State Uniqueness

Each employee must be in exactly one state at each time slot:

[
x_{e,t,d} + y_{e,t,d} + z_{e,t,d} = 1, \quad \forall e,t,d
]

### Demand Coverage

The total assigned workforce plus the shortage must meet or exceed the demand (D_{t,d}):

[
\sum_{e \in E} x_{e,t,d} + s_{t,d}^+ \geq D_{t,d}, \quad \forall t,d
]

### Workload Duration

Total working hours must match the contract type (C_e):

[
\sum_{t \in T} x_{e,t,d} = C_e, \quad \forall e,d
]

### Mandatory Lunch

Each employee must have exactly one lunch period per day:

[
\sum_{t \in T} y_{e,t,d} = 1, \quad \forall e,d
]

### Lunch Window

Lunch must occur within the allowed interval:

[
y_{e,t,d} = 0 \quad \text{if } t \notin [11:00, 14:00]
]

### Shift Continuity

Once an employee starts working, they cannot return to the "Off" state before finishing their shift (except for lunch):

[
x_{e,t,d} + y_{e,t,d} \geq x_{e,t-1,d} + y_{e,t-1,d}
]

### Minimum Staffing

At least one employee must be working when demand is positive:

[
\sum_{e \in E} x_{e,t,d} \geq 1 \quad \text{if } D_{t,d} \geq 1
]
