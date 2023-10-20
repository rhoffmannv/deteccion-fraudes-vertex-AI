# Detección de Fraudes Bancarios
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
   
# Importación de datos

Código en el *notebook* **Obtener Data.ipynb** con líneas de código explicadas en detalle.

### Descarga de *.csv* en Google Storage
Los datos se obtienen desde tabla pública en BigQuery `bigquery-public-data.ml_datasets.ulb_fraud_detection`, correspondiente al siguiente [dataset de Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud) y se almacenan en un Bucket de Google Storage.  
- Los datos a utilizar corresponden a una tabla de transacciones con tarjeta de crédito.
- Las transacciones son clasificadas como fraudulentas (`Class = 1`) o normales (`Class = 0`).
- La tabla está compuesta por 284.807 transacciones.
- Cada transacción está descrita por 28 features `V1, V2, ... V28`, correspondientes a una proyección PCA de los datos reales (no se entregan directamente por motivos de privacidad).
- Ademas de las etiquetas `Class`, se agregan dos features descriptivas sin PCA:
    - `Time`: Segundos entre la transacción y la primera transacción de la tabla.
    - `Amount`: Cantidad de la transacción.

### Crear Dataset en BigQuery
Si no existe aún, se crea un *dataset* dentro de BigQuery para almacenar la tabla con los datos.

### Crear tabla en BigQuery
Si la tabla no existe, crear dentro de *dataset* a partir de archivo .csv en el Bucket.

### Crear copia de tabla en BigQuery
Se crea copia de tabla y se agregan dos columnas importantes:
- transaction_id: Identificador único para cada fila (transacción bancaria)
- splits: Asignar a cada instancia conjunto de entrenamiento, validación o test.

# Modelo en TensorFlow

Código en el *notebook* **Modelo en Vertex AI.ipynb** con código explicado en detalle.

### Creación de modelo

Se crea modelo de regresión logística:

- Definición de funciones auxiliares:
  - Se crea función que separa datos entre features/etiqueta y codifica en One-Hot las etiquetas.
  - Se crea función que lee batches de datos desde la tabla de BigQuery usando Tensorflow I/O (se configura para leer data en paralelo para acelerar el entrenamiento)
- Se definen los inputs del modelo.
- Se usa Batch Normalization para normalizar datos.
- Se construye modelo de regresión logistica en TensorFlow con función de activación *softmax*.
- Se compila el modelo.

### Entrenamiento del modelo
- Entrenar modelo con método *fit*.
- El entrenamiento se lleva a cabo en la maquina local.
- Se guarda el modelo entrenado.

### Evaluación del modelo
- Se visualizan las curvas de entrenamiento y validación con Tensorboard.
- Se evalua el poder de generalización del modelo usando los datos de prueba.
- Se calculan diversas métricas, como *accuracy* y *loss*.

# *Endpoint* en Vertex AI

Código en el *notebook* **Modelo en Vertex AI.ipynb** con código explicado en detalle.

## Creación de *endpoint*

### Registrar modelo en Vertex AI
- Verificar que modelo no haya sido agregado anteriormente.   
- Agregar modelo a registro de Vertex AI.

### Crear endpoint para obtener predicciones en línea
- Verificar si *endpoint* ya existe.   
- Crear *endpoint* (vacio por el momento).

### Desplegar modelo a endpoint
- Agregar modelo en registro de Vertex AI al *endpoint* creado.

## Prueba de *endpoint*

Se extraen 10 datos del conjunto de *test* para hacer predicciones utilizando el *endpoint* implementado.  
Se usan tres métodos distintos para hacer peticiones de predicciones en línea:
- Usando cliente Python
- Usando petición REST
- Usando Google Cloud (CLI)
