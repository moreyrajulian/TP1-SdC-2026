## Selección de Benchmarks

Benchmarks Útiles:

- Cinebench R23/2024: Ideal para renderizado y carga multinúcleo pura.

- C-Ray: Un benchmark de ray-tracing muy ligero que pone a prueba el rendimiento de punto flotante en múltiples hilos.

- Geekbench 6: Para medir rendimiento general (Single-core y Multi-core) en tareas cotidianas.

- PCMark 10: Útil para medir tareas de oficina, navegación web y videoconferencias.

- 7-Zip Benchmark: Para medir velocidad de compresión/descompresión (frecuente en desarrollo).

- Selenium (LibreOffice): Este test automatiza tareas dentro de LibreOffice (como exportar documentos a PDF o procesar hojas de cálculo) para medir el tiempo de respuesta.

- PTS (Phoronix Test Suite) - Build Linux Kernel: Específico para medir potencia de compilación.

- WebRTC Samples: Para medir el rendimiento en videollamadas, se suelen usar herramientas de estrés de WebRTC que simulan múltiples flujos de video y audio para ver cuánto se degrada el CPU.

- Flexible I/O Tester (FIO): La herramienta más precisa para medir la velocidad del SSD o NVMe.

Los benchmarks que representan mejor las tareas que realizamos a diario son los siguientes:

| Tarea diaria                 | Benchmark representativo            |
|-----------------------------|-------------------------------------|
| Navegacion y Office         | PCMark 10 (Essentials)              |
| Compilacion de codigo       | Timed Linux Kernel Compilation      |
| Juegos o graficos           | 3DMark / Unigine Superposition      |
| Gestion de archivos (ZIP)   | 7-Zip Benchmark                     |

# Análisis de Rendimiento: Compilación del Kernel de Linux

## Benchmark utilizado

Para evaluar el rendimiento de los procesadores se utilizó el benchmark **Timed Linux Kernel Compilation 6.8** de la plataforma OpenBenchmarking.org (test `pts/build-linux-kernel-1.16.x`, configuración `defconfig`), basado en 3.493 resultados públicos recopilados desde Marzo de 2024 hasta Febrero de 2026. La métrica utilizada es el tiempo de compilación en segundos, donde un valor menor indica mejor rendimiento.

## Especificaciones de los procesadores

| Procesador | Núcleos/Hilos | TDP | Precio lanzamiento |
|---|---|---|---|
| Intel Core i5-13600K | 14C / 20T | 125W | $320 USD |
| AMD Ryzen 9 5900X 12-Core | 12C / 24T | 105W | $315 USD |
| AMD Ryzen 9 7950X 16-Core | 16C / 32T | 170W | $498 USD |

## Rendimiento en la compilación del Kernel

| Procesador | Tiempo (s) | Percentil | Muestras |
|---|---|---|---|
| Intel Core i5-13600K | 83 ±3 | 52° | 8 |
| AMD Ryzen 9 5900X 12-Core | 97 ±6 | 47° | 36 |
| AMD Ryzen 9 7950X 16-Core | 52 ±3 | 71° | 59 |

Un primer resultado llamativo es que el **i5-13600K supera al Ryzen 9 5900X** a pesar de tener menos núcleos físicos y un precio similar ($320 vs $315). Esto se explica por su arquitectura híbrida Raptor Lake más moderna, que ofrece mayor IPC (instrucciones por ciclo) y frecuencias de boost más altas.

## Aceleración con el Ryzen 9 7950X

Tomando como referencia al **i5-13600K** (el más rápido de los dos primeros):

```
Speedup = 83 / 52 = 1.60x
```

Tomando como referencia al **Ryzen 9 5900X**:

```
Speedup = 97 / 52 = 1.87x
```

El Ryzen 9 7950X termina la compilación en **52 segundos**, siendo **1.60x más rápido** que el i5-13600K y **1.87x más rápido** que el 5900X. Estos valores de speedup son coherentes con el aumento en cantidad de núcleos y la arquitectura Zen 4 del 7950X.

## Eficiencia por núcleo

Para analizar qué procesador aprovecha mejor sus núcleos se calcula el **tiempo por núcleo** (menor es mejor):

| Procesador | Tiempo (s) | Núcleos físicos | Segundos/núcleo |
|---|---|---|---|
| Intel Core i5-13600K | 83 | 14 | **5.93** |
| AMD Ryzen 9 5900X 12-Core | 97 | 12 | **8.08** |
| AMD Ryzen 9 7950X 16-Core | 52 | 16 | **3.25** |

El **i5-13600K hace un uso más eficiente de sus núcleos** entre los dos primeros, obteniendo mejor rendimiento por núcleo que el 5900X. El 7950X lidera en términos absolutos gracias a su combinación de mayor cantidad de núcleos y arquitectura más eficiente.

## Eficiencia energética y económica

Calculando la **energía total consumida por tarea** (Segundos × Watts, menor es mejor):

| Procesador | Tiempo (s) | TDP (W) | Segundos × Watts |
|---|---|---|---|
| Intel Core i5-13600K | 83 | 181 (turbo) | 15.023 |
| AMD Ryzen 9 5900X 12-Core | 97 | 105 | 10.185 |
| AMD Ryzen 9 7950X 16-Core | 52 | 170 | 8.840 |

En términos de **eficiencia energética**, el **Ryzen 9 7950X es el más eficiente** en energía total consumida por tarea completada: a pesar de tener un TDP mayor que el 5900X, su velocidad compensa y termina usando menos energía total para completar la compilación. El i5-13600K es el menos eficiente energéticamente por su alto consumo en turbo.

En términos de **costo económico**, se utilizaron los precios actuales relevados en Amazon. Calculando el tiempo de compilación por dólar invertido (menor es mejor):
 
| Procesador | Tiempo (s) | Precio (Amazon) | Segundos / dólar |
|---|---|---|---|
| Intel Core i5-13600K | 83 | $320 | 0.259 |
| AMD Ryzen 9 5900X 12-Core | 97 | $315 | 0.308 |
| AMD Ryzen 9 7950X 16-Core | 52 | $498 | 0.104 |
 
Con los precios actuales, el **Ryzen 9 7950X resulta ser la mejor opción económica** para tareas de compilación intensiva, ofreciendo el menor tiempo por dólar invertido. El Ryzen 9 5900X queda como la peor alternativa: precio similar al i5-13600K pero con rendimiento notablemente inferior, resultando en la peor relación precio/rendimiento de los tres.
