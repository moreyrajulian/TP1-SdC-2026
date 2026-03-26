# TP1-SdC-2026: Performance y Rendimiento de Computadoras
Sistemas de Computación 2026 - FCEFyN

## Descripción

Este trabajo práctico tiene como objetivo estudiar el rendimiento de sistemas de cómputo desde dos perspectivas: la evaluación de hardware mediante benchmarks de terceros, y la medición de performance del propio código mediante herramientas de profiling.

## Marco Teórico

El **rendimiento de un procesador** está directamente relacionado con su frecuencia de operación. La relación fundamental es:

```
Tiempo = Ciclos / Frecuencia
```

Esto implica que, ante una misma tarea, duplicar la frecuencia reduce el tiempo de ejecución a la mitad.

Un **benchmark** es una prueba estandarizada que permite medir y comparar el rendimiento de distintos sistemas bajo condiciones controladas y reproducibles. Plataformas como [OpenBenchmarking.org](https://openbenchmarking.org) recopilan resultados reales de usuarios para facilitar estas comparaciones.

El **time profiling** es una técnica que permite identificar qué funciones de un programa consumen más tiempo de CPU, ayudando a detectar cuellos de botella y optimizar el código.

## Contenido del TP

- Análisis y comparación de benchmarks para distintas tareas de cómputo.
- Comparación de procesadores (Intel Core i5-13600K, AMD Ryzen 9 5900X y 7950X) en la compilación del Kernel de Linux usando OpenBenchmarking.
- Práctica con ESP32: medición del tiempo de ejecución de operaciones con enteros y flotantes variando la frecuencia del procesador.
- Aplicación de time profiling sobre código propio y análisis de resultados.
