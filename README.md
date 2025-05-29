# botero-mlops-u3
Descripción de un Pipeline de MLOps – Reestructuración 

## Enfoque del Problema
Este trabajo se centrará en el desarrollo de un pipeline de aprendizaje automático orientado a la **clasificación de tumores cerebrales según su grado de malignidad**.

Esta problemática se alinea con la consigna planteada, ya que en el contexto específico de esta clínica, **existe una desproporción significativa en los datos disponibles: se registran muchos más tumores de alto grado que de bajo grado**. Esta disparidad hace que los tumores de bajo grado sean menos representados en el conjunto de datos, lo cual los posiciona de manera análoga a las **enfermedades huérfanas**, caracterizadas por la escasez de información disponible.

En este sentido, **clasificar correctamente un tumor de bajo grado equivale a enfrentar el reto de predecir una enfermedad huérfana**, mientras que los tumores de alto grado representan enfermedades comunes con abundancia de datos. Además, este enfoque me resulta conveniente, ya que podré reutilizar parte del trabajo en mi proyecto de grado, el cual está centrado en la caracterización de tumores cerebrales a partir de imágenes de resonancia magnética.

# Pipeline propuesto

El pipeline recibe directamente archivos que pasaron anteriormente por una primera etapa de feature extraction. Estos archivos corresponden a datos radiomicos extraidos de resonancias magneticas de tumores cerebrales. Es por esta razon que no se puede evidenciar el frature extraction en el pipeline. El pipeline realizara un analisis de datos y procesamiento de tal forma que se puedan entrenar los modelos habiendo almacenado registros estadisticos sobre sus datos de entrenamiento.

El entrenamiento se hara sobre los modelso random forest y XGBoost dinamicamente para determinar cual tienen mejores resultados especialmente en precision y recall. Luego se valida el modelo respecto a las metricas requeridas por el hospital, y luego el despliegue propuesto en esta entrega.