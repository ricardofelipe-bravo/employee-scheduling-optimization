
# Optimización de Turnos — Implementaciones MATLAB

Este directorio contiene las implementaciones de dos métodos de optimización aplicados al problema de asignación de turnos de empleados. Ambos modelos comparten la misma formulación matemática y restricciones, diferenciándose únicamente en el algoritmo de resolución.

---

## El modelo

El problema consiste en asignar turnos a 3 empleados (2 de tiempo completo, TC, y 1 de medio tiempo, MT) sobre 8 franjas horarias durante 2 días. Cada empleado tiene un estado por franja: trabajando, almorzando o inactivo. La función objetivo minimiza el desabastecimiento, definido como el número de personas-hora en que la capacidad disponible no alcanza a cubrir la demanda.

Las principales restricciones del modelo son:

- Cada empleado tiene exactamente un estado por franja horaria.
- Los empleados TC trabajan exactamente 6 horas por día; el MT entre 3 y 4 horas.
- La jornada de cada empleado es continua (un único bloque sin interrupciones).
- El empleado MT cubre la ventana de 3 franjas que más reduce el desabastecimiento, calculada en preprocesamiento.
- Los almuerzos de los TC ocurren únicamente en las franjas 4 o 5, no de forma simultánea, y precedidos de al menos 1 hora de trabajo.

---

## Métodos utilizados

### Algoritmo Genético — `Optimizacion_turnos_ga.mlx`

Metaheurística inspirada en la evolución biológica. Opera sobre una población de 300 individuos que evolucionan durante hasta 400 generaciones mediante selección por torneo, cruce de Laplace y mutación. La función objetivo evalúa directamente el desabastecimiento de forma no lineal, sin necesidad de reformulación. Al ser un método heurístico no garantiza el óptimo global, pero es capaz de explorar espacios de búsqueda enteros de gran dimensión (296 variables binarias/enteras).

### Programación Entera Mixta — `Optimizacion_turnos_intlinprog.mlx`

Método exacto basado en relajación lineal y branch & bound, resuelto con el solver HiGHS a través de `intlinprog`. Para poder expresar la función objetivo como una función lineal, se incorporan 16 variables auxiliares de shortage `sh(t,d)` que satisfacen `sh(t,d) >= demanda(t,d) - sum(x_e,t,d)`, convirtiendo el problema en un MILP de 312 variables. Garantiza la solución óptima global.

---

## Resultados

Ambos métodos alcanzan el mismo valor óptimo de **60 personas-hora de desabastecimiento total**, lo que valida la solución del GA frente al óptimo exacto de intlinprog.

| Método | Desabastecimiento total | Tiempo de ejecución |
|---|---|---|
| GA (`ga`) | 60 personas-hora | ~491 seg |
| MILP (`intlinprog`) | 60 personas-hora | ~0.6 seg |

---

## Requisitos

- MATLAB R2021a o superior
- Optimization Toolbox (requerida para ambos métodos)
- Global Optimization Toolbox (requerida para `ga`)
