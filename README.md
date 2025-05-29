# botero-mlops-u3
Descripción de un Pipeline de MLOps – Reestructuración 


## Enfoque del Problema

Este trabajo se centrará en el desarrollo de un pipeline de aprendizaje automático orientado a la **clasificación de tumores cerebrales según su grado de malignidad**.

Esta problemática se alinea con la consigna planteada, ya que en el contexto específico de esta clínica, **existe una desproporción significativa en los datos disponibles: se registran muchos más tumores de alto grado que de bajo grado**. Esta disparidad hace que los tumores de bajo grado sean menos representados en el conjunto de datos, lo cual los posiciona de manera análoga a las **enfermedades huérfanas**, caracterizadas por la escasez de información disponible.

En este sentido, **clasificar correctamente un tumor de bajo grado equivale a enfrentar el reto de predecir una enfermedad huérfana**, mientras que los tumores de alto grado representan enfermedades comunes con abundancia de datos. Este enfoque resulta particularmente relevante desde el punto de vista clínico, ya que una clasificación temprana y precisa del grado de malignidad puede incidir directamente en la elección del tratamiento y el pronóstico del paciente.

Además, esta problemática presenta desafíos adicionales relacionados con la naturaleza de los datos requeridos para entrenar el modelo. Al tratarse de datos **radiómicos**, es decir, características extraídas de imágenes de resonancia magnética, se requiere que los pacientes:

- Se hayan sometido a una **resonancia magnética cerebral con secuencia FLAIR**, que permite una visualización clara de las lesiones intra-axiales.
- Cuenten con una **biopsia** confirmada que determine el grado de malignidad del tumor, lo cual es esencial para establecer las etiquetas del conjunto de datos.

Estas condiciones reducen drásticamente la disponibilidad de datos etiquetados. Además, debido a los protocolos médicos y los tiempos asociados al diagnóstico y tratamiento, **la cantidad de estos datos crece lentamente a lo largo del tiempo**, lo que limita la frecuencia con la que se puede actualizar el modelo.

Este enfoque no solo responde a los desafíos planteados en la consigna del ejercicio, sino que también me resulta conveniente desde el punto de vista académico, ya que podré **reutilizar parte del trabajo en mi proyecto de grado**, centrado en la caracterización de tumores cerebrales a partir de imágenes de resonancia magnética mediante técnicas de inteligencia artificial.


---

# Pipeline propuesto


### 1. Data Input

- Se reciben archivos generados a partir de la extracción de datos radiómicos de imágenes de resonancia magnética cerebral.
- Estos archivos son entregados por la clínica directamente al equipo técnico responsable.
- Posteriormente, se alojan en un bucket para que sean accesibles tanto para el pipeline como para el resto del equipo de desarrollo.  
  > Propuesta: **AWS S3** - confiabilidad y facilidad de uso. Es casi un estandar para almacenar archivos en la nube.


### 2. Data Processing
- Los archivos se reformatean para poder tratarlos como registros en una tabla estructurada. Para esto se usan principalmente Pandas y Numpy. 
- Se realiza un análisis exploratorio de los datos, generando un reporte con estadísticas descriptivas relevantes del dataset.
- Si se detecta desbalanceo entre clases, se aplica **oversampling** a la clase minoritaria. Esto puede tratarse de un balanceo de los pesos de los modelos basados en arboles o la generacion de datos sinteticos con SMOTE para el SVM. 
- Se preparan los datos mediante:
  - Gestion de variables categoricas
  - Normalización de características.
  - Detección y tratamiento de outliers.
  - feature extraction
  - Reduccion de dimensionalidad
  - Divisiones en conjuntos de entrenamiento y prueba.
- Finalmente se registra el dataset para mantener un versionamiento.
    > Propuesta: **Pandas, Numpy y AWS Aurora PostgreSQL** - las primeras son las mejores librerias para trabajar los datos. Ademas AWS Aurora Postgres es una excelente opcion para almacenar los datos tabulares resultantes, con opcion de restaurar la Base de Datos en caso de emergencia.  

### 3. Model Iterations

- Se entrenan tres modelos:
  - **XGBoost**
  - **Random Forest**
  - **SVM**
- Todos los modelos utilizan **validación cruzada (5 pliegues)**.
- Se realiza una búsqueda de hiperparámetros para cada modelo con el fin de optimizar su desempeño.
- Se evalúan las siguientes métricas:
  - Precisión
  - Recall
  - F1-score
  - AUC


### 4. Model Selection

- Se selecciona el modelo con mejor desempeño en las métricas clave, priorizando **precisión** y **recall**, dado el contexto clínico. 
- El modelo seleccionado es validado de acuerdo con los estándares de calidad establecidos por la clínica colaboradora.
- Se generan Dashboards con las metricas y demas informacion relevante del modelo para mantener registro y versionamiento. 
    > Propuesta: **AWS S3** 



### 5. Model Deployment

- Se desarrollan y despliegan dos componentes:
  - Un **frontend** construido en Flask en forma de aplicación web para interacción con radiologos y personal de salud.
  - Un **backend** que expone una **API** construida en FastAPI encargada de cargar el modelo entrenado y realizar predicciones bajo demanda.
- El sistema completo se ejecuta en contenedores mediante `docker-compose` para facilitar su instalación.
    > Propuesta: **Flask y FastAPI** - Esto por su amplia comunidad y practicidad. 


### 6. Model Predictions

- El usuario accede a la aplicación web, carga un archivo y solicita una predicción.
- La predicción es procesada por el modelo alojado en el backend.
- Las predicciones son almacenadas automáticamente en una base de datos relacional para su consulta futura.  
  > Propuesta: **AWS Aurora PostgreSQL**
