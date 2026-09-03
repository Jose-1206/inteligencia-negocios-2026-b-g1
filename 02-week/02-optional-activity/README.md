# ACTIVIDAD 2 — KPI y cadena del dato

**Materia:** Inteligencia de Negocios
**Programa:** Ingeniería de Sistemas
**Semestre:** Sexto semestre

---

## 1. Organización seleccionada

Para esta actividad se selecciona como organización una **universidad**, debido a que es un entorno conocido y permite trabajar con información académica.

El proceso seleccionado será el seguimiento de estudiantes que abandonan sus estudios.

---

# 2. Definición del KPI

## KPI: Tasa de deserción estudiantil

Este KPI mide el porcentaje de estudiantes que abandonan sus estudios durante un periodo académico determinado.

### Fórmula

$$
Tasa\ de\ deserción =
\frac{Número\ de\ estudiantes\ que\ abandonan}
{Número\ de\ estudiantes\ matriculados}
\times100
$$

### Ejemplo

Si una universidad tiene:

* 1.000 estudiantes matriculados.
* 40 estudiantes que abandonaron sus estudios.

Entonces:

$$
\frac{40}{1000}\times100=4\%
$$

La tasa de deserción sería del **4 %**.

### Meta

**Tasa de deserción ≤ 5 %.**

Mientras menor sea el indicador, mejor será el resultado.

---

# 3. Decisión que habilita el KPI

Este indicador permitiría a la universidad identificar si existe un aumento significativo en la cantidad de estudiantes que abandonan sus programas académicos.

Si el indicador supera la meta establecida, la institución podría investigar las causas y tomar decisiones como:

* Implementar programas de acompañamiento académico.
* Realizar tutorías.
* Detectar estudiantes con bajo rendimiento.
* Mejorar los procesos de orientación.
* Analizar problemas relacionados con horarios o carga académica.
* Implementar estrategias de permanencia estudiantil.

Por lo tanto, el KPI no solamente permite conocer una situación, sino que proporciona información para apoyar una decisión administrativa.

---

# 4. Cadena del dato

La cadena puede representarse de la siguiente manera:

```text
DATO
  │
  ▼
INFORMACIÓN
  │
  ▼
CONOCIMIENTO
  │
  ▼
DECISIÓN
```

## Dato

Los datos individuales podrían ser:

* ID del estudiante.
* Programa académico.
* Estado de matrícula.
* Periodo académico.
* Estado final del estudiante.

Por ejemplo:

```text
Estudiante: 1025
Programa: Ingeniería de Sistemas
Periodo: 2026-2
Estado: Retirado
```

---

## Información

Al procesar y agrupar los datos se obtiene información:

```text
Estudiantes matriculados: 1.000
Estudiantes retirados: 40
Tasa de deserción: 4 %
```

La información proporciona un contexto que no estaba presente en los datos individuales.

---

## Conocimiento

Al analizar la información a través del tiempo se podría identificar que la deserción aumenta en determinados semestres o programas.

Por ejemplo:

```text
Ingeniería de Sistemas
2025-1 → 3 %
2025-2 → 4 %
2026-1 → 6 %
```

Esto permite identificar una posible tendencia de aumento.

---

## Decisión

La universidad podría investigar las causas del aumento y establecer estrategias para reducir la deserción.

---

# 5. Tipo de analítica

El KPI utiliza principalmente **analítica descriptiva**, porque permite conocer qué ocurrió con la deserción durante un periodo.

También puede utilizarse como punto de partida para:

### Analítica diagnóstica

Busca responder:

> ¿Por qué aumentó la deserción?

### Analítica predictiva

Podría utilizarse posteriormente para responder:

> ¿Qué estudiantes presentan mayor probabilidad de abandonar sus estudios?

### Analítica prescriptiva

Finalmente podría ayudar a responder:

> ¿Qué acciones debería implementar la universidad para disminuir la deserción?

---

# 6. Resumen

| Elemento            | Aplicación                             |
| ------------------- | -------------------------------------- |
| Organización        | Universidad                            |
| KPI                 | Tasa de deserción                      |
| Fórmula             | Retirados / Matriculados × 100         |
| Meta                | ≤ 5 %                                  |
| Decisión            | Implementar estrategias de permanencia |
| Dato                | Registro individual del estudiante     |
| Información         | Tasa de deserción                      |
| Conocimiento        | Tendencias y posibles causas           |
| Analítica principal | Descriptiva                            |
