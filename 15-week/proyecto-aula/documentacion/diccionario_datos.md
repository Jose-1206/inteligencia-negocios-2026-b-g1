
# Diccionario de Datos — PixelZone Gaming

**Materia:** Inteligencia de Negocios
**Proyecto:** PixelZone Gaming
**Integrantes:** José Miguel Aya Peralta y Juan José Guzmán Bohórquez
**Responsable de datos y ETL:** Juan José Guzmán Bohórquez
**Periodo de análisis:** Enero a diciembre de 2025
**Herramienta:** Power BI Web — Power Query
**Moneda:** Pesos colombianos (COP)

---

## 1. Descripción general

PixelZone Gaming es una tienda ficticia dedicada a la comercialización de videojuegos, consolas, accesorios y periféricos.

El objetivo del proyecto es analizar las ventas, los costos y los gastos de la empresa para identificar qué categorías de productos y canales de venta generan mayor rentabilidad.

Para ello se construyó un conjunto de datos sintéticos correspondiente al año 2025, organizado en seis tablas importadas desde archivos CSV.

Los datos fueron procesados mediante Power Query, donde se realizaron operaciones de limpieza, transformación y validación.

Posteriormente, las tablas se incorporaron al modelo semántico de Power BI para permitir el análisis financiero.

---

## 2. Resumen de las tablas

| Tabla | Registros | Descripción |
|---|---:|---|
| FacVentas | 1.500 | Transacciones de ventas después de la limpieza |
| DimProducto | 24 | Catálogo de productos |
| DimCliente | 251 esperados | Información de clientes y registro sin identificar |
| DimCanal | 2 | Canales de venta |
| FactGastos | 108 esperados | Gastos mensuales por concepto |
| DimConceptoGasto | 9 | Clasificación de gastos |

**Nota:** Los 1.500 registros de ventas, 24 productos, 2 canales y 9 conceptos fueron comprobados en Power Query. Los totales de clientes y gastos se basan en los archivos originales y deben confirmarse en el modelo final.

---

## 3. Diccionario de datos por tabla

### 3.1. Tabla FacVentas

**Tipo:** Tabla de hechos
**Clave principal:** venta_id
**Registros:** 1.500
**Granularidad:** Una fila por registro único de venta.

Esta tabla contiene las operaciones comerciales de PixelZone Gaming durante el año 2025.

| Campo | Tipo | Descripción |
|---|---|---|
| venta_id | Texto | Identificador único de la venta |
| fecha | Fecha | Fecha en que se realizó la venta |
| producto_id | Texto | Identificador del producto vendido |
| cliente_id | Texto | Identificador del cliente |
| canal_id | Texto | Identificador del canal de venta |
| cantidad | Número entero | Cantidad de unidades vendidas |
| precio_unitario_cop | Número entero | Precio unitario del producto en COP |
| descuento_pct | Número entero | Porcentaje de descuento aplicado |
| devuelta | Número entero | Indica si la venta fue devuelta: 1 = Sí, 0 = No |
| venta_bruta_cop | Número entero | Valor de la venta antes de descuentos y devoluciones |
| descuento_cop | Número entero | Valor del descuento aplicado |
| devolucion_cop | Número entero | Importe asociado a la devolución |
| venta_neta_cop | Número entero | Valor de la venta después de descuentos y devoluciones |
| costo_venta_cop | Número entero | Costo de los productos vendidos, ajustado por devoluciones |

#### Reglas de negocio

- Las ventas tienen un identificador único.
- Los clientes sin identificación se registran como SIN_IDENTIFICAR.
- Los códigos de canales se normalizaron a mayúsculas.
- Las devoluciones se descuentan del valor de las ventas.
- Los valores monetarios se expresan en pesos colombianos.

#### Cálculos financieros

Venta bruta:

`venta_bruta_cop = cantidad * precio_unitario_cop`

Venta neta:

`venta_neta_cop = venta_bruta_cop - descuento_cop - devolucion_cop`

Utilidad bruta:

`Utilidad bruta = Ventas netas - Costo de ventas`

---

### 3.2. Tabla DimProducto

**Tipo:** Tabla de dimensión
**Clave principal:** producto_id
**Registros:** 24
**Granularidad:** Una fila por producto.

