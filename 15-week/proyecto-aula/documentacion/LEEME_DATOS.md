# PixelZone Gaming — Generación y Preparación de Datos

**Materia:** Inteligencia de Negocios
**Proyecto:** PixelZone Gaming
**Integrantes:** José Miguel Aya Peralta y Juan José Guzmán Bohórquez
**Responsable de datos y ETL:** Juan José Guzmán Bohórquez
**Herramienta:** Power BI Web — Power Query
**Periodo:** Enero a diciembre de 2025

---

## 1. Introducción

PixelZone Gaming es una empresa ficticia dedicada a la comercialización de videojuegos, consolas, accesorios y periféricos.

El proyecto tiene como objetivo analizar las ventas, costos y gastos de la empresa para determinar cuáles categorías de productos y canales comerciales presentan mayor rentabilidad.

Para desarrollar este análisis se utilizaron datos sintéticos correspondientes al año 2025. Estos datos fueron importados, transformados y organizados mediante Power Query en Power BI Web.

## 2. Origen de los datos

Los datos fueron generados de manera sintética para fines académicos. No corresponden a operaciones reales ni contienen información personal de clientes verdaderos.

Se utilizaron archivos CSV organizados en dos carpetas:

- `datos/originales`: contiene las ventas con errores intencionales para realizar el proceso ETL.
- `datos/limpios`: contiene los conjuntos de datos de referencia de productos, clientes, canales, ventas y gastos.

### 2.1. Conjuntos de datos utilizados

| Archivo | Registros de referencia | Descripción |
|---|---:|---|
| ventas_con_errores.csv | 1.508 | Ventas con errores intencionales |
| ventas.csv | 1.500 | Ventas de referencia sin duplicados |
| productos.csv | 24 | Catálogo de productos |
| clientes.csv | 250 | Clientes sintéticos originales |
| canales.csv | 2 | Canales comerciales |
| gastos.csv | 108 | Registros mensuales de gastos |
| conceptos_gasto.csv | 9 | Conceptos de gastos empresariales |

Los 250 clientes originales se complementaron en Power Query con un registro denominado `SIN_IDENTIFICAR`.

## 3. Generación de datos sintéticos

La información fue preparada para simular el funcionamiento comercial de PixelZone Gaming durante los doce meses de 2025.

### 3.1. Ventas

Se crearon transacciones de productos con fechas, clientes, canales, cantidades, precios, descuentos y devoluciones.

Los importes financieros se calcularon mediante las siguientes reglas:

**Venta bruta:**

`Cantidad × Precio unitario`

**Descuento:**

`Venta bruta × Porcentaje de descuento / 100`

**Venta neta:**

`Venta bruta − Descuento − Devolución`

**Costo de ventas:**

`Cantidad × Costo unitario`

Para las ventas devueltas, el conjunto de datos considera la devolución del valor después del descuento y un costo neto de ventas igual a cero.

### 3.2. Productos

Se utilizaron 24 productos organizados en cuatro categorías:

- Consolas
- Videojuegos
- Accesorios
- Periféricos

Cada producto tiene un identificador, nombre, categoría, precio de lista y costo unitario.

### 3.3. Clientes

Se generaron 250 clientes ficticios, identificados mediante códigos.

Durante la limpieza se incorporó un cliente adicional denominado `SIN_IDENTIFICAR` para conservar las ventas cuyo identificador estaba vacío.

### 3.4. Canales comerciales

Se utilizaron dos canales:

| Código | Canal |
|---|---|
| CAN01 | Tienda física |
| CAN02 | En línea |

### 3.5. Gastos empresariales

Se prepararon 108 registros de gastos para el periodo enero-diciembre de 2025.

Los gastos corresponden a nueve conceptos, clasificados según su naturaleza operacional o financiera y su asignación directa o compartida.

Esta información permitirá analizar la rentabilidad de la empresa después de considerar los gastos de funcionamiento.

## 4. Proceso ETL en Power Query

El proceso ETL se realizó en Power BI Web.

Se importó el archivo `ventas_con_errores.csv` y se aplicaron transformaciones para solucionar problemas intencionales de calidad.

### 4.1. Eliminación de duplicados

**Problema identificado:** el archivo original contenía registros repetidos.

**Solución aplicada:** se seleccionó la columna `venta_id` y se utilizó la función **Quitar duplicados**.

**Resultado:**

- Registros originales: 1.508.
- Registros después de la limpieza: 1.500.
- Registros duplicados eliminados: 8.

El resultado de 1.500 filas fue comprobado mediante la función Contar filas de Power Query.

### 4.2. Corrección de clientes sin identificar

**Problema identificado:** cinco registros presentaban el campo `cliente_id` vacío en los datos originales.

**Solución aplicada:** mediante una transformación personalizada se reemplazaron los valores vacíos o nulos por `SIN_IDENTIFICAR`.

La transformación utilizó la función `Table.TransformColumns`.

