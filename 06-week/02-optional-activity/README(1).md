# Inteligencia de Negocios – Semana 6

## Define el grano y prepara datos de prueba

### Caso seleccionado

Para esta actividad se continúa con el modelo de la Semana 4, correspondiente al proceso de **matrícula académica universitaria**.

La tabla de hechos utilizada es:

`FACT_MATRICULA`

El modelo permite analizar las matrículas según estudiante, materia, programa académico y periodo.

---

## 1. Grano de la tabla de hechos

El grano definido para `FACT_MATRICULA` es:

> **Una fila representa la matrícula de un estudiante en una materia específica, perteneciente a un programa académico, durante un periodo académico determinado.**

Esto significa que si un estudiante matricula cinco materias durante un semestre, existirán cinco registros diferentes en la tabla de hechos.

Este nivel de detalle permite analizar las matrículas desde diferentes perspectivas:

- Estudiante.
- Materia.
- Programa académico.
- Periodo.

También permite responder preguntas como:

- ¿Qué materias tienen mayor número de matrículas?
- ¿Cuántas matrículas existen por semestre?
- ¿Cuál es el valor total matriculado por programa?
- ¿Cuál es la nota promedio por materia?
- ¿Qué estudiantes están matriculados en una asignatura?

---

## 2. Medidas

### cantidad_matriculas

Cada fila representa una matrícula, por lo que:

```text
cantidad_matriculas = 1
```

Esta medida puede sumarse mediante:

```text
SUM(cantidad_matriculas)
```

para conocer la cantidad total de matrículas por materia, programa o periodo.

### valor_matricula

Representa el valor económico asociado a la matrícula de la materia.

Puede utilizarse:

```text
SUM(valor_matricula)
```

para obtener valores totales o:

```text
AVG(valor_matricula)
```

para calcular valores promedio.

### nota_final

Representa la calificación obtenida por el estudiante en una materia.

La operación más adecuada es:

```text
AVG(nota_final)
```

También pueden utilizarse:

```text
MAX(nota_final)
MIN(nota_final)
```

para conocer las notas máximas y mínimas.

---

## 3. Datos de prueba

Para validar el modelo se creó una tabla de prueba en Excel con **12 registros**.

| id_matricula | id_estudiante | estudiante | id_materia | materia | id_programa | programa | id_tiempo | semestre | cantidad_matriculas | valor_matricula | nota_final |
|---:|---:|---|---:|---|---:|---|---:|---|---:|---:|---:|
| 1 | 1001 | Ana Torres | 25 | Programación | 1 | Ingeniería de Sistemas | 202601 | 2026-1 | 1 | 350000 | 4.5 |
| 2 | 1001 | Ana Torres | 31 | Bases de Datos | 1 | Ingeniería de Sistemas | 202601 | 2026-1 | 1 | 400000 | 4.2 |
| 3 | 1002 | Carlos Rojas | 25 | Programación | 1 | Ingeniería de Sistemas | 202601 | 2026-1 | 1 | 350000 | 3.8 |
| 4 | 1002 | Carlos Rojas | 40 | Matemáticas | 1 | Ingeniería de Sistemas | 202601 | 2026-1 | 1 | 300000 | 3.5 |
| 5 | 1003 | Laura Díaz | 31 | Bases de Datos | 1 | Ingeniería de Sistemas | 202601 | 2026-1 | 1 | 400000 | 4.7 |
| 6 | 1003 | Laura Díaz | 45 | Estadística | 1 | Ingeniería de Sistemas | 202601 | 2026-1 | 1 | 320000 | 4.1 |
| 7 | 1004 | Juan Pérez | 25 | Programación | 2 | Ingeniería Industrial | 202602 | 2026-2 | 1 | 350000 | 3.9 |
| 8 | 1004 | Juan Pérez | 40 | Matemáticas | 2 | Ingeniería Industrial | 202602 | 2026-2 | 1 | 300000 | 4.0 |
| 9 | 1005 | Sofía López | 45 | Estadística | 2 | Ingeniería Industrial | 202602 | 2026-2 | 1 | 320000 | 4.6 |
| 10 | 1005 | Sofía López | 40 | Matemáticas | 2 | Ingeniería Industrial | 202602 | 2026-2 | 1 | 300000 | 4.3 |
| 11 | 1006 | Diego Gómez | 25 | Programación | 1 | Ingeniería de Sistemas | 202602 | 2026-2 | 1 | 350000 | 3.7 |
| 12 | 1006 | Diego Gómez | 31 | Bases de Datos | 1 | Ingeniería de Sistemas | 202602 | 2026-2 | 1 | 400000 | 4.4 |

