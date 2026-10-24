# 🏔️ EverPeak Analysis

Análisis exploratorio, limpieza y segmentación de datos de ventas del retailer ficticio **EverPeak**, desarrollado en Python como parte del bootcamp de Análisis de Datos de **TripleTen**.

## 📌 Objetivo

Preparar un dataset de pedidos de comercio minorista para que sea confiable y útil en la toma de decisiones del equipo de **Inteligencia Comercial**, **Marketing** y **Finanzas**. Para eso:

1. Diagnosticar la calidad de los datos (faltantes, fechas inválidas, valores centinela, tipos incorrectos).
2. Construir un pipeline de limpieza reutilizable basado en funciones.
3. Describir el comportamiento de clientes, productos y ciudades.
4. Detectar y tratar valores atípicos.
5. Segmentar clientes y transacciones con reglas de negocio.

## 🗂️ Dataset

- **Fuente:** `everpeak_clean.csv` (TripleTen / Practicum)
- **Variables principales:**

| Columna | Descripción |
|---|---|
| `customer_id` | Identificador del cliente |
| `customer_age` | Edad del cliente |
| `order_date` | Fecha del pedido |
| `product_category` | Categoría del producto (Fashion, Sports, Grocery, Toys, etc.) |
| `price` | Precio unitario |
| `quantity` | Cantidad comprada |
| `order_value` | Valor total del pedido |
| `payment_method` | Método de pago |
| `city` / `state` | Ubicación del cliente |

## 🛠️ Tecnologías

- Python 3
- pandas · NumPy
- Matplotlib · Seaborn
- Google Colab / Jupyter Notebook

## 🔍 Contenido del análisis

### 1. Diagnóstico de calidad de datos
- Conteo de valores faltantes en `payment_method`, `city` y `state`.
- Conversión de `order_date` a fecha y detección de registros con año sospechoso (2026) o fecha ausente.
- Cardinalidad de columnas clave para distinguir IDs de variables categóricas.
- Análisis de patrones de *missingness*: proporción de `city` faltante según `payment_method`.

### 2. Imputación y limpieza
- Comparación del impacto de eliminar vs. imputar faltantes en `order_value` y `customer_age` (media vs. mediana).
- Funciones reutilizables:
  - `convertir_columnas_numericas()` – convierte varias columnas a numérico.
  - `step_strip_text()` – elimina espacios en columnas de texto.
  - `reemplazar_sentinels()` – reemplaza valores centinela (`-999`, `999`, `0`, `-1`) por `NaN`.
  - `rellenar_ausentes()` – imputa faltantes con el promedio.
  - `limpiar_df()` – **pipeline** que orquesta los pasos de limpieza en orden.

### 3. Estadística descriptiva
- Resúmenes numéricos con `describe()` por categoría (Fashion, Sports) y ciudad (New York, Los Angeles).
- Comparación media vs. mediana de `order_value` (Grocery) y `quantity` para evaluar el efecto de outliers.
- Resúmenes categóricos con `value_counts()` (frecuencias absolutas y relativas) por ciudad y categoría.

### 4. Visualización
- Histogramas de `price` y `customer_age`.
- Boxplots de `quantity` (Toys) y `order_value` (Fashion y Sports).

### 5. Detección y tratamiento de outliers
- Método **IQR** (rango intercuartílico) sobre `order_value` y `price`.
- Método **Z-score** (|z| > 3) y comparación con IQR.
- **Winsorización** de `price` en los percentiles 1 y 99 con `np.clip()`.

### 6. Segmentación con reglas de negocio
- Reglas `if / elif / else` para clasificar el volumen de compra (alto, medio, bajo).
- `np.where()` para segmentar pedidos en `high_volume` / `low_volume` (umbral: 22 unidades).
- Funciones con `apply()`:
  - `classify_volume()` – 4 segmentos por edad (> 55 años) y volumen.
  - `classify_payment()` – segmentos por uso de tarjeta (crédito/débito) y volumen.

## 💡 Hallazgos clave

- La **mediana** es más adecuada que la media para imputar `customer_age`, porque introduce menos sesgo.
- `quantity` y `order_value` presentan **valores extremos** que elevan el promedio por encima de la mediana.
- Los valores centinela y los faltantes deben tratarse **antes** de calcular métricas, de lo contrario distorsionan los resultados.
- Un pipeline basado en funciones permite **escalar** la limpieza a nuevas columnas sin reescribir código.

## ▶️ Cómo ejecutar

1. Abre `everpeak_analysis.ipynb` en [Google Colab](https://colab.research.google.com/) o Jupyter.
2. Carga los datos:
   ```python
   import pandas as pd
   url = "https://practicum-content.s3.us-west-1.amazonaws.com/datasets/everpeak_clean.csv"
   df = pd.read_csv(url)
   ```
3. Ejecuta las celdas en orden.

> **Nota:** si trabajas en JupyterLite (Pyodide), carga el CSV con `pd.read_csv(open_url(url))` usando `from pyodide.http import open_url`.

## 📁 Estructura del repositorio

```
everpeak-analysis/
├── everpeak_analysis.ipynb   # Notebook con el análisis completo
└── README.md                 # Este archivo
```

## 👤 Autor

**Germán Ricardo Villamizar Villamizar**
Analista de Datos en formación · TripleTen
GitHub: [@germanricardovilla-collab](https://github.com/germanricardovilla-collab)
