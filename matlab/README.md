# Implementaciones en MATLAB

Esta carpeta contiene los scripts utilizados para resolver el problema de programación de turnos mediante dos enfoques de optimización: un método heurístico y un método exacto.

## Métodos utilizados

### Algoritmo Genético — `Optimizacion_turnos_ga.mlx`

Metaheurística inspirada en la evolución biológica. Opera sobre una población de **300 individuos** que evolucionan durante hasta **400 generaciones** mediante selección por torneo, cruce de Laplace y mutación.

La función objetivo evalúa directamente el **desabastecimiento (shortage)** de forma no lineal sobre **296 variables binarias/enteras**, sin necesidad de reformulación del modelo.

Al ser un método heurístico, **no garantiza el óptimo global**, pero es capaz de explorar espacios de búsqueda discretos de gran dimensión con gran flexibilidad.

### Programación Entera Mixta — `Optimizacion_turnos_intlinprog.mlx`

Método exacto basado en **relajación lineal y branch-and-bound**, resuelto con el solver **HiGHS** a través de `intlinprog`.

Para expresar la función objetivo como una forma lineal, se incorporan **16 variables auxiliares de shortage** (sh(t,d)) que satisfacen:

[
sh(t,d) \geq demanda(t,d) - \sum x_{e,t,d}
]

Esto convierte el problema en un modelo de **312 variables**, formulado como un problema de programación entera mixta.

A diferencia del algoritmo genético, este método **garantiza la solución óptima global** y lo hace en una fracción del tiempo de cómputo.

## Cómo ejecutar

1. Abrir MATLAB.
2. Cargar el archivo correspondiente al método deseado.
3. Ejecutar el script principal.

Archivos disponibles:

* `Optimizacion_turnos_ga.mlx`
* `Optimizacion_turnos_intlinprog.mlx`

## Requisitos

* MATLAB **R2021a o superior**
* **Optimization Toolbox**
* **Global Optimization Toolbox** (requerida para el algoritmo genético)
