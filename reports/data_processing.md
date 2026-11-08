# Informe de procesamiento de datos – Instacart

**Actividad:** GA1-220501094-01-AA1-EV06 – Limpieza, análisis y estructuración de datos con Pandas
**Notebook:** [`notebooks/pandas_labs.ipynb`](../notebooks/pandas_labs.ipynb)
**Entrada:** los cinco CSV originales en `data/raw/`. El diagnóstico previo está en [`data_quality.md`](data_quality.md).
**Salida:** archivos en `data/processed/`

## 1. Limpieza (Lección 2)

### Decisiones

| Tabla | Problema | Decisión | Justificación |
|---|---|---|---|
| `orders` | 15 filas duplicadas | Eliminar | Todas el miércoles a las 2 a. m.: fallo del sistema, no pedidos reales |
| `orders` | Enteros leídos como `int64`/`float64` | Corregir tipos | `int8`/`int16`/`int32`; `Int16` (nullable) donde hay nulos |
| `orders` | 28.817 nulos en `days_since_prior_order` | Conservar | Todos son primeros pedidos; rellenar con 0 falsearía la espera media |
| `products` | 1.258 nulos en `product_name` | Rellenar con `'Unknown'` | El producto existe (pasillo 100, departamento 21 `missing`) |
| `products` | 403 nombres con espacios dobles, 16 con espacio invisible `\xa0`, mayúsculas irregulares | Estandarizar con `.str` | 6.321 nombres cambiaron; quedan 0 espacios dobles o invisibles |
| `products` | 141 nombres repetidos tras estandarizar | Conservar y documentar | Tienen `product_id` distintos y 268 de 280 aparecen en pedidos; se agrupa por `product_id` |
| `order_prods` | Enteros en `int64`/`float64` | Corregir tipos | `add_to_cart_order` pasa a `Int16` |
| `order_prods` | 836 nulos en `add_to_cart_order` | Conservar | Están en los 70 pedidos de más de 64 productos: el sistema solo registra la posición hasta la 64 |
| `departments` | Valor centinela `missing` | Conservar como texto | Como nulo real, `groupby` descartaría 1.258 productos |
| `aisles`, `departments` | — | Sin cambios | No tienen nombres duplicados ni espacios sobrantes |

### Informe de cambios del pipeline

| Tabla | Paso | Filas | Nulos | Memoria (MB) |
|---|---|---:|---:|---:|
| orders | 0 crudo | 478.967 | 28.819 | 23,0 |
| orders | 1 sin duplicados | 478.952 | 28.817 | 23,0 |
| orders | 2 tipos | 478.952 | 28.817 | 7,2 |
| products | 0 crudo | 49.694 | 1.258 | 3,1 |
| products | 1 nombres | 49.694 | 0 | 3,1 |
| order_prods | 0 crudo | 4.545.007 | 836 | 145,4 |
| order_prods | 1 tipos | 4.545.007 | 836 | 54,5 |

El pipeline (`pipe`) produce las mismas tablas que la limpieza manual (`equals` → `True`) y termina con validaciones `assert`: `order_id` único, horas entre 0 y 23, productos con nombre, sin espacios invisibles y `reordered` solo con 0 y 1.

## 2. Análisis (Lección 3)

| Hallazgo | Cifra |
|---|---|
| Hora con más pedidos | 10 h (40.578 pedidos) |
| Celda más activa de la semana | Lunes a las 10 h (7.881 pedidos) |
| Día con más pedidos | Domingo (84.090); su pico es a las 15 h |
| Espera más frecuente después de 30 días | 7 días (44.577 pedidos, 9,31 %) |
| Mediana de espera: entre semana / fin de semana | 7 / 8 días |
| Clientes frecuentes (5 pedidos o más) | 19,4 % de los clientes y 231.946 de 478.952 pedidos; vuelven cada 6 días |
| Pedidos de clientes frecuentes por la mañana | 35,2 % (30,0 % en los ocasionales) |
| Tasa de recompra en la posición 1 / 5 del carrito | 0,679 / 0,617 |

## 3. Combinación (Lección 4)

La tabla maestra se construyó a partir de `order_prods` (tabla de hechos) uniendo `products`, `aisles`, `departments` y `orders` con `how='left'` y `validate='m:1'`.

| Validación | Resultado |
|---|---|
| Filas de la tabla maestra | 4.545.007, igual que `order_prods` (14 columnas) |
| Artículos sin producto, pasillo, departamento o pedido | 0 |
| Pedidos sin productos (`indicator=True`) | 28.906 (`left_only`); un `inner` los habría ocultado |
| Ficha de productos | 45.573 productos, `product_id` único (`validate='1:1'`) |

| Hallazgo | Cifra |
|---|---|
| Producto más pedido | Banana (66.050 veces, tasa de recompra 0,844) |
| Departamentos con más artículos | `produce` (1.328.302, recompra 0,651), `dairy eggs` (759.842, 0,671) |
| Pasillos con mayor recompra (≥ 10.000 artículos) | milk (0,781), water seltzer sparkling water (0,731), fresh fruits (0,718) |
| Pasillo con menor recompra | spices seasonings (0,152) |
| Departamento que entra en el top 3 de madrugada | `beverages` (8,49 % de madrugada frente a 8,26 % del total) |
| Producto más fiel con ≥ 1.000 pedidos | Whole Organic Omega 3 Milk (`dairy eggs`, 0,863) |
| Día con más artículos de `alcohol` | Viernes (3.814) |

## 4. Archivos generados en `data/processed/`

| Archivo | Contenido | Lección |
|---|---|---|
| `cleaning_report.csv` | Informe de cambios del pipeline de limpieza | 2 |
| `orders_clean.parquet` | Pedidos limpios | 2 |
| `products_clean.parquet` | Catálogo con nombres estandarizados | 2 |
| `order_products_clean.parquet` | Artículos por pedido con tipos corregidos | 2 |
| `customer_segments.csv` | Clientes, pedidos y espera mediana por segmento | 3 |
| `segment_time_profile.csv` | % de pedidos por segmento y franja horaria | 3 |
| `master_table.parquet` | Tabla maestra (4.545.007 filas × 14 columnas), compresión `zstd` | 4 |
| `department_summary.csv` | Artículos y tasa de recompra por departamento | 4 |
| `product_profile.parquet` | Una fila por producto: veces, clientes, recompra y posición media | 4 |
| `department_leaders.csv` | Producto con mayor tasa de recompra de cada departamento | 4 |

Los Parquet conservan los tipos de dato corregidos (`int8`, `Int16`, etc.); para leerlos se usa `pd.read_parquet(...)`, que requiere `pyarrow`. La tabla maestra se guarda con `compression='zstd'` para quedar por debajo del límite de 100 MB por archivo de GitHub.