Cada registro respeta el grano establecido.

Por ejemplo:

```text
Ana Torres + Programación + 2026-1
```

representa un evento de matrícula.

Mientras que:

```text
Ana Torres + Bases de Datos + 2026-1
```

representa otro evento diferente.

Por lo tanto:

```text
Estudiante + Materia + Periodo académico = Una fila
```

---

## 4. Ejemplos de análisis

Con los datos de prueba se pueden realizar diferentes análisis.

### Matrículas por materia

| Materia | Matrículas |
|---|---:|
| Programación | 4 |
| Bases de Datos | 3 |
| Matemáticas | 3 |
| Estadística | 2 |

La asignatura con mayor número de matrículas en los datos de prueba es **Programación**, con 4.

### Matrículas por periodo

| Periodo | Matrículas |
|---|---:|
| 2026-1 | 6 |
| 2026-2 | 6 |

### Promedio de Programación

Las notas registradas son:

```text
4.5
3.8
3.9
3.7
```

Cálculo:

```text
(4.5 + 3.8 + 3.9 + 3.7) / 4
= 3.975
```

Promedio aproximado:

```text
3.98
```

---

## 5. Grano alternativo

Un grano más grueso podría ser:

> **Una fila por estudiante y periodo académico.**

Con este nivel, todas las materias de un estudiante durante el semestre quedarían agrupadas en un único registro.

Por ejemplo:

```text
Ana Torres | 2026-1 | 2 materias | $750000
```

Esto permitiría conocer información general sobre el estudiante y el periodo.

Sin embargo, se perdería la posibilidad de saber directamente:

- Qué materias matriculó cada estudiante.
- Qué materia presenta mayor demanda.
- La nota obtenida en cada materia.
- El valor individual de cada materia.
- Cuántos estudiantes están matriculados en una materia.
- El promedio de notas por asignatura.

Por esta razón, el grano detallado es más conveniente para el análisis.

---

## 6. Reflexión

El grano seleccionado conserva el detalle de cada matrícula realizada.

Un estudiante puede matricular varias materias durante un mismo periodo, por lo que almacenar una fila por materia permite realizar análisis más completos.

Si se utilizara un grano más grueso, como una fila por estudiante y semestre, sería necesario agrupar las materias y se perdería información importante.

Aunque un grano más grueso podría reducir el número de registros almacenados, también limitaría los análisis disponibles.

Por esta razón, el grano:

> **Una materia matriculada por estudiante y periodo académico**

es adecuado para el modelo.

---

## 7. Conclusión

La tabla `FACT_MATRICULA` utiliza un grano donde cada registro representa una materia matriculada por un estudiante durante un periodo académico.

Este nivel permite analizar las matrículas según estudiante, materia, programa y tiempo.

Las principales medidas utilizadas son:

- `cantidad_matriculas`
- `valor_matricula`
- `nota_final`

Los datos de prueba permiten verificar que el modelo mantiene correctamente este nivel de detalle.

Finalmente, un grano más grueso reduciría la cantidad de registros, pero también provocaría la pérdida de información necesaria para realizar análisis específicos por materia.

---

## Estructura del modelo

```text
                DIM_TIEMPO
                     |
                     |
DIM_ESTUDIANTE -- FACT_MATRICULA -- DIM_MATERIA
                     |
                     |
               DIM_PROGRAMA
```

`FACT_MATRICULA` se encuentra en el centro del modelo estrella y contiene las medidas y claves necesarias para relacionarse con las dimensiones.
