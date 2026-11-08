# Interactive Sales Dashboard with a Three-Act Story (Power BI)

**Course:** Inteligencia de Negocios · Corte 3
**Author:** José Miguel Aya Peralta · GitHub: Jose-1206
**Tool:** Power BI Desktop

## Dashboard & insight

This dashboard analyzes 3,000 sales orders from Andina Retail, a fictional Colombian retailer, between January 2024 and October 2026, and it is built in Power BI Desktop as a three-page story: context, finding and recommendation. Each page answers one business question with the chart type that fits it: a line chart for the monthly revenue trend, sorted horizontal bars for rankings and comparisons, and a scatter plot for the relationship between revenue and margin. All bar charts start at zero, are sorted in descending order, use direct data labels instead of gridlines, and carry conclusion-style titles, while a single accent color highlights the key data point on each page. The report is interactive: two synchronized slicers (Year and Region) filter every page, the product hierarchy (Category → Subcategory → Product) and the date hierarchy (Year → Quarter → Month) support drill-down, and clicking any bar or point cross-filters the rest of the visuals. The key insight is that Technology generates 49.8% of total revenue but earns only a 14.3% margin, compared with roughly 24.5% in Home and Office, and the Caribbean region (Caribe) has the lowest margin of all regions (14.9% versus 21.0% for the rest) because its average discount is 13.1%, more than double the 6.0% of the other regions. Based on this, I recommend capping discounts in the Caribbean region at around 8% and reviewing the pricing policy for laptops and phones, which yield only about 10% margin. If the Caribbean region reached the margin of the other regions, profit would increase by approximately COP 60 million over the analyzed period. This figure is an upper bound, because it assumes that sales volume does not drop when discounts are reduced. One limitation is that the data is synthetic and 2026 covers only ten months, so the next step would be to validate the recommendation with real sales data and to test how sensitive demand is to price.

## Dashboard structure

| Page | Business question | Main visuals | Highlighted data point |
|---|---|---|---|
| 1. Contexto | How much do we sell, how does revenue evolve and which categories weigh the most? | KPI cards, line chart (Year → Quarter → Month), sorted bar chart (Category → Subcategory → Product) | Total revenue card (COP 3.88 billion) |
| 2. Hallazgo | Where do we sell a lot but earn little? | Scatter plot (revenue vs. margin by subcategory), sorted bars (margin by region), KPI card | Caribbean average discount: 13.1% |
| 3. Recomendación | How much profit do we recover if we fix it? | KPI card, sorted bars (discount by region), text box with the action | Potential profit: ≈ COP 60 million |

## Interactivity

- **Slicers:** Year and Region, synchronized across the three pages. On the pages that compare regions, the Region slicer does not filter the region bar chart (set with *Edit interactions*), so the comparison between regions is never lost.
- **Drill-down:** the bar chart uses the product hierarchy (Category → Subcategory → Product) and the line chart uses the date hierarchy (Year → Quarter → Month). A Region → City hierarchy is also available in the model.
- **Cross-filtering:** selecting a bar, a point or a month filters the other visuals on the page. I checked the behavior on every page and adjusted it with *Edit interactions* where needed.

## Key DAX measures

```
Total Ingresos = SUM(Ventas[Ingresos])

Total Utilidad = SUM(Ventas[Utilidad])

Margen % = DIVIDE([Total Utilidad], [Total Ingresos])

Descuento Promedio = AVERAGE(Ventas[Descuento_Pct])

Utilidad Potencial Caribe =
VAR IngCaribe    = CALCULATE([Total Ingresos], Ventas[Region] = "Caribe")
VAR MargenCaribe = CALCULATE([Margen %],       Ventas[Region] = "Caribe")
VAR MargenResto  = CALCULATE([Margen %],       Ventas[Region] <> "Caribe")
RETURN (MargenResto - MargenCaribe) * IngCaribe
```

A `Calendario` table (created with DAX) is related to `Ventas[Fecha]` with a one-to-many relationship and is marked as the date table.

## Data

The dataset is **synthetic**: no real company data was used. It was generated with `generar_datos.py` (random seed 2026) and saved as `ventas_andina_retail.csv`. It contains 3,000 rows with these columns: `ID_Pedido`, `Fecha`, `Region`, `Ciudad`, `Categoria`, `Subcategoria`, `Producto`, `Canal`, `Cantidad`, `Precio_Unitario`, `Descuento_Pct`, `Ingresos`, `Costo` and `Utilidad`. Amounts are in Colombian pesos (COP).

## Evidence

The `capturas/` folder contains the screenshots of my work: the three dashboard pages, the cross-filtering after selecting a bar, the drill-down in the bar and line charts, the synchronized slicers on different pages and the *Edit interactions* panel.

## Files in this folder

```
README.md
dashboard.pbix              # Power BI report
dashboard.pdf               # PDF export of the three pages
ventas_andina_retail.csv    # synthetic dataset
generar_datos.py            # script that generates the dataset
capturas/                   # screenshots (evidence)
```

To explore the report, open `dashboard.pbix` in Power BI Desktop. The slicers are cleared and the file opens on page 1.

## Limitations and next steps

- The data is synthetic, so the findings illustrate the analysis method and are not real business conclusions.
- 2026 includes data only until October, so annual totals are not comparable between 2026 and previous years.
- The potential profit assumes constant sales volume; a price-elasticity analysis would make the recommendation more realistic.
