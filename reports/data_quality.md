# Informe de calidad de datos – Instacart

**Actividad:** GA1-220501094-01-AA1-EV04 – Fundamentos en Pandas
**Notebook:** [`notebooks/data_quality.ipynb`](../notebooks/data_quality.ipynb)
**Datos:** los cinco CSV originales en `data/raw/` (separador `;`)

En esta fase los datos se exploraron sin modificarlos. Este informe resume los problemas de calidad encontrados, que se corrigen en [`notebooks/pandas_labs.ipynb`](../notebooks/pandas_labs.ipynb) (ver [`data_processing.md`](data_processing.md)).

## 1. Inventario de archivos

| Archivo | Filas | Columnas | Clave | ¿Clave única? |
|---|---:|---:|---|---|
| `instacart_orders.csv` | 478.967 | 6 | `order_id` | No (15 repetidos) |
| `products.csv` | 49.694 | 4 | `product_id` | Sí |
| `aisles.csv` | 134 | 2 | `aisle_id` | Sí |
| `departments.csv` | 21 | 2 | `department_id` | Sí |
| `order_products.csv` | 4.545.007 | 4 | `order_id` + `product_id` | — |

## 2. Relaciones entre tablas

| Relación | Campo común |
|---|---|
| `orders` ↔ `order_products` | `order_id` |
| `products` ↔ `order_products` | `product_id` |
| `products` ↔ `aisles` | `aisle_id` |
| `products` ↔ `departments` | `department_id` |

`order_products` es la tabla puente entre pedidos y productos; `aisles` y `departments` traducen identificadores a nombres.

## 3. Valores ausentes

| Tabla | Columna | Ausentes | % | Observación |
|---|---|---:|---:|---|
| `orders` | `days_since_prior_order` | 28.819 | 6,02 % | Todos corresponden a `order_number == 1` (primer pedido: no hay pedido anterior) |
| `products` | `product_name` | 1.258 | 2,53 % | Productos sin nombre en el catálogo |
| `order_products` | `add_to_cart_order` | 836 | 0,02 % | Concentrados en 70 pedidos de 65 artículos o más |

## 4. Duplicados

- `orders` tiene **15 filas completamente duplicadas**; por eso hay 478.967 filas pero solo 478.952 pedidos distintos.
- `products`, `aisles` y `departments` no tienen filas duplicadas ni claves repetidas.

## 5. Tipos de dato

- `days_since_prior_order` y `add_to_cart_order` son enteros, pero se leen como `float64` porque contienen ausentes.
- Los textos (`product_name`, `aisle`, `department`) aparecen como `str` en pandas 3.

## 6. Limitaciones

- Los datos no incluyen precio, cantidad por artículo ni datos del cliente, así que no permiten analizar gasto ni perfiles demográficos.
- `order_dow` es un código numérico (0 a 6); se asume 0 = domingo, según el material del curso.
- El archivo de pedidos solo recoge parte del historial de cada cliente (el `order_number` promedio es 17,2, pero hay unos 3 pedidos por usuario).

## 7. Tareas para la limpieza

| Problema | Acción prevista |
|---|---|
| 15 filas duplicadas en `orders` | Eliminar |
| Columnas enteras leídas como `float64` | Convertir a tipos enteros (nullable donde hay ausentes) |
| Ausentes en `days_since_prior_order` | Conservar (significan "primer pedido") |
| Ausentes en `product_name` | Rellenar con `'Unknown'` |
| Ausentes en `add_to_cart_order` | Investigar y conservar |
