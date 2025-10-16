# Reto_Brasil_Ciencia_De_Datos_
## 📄 README: Flujo de Trabajo para el Análisis de Datos de Producción Agrícola

**Autores:** Lina Cárdenas, Jennifer Rodríguez, Daniela Villalobos, y Jose Ballesteros.

Este repositorio contiene un *script* de Python diseñado para procesar y analizar datos de producción agrícola provenientes de un archivo de Excel con un formato complejo y multi-encabezado. El objetivo principal es **reestructurar** los datos de un formato amplio y no estándar a un **formato largo (*tidy data*)**, limpiar la información y realizar un **Análisis Exploratorio de Datos (EDA)** básico con visualizaciones clave.

***

## 🚀 Flujo de Trabajo del Algoritmo (Workflow)

El algoritmo sigue una secuencia lógica de **Carga, Transformación, Limpieza, Análisis y Visualización** de los datos, detallada a continuación.

### 1. 📂 Carga de Datos y Extracción de Metadatos

| Paso | Acción | Descripción |
| :--- | :--- | :--- |
| **1.1** | **Configuración Inicial** | Se importan las librerías necesarias: **`pandas`** para manejo de datos, **`os`**, **`matplotlib.pyplot`** y **`seaborn`** para visualización. |
| **1.2** | **Carga del Archivo** | El *script* intenta cargar el archivo de Excel (`Tabela 5457.xlsx`) en un DataFrame (`df_original`). Se incluye manejo de excepciones para `FileNotFoundError`. |
| **1.3** | **Extracción de Años** | Se identifican los **años** y sus **índices de columna iniciales** basándose en la **fila 2** del archivo. |
| **1.4** | **Extracción de Productos** | Se obtienen los **nombres de los productos** a partir de la **fila 3**, excluyendo las dos primeras columnas. |
| **1.5** | **Extracción de Regiones** | Se aísla la columna de **Región Geográfica** (`Geographical Region`) de la primera columna, comenzando desde la **fila 4**. |

***

### 2. 🔄 Reestructuración de Datos (Melting a Tidy Data)

Esta es la etapa crítica de transformación, donde el formato ancho (que usa columnas para los años y productos) se convierte a formato largo (una fila por cada observación única: Región, Año, Producto, Valor).

1.  **Iteración por Bloque de Año:** Se recorre cada año identificado y su índice de columna inicial. Se calcula el rango de columnas que pertenece a cada bloque anual.
2.  **Identificación de Columnas de Cantidad:** Dentro de cada bloque anual, el *script* busca las columnas que coinciden con los nombres de producto extraídos de la fila 3, asumiendo que estas contienen la **'Cantidad producida (Toneladas)'**.
3.  **Fusión (*Melting*):** Para cada columna de cantidad identificada, se extraen los valores de producción (desde la fila 4).
4.  **Creación de Observaciones:** Se crea un DataFrame temporal con las columnas clave: **`Geographical Region`**, **`Year`**, **`Product`** y **`Value`**.
5.  **Concatenación Final:** Se concatenan todos los DataFrames temporales para formar el DataFrame reestructurado **`df_melted`**.

***

### 3. 🧹 Limpieza y Preparación Final

| Paso | Acción | Detalles |
| :--- | :--- | :--- |
| **3.1** | **Conversión de Tipo** | La columna **`Value`** se convierte a tipo numérico (`float`), utilizando `errors='coerce'` para convertir valores no numéricos en `NaN` (nulo). |
| **3.2** | **Manejo de Placeholders** | Los *placeholders* originales del archivo (`'..'`, `'-'`) se reemplazan por valores nulos (`pd.NA`). |
| **3.3** | **Eliminación de Nulos** | Se eliminan las filas donde la columna **`Value`** es nula, ya que representan datos de producción faltantes o no reportados. |
| **3.4** | **Clasificación** | El DataFrame final se ordena por **`Year`** (`df_sorted_by_year`). |

***

### 4. 📊 Análisis Exploratorio de Datos (EDA) y Visualización

El *script* procede a analizar el DataFrame limpio y ordenado, extrayendo información clave:

#### Análisis Estadístico

* **Inspección:** Se muestran las estadísticas descriptivas de la columna `Value`.
* **Productos Principales:** Se calcula la producción total por producto e identifica el **Top 10** de productos más producidos.
* **Tendencias Temporales:** Se calcula la producción total por año y la producción del **Top 10 por Año**.
* **Distribución Regional:** Se calcula la producción total por **Región Geográfica**.
* **Valores Faltantes:** Se cuantifica el porcentaje de valores nulos finales.

#### Visualizaciones Clave

1.  **Tendencia Total (Línea):** Producción Agrícola Total a lo Largo del Tiempo.
2.  **Contribución por Producto (Barras Apiladas):** Producción de los 10 Productos Principales a lo Largo del Tiempo.
3.  **Distribución Geográfica (Barras):** Producción Agrícola Total por Región Geográfica.

El *script* finaliza con un resumen de los **Hallazgos Clave** y posibles **Próximos Pasos** para un análisis más profundo.
