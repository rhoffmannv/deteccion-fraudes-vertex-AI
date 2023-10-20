# Detección de Fraude Bancarios
- En este proyecto se implementa en Vertex AI un modelo de regresión logística para predecir fraudes bancarios.
- Se utilizan diversas herramientas de Google Cloud Platform (GCP) para el desarrollo:
  - Google Storage: Para almacenar archivo *.csv* con los datos.
  - BigQuery: Para crear tabla a partir de archivo *.csv* y luego alimentarcon datos de entrenamiento modelo de TensorFlow.
  - Vertex AI: Para implemetar el modelo como API, capaz de recibir peticiones vía internet y entregar predicciones en tiempo real.
- El proyecto esta compuesto por dos *jupyter notebooks*:
  - **Obtener Data.ipynb**: Donde se descarga *dataset* en formato  *.csv* a bucket en GS.
  - **Modelo en Vertex AI.ipynb**: Donde se crea modelo con TensorFlow, se entrena con datos comunicandose con tabla de BigQuery y se implementa modelo entrenado en *endpoint* de Vertex AI para recibir peticiones y entregar predicciones en línea.
 
# Detalles del Proyecto

A grandes rasgos el proyecto se divide en:

- Importación de datos
  - Descarga de *.csv* en Google Storage
  - Creación de tabla en BigQuery
- Modelo
  - Creación de modelo
  - Entrenamiento de modelo
  - Evaluación de modelo
- *Endpoint*
  - Creación de *endpoint* 
  - Prueba de *endpoint*
    - Con cliente Python
    - Con petición REST
    - Con cliente de Google (CLI)
