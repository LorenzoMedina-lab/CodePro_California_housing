# Análisis del consumo de energía en Brasil

Análisis exploratorio con Python del consumo de energía en Brasil entre **enero de 2014 y diciembre de 2023**. El objetivo es estudiar las distribuciones de consumo y cantidad de consumidores, comparar estados y tipos de consumidor, y observar la evolución mensual.

El trabajo se desarrolla en [Analisis_Energia_Brasil.ipynb](Analisis_Energia_Brasil.ipynb), con gráficos e interpretaciones para cada etapa. No incluye modelos predictivos.

## Datos utilizados

El archivo [Dados_brutos.xlsx](Dados_brutos.xlsx) contiene **285.499 registros y 14 columnas**, con **120 fechas mensuales únicas**. Cada mes reúne múltiples registros correspondientes a estados y categorías; una fecha repetida no implica por sí sola una fila duplicada.

| Columna o grupo | Descripción |
| --- | --- |
| `Data` | Fecha original en formato `AAAAMMDD`, convertida a fecha para el análisis temporal. |
| `TipoConsumidor` | Tipo de consumidor: `Cativo` o `Livre`. |
| `Sistema` | Sistema eléctrico indicado en el archivo. |
| `UF` | Sigla de la unidad federativa de Brasil. |
| `Setor Econômico - N1`, `N2`, `N3` | Niveles de clasificación del sector económico. |
| `Tipo Tensão - N1`, `N2`, `N3` | Niveles de clasificación del tipo de tensión. |
| `Faixa de Consumo N1`, `N2` | Categorías de rango de consumo. |
| `Consumidores` | Cantidad de consumidores registrada. |
| `Consumo` | Consumo energético registrado. |

El notebook no especifica la unidad física de `Consumo`; los resultados se presentan en la escala del archivo, sin asumir kWh o MWh.

## Preparación y calidad de los datos

- Se convierte `Data` a `datetime` y se utiliza como índice para agrupar por mes.
- Se identifican **3.988 valores faltantes en `Consumidores`** y **9.700 en `Consumo`**. No se realiza una imputación general.
- Para estudiar la relación y correlación entre ambas variables, se excluyen las filas que tienen algún valor faltante en ellas.
- Se conservan los valores negativos. El notebook plantea que podrían ser ajustes o correcciones, pero no verifica su significado.
- Se crea una copia sin índice temporal para las visualizaciones no temporales.

Las sumas por estado y mes omiten los valores faltantes y conservan los negativos. Por ello, representan agregados de los valores disponibles en el dataset.

## Recorrido del análisis

1. **Exploración inicial:** dimensiones, tipos de datos, primeras filas y estadísticas descriptivas.
2. **Preparación temporal:** conversión de fechas, revisión del período y conteo de faltantes.
3. **Distribuciones individuales:** histogramas de `Consumo` y `Consumidores` en escala logarítmica.
4. **Segmentación:** boxplot del consumo para las categorías `Cativo` y `Livre`.
5. **Relaciones numéricas:** gráfico de dispersión, matriz de correlación y pairplot. Este último utiliza una muestra de 2.000 registros con `random_state=42`.
6. **Comparación por UF:** suma del consumo durante todo el período y visualización de las diez unidades federativas con mayor total.
7. **Serie temporal:** suma mensual de `Consumo` mediante `resample('ME').sum()`.
8. **Análisis combinado:** comparación de `Cativo` y `Livre` en las cinco UF con mayor consumo acumulado.

La muestra del pairplot se utiliza únicamente para visualizar; no reduce los datos utilizados en los agregados.

## Principales hallazgos

Según las salidas e interpretaciones guardadas en el notebook:

- `Consumo` y `Consumidores` presentan distribuciones asimétricas, con muchos valores pequeños y algunos muy elevados.
- Hay **251.992 registros de `Cativo`** y **33.507 de `Livre`**. Estas cifras cuentan filas, no consumidores individuales.
- La relación entre cantidad de consumidores y consumo muestra una dispersión importante; no es una relación proporcional perfecta.
- **São Paulo (`SP`) registra el mayor consumo acumulado** entre las UF analizadas.
- La serie mensual muestra crecimiento general, una caída marcada alrededor de 2020 y recuperación posterior, con valores elevados hacia 2023.
- Entre las cinco UF principales, el consumo `Cativo` predomina en `SP`, `PR`, `RJ` y `RS`. En `MG`, ambos tipos están más equilibrados y `Livre` aparece ligeramente por encima.

## Estructura

```text
Energia_Brasil/
├── Analisis_Energia_Brasil.ipynb
├── Dados_brutos.xlsx
└── README.MD
```

Las dependencias compartidas están en [requirements.txt](../requirements.txt), en la raíz del repositorio: pandas, NumPy, Matplotlib y seaborn.

## Cómo ejecutar el notebook

Necesitas Python 3. Los siguientes comandos están preparados para **Windows PowerShell**, desde la **raíz del repositorio**, donde se encuentra `requirements.txt`.

### 1. Crear un entorno e instalar las dependencias

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m pip install openpyxl jupyterlab
```

`openpyxl` permite leer el archivo `.xlsx` y JupyterLab permite ejecutar el notebook. Ambos se instalan por separado porque no figuran en el archivo de dependencias compartido.

### 2. Definir la ruta del Excel

```powershell
$env:RUTA_ARCHIVO_ENERGIA = (Resolve-Path .\Energia_Brasil\Dados_brutos.xlsx).Path
```

El notebook obtiene esta ruta mediante `os.getenv("RUTA_ARCHIVO_ENERGIA")`. La variable queda definida para la sesión actual de PowerShell y los procesos iniciados desde ella. El notebook no carga automáticamente un archivo `.env`.

### 3. Abrir JupyterLab

En la misma terminal:

```powershell
.\.venv\Scripts\python.exe -m jupyterlab
```

Abre `Energia_Brasil/Analisis_Energia_Brasil.ipynb` y ejecuta las celdas en orden. Las tablas y los gráficos se muestran dentro del notebook.

Si falla la lectura del Excel, verifica la ruta y la instalación de `openpyxl`. Si modificas la variable de entorno después de iniciar Jupyter, cierra el servidor y vuelve a iniciarlo desde la terminal configurada.

## Alcance y limitaciones

- El análisis es descriptivo: las correlaciones y los cambios temporales no prueban causalidad. El gráfico por sí solo no permite explicar la caída observada alrededor de 2020.
- La escala logarítmica no representa valores cero o negativos; esos gráficos no muestran toda la distribución, aunque tales registros permanezcan en los datos.
- Los totales por UF son acumulados del período y no están normalizados por población ni por cantidad de consumidores.
- Los valores faltantes y el significado pendiente de confirmar de los negativos condicionan la interpretación de los agregados.
- Las dependencias no tienen versiones fijadas. El notebook utiliza opciones como `resample('ME')` y `log_scale=True` en boxplots, que requieren versiones compatibles de pandas y seaborn.
