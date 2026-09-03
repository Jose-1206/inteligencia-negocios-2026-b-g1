# ACTIVIDAD 5 — Resumen y aplicación de conceptos del Corte 1

**Materia:** Inteligencia de Negocios
**Programa:** Ingeniería de Sistemas
**Semestre:** Sexto semestre

---

# 1. Resumen de conceptos del Corte 1

## Cadena del dato

La cadena del dato representa el proceso mediante el cual los datos se transforman en elementos útiles para tomar decisiones.

Puede representarse como:

```text
DATO → INFORMACIÓN → CONOCIMIENTO → DECISIÓN
```

El **dato** es un registro individual sin suficiente contexto. Al organizar y procesar diferentes datos obtenemos **información**. Cuando esta información es analizada y permite identificar patrones, tendencias o relaciones, se transforma en **conocimiento**. Finalmente, el conocimiento puede utilizarse para tomar decisiones.

Por ejemplo, una universidad puede tener datos individuales sobre las notas de sus estudiantes. Al agruparlos se puede obtener el promedio por materia y, posteriormente, identificar que una asignatura presenta un alto porcentaje de pérdida. Con esta información la institución puede tomar decisiones relacionadas con tutorías o estrategias académicas.

---

## KPI

Un **KPI (Key Performance Indicator)** es un indicador utilizado para medir el desempeño de un proceso respecto a un objetivo.

Un KPI debe permitir conocer una situación y apoyar la toma de decisiones.

Por ejemplo:

$$
Tasa\ de\ deserción =
\frac{Estudiantes\ retirados}
{Estudiantes\ matriculados}
\times100
$$

Si la meta de una universidad es mantener la deserción por debajo del 5 %, este indicador permite comprobar si el objetivo está siendo cumplido.

---

## OLTP vs OLAP

**OLTP** significa *Online Transaction Processing* y está orientado a las operaciones diarias de una organización.

Ejemplos:

* Registrar una matrícula.
* Realizar una venta.
* Registrar una factura.
* Actualizar un inventario.

OLTP trabaja principalmente con transacciones operativas.

**OLAP** significa *Online Analytical Processing* y está orientado al análisis de información.

Ejemplos:

* Analizar ventas por año.
* Comparar ventas entre sucursales.
* Analizar tendencias.
* Calcular indicadores.

La principal diferencia es que OLTP está orientado a **operar**, mientras que OLAP está orientado a **analizar**.

---

## Data Warehouse

Un **Data Warehouse** es un repositorio diseñado para almacenar información que será utilizada principalmente para análisis y toma de decisiones.

Permite:

* Integrar información de diferentes fuentes.
* Mantener datos históricos.
* Facilitar consultas analíticas.
* Separar las consultas de análisis de las operaciones diarias.

Una arquitectura típica puede ser:

```text
Fuentes
   │
   ▼
 ETL
   │
   ▼
Data Warehouse
   │
   ▼
Power BI
   │
   ▼
Dashboard
   │
   ▼
Decisión
```

---

## Modelo estrella

El modelo estrella es un modelo dimensional compuesto principalmente por una **tabla de hechos** y varias **dimensiones**.

La tabla de hechos contiene:

* Claves.
* Medidas.

Las dimensiones contienen los atributos utilizados para analizar las medidas.

Ejemplo:

```text
                  DIM_TIEMPO
                       │
                       │
DIM_PRODUCTO ─── FACT_VENTAS ─── DIM_SUCURSAL
                       │
                       │
                  DIM_CLIENTE
```

Este modelo facilita realizar análisis desde diferentes perspectivas.

---

# 2. Caso: cadena de farmacias

## Situación

Una cadena de farmacias desea analizar sus ventas por:

* Producto.
* Sucursal.
* Mes.

Se diseñará un modelo estrella que permita responder estas preguntas de negocio.

---

# 3. Tabla de hechos

La tabla central será:

## FACT_VENTAS

Cada registro representa una venta realizada de un producto en una sucursal durante una fecha determinada.

### Claves

* `id_venta`
* `id_producto`
* `id_sucursal`
* `id_tiempo`

### Medidas

* `cantidad_vendida`
* `precio_unitario`
* `total_venta`
* `costo`
* `ganancia`

---

# 4. Dimensión Producto

## DIM_PRODUCTO

| Atributo        | Descripción                |
| --------------- | -------------------------- |
| id_producto     | Identificador del producto |
| nombre_producto | Nombre                     |
| categoria       | Categoría                  |
| marca           | Marca                      |
| laboratorio     | Laboratorio                |
| presentación    | Presentación               |
| tipo_producto   | Medicamento, higiene, etc. |

---

# 5. Dimensión Sucursal

## DIM_SUCURSAL

| Atributo        | Descripción      |
| --------------- | ---------------- |
| id_sucursal     | Identificador    |
| nombre_sucursal | Nombre           |
| ciudad          | Ciudad           |
| departamento    | Departamento     |
| dirección       | Dirección        |
| tipo_sucursal   | Tipo de sucursal |

---

# 6. Dimensión Tiempo

## DIM_TIEMPO

