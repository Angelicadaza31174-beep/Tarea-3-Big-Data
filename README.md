| | |
|---|---|
| **Autora** | Angelica Maria Daza Ochoa |
| **Programa** | Ingeniería de Sistemas |
| **Universidad** | UNAD – Escuela de Ciencias Básicas, Tecnología e Ingeniería (ECBTI) |
| **Curso** | Procesamiento de Datos con Apache Spark |
| **Tarea** | Tarea 3: Análisis y Procesamiento de Datos |
 
</div>
---
 
## 📌 Descripción del Proyecto
 
Este proyecto analiza grandes volúmenes de datos históricos de la **Tasa de Cambio Representativa del Mercado (TRM)** en Colombia, empleando un stack de Big Data de nivel industrial:
 
- 🔥 **Apache Spark** para procesamiento batch y streaming estructurado
- 📨 **Apache Kafka** como broker de mensajería para simular flujo de datos en tiempo real
- 🐍 **PySpark** como API principal de desarrollo
> 📂 **Fuente de datos:** [Datos Abiertos de Colombia](https://www.datos.gov.co/) — *Tasa de Cambio Representativa del Mercado - Histórico*
 
---
 
## 🏗️ Arquitectura de la Solución
 
```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────────┐
│   Dataset CSV   │────▶│   Spark Batch    │────▶│  Parquet / Gráficas │
│  (TRM Histórico)│     │  (EDA + Stats)   │     │  (Resultados)       │
└─────────────────┘     └──────────────────┘     └─────────────────────┘
 
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────────┐
│ Productor Kafka │────▶│ Spark Structured │────▶│ Ventanas Temporales │
│ (Tiempo Real)   │     │    Streaming     │     │ Promedio / Alertas  │
└─────────────────┘     └──────────────────┘     └─────────────────────┘
```
 
---
 
## 🧩 Componentes Implementados
 
| Componente | Tecnología | Descripción |
|---|---|---|
| **Procesamiento Batch** | Apache Spark | Carga el CSV completo, limpia nulos, convierte fechas y calcula estadísticas anuales/mensuales de la TRM. |
| **Análisis Exploratorio (EDA)** | Spark DataFrames / RDDs | Filtrado, agrupación, detección de outliers y estadísticas descriptivas persistidas en Parquet. |
| **Broker de Mensajería** | Apache Kafka | Topic `trm-topic` recibe mensajes JSON del productor Python, simulando actualizaciones en tiempo real. |
| **Streaming Estructurado** | Spark Structured Streaming | Consume del topic Kafka, aplica ventanas de 1 minuto con watermark y calcula promedio/máx/mín dinámicos. |
| **Visualización** | Matplotlib / Power BI | Gráficas de tendencias históricas y dashboard de resultados del streaming. |
 
---
 
## 📁 Archivos del Proyecto
 
| Archivo | Etapa | Función |
|---|---|---|
| `batch_processing.py` | 1 – Ingesta | Implementa el dataset en Spark; carga el CSV y registra el DataFrame como vista SQL temporal. |
| `eda_spark.py` | 2 – EDA | Limpieza, transformación de tipos, análisis estadístico y almacenamiento en Parquet. |
| `spark_kafka_streaming.py` | 3 – Streaming | Conecta Spark al topic Kafka, deserializa JSON y aplica ventanas de agregación temporal. |
| `procesamiento_streaming.py` | 4 – Visualización | Procesa datos en tiempo real, verifica calidad y genera gráficas dinámicas. |
 
---
 
## ▶️ Instrucciones de Ejecución
 
### ✅ Prerrequisitos
 
- Apache Spark `3.5.x` instalado y configurado en el `PATH`
- Apache Kafka corriendo en `localhost:9092` con el topic `trm-topic` creado
- Python `3.9+` con las dependencias instaladas:
```bash
pip install pyspark kafka-python matplotlib pandas
```
 
- Dataset CSV de TRM descargado desde [datos.gov.co](https://www.datos.gov.co/) y ubicado en la ruta configurada en los scripts
---
 
### Paso 1 – Implementación del Dataset en Spark
 
```bash
spark-submit batch_processing.py
```
 
---
 
### Paso 2 – Limpieza, Transformación y EDA
 
```bash
spark-submit eda_spark.py
```
 
> Los resultados se almacenan en formato **Parquet** en el directorio `./output/`
 
---
 
### Paso 3 – Spark Streaming con Kafka
 
Asegúrate de tener Kafka activo en `localhost:9092` y ejecuta:
 
```bash
spark-submit \
  --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.3 \
  spark_kafka_streaming.py
```
 
---
 
### Paso 4 – Procesamiento y Visualización en Tiempo Real
 
En una terminal separada, ejecuta:
 
```bash
python3 procesamiento_streaming.py
```
 
---
 
## 📈 Resultados Esperados
 
| Resultado | Tipo | Descripción |
|---|---|---|
| **Estadísticas Anuales** | Batch | Promedio, máximo y mínimo de la TRM por año para toda la serie histórica. |
| **Promedios Mensuales** | Batch | Promedio mensual de los últimos cinco años para detectar estacionalidad. |
| **Tendencia Histórica** | Visualización | Gráfica de línea con la evolución anual y mensual de la TRM. |
| **Agregaciones en Tiempo Real** | Streaming | Promedio, máx y mín de TRM en ventanas deslizantes de 1 minuto desde Kafka. |
| **Datos en Parquet** | Persistencia | Resultados del EDA en formato columnar para consultas analíticas posteriores. |
 
---
 
## 🛠️ Stack Tecnológico
 
| Tecnología | Versión | Uso |
|---|---|---|
| Apache Spark | 3.5.x | Motor de procesamiento batch y streaming estructurado |
| PySpark | 3.5.x | API Python para DataFrames, RDDs y Structured Streaming |
| Apache Kafka | 2.13-3.x | Broker de mensajes para simular flujo TRM en tiempo real |
| Python | 3.9+ | Lenguaje principal; incluye kafka-python, matplotlib y pandas |
| Parquet | — | Formato columnar para persistencia de resultados |
| Matplotlib | 3.x | Visualización de tendencias históricas y resultados de streaming |
 
---
 
<div align="center">
*Angelica Maria Daza Ochoa · Ingeniería de Sistemas · UNAD 2025*
 
</div>
