## Optimization Model (MILP)

This section presents the mathematical formulation of the **Mixed Integer Linear Programming (MILP)** model developed to optimize workforce scheduling.

### 1. Problem Description
The project addresses a shift-scheduling problem where the objective is to assign specific work states to a group of employees. The model balances operational requirements with labor regulations, such as mandatory lunch windows and contract types, while minimizing the service gap (understaffing).

### 2. Sets and Indices
* $e \in E$: Set of employees (e.g., $E = \{1, 2, 3\}$).
* $t \in T$: Set of hourly time slots (8:00 AM to 4:00 PM).
* $d \in D$: Planning horizon (days).

### 3. Decision Variables
Binary variables represent the state of each employee $e$ at time $t$ on day $d$:
* $x_{e,t,d} = 1$: If employee $e$ is **working**.
* $y_{e,t,d} = 1$: If employee $e$ is **at lunch**.
* $z_{e,t,d} = 1$: If employee $e$ is **off (nothing)**.
* $s_{t,d}^+$: Integer variable representing the **understaffing shortage** at time $t$.

### 4. Objective Function
The objective is to **minimize the total operational shortage** throughout the scheduled horizon:

$$\min Z = \sum_{d \in D} \sum_{t \in T} s_{t,d}^+$$



### 5. Constraints
The following constraints define the feasible region of the schedule based on labor laws and operational policies:

* **State Uniqueness:** Each employee must be in exactly one state per time slot:
  $$x_{e,t,d} + y_{e,t,d} + z_{e,t,d} = 1, \quad \forall e, t, d$$

* **Demand Coverage:** The total assigned capacity plus the shortage must meet or exceed the hourly demand $D_{t,d}$:
  $$\sum_{e \in E} x_{e,t,d} + s_{t,d}^+ \geq D_{t,d}, \quad \forall t, d$$

* **Workload Duration:** Total working hours must strictly match the contract type $C_e$:
  $$\sum_{t \in T} x_{e,t,d} = C_e, \quad \forall e, d$$

* **Mandatory Lunch:** Each employee must have exactly one hour of lunch (excluding specific part-time contracts):
  $$\sum_{t \in T} y_{e,t,d} = 1, \quad \forall e, d$$

* **Lunch Window:** Lunch is restricted to the interval between 11:00 AM and 2:00 PM:
  $$y_{e,t,d} = 0 \quad \text{if } t \notin [11:00, 14:00]$$

* **Shift Continuity:** Once an employee starts their shift, they cannot return to the "Off" state until the workday is complete:
  $$x_{e,t,d} + y_{e,t,d} \geq x_{e,t-1,d} + y_{e,t-1,d}, \quad \forall t > 1$$

* **Mandatory End State:** To ensure the shift doesn't end prematurely in a "Nothing" state, the last slot must be "Working":
  $$x_{e,T,d} = 1, \quad \forall e, d \quad (\text{if assigned to a shift})$$

* **Minimum Staffing:** At least one employee must be on duty if there is registered demand:
  $$\sum_{e \in E} x_{e,t,d} \geq 1 \quad \text{if } D_{t,d} \geq 1$$