Contiene el catálogo de productos comercializados por PixelZone Gaming.

| Campo | Tipo | Descripción |
|---|---|---|
| producto_id | Texto | Identificador único del producto |
| producto | Texto | Nombre del producto |
| categoria | Texto | Categoría comercial del producto |
| precio_lista_cop | Número entero | Precio de referencia del producto |
| costo_unitario_cop | Número entero | Costo unitario de adquisición |

#### Categorías de productos

1. Consolas
2. Videojuegos
3. Accesorios
4. Periféricos

Esta tabla permite identificar cuáles categorías producen mayores ventas y márgenes de rentabilidad.

---

### 3.3. Tabla DimCliente

**Tipo:** Tabla de dimensión
**Clave principal:** cliente_id
**Registros esperados:** 251
**Granularidad:** Una fila por cliente.

Contiene información sintética de los clientes asociados a las ventas.

| Campo | Tipo | Descripción |
|---|---|---|
| cliente_id | Texto | Identificador único del cliente |
| cliente | Texto | Nombre ficticio del cliente |
| ciudad | Texto | Ciudad de procedencia del cliente |

#### Tratamiento de clientes sin identificar

Durante la limpieza de ventas se encontraron registros sin identificador de cliente.

Para conservar estas transacciones, los identificadores vacíos se reemplazaron por:

`SIN_IDENTIFICAR`

También se agregó a DimCliente el siguiente registro:

| cliente_id | cliente | ciudad |
|---|---|---|
| SIN_IDENTIFICAR | Cliente sin identificar | Desconocida |

Esto permite relacionar correctamente las ventas que no cuentan con un cliente identificado.

---

### 3.4. Tabla DimCanal

**Tipo:** Tabla de dimensión
**Clave principal:** canal_id
**Registros:** 2
**Granularidad:** Una fila por canal.

Contiene los canales comerciales utilizados por PixelZone Gaming.

| Campo | Tipo | Descripción |
|---|---|---|
| canal_id | Texto | Identificador único del canal |
| canal | Texto | Nombre del canal de venta |

#### Canales registrados

| Código | Canal |
|---|---|
| CAN01 | Tienda física |
| CAN02 | En línea |

Esta tabla permite comparar las ventas y la rentabilidad de los canales físicos y digitales.

---

### 3.5. Tabla FactGastos

**Tipo:** Tabla de hechos
**Clave principal:** gasto_id
**Registros esperados:** 108
**Granularidad:** Un registro por concepto de gasto y mes.

Registra los gastos operacionales y financieros de PixelZone Gaming durante el año 2025.

| Campo | Tipo | Descripción |
|---|---|---|
| gasto_id | Texto | Identificador único del gasto |
| fecha | Fecha | Fecha de referencia del gasto mensual |
| concepto_id | Texto | Identificador del concepto asociado |
| valor_cop | Número entero | Valor del gasto expresado en COP |

#### Características

- Los gastos están organizados mensualmente.
- Cada registro corresponde a un concepto de gasto.
- Los gastos pueden ser operacionales o financieros.
- Algunos gastos corresponden directamente a un canal.
- Otros gastos son compartidos y requieren distribución.

Esta tabla permite calcular los gastos de operación y apoyar el análisis posterior de utilidad neta.

Los gastos mensuales no deben sumarse una vez por cada venta, porque esto provocaría duplicación de importes.

---

### 3.6. Tabla DimConceptoGasto

**Tipo:** Tabla de dimensión
**Clave principal:** concepto_id
**Registros:** 9
**Granularidad:** Una fila por concepto de gasto.

Contiene las categorías utilizadas para clasificar los gastos empresariales.

| Campo | Tipo | Descripción |
|---|---|---|
| concepto_id | Texto | Identificador único del concepto |
| concepto | Texto | Nombre del concepto de gasto |
| tipo_asignacion | Texto | Indica si el gasto es directo o compartido |
| canal_destino | Texto | Canal al cual se asigna directamente el gasto; puede quedar vacío |
| clasificacion | Texto | Identifica si el gasto es operacional o financiero |

#### Conceptos de gastos registrados

