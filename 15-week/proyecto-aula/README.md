# PixelZone Gaming — Proyecto de Inteligencia de Negocios

## Fase 0 — Definición del proyecto

### 1. Integrantes

- José Miguel Aya Peralta
- Juan José Guzmán Bohórquez

**Asignatura:** Inteligencia de Negocios
**Institución:** Corporación Universitaria del Huila (CORHUILA)
**Docente:** Jesús Ariel González Bonilla
**Periodo académico:** 2026-B

### 2. Empresa

**Nombre:** PixelZone Gaming
**Tipo:** Empresa ficticia
**Ubicación:** Neiva, Huila, Colombia
**Sector:** Comercio de videojuegos y accesorios tecnológicos.

### 3. Descripción de la empresa

PixelZone Gaming es una empresa ficticia dedicada a la comercialización de videojuegos y accesorios para diferentes plataformas, como PlayStation, Xbox, Nintendo y PC.

Su catálogo incluye videojuegos de diferentes géneros, controles, audífonos, cargadores y otros accesorios relacionados con el entretenimiento digital.

La empresa comercializa sus productos mediante dos canales de venta:

- **Tienda física:** ventas realizadas directamente en el establecimiento.
- **Tienda en línea:** pedidos realizados por medios digitales.

PixelZone Gaming busca mejorar sus decisiones comerciales mediante el análisis de sus ventas, costos y gastos, identificando los productos y canales que generan mayores beneficios económicos.

### 4. Planteamiento del problema

Actualmente, PixelZone Gaming no cuenta con un sistema de inteligencia de negocios que permita analizar de manera integral la rentabilidad de sus productos y canales de venta.

La empresa puede conocer cuánto dinero obtiene por sus ventas, pero no dispone de información organizada que le permita determinar qué categorías generan mayores ganancias después de considerar los costos de adquisición, descuentos, devoluciones, gastos operacionales, gastos financieros e impuestos.

Esta situación dificulta la toma de decisiones relacionadas con la compra de inventario, la planificación de promociones y la distribución de recursos entre la tienda física y el canal en línea.

Por esta razón, se propone desarrollar un tablero interactivo en Power BI que permita visualizar y comparar los indicadores financieros de la empresa para identificar oportunidades de mejora en su rentabilidad.

### 5. Pregunta de negocio

¿Qué categorías de videojuegos y accesorios, y qué canales de venta generan mayor utilidad neta y margen neto porcentual en PixelZone Gaming durante el periodo comprendido entre enero y diciembre de 2025?

### 6. Objetivo general

Desarrollar un tablero interactivo de inteligencia de negocios utilizando Microsoft Power BI para analizar la rentabilidad de las categorías de productos y los canales de venta de PixelZone Gaming durante el año 2025, mediante el procesamiento de datos sintéticos de ventas, costos y gastos.

### 7. Objetivos específicos

1. Generar y depurar un conjunto de datos sintéticos que represente las operaciones comerciales y financieras de PixelZone Gaming durante 2025.

2. Construir un modelo de datos que permita relacionar las ventas, los productos, los clientes, los canales de comercialización y los gastos de la empresa.

3. Implementar medidas DAX para calcular los principales indicadores financieros, incluyendo ventas netas, utilidad bruta, utilidad operacional y utilidad neta.

4. Diseñar un tablero interactivo que permita comparar la rentabilidad por categoría de producto, canal de venta y periodo.

5. Formular recomendaciones comerciales a partir de los resultados obtenidos mediante el análisis de datos.

### 8. Indicadores clave de desempeño (KPI)

#### 8.1. KPI principal: Margen neto porcentual

El margen neto porcentual representa la proporción de las ventas netas que permanece como utilidad después de descontar los costos, gastos e impuestos considerados.

**Fórmula:**

Margen neto (%) = (Utilidad neta / Ventas netas) × 100

Este indicador permitirá comparar la rentabilidad entre las categorías de productos y los canales de comercialización.

#### 8.2. KPI de apoyo: Margen bruto porcentual

Permite identificar la rentabilidad de las ventas después de descontar el costo de adquisición de los productos vendidos.

**Fórmula:**

Utilidad bruta = Ventas netas − Costo de ventas

Margen bruto (%) = (Utilidad bruta / Ventas netas) × 100

#### 8.3. KPI de apoyo: Margen operacional porcentual

Permite evaluar la rentabilidad obtenida después de descontar los gastos necesarios para el funcionamiento de la empresa.

**Fórmula:**

Utilidad operacional = Utilidad bruta − Gastos operacionales

Margen operacional (%) = (Utilidad operacional / Ventas netas) × 100

### 9. Estructura del estado de resultados

El análisis financiero seguirá la siguiente estructura:

1. Ventas brutas
2. Menos descuentos y devoluciones
3. Igual a ventas netas
4. Menos costo de ventas
5. Igual a utilidad bruta
6. Menos gastos operacionales
7. Igual a utilidad operacional
8. Más otros ingresos y menos gastos no operacionales
9. Igual a utilidad antes de impuestos
10. Menos impuesto de renta simulado
11. Igual a utilidad neta

Esta estructura permitirá determinar la rentabilidad real dentro de los supuestos utilizados para la empresa ficticia.

### 10. Fuentes de datos

El proyecto utilizará archivos CSV con información sintética correspondiente a las operaciones de PixelZone Gaming durante el año 2025.

Se contemplan las siguientes tablas:

