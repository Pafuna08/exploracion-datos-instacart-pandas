# Exploración de datos de Instacart con Python y Pandas

**Actividad:** GA1-220501094-01-AA1-EV04 – Fundamentos en Pandas · *Explorando Instacart: descubre patrones de compra*

Exploración inicial de cinco conjuntos de datos de Instacart (pedidos, productos, pasillos, departamentos y artículos por pedido) para responder la pregunta orientadora:

> ¿Cómo utilizar Python y la librería Pandas para explorar los datos de Instacart, identificar las características de los pedidos y productos, y obtener información relevante sobre los hábitos de compra de sus clientes?

## Contenido del notebook

1. Comprensión del problema y reconocimiento de los datos
2. Importación y exploración inicial de los archivos (`read_csv`, `head`, `tail`, `shape`)
3. Estructura y calidad inicial de los datos (`columns`, `dtypes`, `info`, valores ausentes, tabla de diagnóstico)
4. Indexación y filtrado lógico (`loc[]`, condiciones simples y simultáneas)
5. Indicadores básicos (`mean`, `sum`, `count`, `describe`)
6. Resolución del problema: hallazgos, limitaciones y conclusiones

## Datos

Los CSV no se incluyen en el repositorio por su tamaño (`order_products.csv` pesa unos 86 MB). Descárguelos desde la carpeta de la actividad:

https://drive.google.com/drive/folders/19vYGYdE0hI1DzvpBcCZskzJ0u2TZjwas?usp=drive_link

| Archivo | Filas | Columnas |
|---|---|---|
| `instacart_orders.csv` | 478.967 | 6 |
| `products.csv` | 49.694 | 4 |
| `aisles.csv` | 134 | 2 |
| `departments.csv` | 21 | 2 |
| `order_products.csv` | 4.545.007 | 4 |

Los archivos usan punto y coma (`;`) como separador.

## Estructura del proyecto

```
Proyecto_de_datos/
├── .venv/                (ignorada)
├── data/                 (los CSV se ignoran; solo se versiona .gitkeep)
│   └── .gitkeep
├── notebooks/
│   ├── 00_entorno.ipynb
│   └── Exploracion_de_datos_de_Instacart_con_Python_y_Pandas.ipynb
├── .gitignore
├── README.md
└── requirements.txt
```

## Cómo ejecutar

1. Clone el repositorio y copie los cinco CSV dentro de `data/`.
2. Cree y active el entorno virtual:
   ```
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1      # macOS/Linux: source .venv/bin/activate
   ```
3. Reconstruya el entorno: `python -m pip install -r requirements.txt`
4. Abra JupyterLab desde la raíz del proyecto con `jupyter lab`.
5. Ejecute `notebooks/00_entorno.ipynb` para comprobar que pandas está disponible.
6. Abra `notebooks/Exploracion_de_datos_de_Instacart_con_Python_y_Pandas.ipynb` y ejecute *Run All*.