| Código | Concepto | Asignación | Canal | Clasificación |
|---|---|---|---|---|
| G01 | Arriendo | Directo | CAN01 | Operacional |
| G02 | Nómina comercial | Compartido | — | Operacional |
| G03 | Servicios públicos | Directo | CAN01 | Operacional |
| G04 | Publicidad | Compartido | — | Operacional |
| G05 | Plataforma web | Directo | CAN02 | Operacional |
| G06 | Comisiones online | Directo | CAN02 | Operacional |
| G07 | Envíos subsidiados | Directo | CAN02 | Operacional |
| G08 | Administración | Compartido | — | Operacional |
| G09 | Intereses bancarios | Compartido | — | Financiero |

#### Tipos de asignación

**Directo:** gasto asociado específicamente a un canal de venta.

**Compartido:** gasto que beneficia a varios canales y debe distribuirse mediante una regla documentada.

La distribución definitiva de estos gastos se realizará en el modelo financiero para evitar duplicaciones.

---

## 4. Relaciones entre tablas

Las siguientes relaciones fueron revisadas en Power BI:

| Tabla origen | Campo | Tabla destino | Campo | Cardinalidad |
|---|---|---|---|---|
| FacVentas | producto_id | DimProducto | producto_id | Muchos a uno (*:1) |
| FacVentas | cliente_id | DimCliente | cliente_id | Muchos a uno (*:1) |
| FacVentas | canal_id | DimCanal | canal_id | Muchos a uno (*:1) |
| FactGastos | concepto_id | DimConceptoGasto | concepto_id | Muchos a uno (*:1) |

Las relaciones tienen dirección de filtro cruzado única y se configuraron como activas.

El modelo permitirá analizar las ventas utilizando las dimensiones de productos, clientes y canales.

Para la entrega final se deberá incorporar una dimensión calendario compartida y establecer una metodología para distribuir gastos por canal y categoría.

---

## 5. Proceso ETL en Power Query

El proceso ETL consistió en importar, depurar y transformar los archivos CSV.

### 5.1. Extracción

Se importaron las siguientes fuentes:

- Ventas
- Productos
- Clientes
- Canales
- Gastos
- Conceptos de gastos

Los archivos corresponden a datos sintéticos generados para el periodo enero-diciembre de 2025.

### 5.2. Transformación

Se realizaron tres correcciones principales en FacVentas.

#### Corrección 1. Eliminación de duplicados

**Problema:** Existían registros de ventas con identificadores repetidos.

**Solución:** Se aplicó la opción Quitar duplicados usando venta_id.

**Resultado:** La tabla quedó con 1.500 registros únicos de ventas.

#### Corrección 2. Clientes sin identificar

**Problema:** Algunos registros de ventas no tenían identificador de cliente.

**Solución:** Se reemplazaron los valores nulos o vacíos por SIN_IDENTIFICAR.

También se agregó el correspondiente registro a DimCliente.

**Resultado:** Los registros de ventas sin cliente conocido conservan una referencia válida para el modelo.

#### Corrección 3. Normalización de canales

**Problema:** Algunos códigos de canales tenían diferencias entre mayúsculas y minúsculas.

**Solución:** Se aplicó la transformación de texto a mayúsculas sobre canal_id.

**Resultado:** Los canales quedaron identificados mediante CAN01 y CAN02.

### 5.3. Carga

Las seis tablas se cargaron al modelo semántico de Power BI web.

Se revisaron sus encabezados, tipos de datos y relaciones principales.

---

## 6. Validaciones de calidad

| Validación | Resultado |
|---|---|
| Ventas después de ETL | 1.500 registros confirmados |
| Productos | 24 registros confirmados |
| Categorías de productos | 4 |
| Canales de venta | 2 registros confirmados |
| Conceptos de gastos | 9 registros confirmados |
| Clientes sin identificar | Registro SIN_IDENTIFICAR agregado |
| Relación ventas-productos | Revisada |
| Relación ventas-clientes | Revisada |
| Relación ventas-canales | Revisada |
| Relación gastos-conceptos | Revisada |

**Validaciones pendientes:**

- Confirmar el conteo final de DimCliente.
- Confirmar el conteo final de FactGastos.
- Verificar que todos los identificadores de las tablas de hechos tengan correspondencia en las dimensiones.
- Incorporar la dimensión calendario.
- Validar la distribución financiera de gastos compartidos.

