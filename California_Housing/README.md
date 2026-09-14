# Análisis exploratorio de California Housing

Análisis de datos inmobiliarios de California con Python para identificar qué características están más relacionadas con el valor mediano de las viviendas, especialmente el ingreso, la antigüedad y la cercanía al océano.

El trabajo se desarrolla en [Analisis_California_housing.ipynb](Analisis_California_housing.ipynb). Incluye exploración, limpieza y visualización de datos, sin entrenamiento de modelos predictivos.

## Datos

El archivo [housing.csv](housing.csv) contiene **20.640 registros y 10 columnas**. Cada registro describe una zona con características agregadas, no una vivienda individual.

| Variable | Descripción |
| --- | --- |
| `longitude` | Longitud geográfica. |
| `latitude` | Latitud geográfica. |
| `housing_median_age` | Antigüedad mediana de las viviendas. |
| `total_rooms` | Total de habitaciones de la zona. |
| `total_bedrooms` | Total de dormitorios de la zona. |
| `population` | Población de la zona. |
| `households` | Número de hogares. |
| `median_income` | Ingreso mediano, en la escala del dataset. |
| `median_house_value` | Valor mediano de las viviendas; variable principal del análisis. |
| `ocean_proximity` | Categoría de cercanía al océano. |

La revisión inicial identifica **207 valores faltantes en `total_bedrooms`** y **ninguna fila completamente duplicada**.

## Metodología

1. **Exploración:** revisión de dimensiones, tipos de datos, faltantes, duplicados y estadísticas descriptivas.
2. **Limpieza:** imputación de dormitorios faltantes con la mediana y creación de una copia con One-Hot Encoding de `ocean_proximity` para calcular correlaciones.
3. **Distribución del valor:** histograma con curva de densidad y revisión de la acumulación en el máximo registrado.
4. **Segmentación:** comparación por antigüedad mediana —menor a 20 años frente a 20 años o más— y boxplots por cercanía al océano.
5. **Análisis por ubicación:** cálculo y gráfico de la mediana de `median_house_value` por categoría de cercanía al océano.
6. **Correlaciones:** matriz de correlación y mapa de calor.
7. **Relaciones entre variables:** pairplot de valor, ingreso, antigüedad y habitaciones con una muestra de 1.000 registros.
8. **Análisis multivariado:** dispersión de ingreso y valor, diferenciada por cercanía al océano, con una muestra de 5.000 registros.

Las muestras utilizan `random_state=42` para reproducir la selección y se aplican únicamente a esas visualizaciones. Los resúmenes y las correlaciones utilizan el conjunto completo.

## Principales resultados

Según las salidas guardadas en el notebook:

- **El ingreso mediano presenta la mayor correlación positiva con el valor** entre las variables analizadas: aproximadamente **0,69**.
- **La ubicación se asocia con diferencias de valor:** `NEAR BAY` y `NEAR OCEAN` presentan medianas superiores a `INLAND`.
- La antigüedad y el total de habitaciones muestran relaciones menos claras con el valor que el ingreso mediano.
- Existen **965 registros en el máximo de 500.001**, lo que sugiere un posible límite superior de registro.

| Cercanía al océano | Mediana de `median_house_value` |
| --- | ---: |
| `ISLAND` | 414.700 |
| `NEAR BAY` | 233.800 |
| `NEAR OCEAN` | 229.450 |
| `<1H OCEAN` | 214.850 |
| `INLAND` | 108.500 |

Las asociaciones observadas **no prueban causalidad**. La categoría `ISLAND` requiere cautela por su reducido número de observaciones. El posible límite superior del valor también condiciona la interpretación de las distribuciones y correlaciones.

## Archivos del proyecto

| Archivo | Contenido |
| --- | --- |
| [Analisis_California_housing.ipynb](Analisis_California_housing.ipynb) | Análisis principal, gráficos y conclusiones. |
| [housing.csv](housing.csv) | Dataset utilizado. |
| [requirements.txt](requirements.txt) | Dependencias: pandas, NumPy, Matplotlib y seaborn. |
| `README.md` | Documentación e instrucciones de ejecución. |

El repositorio también contiene `Analisis_Energia_Brasil.ipynb` y `Dados_brutos.xlsx`, correspondientes a otro análisis.

## Cómo ejecutar el análisis

Necesitas Python 3 y una terminal abierta en la carpeta del proyecto. Los comandos siguientes están preparados para **Windows PowerShell**.

### 1. Crear el entorno e instalar las dependencias

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m pip install jupyterlab
```

JupyterLab se instala por separado porque no está incluido en `requirements.txt`.

### 2. Configurar la ruta del CSV

El notebook carga los datos mediante la variable de entorno `RUTA_ARCHIVO`. Defínela antes de iniciar Jupyter:

```powershell
$env:RUTA_ARCHIVO = (Resolve-Path .\housing.csv).Path
```

La variable se aplica a la sesión actual de PowerShell y a los procesos iniciados desde ella. Debes volver a definirla si abres una terminal nueva.

### 3. Abrir y ejecutar el notebook

Desde la misma terminal:

```powershell
.\.venv\Scripts\python.exe -m jupyterlab
```

Abre `Analisis_California_housing.ipynb` y ejecuta todas las celdas en orden. Los resultados y gráficos se muestran dentro del notebook.

Si falla la carga de datos, verifica que `RUTA_ARCHIVO` esté definida y apunte a `housing.csv`. Si la cambias después de abrir Jupyter, cierra el servidor y vuelve a iniciarlo desde la terminal configurada.

## Alcance y reproducibilidad

El análisis describe patrones del dataset incluido; no estima precios actuales ni evalúa capacidad predictiva. Las dependencias no tienen versiones fijadas, por lo que algunos detalles de ejecución o la apariencia de los gráficos pueden variar entre entornos.
