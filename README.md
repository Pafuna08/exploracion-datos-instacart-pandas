# Análisis de datos de Instacart con Python y Pandas

Proyecto de análisis de los datos de pedidos de Instacart, desarrollado en dos actividades:

| Actividad | Notebook | Informe |
|---|---|---|
| **GA1-220501094-01-AA1-EV04** – Fundamentos en Pandas · *Explorando Instacart: descubre patrones de compra* | [`notebooks/data_quality.ipynb`](notebooks/data_quality.ipynb) | [`reports/data_quality.md`](reports/data_quality.md) |
| **GA1-220501094-01-AA1-EV06** – Limpieza, análisis y estructuración de datos con Pandas | [`notebooks/pandas_labs.ipynb`](notebooks/pandas_labs.ipynb) | [`reports/data_processing.md`](reports/data_processing.md) |

**Pregunta orientadora (EV06):**

> ¿Cómo preparar y estructurar los datos de Instacart para obtener información confiable sobre los hábitos de compra?

## Contenido de los notebooks

### `data_quality.ipynb` (EV04)

Exploración inicial de los cinco archivos, sin modificarlos: importación (`read_csv`, `head`, `tail`, `shape`), estructura y calidad (`dtypes`, `info`, valores ausentes, tabla de diagnóstico), indexación y filtrado (`loc[]`), indicadores básicos (`mean`, `sum`, `count`, `describe`) y conclusiones.

### `pandas_labs.ipynb` (EV06)

Lecciones 2, 3 y 4 del material *Preparar y estructurar datos con Python*, en orden. Cada lección incluye los ejemplos, el laboratorio guiado y los 7 ejercicios del reto, con su interpretación.

1. **Lección 2 · Limpieza y estandarización:** duplicados, tipos de dato, nulos, texto y duplicados implícitos. *Laboratorio guiado 2:* pipeline de limpieza con informe de cambios.
2. **Lección 3 · Filtrado, ordenamiento y agrupación:** máscaras booleanas, `query`, `sort_values`, `groupby`, `pivot_table` y `transform`. *Laboratorio guiado 3:* perfil de hábitos de compra.
3. **Lección 4 · Combinación de datasets:** `merge` con `how`, `validate` e `indicator`, tabla maestra, `concat` y `melt`. *Laboratorio guiado 4:* tabla de análisis por producto.

## Datos

Los CSV originales no se incluyen en el repositorio por su tamaño (`order_products.csv` pesa unos 90 MB). Descárguelos desde la carpeta de la actividad y cópielos en `data/raw/`:

https://drive.google.com/drive/folders/19vYGYdE0hI1DzvpBcCZskzJ0u2TZjwas?usp=drive_link

| Archivo | Filas | Columnas |
|---|---|---|
| `instacart_orders.csv` | 478.967 | 6 |
| `products.csv` | 49.694 | 4 |
| `aisles.csv` | 134 | 2 |
| `departments.csv` | 21 | 2 |
| `order_products.csv` | 4.545.007 | 4 |

Los archivos usan punto y coma (`;`) como separador.

Los archivos procesados sí se incluyen en `data/processed/` (ver [`reports/data_processing.md`](reports/data_processing.md)). Todos se pueden regenerar ejecutando `pandas_labs.ipynb`.

## Estructura del proyecto

```
Proyecto_de_datos/
├── README.md
├── .gitignore
├── requirements.txt
│
├── data/
│   ├── raw/                  (CSV originales; se ignoran en git)
│   │   ├── instacart_orders.csv
│   │   ├── order_products.csv
│   │   ├── products.csv
│   │   ├── aisles.csv
│   │   └── departments.csv
│   └── processed/            (archivos generados por pandas_labs.ipynb)
│
├── notebooks/
│   ├── data_quality.ipynb    (actividad anterior, EV04)
│   └── pandas_labs.ipynb     (lecciones 2, 3 y 4, EV06)
│
└── reports/
    ├── data_quality.md       (actividad anterior, EV04)
    └── data_processing.md    (limpieza, análisis y combinación, EV06)
```

## Cómo ejecutar

1. Clone el repositorio y copie los cinco CSV dentro de `data/raw/`.
2. Cree y active el entorno virtual:
   ```
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1      # macOS/Linux: source .venv/bin/activate
   ```
3. Instale las dependencias (incluye `pandas` y `pyarrow`, necesario para los archivos Parquet):
   ```
   python -m pip install -r requirements.txt
   ```
4. Abra Jupyter desde la raíz del proyecto con `jupyter lab`.
5. Abra el notebook y use *Kernel → Restart & Run All*:
   - `notebooks/data_quality.ipynb`: exploración inicial (EV04).
   - `notebooks/pandas_labs.ipynb`: lecciones 2, 3 y 4 (EV06). Las lecciones se ejecutan en orden porque cada una usa las variables de la anterior. Los resultados se guardan en `data/processed/`. La ejecución completa tarda menos de un minuto.

Las cifras se obtuvieron con Python 3.14, pandas 3.1 y pyarrow 26.

## Versionamiento

- `main`: versiones entregadas, marcadas con etiquetas.
- `develop`: rama de integración.
- `feature/pandas-data-processing`: desarrollo de la actividad EV06 (lecciones 2, 3 y 4), fusionada en `develop`.