---

## 7. Consideraciones financieras

El objetivo final del modelo es calcular la rentabilidad de PixelZone Gaming.

Los indicadores contemplados son:

### Ventas netas

Ventas brutas menos descuentos y devoluciones.

### Utilidad bruta

Ventas netas menos costo de ventas.

### Utilidad operacional

Utilidad bruta menos gastos operacionales asignados.

### Utilidad neta

Utilidad después de gastos operacionales, gastos financieros e impuestos, según las reglas de simulación documentadas.

### Margen neto porcentual

`Margen neto (%) = Utilidad neta / Ventas netas * 100`

Los archivos CSV contienen ventas, costos y gastos, pero no una tasa tributaria real. Cualquier tasa de impuestos que se utilice posteriormente deberá declararse como un supuesto académico.

Estos indicadores todavía requieren las medidas DAX y la distribución final de gastos.

---


## 8. Evidencias del proyecto

Durante el desarrollo del proyecto PixelZone Gaming se recopilaron evidencias del proceso de extracción, transformación y carga de datos (ETL), realizado mediante Power Query en Power BI Web.

Estas evidencias permiten comprobar la preparación de los datos, la corrección de problemas de calidad y la estructura de las tablas utilizadas en el modelo de inteligencia de negocios.

### 8.1. Evidencia del proceso ETL de ventas

![Proceso ETL de ventas](../evidencias/Evidencia_ETL_Ventas.png)

**Descripción:** La captura presenta la tabla `FacVentas` y los pasos aplicados en Power Query.

Se realizaron tres transformaciones principales:

1. **Eliminación de duplicados:** se eliminaron registros repetidos utilizando el identificador `venta_id`.
2. **Corrección de clientes:** se reemplazaron identificadores nulos o vacíos por `SIN_IDENTIFICAR`.
3. **Normalización de canales:** se convirtieron los códigos de `canal_id` a mayúsculas para unificar su formato.

Estas transformaciones permitieron mejorar la calidad y consistencia de los registros utilizados en el análisis.

### 8.2. Evidencia de la dimensión de productos

![Dimensión de productos](../evidencias/Evidencia_DimProducto.png)

**Descripción:** Se presenta la tabla `DimProducto`, compuesta por 24 productos distribuidos en cuatro categorías: Consolas, Videojuegos, Accesorios y Periféricos.

La tabla contiene identificadores, nombres, categorías, precios de lista y costos unitarios de los productos.

Esta información permite clasificar las ventas y analizar la rentabilidad por categoría.

### 8.3. Evidencia de la dimensión de clientes

![Dimensión de clientes](../evidencias/Evidencia_DimCliente.png)

**Descripción:** La captura presenta la tabla `DimCliente`, que contiene los campos `cliente_id`, `cliente` y `ciudad`.

Durante el proceso ETL se agregó un registro denominado `SIN_IDENTIFICAR`, con el objetivo de conservar las ventas que originalmente no contaban con un identificador de cliente.

Esta transformación permite mantener la integridad de las relaciones entre la dimensión de clientes y los registros de ventas.

### 8.4. Evidencia de los canales de venta

![Canales de venta](../evidencias/Evidencia_DimCanal.png)

**Descripción:** La captura presenta la tabla `DimCanal`, compuesta por los dos canales comerciales de PixelZone Gaming:

- `CAN01`: Tienda física.
- `CAN02`: En línea.

Esta dimensión permite segmentar las ventas y comparar el desempeño comercial y financiero de ambos canales.

### 8.5. Evidencia de los gastos empresariales

![Tabla de gastos](../evidencias/Evidencia_FactGastos.png)

**Descripción:** Se presenta la tabla `FactGastos`, que contiene los registros de gastos asociados a los diferentes conceptos empresariales.

Sus campos principales son `gasto_id`, `fecha`, `concepto_id` y `valor_cop`.

Los gastos se encuentran registrados mensualmente y permiten incorporar costos operacionales y financieros al análisis de rentabilidad de la empresa.

### 8.6. Evidencia de los conceptos de gasto

