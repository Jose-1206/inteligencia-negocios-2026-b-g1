# Medidas DAX y cifras de control — PixelZone Gaming

**Proyecto:** PixelZone Gaming
**Periodo:** 1 de enero a 31 de diciembre de 2025
**Herramienta:** Microsoft Power BI Desktop
**Moneda:** pesos colombianos (COP)

## 1. Modelo analítico

Tablas de origen: `ventas` (1.500 registros), `productos` (24), `clientes` (250), `canales` (2), `gastos` (108) y `conceptos_gasto` (9). Se agregó `DimCalendario` (365 fechas) y `GastosDistribuidos` (3.396 filas de asignación).

`ventas` contiene una fila por venta sintética. `gastos` contiene un registro por concepto y mes. `GastosDistribuidos` contiene una fila por combinación válida de gasto original, mes, canal y producto; sus valores son asignaciones, no gastos adicionales.

Las dimensiones filtran los hechos mediante relaciones 1:* de dirección única. No se conectan entre sí las tablas de hechos.

### Regla de asignación

1. Los gastos directos se asignan al canal indicado por `conceptos_gasto[canal_destino]`.
2. Los gastos compartidos se reparten entre canales de acuerdo con la participación de las ventas netas mensuales.
3. Dentro del canal y mes, los gastos se distribuyen entre productos según la participación de sus ventas netas.
4. Los importes repartidos deben sumar exactamente el gasto original por mes y concepto, salvo residuos insignificantes de punto flotante.
5. La simulación utiliza ventas positivas en cada canal y mes; si un canal no tiene ventas, debe definirse otra regla antes de utilizar este método con nuevos datos.

## 2. Medidas DAX

Las siguientes expresiones reflejan las medidas utilizadas en el desarrollo. Verificar su correspondencia con las fórmulas existentes en el `.pbix` final antes de entregar.

### Ventas Netas

```dax
Ventas Netas =
SUM(ventas[venta_neta_cop])
```

### Costo de Ventas

```dax
Costo de Ventas =
SUM(ventas[costo_venta_cop])
```

### Utilidad Bruta

```dax
Utilidad Bruta =
[Ventas Netas] - [Costo de Ventas]
```

### Margen Bruto %

```dax
Margen Bruto % =
DIVIDE([Utilidad Bruta], [Ventas Netas], 0)
```

### Gastos Operacionales

```dax
Gastos Operacionales =
CALCULATE(
    SUM(GastosDistribuidos[gasto_asignado_cop]),
    GastosDistribuidos[clasificacion] = "Operacional"
)
```

### Gastos Financieros

```dax
Gastos Financieros =
CALCULATE(
    SUM(GastosDistribuidos[gasto_asignado_cop]),
    GastosDistribuidos[clasificacion] = "Financiero"
)
```

### Utilidad Operacional

```dax
Utilidad Operacional =
[Utilidad Bruta] - [Gastos Operacionales]
```

### Margen Operacional %

```dax
Margen Operacional % =
DIVIDE([Utilidad Operacional], [Ventas Netas], 0)
```

### Utilidad Antes de Impuestos

```dax
Utilidad Antes de Impuestos =
[Utilidad Operacional] - [Gastos Financieros]
```

### Tasa Impuesto

```dax
Tasa Impuesto = 0.35
```

### Impuesto de Renta

```dax
Impuesto de Renta =
VAR UtilidadAnual =
    CALCULATE(
        [Utilidad Antes de Impuestos],
        REMOVEFILTERS(DimCalendario),
        REMOVEFILTERS(productos),
        REMOVEFILTERS(canales)
    )
VAR VentasAnuales =
    CALCULATE(
        [Ventas Netas],
        REMOVEFILTERS(DimCalendario),
        REMOVEFILTERS(productos),
        REMOVEFILTERS(canales)
    )
VAR ImpuestoAnual =
    ROUND(MAX(0, UtilidadAnual) * [Tasa Impuesto], 0)
VAR ParticipacionVentas =
    DIVIDE([Ventas Netas], VentasAnuales, 0)
RETURN
    ImpuestoAnual * ParticipacionVentas
```

**Supuesto:** el 35 % es una tarifa ficticia para fines académicos, no una liquidación tributaria real. Se aplica a la utilidad anual antes de impuestos positiva y se distribuye por participación en ventas netas. Evitar visualizaciones que pretendan asignar el impuesto por cliente o día, porque el reparto de gastos se realizó por producto, canal y mes.

### Utilidad Neta

```dax
Utilidad Neta =
[Utilidad Antes de Impuestos] - [Impuesto de Renta]
```

### Margen Neto %

```dax
Margen Neto % =
DIVIDE([Utilidad Neta], [Ventas Netas], 0)
```

### Total Gastos Distribuidos

```dax
Total Gastos Distribuidos =
SUM(GastosDistribuidos[gasto_asignado_cop])
```

### Gastos Originales

```dax
Gastos Originales =
SUM(gastos[valor_cop])
```

### Diferencia Reparto

```dax
Diferencia Reparto =
ROUND([Gastos Originales] - [Total Gastos Distribuidos], 2)
```

## 3. Cifras de control anual — sin filtros

| Medida | Valor de control |
|---|---:|
| Ventas Netas | $757.610.150 |
| Costo de Ventas | $523.628.000 |
| Utilidad Bruta | $233.982.150 |
| Margen Bruto % | 30,88 % |
| Gastos Operacionales | $188.977.000 |
| Gastos Financieros | $5.832.000 |
| Gastos Originales | $194.809.000 |
| Total Gastos Distribuidos | $194.809.000 |
| Diferencia Reparto | $0,00 |
| Utilidad Operacional | $45.005.150 |
| Utilidad Antes de Impuestos | $39.173.150 |
| Impuesto de Renta (35 %, redondeado al peso) | $13.710.603 |
| Utilidad Neta (con impuesto redondeado) | $25.462.547 |
| Margen Neto % | 3,36 % |

**Nota de redondeo:** sin redondear previamente el impuesto, $39.173.150 × 35 % = $13.710.602,50 y la utilidad neta es $25.462.547,50, que una tarjeta puede presentar como $25.462.548. La política documentada aquí es redondear el impuesto anual a $13.710.603 *antes* de restarlo, de modo que la utilidad anual sea $25.462.547. Verificar y uniformar la medida real del archivo `.pbix`.

## 4. Conciliación y validación

- Los 108 gastos originales suman $194.809.000.
- Las 3.396 filas de asignación suman el mismo importe.
- La matriz mensual de Power BI mostró diferencia $0,00 para cada uno de los 12 meses y para el total del año.
- Antes de la entrega, expandir también los conceptos en la matriz para comprobar la conciliación mensual por concepto.
- Se verificaron tres selecciones en el tablero (ver `pruebas-clic.md`).

## 5. Interpretación financiera

La utilidad bruta anual es $233.982.150 (margen bruto 30,88 %), pero los gastos operacionales consumen $188.977.000. Los gastos financieros ascienden a $5.832.000, por lo que el margen neto simulado se reduce al 3,36 %. La diferencia entre venta y utilidad muestra la necesidad de evaluar los gastos, no solamente los ingresos.
