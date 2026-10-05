# Semana 9 - Primeras medidas DAX

## Inteligencia de Negocios

### Objetivo

Crear medidas básicas en DAX y observar cómo cambia el resultado de una medida dependiendo del contexto de filtro en Power BI.

---

## Dataset utilizado

Se utilizó una tabla de ventas llamada:

`ventas_sucias (2)`

La tabla contiene información como:

- ID de venta
- Producto
- Categoría
- Ciudad
- Fecha
- Cantidad
- Precio unitario
- Total
- Vendedor

Antes de crear las medidas, los datos fueron revisados y transformados en Power Query para asegurar tipos de datos correctos y una estructura adecuada para el análisis.

---

## Medidas DAX creadas

### 1. Total Ventas

```DAX
Total Ventas =
SUM('ventas_sucias (2)'[Total])
```

Esta medida calcula el valor total de todas las ventas.

### 2. Promedio Ventas

```DAX
Promedio Ventas =
AVERAGE('ventas_sucias (2)'[Total])
```

Esta medida calcula el promedio del valor total de las ventas.

### 3. Cantidad Ventas

```DAX
Cantidad Ventas =
DISTINCTCOUNT('ventas_sucias (2)'[ID_Venta])
```

Esta medida cuenta la cantidad de ventas diferentes registradas en el dataset.

### 4. Precio Promedio por Unidad

```DAX
Precio Promedio por Unidad =
DIVIDE(
    SUM('ventas_sucias (2)'[Total]),
    SUM('ventas_sucias (2)'[Cantidad]),
    0
)
```

Esta medida divide el total de ventas entre la cantidad total de unidades vendidas.

---

## Evidencia del contexto de filtro

### Tarjeta con Total Ventas

En la siguiente evidencia se muestra la medida `Total Ventas` en una tarjeta. En este caso, Power BI muestra el total general de todas las ventas.

/Evidencias/01

### Tabla por categoría

En la siguiente evidencia se utiliza la misma medida `Total Ventas`, pero esta vez dentro de una tabla junto con la columna `Categoria`.

/Evidencias/02

En este caso, el valor cambia para cada categoría porque Power BI aplica un filtro diferente en cada fila de la tabla.

---

## Contexto de filtro

El contexto de filtro es el conjunto de filtros que Power BI tiene en cuenta al calcular una medida.

Por ejemplo, cuando la medida `Total Ventas` se coloca en una tarjeta, se calcula utilizando todos los registros disponibles y muestra el total general.

Cuando la misma medida se coloca en una tabla junto con `Categoria`, Power BI calcula el total únicamente para los registros que pertenecen a cada categoría.

La fórmula DAX no cambia, pero el resultado cambia dependiendo del contexto en el que se utiliza.

---

## Medida vs. columna calculada

En este modelo, el campo `Total` es adecuado como columna porque su valor se obtiene a partir de los datos de cada fila, por ejemplo:

```text
Total = Cantidad × Precio_Unitario
```

En cambio, medidas como `Total Ventas`, `Promedio Ventas`, `Cantidad Ventas` y `Precio Promedio por Unidad` deben mantenerse como medidas porque sus resultados cambian dinámicamente dependiendo de los filtros aplicados en el reporte.

Por esta razón, las columnas son útiles para cálculos a nivel de cada registro, mientras que las medidas son más apropiadas para cálculos agregados y análisis dinámicos.

---

## Conclusión

Con esta actividad se crearon cuatro medidas DAX utilizando las funciones `SUM`, `AVERAGE`, `DISTINCTCOUNT` y `DIVIDE`.

Además, se pudo observar cómo una misma medida puede mostrar resultados diferentes según el contexto de filtro utilizado en los visuales de Power BI.

Esto permite comprender la diferencia entre realizar cálculos a nivel de fila mediante columnas y realizar cálculos dinámicos mediante medidas.