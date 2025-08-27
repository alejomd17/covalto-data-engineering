# The High Level Problem

**Candidato**: Alejandro Moscoso Deossa

**Fecha**: 27/08/2025

---

## 1. Arquitectura End-to-End

```mermaid
flowchart  TD
    %% --- Flujo Central Vertical ---
    A[(3rd party XML API<br>Annual Tax Returns)] --> B@{ shape: lin-cyl, label: "Lake de Datos<br>Azure Blob Storage" }
    P[(3rd party JSON API and internal pg databases<br>Credit Card Transaction History)] --> B
    T[(3rd party XML API and an S3 bucket of PDFs and images<br>Bank Statements)] --> B
    B --> D[Preprocesamiento ETL<br>Clean, merging]
    D --> E[Extracción de Features Clave]
    E --> F[Modelo de Tendencias]
    F --> H[Sistema de Recomendación]
    H --> J[Interfaz para Food Bloggers <br>Frontend o Dashboard]

    %% --- Conexiones Laterales Simétricas ---
    F -.-> G[Monitoreo + Logging]
    H -.-> G[Monitoreo + Logging]
    F -.-> I[API REST<br>FastAPI]
    H -.-> I[API REST<br>FastAPI] 

    %% --- Alineación Horizontal ---
    subgraph " "
        direction LR
        G ~~~ I
    end

    %% --- Estilos ---
    classDef center fill:#f0f0ff,stroke:#333;
    classDef leftRight fill:#e6f7ff,stroke:#0066cc;
    class A,B,C,D,E,F,H,J center;
    class I,G leftRight;
```

## 2. Stack Propuesto y Justificación

| Componente | Tecnología | Justificación |   Alternativa |
|------------|------------|---------------|---------------|
| Orquestación | Azure Data Factory | Orquestación visual, scheduling de pipelines y transformaciones básicas | Apache Kafka |
| Almacenamiento |  Data Lake Gen2 (Blob Storage) + SQL DB	Data Lake | Centralizado y escalable | PostgreSQL, S3, Azure SQL |
| Procesamiento | Azure Databricks (PySpark, Pandas) | Limpieza, unión y feature engineering | SQL |
| Modelado | Script en Azure Databricks + modelo registrado en Azure ML | Experimentos reproducibles | Prophet, MLflow, LightGBM, Word2Vec |
| Feature Store | Delta Lake | Versionado de features accesible desde Databricks | Opcional |
| API REST | FastAPI, Azure Function | Bajo costo y escalable | Azure App Service |
| Monitorización | Azure ML Studio | Métricas, alertas y trazabilidad | Prometheus + Grafana |
| Resultados | Azure Blob Storage, Azure SQL | Consulta, dashboarding y analítica posterior | Synapse |
| Frontend / BI | Power BI | Consulta, dashboarding y analítica posterior | Web App JS, CSS, React |
