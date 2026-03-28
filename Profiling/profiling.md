# Time Profiling con gprof

## Herramienta utilizada

Se utilizó **gprof** (GNU Profiler), una herramienta de profiling para programas compilados con GCC. El proceso consistió en tres pasos:

1. Compilar el código con el flag `-pg` para habilitar la generación de datos de perfil:
```bash
$ gcc -Wall -pg test_gprof.c test_gprof_new.c -o test_gprof
```

2. Ejecutar el binario para generar el archivo de datos `gmon.out`:
```bash
$ ./test_gprof
```

3. Analizar los datos con gprof:
```bash
$ gprof test_gprof gmon.out > analysis.txt
```



## Código analizado

El programa consta de dos archivos. `main` llama a `func1` y `func2`. A su vez, `func1` llama a `new_func1`. Cada función contiene un bucle `for` diseñado para consumir tiempo de CPU, lo que permite observar el comportamiento del profiler.


## Resultados obtenidos

### Flat Profile

| Función | % tiempo | Tiempo propio (s) | Tiempo acumulado (s) | Llamadas |
|---|---|---|---|---|
| `new_func1` | 34.87% | 7.89 | 7.89 | 1 |
| `func1` | 32.97% | 7.46 | 15.35 | 1 |
| `func2` | 31.95% | 7.23 | 22.58 | 1 |
| `main` | 0.22% | 0.05 | 22.63 | — |

**Tiempo total de ejecución: 22.63 segundos**

(Los resultados se encuentran en el archivo **_Profiling/tests/analysis.txt_**)

### Call Graph

| Índice | % tiempo total | Función | Tiempo propio (s) | Tiempo hijos (s) |
|---|---|---|---|---|
| [1] | 100.0% | `main` | 0.05 | 22.58 |
| [2] | 67.8% | `func1` | 7.46 | 7.89 |
| [3] | 34.9% | `new_func1` | 7.89 | 0.00 |
| [4] | 31.9% | `func2` | 7.23 | 0.00 |

---

## Análisis de resultados

### Distribución del tiempo

El tiempo de ejecución está distribuido de forma casi uniforme entre las tres funciones con bucles (`new_func1`, `func1` y `func2`), cada una consumiendo aproximadamente un tercio del tiempo total. Esto es consistente con el diseño del código, donde cada bucle `for` tiene un límite de iteraciones similar (`0xffffffee`, `0xffffffff` y `0xffffffaa` respectivamente).

`main` consume apenas el **0.22%** del tiempo total, lo que confirma que su propio bucle (con límite `0xffffff`, mucho menor) es insignificante respecto a las funciones que llama.

### Flat Profile vs Call Graph

Una diferencia clave se observa al comparar ambas vistas para `func1`:

- En el **flat profile**, `func1` aparece con **7.46 segundos** de tiempo propio (32.97%).
- En el **call graph**, `func1` aparece consumiendo el **67.8% del tiempo total**, porque ese porcentaje incluye el tiempo de su hija `new_func1` (7.89s adicionales).

Esto ilustra perfectamente la diferencia entre tiempo *propio* y tiempo *total* de una función: `func1` por sí sola no es la más costosa, pero al incluir a `new_func1` que ella invoca, se convierte en el mayor cuello de botella del programa.

### Función más costosa

Aunque en el flat profile `new_func1` aparece primera (34.87% de tiempo propio), el verdadero cuello de botella del programa es la cadena `func1 → new_func1`, que juntas consumen el **67.8% del tiempo total**. En un escenario real de optimización, este sería el primer lugar donde intervenir.

---

## Conclusión

El análisis con gprof permitió identificar con precisión cuánto tiempo consume cada función del programa, algo imposible de determinar solo con intuición o análisis teórico de complejidad.

Los resultados muestran que:

- Las tres funciones con bucles consumen prácticamente todo el tiempo de ejecución (~99.8%), mientras que `main` es despreciable.
- La herramienta diferencia correctamente entre el tiempo propio de una función y el tiempo total incluyendo sus llamadas, lo cual es esencial para identificar cuellos de botella reales.
- En programas reales, este tipo de análisis guía las decisiones de optimización hacia las funciones que realmente impactan en el rendimiento, evitando optimizar código que representa una fracción mínima del tiempo de ejecución.