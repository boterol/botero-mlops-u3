# Changelog

Todas las modificaciones importantes en este proyecto serán documentadas aquí.


## [1.1.0] - 2025-05-28

### Added
- Descripción explícita de las herramientas propuestas en cada etapa (e.g., AWS S3, Aurora PostgreSQL, Pandas, FastAPI).
- Descripción detallada del proceso de entrenamiento de modelos (XGBoost, Random Forest, SVM) con validación cruzada y búsqueda de hiperparámetros.
- Proceso completo de predicción con almacenamiento de resultados en base de datos.

### Changed
- Sección de **despliegue de modelo** con frontend en Flask, backend en FastAPI y orquestación con Docker Compose.
- Inclusión detallada de cada etapa del pipeline de aprendizaje automático para clasificación de tumores cerebrales según su grado de malignidad.
- Reemplazo del pipeline anterior, más general, por una versión significativamente más específica y técnica.
- Se reorganizaron las etapas de la descripcion del pipeline en el README.md.
- fase de **procesamiento de datos** reinventada:
  - Reestructuración de archivos radiómicos en registros estructurados.
  - Análisis exploratorio de datos con generación de reportes estadísticos.
  - Detección y corrección de desbalanceo usando técnicas como **SMOTE** y ajuste de pesos.
  - Tratamiento de variables categóricas, normalización, detección de outliers y reducción de dimensionalidad.
  - Versionamiento del dataset final.
---

## [1.0.0] - 2025-05-28
### Agregado
- Diagrama del pipeline de aprendizaje automático.
- README con la explicación detallada del pipeline y su relación con la problemática.