| Tabla | Descripción |
|---|---|
| Fact_Venta | Transacciones de venta, productos, cantidades, precios, descuentos, devoluciones y costos. |
| Fact_Gasto | Gastos mensuales operacionales y financieros. |
| Dim_Producto | Información y clasificación de productos. |
| Dim_Cliente | Identificadores y características generales de clientes ficticios. |
| Dim_Canal | Canales de comercialización de la empresa. |
| Dim_Concepto_Gasto | Clasificación de los diferentes gastos. |
| Dim_Calendario | Fechas, meses, trimestres y años para el análisis temporal. |

El conjunto de datos deberá cumplir con los siguientes requisitos:

- Mínimo 1.000 registros válidos en la tabla de ventas.
- Información correspondiente a los 12 meses de 2025.
- Al menos tres tablas de dimensiones y una tabla calendario.
- Inclusión intencional de problemas de calidad para documentar su corrección mediante Power Query.
- Valores y comportamientos comerciales coherentes con una tienda de videojuegos y accesorios.

### 11. Reglas para la asignación de gastos

Para determinar la utilidad neta por categoría y canal, los gastos se clasificarán en dos grupos:

**Gastos directos:**

Son aquellos que se pueden asociar directamente con un canal de comercialización.

Ejemplos:

- Arriendo del establecimiento: tienda física.
- Comisiones de plataformas digitales: tienda en línea.
- Costos de envío: tienda en línea.

**Gastos compartidos:**

Son aquellos utilizados para el funcionamiento general de la empresa.

Ejemplos:

- Nómina administrativa.
- Servicios de contabilidad.
- Gastos administrativos generales.

Los gastos compartidos se distribuirán proporcionalmente según la participación de cada canal en las ventas netas mensuales.

Posteriormente, los gastos asignados a cada canal se distribuirán entre sus productos y categorías utilizando su participación en las ventas netas.

La metodología deberá contemplar una regla específica para periodos sin ventas y garantizar que la suma de los gastos distribuidos coincida con el importe original.

### 12. Supuestos financieros

Por tratarse de una empresa ficticia, los datos y las condiciones financieras serán simulados.

Se establecen los siguientes supuestos iniciales:

- Todos los valores monetarios se expresarán en pesos colombianos (COP).
- El periodo de análisis será del 1 de enero al 31 de diciembre de 2025.
- Los precios de venta y costos de adquisición serán coherentes con los tipos de productos comercializados.
- Las devoluciones se considerarán dentro del cálculo de las ventas netas.
- Los gastos serán clasificados según su naturaleza operacional o no operacional.
- Se utilizará como supuesto académico una tasa de impuesto del 35 % aplicada a la utilidad gravable positiva, sin representar una liquidación tributaria real.
- Los impuestos simulados se distribuirán para permitir el análisis de rentabilidad por categoría y canal.
- Se documentarán los criterios de generación, limpieza y asignación de los datos.

### 13. Herramientas tecnológicas

**Microsoft Power BI Desktop**

Herramienta principal para desarrollar el modelo analítico, calcular indicadores y construir el tablero.

**Power Query**

Utilizado para la importación, transformación, limpieza y preparación de los datos.

**DAX**

Lenguaje de expresiones utilizado para desarrollar las medidas e indicadores financieros.

**Archivos CSV**

Formato utilizado para almacenar e intercambiar los conjuntos de datos sintéticos.

**Git y GitHub**

Herramientas utilizadas para el control de versiones, organización de archivos y almacenamiento de evidencias del proyecto.

### 14. Metodología del proyecto

El proyecto se desarrollará mediante cuatro fases.

#### Fase 0 — Definición

- Selección y descripción de la empresa.
- Identificación del problema.
- Formulación de la pregunta de negocio.
- Definición de los indicadores clave.
- Identificación de las fuentes de información.

#### Fase 1 — Datos y transformación

- Generación de datos sintéticos.
- Importación de las fuentes en Power Query.
- Identificación y corrección de problemas de calidad.
- Elaboración del diccionario de datos.
- Documentación del proceso de transformación.

#### Fase 2 — Modelo y tablero

- Construcción del modelo analítico.
- Creación de una tabla calendario.
- Definición de las relaciones entre tablas.
- Implementación de medidas DAX.
- Diseño de visualizaciones interactivas.
- Validación de los indicadores mediante pruebas.

#### Fase 3 — Análisis y presentación

- Interpretación de los resultados obtenidos.
- Identificación de hallazgos relevantes.
- Formulación de recomendaciones comerciales.
- Elaboración del informe técnico.
- Diligenciamiento del formato institucional FO-IV-159.
- Preparación de la sustentación final.

### 15. Resultados esperados

Se espera obtener un tablero de inteligencia de negocios que permita:

- Identificar las categorías de productos con mayores márgenes de utilidad.
- Comparar la rentabilidad de la tienda física y del canal en línea.
- Analizar la evolución mensual de las ventas y utilidades.
- Reconocer el impacto de los costos y gastos sobre la rentabilidad.
- Identificar oportunidades para mejorar la gestión comercial.
- Fundamentar decisiones de inventario, promociones e inversión mediante indicadores financieros.

Los hallazgos definitivos serán registrados cuando se complete el procesamiento y análisis de los datos.

### 16. Autorización y tratamiento de datos

No se requiere autorización de una empresa real, debido a que PixelZone Gaming es una organización completamente ficticia.

Los datos utilizados serán sintéticos y no contendrán información personal real de clientes.

### 17. Repositorio del proyecto

El código, los datos sintéticos, la documentación y las evidencias académicas se organizarán en el repositorio de GitHub del proyecto.

**Enlace:** https://github.com/Jose-1206/inteligencia-negocios-2026-b-g1.git

**Carpeta principal:** `15-week/proyecto-aula/`