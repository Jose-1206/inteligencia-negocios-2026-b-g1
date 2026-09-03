# ACTIVIDAD 3 — OLTP, OLAP y Data Warehouse

**Materia:** Inteligencia de Negocios
**Programa:** Ingeniería de Sistemas
**Semestre:** Sexto semestre

---

# 1. Sistemas seleccionados

Para esta actividad se seleccionan dos sistemas relacionados con el entorno universitario.

---

## Sistema 1 — Sistema de matrícula académica

El sistema de matrícula registra las asignaturas que los estudiantes inscriben durante cada periodo académico.

### Clasificación: OLTP

El sistema corresponde a un sistema **OLTP (Online Transaction Processing)**.

### Justificación

Su objetivo principal es procesar operaciones del día a día, como:

* Matricular una asignatura.
* Cancelar una asignatura.
* Registrar un estudiante.
* Actualizar información.
* Consultar el estado de una matrícula.

Estas operaciones son transacciones individuales que deben realizarse de manera rápida y consistente.

```text
Estudiante
    │
    ▼
Sistema de matrícula
    │
    ├── Matricular materia
    ├── Cancelar materia
    └── Actualizar matrícula
```

---

# 2. Sistema 2 — Reporte académico gerencial

El segundo sistema sería un reporte utilizado por coordinadores o directivos para analizar información académica histórica.

### Clasificación: OLAP

Este sistema corresponde a **OLAP (Online Analytical Processing)**.

### Justificación

Su objetivo no es registrar transacciones individuales, sino analizar grandes cantidades de información.

Por ejemplo:

* Estudiantes matriculados por semestre.
* Promedio de notas por programa.
* Materias con mayor porcentaje de pérdida.
* Evolución de la matrícula durante varios años.

Por lo tanto, está orientado al análisis y la toma de decisiones.

---

# 3. Pregunta de análisis

Para el sistema de matrícula se podría formular la siguiente pregunta:

> **¿Cuál fue el promedio de estudiantes matriculados por programa académico y periodo durante los últimos cinco años?**

Esta pregunta no conviene resolver directamente sobre la base operativa OLTP.

### ¿Por qué?

Porque requiere:

* Consultar una gran cantidad de registros.
* Agrupar información.
* Comparar múltiples periodos.
* Realizar cálculos históricos.
* Relacionar diferentes tablas.

Una consulta de este tipo podría consumir recursos del sistema operativo de matrícula y afectar las operaciones que realizan los usuarios.

---

# 4. Ventajas de utilizar un Data Warehouse

Llevar la información a un **Data Warehouse** permite separar las operaciones diarias del análisis.

### Rendimiento

Las consultas analíticas pueden ejecutarse sin afectar directamente el sistema transaccional.

### Estructura

La información puede organizarse específicamente para realizar análisis y generar reportes.

### Histórico

El Data Warehouse permite conservar información de diferentes periodos para realizar comparaciones.

### Integración

Puede reunir información proveniente de diferentes sistemas.

---

# 5. Flujo OLTP → ETL → Data Warehouse → Reporte

```text
┌─────────────────────┐
│ Sistema de matrícula│
│       OLTP          │
└──────────┬──────────┘
           │
           │ Datos
           ▼
┌─────────────────────┐
│        ETL          │
│                     │
│ Extract             │
│ Transform           │
│ Load                │
└──────────┬──────────┘
           │
           │ Datos transformados
           ▼
┌─────────────────────┐
│   DATA WAREHOUSE    │
│                     │
│ Datos históricos    │
│ Datos integrados    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│       POWER BI      │
│      Dashboard      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│     DECISIÓN        │
│    GERENCIAL        │
└─────────────────────┘
```

---

# 6. Aplicación al caso universitario

Por ejemplo, diariamente el sistema OLTP registra las matrículas:

```text
Estudiante 001 → Ingeniería de Sistemas → Bases de Datos
Estudiante 002 → Ingeniería de Sistemas → Programación
Estudiante 003 → Ingeniería Industrial → Estadística
```

El proceso ETL puede extraer estos datos, transformarlos y cargarlos al Data Warehouse.

Posteriormente Power BI podría generar un dashboard como:

```text
ESTUDIANTES MATRICULADOS

2024 → 1.800
2025 → 2.050
2026 → 2.230
```

Esto permite que los responsables puedan analizar tendencias sin realizar las consultas directamente sobre el sistema operativo.
