## Optimization Model (MILP)

This section describes the mathematical formulation of the **Mixed Integer Linear Programming (MILP)** model used to optimize the workforce scheduling.

### 1. Problem Description
The goal is to determine the optimal assignment of shifts for a set of employees over a specific time horizon. The model aims to cover customer demand as accurately as possible, balancing operational requirements with labor regulations and minimizing the service gap (understaffing).

### 2. Decision Variables
The model utilizes binary and integer variables to represent assignment decisions:
* $X_{i,j}$: Binary variable that equals 1 if employee $i$ is assigned to shift $j$, and 0 otherwise.
* $S_t$: Integer variable representing the capacity shortage (understaffing) at time slot $t$.
* $Y_{i,t}$: Binary variable indicating if employee $i$ is working during time slot $t$.

### 3. Objective Function
The objective is to **minimize the total operational shortage** throughout the day:

$$\min Z = \sum_{t \in T} S_t$$

This function ensures that the difference between the required demand and the assigned capacity is as small as possible.

### 4. Constraints
The following constraints ensure the feasibility of the schedule according to labor laws and company policies:
* **Demand Coverage:** The total assigned capacity plus the shortage must equal or exceed the hourly demand for every time slot $t$.
* **Shift Assignment:** Each employee can be assigned to at most one shift per day.
* **Workload Limits:** Total working hours per employee must comply with contract types (Full-time: 8 hours; Part-time: 4 hours).
* **Mandatory Breaks:** Shifts longer than 4 hours must include a mandatory lunch break within a specific time window.
* **Shift Continuity:** Once an employee starts a shift, the working hours must be consecutive (excluding the designated break).
