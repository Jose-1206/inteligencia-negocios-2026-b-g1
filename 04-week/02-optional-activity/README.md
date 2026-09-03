# ACTIVIDAD 4 — Diseño de un modelo estrella

**Materia:** Inteligencia de Negocios
**Programa:** Ingeniería de Sistemas
**Semestre:** Sexto semestre

---

# 1. Proceso de negocio seleccionado

Se selecciona el proceso de **matrícula académica universitaria**.

El objetivo será analizar las matrículas realizadas por los estudiantes según diferentes características.

El evento central será:

> **Una matrícula de una asignatura realizada por un estudiante durante un periodo académico.**

---

# 2. Tabla de hechos

La tabla de hechos será:

## FACT_MATRICULA

Representa cada evento de matrícula.

### Claves

* `id_matricula`
* `id_estudiante`
* `id_materia`
* `id_programa`
* `id_tiempo`

### Medidas

Las medidas son valores numéricos que pueden agregarse.

* `cantidad_matriculas`
* `valor_matricula`
* `nota_final`

### Ejemplo

| id_matricula | id_estudiante | id_materia | id_tiempo | cantidad |  valor |
| -----------: | ------------: | ---------: | --------: | -------: | -----: |
|            1 |          1001 |         25 |    202601 |        1 | 350000 |
|            2 |          1002 |         25 |    202601 |        1 | 350000 |
|            3 |          1003 |         31 |    202602 |        1 | 400000 |

---

# 3. Dimensiones

## DIM_TIEMPO

Esta dimensión permite analizar los datos temporalmente.

| Atributo   |
| ---------- |
| id_tiempo  |
| fecha      |
| día        |
| mes        |
| nombre_mes |
| trimestre  |
| semestre   |
| año        |

---

## DIM_ESTUDIANTE

| Atributo        |
| --------------- |
| id_estudiante   |
| nombre          |
| género          |
| edad            |
| ciudad          |
| tipo_estudiante |

---

## DIM_MATERIA

| Atributo       |
| -------------- |
| id_materia     |
| nombre_materia |
| código         |
| créditos       |
| área           |
| semestre       |

---

## DIM_PROGRAMA

| Atributo        |
| --------------- |
| id_programa     |
| nombre_programa |
| facultad        |
| modalidad       |
| nivel           |

---

# 4. Modelo estrella

El modelo puede representarse de la siguiente manera:

```text
                    ┌──────────────────┐
                    │   DIM_TIEMPO     │
                    ├──────────────────┤
                    │ id_tiempo        │
                    │ fecha            │
                    │ mes              │
                    │ semestre         │
                    │ año              │
                    └────────┬─────────┘
                             │
                             │
                             ▼
┌──────────────────┐    ┌───────────────────┐    ┌──────────────────┐
│ DIM_ESTUDIANTE   │    │  FACT_MATRICULA   │    │   DIM_MATERIA    │
├──────────────────┤    ├───────────────────┤    ├──────────────────┤
│ id_estudiante    │───►│ id_matricula      │◄───│ id_materia       │
│ nombre           │    │ id_estudiante     │    │ nombre           │
│ ciudad           │    │ id_materia        │    │ código           │
│ edad             │    │ id_programa       │    │ créditos         │
│ tipo_estudiante  │    │ id_tiempo         │    │ área             │
└──────────────────┘    │ cantidad          │    └──────────────────┘
                        │ valor_matricula   │
                        │ nota_final        │
                        └─────────┬─────────┘
                                  │
                                  │
                                  ▼
                         ┌──────────────────┐
                         │  DIM_PROGRAMA    │
                         ├──────────────────┤
                         │ id_programa      │
                         │ nombre_programa  │
                         │ facultad         │
                         │ modalidad        │
                         │ nivel            │
                         └──────────────────┘
```

---

# 5. Preguntas de negocio

## Pregunta 1

> **¿Cuántos estudiantes se matricularon en cada programa académico durante cada semestre?**

El modelo puede responderla utilizando:

* `DIM_PROGRAMA` para conocer el programa.
* `DIM_TIEMPO` para conocer el semestre.
* `FACT_MATRICULA` para contar las matrículas.

Por ejemplo:

```text
Ingeniería de Sistemas

2025-1 → 450 estudiantes
2025-2 → 480 estudiantes
2026-1 → 510 estudiantes
```

Esto permitiría analizar el crecimiento o disminución de la matrícula.

---

## Pregunta 2

> **¿Qué materias presentan el mayor número de estudiantes matriculados?**

Para responderla se utilizan:

* `DIM_MATERIA`.
* `FACT_MATRICULA`.

El resultado podría ser:

```text
Programación        → 180
Bases de Datos      → 165
Matemáticas         → 150
Estadística         → 120
```

Esto permitiría identificar las asignaturas con mayor demanda.

---

# 6. Justificación del modelo

El modelo estrella es apropiado porque separa el **evento que se desea analizar**, representado por `FACT_MATRICULA`, de las características que permiten analizar ese evento, representadas mediante las dimensiones.

La tabla de hechos contiene las medidas y las claves que conectan la información con las dimensiones.

Las dimensiones permiten analizar las matrículas desde diferentes perspectivas:

* Tiempo.
* Estudiante.
* Materia.
* Programa académico.

Esta estructura facilita la creación de consultas analíticas y dashboards en herramientas como Power BI.
