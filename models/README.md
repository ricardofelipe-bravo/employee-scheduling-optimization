## Optimization Model (MILP)

This section describes the mathematical formulation of the **Mixed Integer Linear Programming (MILP)** model used to optimize workforce scheduling.

### 1. Problem Description
The project addresses a shift-scheduling problem where the objective is to assign work states (Working, Lunch, or Off) to a group of employees. The model balances operational requirements with labor regulations, such as mandatory lunch windows and contract types, while minimizing the service gap.

### 2. Decision Variables
The model utilizes binary variables to represent the state of each employee $e$ at each time slot $t$ on day $d$:
* $x_{e,t,d} = 1$ if employee $e$ is **working** at time $t$ on day $d$; $0$ otherwise.
* $y_{e,t,d} = 1$ if employee $e$ is **at lunch** at time $t$ on day $d$; $0$ otherwise.
* $z_{e,t,d} = 1$ if employee $e$ is **off (nothing)** at time $t$ on day $d$; $0$ otherwise.
* $s_{t,d}^+$: Integer variable representing the **understaffing shortage** at time $t$ on day $d$.

### 3. Objective Function
The objective is to **minimize the total operational shortage** throughout the scheduled horizon:

$$\min Z = \sum_{d \in D} \sum_{t \in T} s_{t,d}^+$$

This function ensures that the difference between the required demand and the assigned capacity is minimized for every time slot.

### 4. Constraints
The following constraints define the feasible region of the schedule:

* **State Uniqueness:** Each employee must be in exactly one state per time slot:
  $$x_{e,t,d} + y_{e,t,d} + z_{e,t,d} = 1, \quad \forall e, t, d$$

* **Demand Coverage:** The sum of working employees plus the shortage must meet or exceed the demand $D_{t,d}$:
  $$\sum_{e \in E} x_{e,t,d} + s_{t,d}^+ \geq D_{t,d}, \quad \forall t, d$$

* **Workload Duration:** Total working hours must match the contract type $C_e$ (e.g., 6 hours for Full-time):
  $$\sum_{t \in T} x_{e,t,d} = C_e, \quad \forall e, d$$

* **Mandatory Lunch:** Every employee must have exactly one hour of lunch per day (except for specific part-time contracts):
  $$\sum_{t \in T} y_{e,t,d} = 1, \quad \forall e, d$$

* **Lunch Window:** Lunch must occur within the allowed interval (e.g., between 11:00 and 14:00):
  $$y_{e,t,d} = 0 \quad \text{if } t \notin [T_{start}, T_{end}]$$

* **Shift Continuity:** Once an employee starts working, they cannot return to the "Off" state until the workday ends:
  $$z_{e,t,d} \geq z_{e,t+1,d} \quad \text{(at the start of the day)}$$
  $$x_{e,t,d} + y_{e,t,d} \geq x_{e,t-1,d} + y_{e,t-1,d} \quad \text{(during the shift)}$$

* **Minimum Staffing:** At least one employee must be working if there is registered demand:
  $$\sum_{e \in E} x_{e,t,d} \geq 1 \quad \text{if } D_{t,d} \geq 1$$