Posteriormente, se agregó el mismo identificador a la dimensión `DimCliente` utilizando `Table.InsertRows`.

**Resultado:** se conservan las ventas sin identificación de cliente y existe un registro correspondiente en la dimensión de clientes.

### 4.3. Normalización de los canales

**Problema identificado:** cinco registros del archivo original utilizaban el código `can02` en minúsculas.

**Solución aplicada:** se convirtió la columna `canal_id` a mayúsculas.

**Resultado:** los canales se identifican mediante los códigos estandarizados `CAN01` y `CAN02`.

## 5. Importación al modelo semántico

Después de realizar las transformaciones, se incorporaron seis tablas al modelo de Power BI:

| Tabla | Función |
|---|---|
| FacVentas | Tabla de hechos de ventas |
| DimProducto | Dimensión de productos |
| DimCliente | Dimensión de clientes |
| DimCanal | Dimensión de canales |
| FactGastos | Tabla de hechos de gastos |
| DimConceptoGasto | Dimensión de conceptos de gastos |

Se revisaron los nombres de las columnas, sus tipos de datos y las relaciones principales entre las tablas.

## 6. Relaciones del modelo

Se configuraron las siguientes relaciones:

| Tabla de hechos | Tabla de dimensión | Columna |
|---|---|---|
| FacVentas | DimProducto | producto_id |
| FacVentas | DimCliente | cliente_id |
| FacVentas | DimCanal | canal_id |
| FactGastos | DimConceptoGasto | concepto_id |

Las relaciones fueron configuradas con cardinalidad muchos a uno (*:1), dirección de filtro cruzado única y estado activo.

La dimensión calendario y la distribución de gastos compartidos forman parte del desarrollo posterior del modelo financiero.

## 7. Validación de resultados

Después de ejecutar el ETL se realizó una comprobación del número de registros de ventas.

**Resultado confirmado en Power Query: 1.500 ventas.**

También se revisaron:

- Los encabezados y tipos de datos de las seis tablas.
- La estructura de los productos y sus categorías.
- Los códigos de los canales.
- La existencia del cliente `SIN_IDENTIFICAR`.
- Los conceptos de gastos y su clasificación.
- Las relaciones principales del modelo semántico.

### 7.1. Controles financieros preliminares

Los archivos de referencia originales contienen los siguientes totales:

| Indicador | Resultado de referencia |
|---|---:|
| Ventas netas | $757.610.150 COP |
| Costo de ventas | $523.628.000 COP |
| Gastos anuales | $194.809.000 COP |
| Devoluciones marcadas | 91 |
| Meses analizados | 12 |

**Nota:** Los valores financieros presentados corresponden a los datos sintéticos de referencia utilizados en el proyecto PixelZone Gaming. Estos resultados serán validados mediante las medidas DAX del modelo de Power BI para garantizar su consistencia antes del análisis financiero final.

## 8. Tratamiento financiero de los gastos

Para calcular la rentabilidad por canal y categoría se propone distribuir los gastos de la siguiente manera:

**Gastos directos:** asignación al canal correspondiente según la tabla de conceptos.

**Gastos compartidos:** distribución mensual según la participación de cada canal en las ventas netas. Posteriormente, los importes asignados a cada canal se distribuirán por categoría.

Debe evitarse relacionar directamente cada gasto mensual con todas las ventas individuales, porque esto podría duplicar los importes.

La distribución definitiva y el cálculo de impuestos deberán documentarse y validarse durante la construcción del tablero financiero.

## 9. Evidencias del proceso

Las evidencias del trabajo realizado se encuentran organizadas en la carpeta `evidencias`.

Entre ellas se incluyen:

- Captura de los pasos aplicados en Power Query.
- Captura de la validación de 1.500 ventas.
- Capturas de las dimensiones de productos, clientes y canales.
- Capturas de las tablas de gastos.
- Captura del modelo semántico y sus relaciones.

Las evidencias respaldan las transformaciones y validaciones realizadas durante la preparación de datos.

## 10. Limitaciones

- Los datos fueron generados exclusivamente para fines académicos.
- La información no representa operaciones reales.
- El análisis comprende únicamente el año 2025.
- Los gastos tienen granularidad mensual, mientras que las ventas se registran individualmente.
- La distribución definitiva de gastos y los cálculos DAX siguen pendientes.
- Los valores financieros de referencia requieren conciliación con el modelo final.

## 11. Conclusión

La preparación de los datos de PixelZone Gaming permitió construir una base analítica con información de ventas, productos, clientes, canales y gastos.

Mediante Power Query se corrigieron tres problemas de calidad: duplicados, clientes sin identificar y códigos de canales inconsistentes.

Después de la limpieza se comprobó un total de 1.500 registros de ventas y se incorporaron seis tablas al modelo semántico de Power BI.

Este proceso constituye la base para desarrollar los indicadores financieros, las medidas DAX y el tablero interactivo de rentabilidad de PixelZone Gaming.
