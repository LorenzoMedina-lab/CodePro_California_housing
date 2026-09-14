# Desafío de análisis exploratorio de datos

Este repositorio reúne dos exploraciones realizadas con Python: **el valor de las viviendas en California** y **el consumo de energía en Brasil**. El desafío consiste en transformar datos en preguntas, visualizaciones e interpretaciones fundamentadas, avanzando desde la descripción de variables hasta el análisis de sus relaciones.

Ambos trabajos se presentan en notebooks de Jupyter con código, gráficos y conclusiones. Su alcance es exploratorio y descriptivo; no incluyen entrenamiento de modelos predictivos.

## Las dos exploraciones

| Aspecto | California Housing | Energía en Brasil |
| --- | --- | --- |
| Pregunta principal | ¿Qué características se relacionan con el valor mediano de las viviendas? | ¿Cómo se distribuye el consumo energético y cómo evoluciona entre 2014 y 2023? |
| Datos | 20.640 registros y 10 columnas. | 285.499 registros y 14 columnas. |
| Variables centrales | Valor de las viviendas, ingreso mediano, antigüedad y cercanía al océano. | Consumo, cantidad de consumidores, UF, tipo de consumidor y fecha. |
| Comparaciones | Segmentos por antigüedad y cercanía al océano. | Estados, tipos de consumidor y meses. |
| Enfoque final | Relación conjunta entre ingreso, valor y ubicación. | Evolución mensual y composición del consumo por UF y tipo de consumidor. |

### California Housing

La exploración estudia características agregadas de zonas de California para identificar patrones asociados con `median_house_value`. Incluye imputación de valores faltantes en dormitorios, codificación de la cercanía al océano, histogramas, comparaciones de distribuciones, correlaciones y gráficos multivariados.

Las salidas guardadas muestran una correlación positiva aproximada de **0,69 entre ingreso mediano y valor de las viviendas**. También se observan diferencias por ubicación: las zonas `INLAND` presentan una mediana inferior a las categorías cercanas a la bahía y al océano. La acumulación de registros en el valor máximo requiere cautela al interpretar la distribución.

Consulta el [notebook de California Housing](California_Housing/Analisis_California_housing.ipynb) y su [documentación específica](California_Housing/README.md).

### Energía en Brasil

La exploración analiza registros de enero de 2014 a diciembre de 2023. Examina las distribuciones de `Consumo` y `Consumidores`, compara las categorías `Cativo` y `Livre`, agrega el consumo por unidad federativa (`UF`) y construye una serie mensual.

Según las interpretaciones guardadas en el notebook, **São Paulo concentra el mayor consumo acumulado** entre las UF analizadas. Las variables principales presentan distribuciones asimétricas y la serie temporal muestra crecimiento general, una caída marcada alrededor de 2020 y recuperación posterior. El análisis identifica ese cambio sin atribuirle una causa.

Consulta el [notebook de energía en Brasil](Energia_Brasil/Analisis_Energia_Brasil.ipynb) y su [documentación específica](Energia_Brasil/README.MD).

## Metodología del desafío

1. **Comprender los datos:** revisar dimensiones, columnas, tipos y estadísticas descriptivas.
2. **Evaluar su calidad:** identificar faltantes y valores extremos, y preparar las variables necesarias.
3. **Explorar distribuciones:** reconocer concentraciones, asimetrías y diferencias de magnitud.
4. **Comparar grupos:** analizar segmentos geográficos y categorías relevantes para cada problema.
5. **Estudiar relaciones:** combinar dispersión, correlaciones y pairplots para observar patrones entre variables.
6. **Integrar e interpretar:** responder preguntas multivariadas y, en energía, temporales, explicando el alcance de los resultados.

La preparación se adapta a cada conjunto: California imputa los dormitorios faltantes con la mediana; energía conserva los faltantes en el conjunto general y utiliza filas completas para la relación entre consumo y consumidores. Las muestras de los pairplots facilitan la visualización y no sustituyen los datos completos en los agregados.

## Herramientas

- **Python y pandas:** carga, preparación y agregación de datos.
- **NumPy:** biblioteca numérica incluida en el entorno del proyecto.
- **Matplotlib y seaborn:** visualización de distribuciones, relaciones y series temporales.
- **JupyterLab:** ejecución y lectura de los notebooks.
- **openpyxl:** lectura del archivo Excel de energía.

## Organización del repositorio

```text
.
├── README.md
├── requirements.txt
├── California_Housing/
│   ├── Analisis_California_housing.ipynb
│   └── README.md
└── Energia_Brasil/
    ├── Analisis_Energia_Brasil.ipynb
    └── README.MD
```

Los archivos de entrada son `housing.csv` y `Dados_brutos.xlsx`. Están contemplados en `.gitignore`, por lo que pueden no estar disponibles al clonar el repositorio. Para ejecutar los análisis, necesitas disponer de ambos archivos y configurar sus rutas.

## Ejecución

Con Python 3 instalado, abre **PowerShell en la raíz del repositorio**.

### 1. Preparar el entorno

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m pip install jupyterlab openpyxl
```

JupyterLab y openpyxl se instalan por separado porque no figuran en `requirements.txt`.

### 2. Configurar los archivos de entrada

Coloca `housing.csv` en `California_Housing/` y `Dados_brutos.xlsx` en `Energia_Brasil/`, o ajusta los comandos a su ubicación:

```powershell
$env:RUTA_ARCHIVO = (Resolve-Path .\California_Housing\housing.csv).Path
$env:RUTA_ARCHIVO_ENERGIA = (Resolve-Path .\Energia_Brasil\Dados_brutos.xlsx).Path
```

Los notebooks leen estas variables mediante `os.getenv()`; no cargan automáticamente un archivo `.env`. Las variables deben definirse en la misma terminal desde la que se inicia Jupyter.

### 3. Abrir los notebooks

```powershell
.\.venv\Scripts\python.exe -m jupyterlab
```

Abre el notebook que quieras explorar y ejecuta todas sus celdas en orden. Cada análisis puede ejecutarse de forma independiente, siempre que tenga disponible su archivo de entrada.

## Interpretación y límites

Los resultados describen los conjuntos analizados y no prueban relaciones causales. En California, los registros representan zonas, no viviendas individuales. En energía, los agregados conservan valores negativos y omiten faltantes; además, las escalas logarítmicas no representan ceros ni negativos. Estas decisiones deben considerarse al leer los gráficos.

Las dependencias no tienen versiones fijadas, por lo que la reproducción requiere versiones compatibles con las operaciones utilizadas en los notebooks. La documentación de cada exploración amplía sus decisiones metodológicas y limitaciones.
