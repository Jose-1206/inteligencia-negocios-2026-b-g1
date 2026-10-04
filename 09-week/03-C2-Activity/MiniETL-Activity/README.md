# Mini ETL and DAX Measures

## Description

This project was developed as part of the Business Intelligence course.

The objective of the activity is to perform a small ETL process using Power Query, clean a sales dataset, load the information into Power BI, create DAX measures, and demonstrate the use of filter context.

## Dataset

The project uses a CSV file called `ventas_sucias.csv`.

The dataset contains sales information with the following fields:

- ID_Venta
- Fecha
- Producto
- Categoria
- Cantidad
- Precio_Unitario
- Ciudad
- Vendedor

The original dataset intentionally contained duplicate records, inconsistent text values, unnecessary spaces, and different text formats.

## ETL process

The dataset was imported into Power BI using Power Query.

The following transformations were applied:

1. Data types were corrected for numeric, date, and text columns.
2. Duplicate records were removed.
3. Unnecessary spaces were removed from the `Categoria` column.
4. Category values were standardized using the same text format.
5. A conditional column called `Tipo_Venta` was created.
6. A custom column called `Total` was created by multiplying `Cantidad` by `Precio_Unitario`.

The conditional column uses the following rule:

- If `Cantidad >= 5`, the result is `Venta alta`.
- Otherwise, the result is `Venta normal`.

The `Total` column was calculated as:

```text
Cantidad * Precio_Unitario
```

## DAX Measures

Three DAX measures were created in Power BI.

### Total Sales

```DAX
Total Ventas =
SUM(ventas_sucias[Total])
```

This measure calculates the total value of all sales.

### Average Sales

```DAX
Promedio Ventas =
AVERAGE(ventas_sucias[Total])
```

This measure calculates the average sales value.

### Average Price per Unit

```DAX
Precio Promedio por Unidad =
DIVIDE(
    SUM(ventas_sucias[Total]),
    SUM(ventas_sucias[Cantidad]),
    0
)
```

This measure calculates the average value per unit sold and uses `DIVIDE` to safely handle division by zero.

## Filter Context

A slicer using the `Categoria` field was added to the Power BI report.

When a category such as `Tecnologia`, `Oficina`, or `Papeleria` is selected, the `Total Ventas` measure changes automatically.

This demonstrates that DAX measures respond to the active filter context in Power BI.

A column chart was also created to compare total sales between categories.

## ETL steps & measures

The sales dataset was imported from a CSV file and transformed using Power Query. First, the data types were corrected for date, numeric, and text columns. Duplicate sales records were removed to improve data quality. Text values were cleaned by removing unnecessary spaces and standardizing category names. A conditional column was created to classify sales according to the quantity sold. A custom column was also created to calculate the total value of each sale. After loading the cleaned dataset into the Power BI model, three DAX measures were created: Total Sales using SUM, Average Sales using AVERAGE, and Average Price per Unit using DIVIDE. Finally, a category slicer was added to demonstrate how the Total Sales measure changes according to the active filter context.

## Evidence

### Original dataset

![Original dataset](evidence/01-dataset-original.png)

### Power Query

![Power Query](evidence/02-power-query.png)

### Applied steps

![Applied steps](evidence/03-pasos-aplicados.png)

### Cleaned dataset

![Cleaned dataset](evidence/04-datos-limpios.png)

### DAX measures

![DAX measures](evidence/05)

### Filter context

![Filter context](evidence/06)

## Technologies

- Microsoft Power BI
- Power Query
- DAX
- CSV
- Git
- GitHub

## Conclusion

The activity demonstrates a complete basic Business Intelligence workflow. The original data was cleaned and transformed using Power Query, loaded into the Power BI model, and analyzed using DAX measures. The use of a slicer also demonstrates how measures dynamically respond to filter context.