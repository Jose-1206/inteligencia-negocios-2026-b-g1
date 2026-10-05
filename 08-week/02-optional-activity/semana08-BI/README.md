# Semana 8 - Limpieza de datos con Power Query

## Dataset utilizado

Se utilizó un dataset público descargado del portal Datos Abiertos Colombia.

Archivo:
dataset_original.csv

## Problemas encontrados

Durante el análisis inicial se identificaron problemas de calidad:

- Valores duplicados.
- Diferentes formatos de texto.
- Tipos de datos incorrectos.
- Valores vacíos.

## Transformaciones aplicadas

### 1. Cambio de tipos de datos

Se corrigieron columnas numéricas y fechas que estaban almacenadas como texto.

Problema solucionado:
Permitir cálculos y análisis correctos.

### 2. Eliminación de duplicados

Se eliminaron registros repetidos.

Problema solucionado:
Evitar contar información repetida.

### 3. Limpieza de texto

Se aplicó formato uniforme a campos de texto.

Problema solucionado:
Unificar valores como ciudades escritas con diferentes formatos.

### 4. Columna condicional

Se creó una nueva clasificación según el valor de ventas.

Problema solucionado:
Facilitar análisis por categorías.

### 5. Filtros

Se eliminaron registros incompletos.

Problema solucionado:
Mejorar la calidad del dataset final.

## Actualización

Las transformaciones quedan guardadas en Power Query como una receta reutilizable. Si llegan nuevos datos con la misma estructura, solamente se actualiza la consulta y se aplican automáticamente los mismos pasos.