| Atributo   | Descripción    |
| ---------- | -------------- |
| id_tiempo  | Identificador  |
| fecha      | Fecha          |
| día        | Día            |
| mes        | Mes            |
| nombre_mes | Nombre del mes |
| trimestre  | Trimestre      |
| año        | Año            |

La dimensión Tiempo es especialmente importante porque el requerimiento indica que las ventas deben analizarse **por mes**.

---

# 7. Modelo estrella de la farmacia

```text
                         ┌────────────────────┐
                         │    DIM_TIEMPO      │
                         ├────────────────────┤
                         │ id_tiempo          │
                         │ fecha              │
                         │ día                │
                         │ mes                │
                         │ nombre_mes         │
                         │ trimestre          │
                         │ año                │
                         └─────────┬──────────┘
                                   │
                                   │
                                   ▼
┌────────────────────┐    ┌──────────────────────┐    ┌────────────────────┐
│   DIM_PRODUCTO     │    │     FACT_VENTAS      │    │    DIM_SUCURSAL    │
├────────────────────┤    ├──────────────────────┤    ├────────────────────┤
│ id_producto        │───►│ id_venta             │◄───│ id_sucursal        │
│ nombre_producto    │    │ id_producto          │    │ nombre_sucursal    │
│ categoria          │    │ id_sucursal          │    │ ciudad             │
│ marca              │    │ id_tiempo            │    │ departamento       │
│ laboratorio        │    │ cantidad_vendida     │    │ dirección          │
│ presentación       │    │ precio_unitario      │    │ tipo_sucursal     │
│ tipo_producto      │    │ total_venta          │    └────────────────────┘
└────────────────────┘    │ costo                │
                          │ ganancia             │
                          └──────────────────────┘
```

---

# 8. Preguntas que puede responder el modelo

## Pregunta 1

> ¿Cuál fue el total de ventas de cada producto durante cada mes?

Se utilizarían:

* `DIM_PRODUCTO` → producto.
* `DIM_TIEMPO` → mes.
* `FACT_VENTAS` → total de venta.

Ejemplo:

```text
Producto: Acetaminofén

Enero      → $12.500.000
Febrero    → $13.200.000
Marzo      → $14.100.000
```

---

## Pregunta 2

> ¿Qué sucursal tuvo mayores ventas durante el año?

Se utilizarían:

* `DIM_SUCURSAL` → sucursal.
* `DIM_TIEMPO` → año.
* `FACT_VENTAS` → total de ventas.

Ejemplo:

```text
Sucursal Centro       → $180.000.000
Sucursal Norte        → $165.000.000
Sucursal Sur          → $143.000.000
```

Esto permitiría identificar las sucursales con mayor rendimiento comercial.

---

# 9. Justificación

El modelo estrella es adecuado porque la **venta** es el evento central que se desea analizar.

`FACT_VENTAS` contiene las medidas numéricas necesarias para realizar cálculos, mientras que las dimensiones proporcionan el contexto necesario para interpretar dichas medidas.

La dimensión Producto permite analizar:

* Qué productos se venden más.
* Qué categorías tienen mayor demanda.
* Qué marcas generan mayores ventas.

La dimensión Sucursal permite analizar:

* Qué sucursales venden más.
* Qué ciudades tienen mayor volumen de ventas.
* Comparaciones entre establecimientos.

La dimensión Tiempo permite analizar:

* Ventas mensuales.
* Ventas trimestrales.
* Ventas anuales.
* Tendencias a través del tiempo.

Por lo tanto, el modelo permite construir dashboards en Power BI para analizar las ventas desde diferentes perspectivas.

---

# 10. Autoevaluación antes del parcial

## 🟢 Temas que domino

Considero que tengo un buen manejo de:

* Concepto general de Inteligencia de Negocios.
* Diferencia básica entre dato e información.
* Concepto de KPI.
* Diferencia entre OLTP y OLAP.
* Concepto de Data Warehouse.
* Estructura básica de un modelo estrella.
* Identificación de tablas de hechos y dimensiones.

---

## 🟡 Temas que debo repasar

Necesito reforzar:

* Diseño detallado de modelos dimensionales.
* Identificación correcta de medidas.
* Relaciones entre hechos y dimensiones.
* Procesos ETL.
* Transformación de datos.
* Construcción de dashboards en Power BI.
* Definición de KPIs con fórmulas y metas.

---

## 🔴 Temas que debo estudiar con mayor profundidad

Antes del parcial debo prestar especial atención a:

* Diferencias prácticas entre OLTP y OLAP.
* Funcionamiento completo de un Data Warehouse.
* Diseño de tablas de hechos y dimensiones.
* Flujo ETL.
* Interpretación de indicadores.
* Aplicación de analítica descriptiva, diagnóstica, predictiva y prescriptiva.

---

# Conclusión

Durante el primer corte se estudian los fundamentos necesarios para comprender cómo una organización puede transformar datos en información útil para tomar decisiones.

La cadena del dato permite comprender el proceso desde el registro inicial hasta la decisión. Los KPI permiten medir el desempeño, mientras que OLTP y OLAP representan diferentes necesidades de procesamiento. El Data Warehouse permite almacenar y organizar información histórica para análisis, y el modelo estrella proporciona una estructura adecuada para consultar dicha información y construir herramientas de visualización como Power BI.
