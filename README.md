Proyecto: Modelo Predictivo de Campaña
Este repositorio contiene un proyecto de machine learning supervisado dividido en dos partes principales:

Limpieza y Preparación de Datos (Limpieza_Data_Campaña.ipynb)

Entrenamiento y Evaluación del Modelo (Modelo_Pred_Pycaret_Campaña.ipynb)

El objetivo final es entrenar un modelo de regresión para predecir el "% total primera" basado en las características de una campaña (posiblemente avícola, según los nombres de las variables).

workflow del Proyecto
El flujo de trabajo es secuencial: el primer notebook toma datos brutos, los limpia y los prepara; el segundo notebook utiliza esos datos limpios para construir y evaluar el modelo predictivo.

Parte 1: Limpieza y Preparación de Datos
(Notebook: Limpieza_Data_Campaña.ipynb)

Este notebook se encarga de la ingesta, exploración (EDA) y limpieza de los datos iniciales.

Pasos Clave:

Carga de Datos: Se leen los datos desde el archivo Campaña Prueba.xlsx.

Análisis Exploratorio (EDA):

Se revisa la estructura (.shape), tipos de datos (.dtypes) y valores nulos (.isnull().sum()).

Se analizan las variables categóricas como Zona y Proveedor para entender su distribución.

Limpieza de Tipos de Datos: Las columnas Centro y Código de Campaña se convierten al tipo object (categórico).

Ingeniería de Características (Feature Engineering):

Se identifica que la columna Proveedor tiene 39 categorías únicas.

Se crea una nueva columna ProveedorRS extrayendo solo la parte textual del nombre (ej. "REYNA 2" -> "REYNA"), reduciendo la cardinalidad.

Se elimina la columna original Proveedor.

Manejo de Outliers (Valores Atípicos):

Se identifican outliers en las columnas numéricas usando el método del Rango Intercuartílico (IQR).

Se aplica una función recortar_outliers que "recorta" (clipping) los valores atípicos, ajustándolos a los límites (bigotes) definidos por el IQR, en lugar de eliminarlos.

Visualización: Se utilizan histogramas y diagramas de caja (boxplot) para visualizar la distribución de las variables y el efecto de la limpieza de outliers.

Parte 2: Modelo Predictivo con PyCaret
(Notebook: Modelo_Pred_Pycaret_Campaña.ipynb)

Este notebook toma los datos procesados de la parte 1 y utiliza la librería PyCaret para encontrar el mejor modelo de regresión.

Pasos Clave:

Carga de Datos Limpios: Se carga el archivo Copia de Data para prediccion.xlsx (el resultado de la limpieza anterior).

Definición del Problema:

Variable Objetivo (Target): target1 = "% total primera".

Variables Predictoras (Features): Se seleccionan 11 características, incluyendo Sexo, Carga granja, Día 7, Día 14, Top ICA, Top IEP, etc..

Configuración del Entorno PyCaret:

Se importa el módulo de regresión: from pycaret.regression import *.

Se inicializa el entorno (setup) especificando:

El target (% total primera).

Normalización de datos (normalize=True).

Eliminación de multicolinealidad (remove_multicollinearity=True).

Transformación de datos (transformation=True, usando Yeo-Johnson).

Eliminación de outliers (remove_outliers=True).

Entrenamiento y Selección de Modelos:

Se comparan múltiples modelos de regresión para encontrar el de mejor rendimiento (compare_models).

Se selecciona el modelo Random Forest (rf) como el modelo a optimizar (create_model('rf')).

Optimización (Tuning):

Se realiza un ajuste fino (tuning) del modelo Random Forest, optimizando primero para MAPE (tune_model(model_select, optimize="MAPE")) y luego para MAE (tune_model(model_select, optimize="MAE")).

Evaluación: Se generan visualizaciones para evaluar el rendimiento del modelo afinado, incluyendo gráficos de residuos, error de predicción e importancia de características (plot_model).
