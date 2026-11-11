# Pruebas del clic — PixelZone Gaming

**Periodo analizado:** enero–diciembre de 2025
**Responsable de las pruebas del tablero:** José Miguel Aya Peralta
**Estado:** las tres pruebas fueron confirmadas como coincidentes en Power BI por el responsable.

## Objetivo

Verificar que los segmentadores por canal y periodo filtren correctamente las medidas de ventas netas, utilidad neta y margen neto. Contrastar los valores mostrados por el tablero con cifras de control calculadas de los CSV y del reparto documentado de gastos.

## Procedimiento

1. Abrir la página `Rentabilidad General` del informe.
2. Limpiar selecciones previas y aplicar los filtros indicados.
3. Registrar los tres KPI del tablero y contrastarlos con las cifras de control.
4. Guardar una captura donde se vean la selección activa y las tarjetas.
5. Repetir el proceso para los tres casos.

## Valores de control y resultados

| Prueba | Filtros | Ventas netas | Utilidad neta | Margen neto | Estado |
|---|---|---:|---:|---:|---|
| 1 | Tienda física, todo 2025 | $428.609.000 | $19.954.268 | 4,66 % | Coincide, según validación del responsable |
| 2 | En línea, todo 2025 | $329.001.150 | $5.508.279 | 1,67 % | Coincide, según validación del responsable |
| 3 | Tienda física, octubre–diciembre 2025 | $108.578.250 | $6.978.154 | 6,43 % | Coincide, según validación del responsable |

El modelo redondea importes monetarios a pesos para su presentación y muestra los márgenes con dos decimales. Las utilidades se obtienen de ventas netas menos costos de venta, gastos asignados y una porción del impuesto anual simulado (35 %) proporcional a las ventas netas.

## Comprobaciones complementarias

- Los gastos originales de 2025 suman $194.809.000.
- La suma de `GastosDistribuidos[gasto_asignado_cop]` es $194.809.000.
- La matriz `AnioMes` de enero a diciembre muestra diferencia de gastos $0,00 en cada mes y en el total.
- Verificar también los conceptos al expandir cada mes, y conservar captura de esa conciliación si está disponible.

## Evidencias fotográficas

Las capturas de los tres casos deben estar ubicadas en:

- `capturas/pruebas/prueba-1-tienda-fisica.png`
- `capturas/pruebas/prueba-2-en-linea.png`
- `capturas/pruebas/prueba-3-trimestre-final.png`

Si se guardaron con otros nombres, actualizar esta lista para reflejar los archivos realmente existentes.

## Conclusión

En los tres casos probados, los filtros de Power BI permiten reproducir los indicadores esperados con el método de distribución de gastos documentado. La tienda física presenta una utilidad neta y un margen neto superiores a los del canal en línea para el año 2025. Esta conclusión se limita al conjunto de datos sintéticos y a los supuestos de asignación e impuestos del proyecto.
