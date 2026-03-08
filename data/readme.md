## Data Description
The dataset used in this project originates from the **Datathon 2023 – Etapa 2**.

### Source
The raw information was provided as a spreadsheet containing customer demand recorded every 15 minutes.
* **Primary Data Source:** `Dataton 2023 Etapa 2.xlsx`

### Data Processing
To improve the efficiency and manageability of the optimization model, a temporal aggregation was applied to the raw dataset.

* **Temporal Aggregation:** The original 15-minute intervals were consolidated into one-hour blocks by calculating the average demand of the four subintervals within each hour.
* **Mathematical Representation:** $t \in T = \{8:00, 9:00, 10:00, \dots, 16:00\}$.

### Processed Hourly Demand
This processed demand serves as the primary input for the scheduling optimization model to determine the required number of employees per time slot.

| Time Slot (t) | Average Demand (Day 1) | Average Demand (Day 2) |
| :--- | :---: | :---: |
| **8:00 – 9:00** | 5 | 9 |
| **9:00 – 10:00** | 6 | 9 |
| **10:00 – 11:00** | 5 | 7 |
| **11:00 – 12:00** | 4 | 5 |
| **12:00 – 13:00** | 3 | 9 |
| **13:00 – 14:00** | 3 | 5 |
| **14:00 – 15:00** | 4 | 8 |
| **15:00 – 16:00** | 3 | 7 |

### Purpose
The processed demand values are critical for the **Mixed Integer Linear Programming (MILP)** model to balance operational capacity against actual customer requirements, ensuring minimal understaffing during peak hours.