![Conceptos de gastos](../evidencias/Evidencia_Conceptos_Gasto.png)

**Descripción:** La captura muestra la tabla `DimConceptoGasto`, que contiene nueve conceptos de gastos empresariales.

Entre estos se encuentran el arriendo, la nómina, los servicios públicos, la publicidad, la plataforma web, las comisiones, los envíos, la administración y los intereses bancarios.

La tabla también incluye información sobre el tipo de asignación, el canal de destino y la clasificación de cada gasto.

Esta información es fundamental para distribuir los gastos directos y compartidos en el modelo financiero.

### 8.7. Evidencia de validación del número de ventas

![Validación de ventas](../evidencias/Evidencia_Conteo_Ventas.png)

**Descripción:** Mediante la función `Contar filas` de Power Query se comprobó que la tabla de ventas contiene 1.500 registros después de eliminar los identificadores duplicados.

Este resultado cumple el requisito mínimo de 1.000 registros establecido para el proyecto de aula.

La operación de conteo se utilizó únicamente como validación y posteriormente se eliminó el paso temporal `Filas contadas`, conservando la estructura original de la tabla.

### 8.8. Evidencia del modelo de datos

![Modelo de datos](../evidencias/Evidencia_Modelo.png)

**Descripción:** La captura presenta el modelo semántico desarrollado en Power BI Web con las seis tablas utilizadas en el proyecto.

Se configuraron y revisaron las siguientes relaciones:

| Tabla de hechos | Tabla de dimensión | Campo relacionado | Cardinalidad |
|---|---|---|---|
| FacVentas | DimProducto | producto_id | Muchos a uno (*:1) |
| FacVentas | DimCliente | cliente_id | Muchos a uno (*:1) |
| FacVentas | DimCanal | canal_id | Muchos a uno (*:1) |
| FactGastos | DimConceptoGasto | concepto_id | Muchos a uno (*:1) |

Las relaciones utilizan dirección de filtro cruzado única y se configuraron como activas.

Estas conexiones constituyen la base del modelo analítico. La dimensión calendario y las reglas de distribución de gastos se incorporarán durante el desarrollo del modelo financiero definitivo.

---

### 8.9. Resumen de evidencias recopiladas

| N.º | Evidencia | Propósito |
|---|---|---|
| 1 | Evidencia_ETL_Ventas.png | Demostrar las transformaciones realizadas en Power Query |
| 2 | Evidencia_DimProducto.png | Documentar los productos y categorías |
| 3 | Evidencia_DimCliente.png | Mostrar la estructura de clientes y la corrección de identificadores |
| 4 | Evidencia_DimCanal.png | Identificar los canales comerciales |
| 5 | Evidencia_FactGastos.png | Documentar los gastos empresariales |
| 6 | Evidencia_Conceptos_Gasto.png | Mostrar la clasificación de los conceptos de gasto |
| 7 | Evidencia_Conteo_Ventas.png | Comprobar los 1.500 registros de ventas |
| 8 | Evidencia_Modelo.png | Mostrar las relaciones entre las tablas |


---

## 9. Limitaciones de los datos

- Los registros son sintéticos y no representan transacciones reales.
- El periodo analizado corresponde exclusivamente a 2025.
- Los gastos están registrados mensualmente, mientras que las ventas tienen fechas de transacción.
- La distribución de gastos compartidos requiere reglas adicionales en el modelo.
- La utilidad neta todavía debe calcularse y validarse en Power BI.
- Los resultados financieros tendrán carácter académico y no contable oficial.

---

## 10. Conclusión

Se preparó un conjunto de datos sintéticos de PixelZone Gaming mediante Power Query en Power BI web.

El proceso permitió integrar seis tablas, corregir problemas de calidad en las ventas y establecer relaciones entre hechos y dimensiones.

La limpieza de datos permitió obtener 1.500 registros de ventas sin identificadores duplicados, normalizar los códigos de canales y conservar las transacciones correspondientes a clientes sin identificar.

El diccionario documenta la estructura de las fuentes y constituye una base para el desarrollo del modelo analítico, las medidas DAX y el tablero interactivo de rentabilidad.


**Proyecto académico:** Inteligencia de Negocios — PixelZone Gaming.
