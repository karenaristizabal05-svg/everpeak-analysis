# Análisis de Clientes — ConnectaTel

Análisis exploratorio de datos para una empresa de telecomunicaciones en Latinoamérica, con el objetivo de identificar patrones de consumo, detectar comportamientos atípicos y construir segmentos de clientes accionables.


## Objetivo del Proyecto
Evaluar el comportamiento de los clientes de ConnectaTel a partir de sus datos de registro y uso de servicios (llamadas y mensajes), cubriendo el período hasta el año 2024.
El análisis busca responder preguntas clave del negocio:

¿Qué problemas de calidad tienen los datos y cómo corregirlos?
¿Qué perfiles de usuario existen según edad y nivel de uso?
¿Qué segmentos representan mayor oportunidad comercial?
¿Qué recomendaciones se pueden hacer para mejorar la oferta de planes?

##  Datasets Utilizados
### plans.csv 
Información de los planes actuales (precio, minutos incluidos, GB, costo por extra)
### users_latam.csv 
Información de clientes (edad, ciudad, fecha de registro, plan)
### usage.csv 
Detalle del uso real de servicios (llamadas y mensajes)

Los datasets se relacionan a través de la columna user_id, formando un modelo one-to-many (1 usuario → múltiples registros de uso). Dichos datasets fueron provistos por la institución (Tripleten)

## Etapas del Análisis
1. Carga y Exploración Inicial

Carga de los tres datasets con pandas
Revisión de estructura, tipos de datos y primeras filas

2. Identificación de Problemas de Calidad

Detección de valores nulos por columna y proporción
Identificación de valores centinela (-999 en age, "?" en city)
Análisis de columnas categóricas (plan, type, city)
Revisión de nulos estructurales en duration y length (missing by design)

3. Limpieza de Datos

Reemplazo de centinela -999 en age por la mediana (calculada excluyendo el centinela)
Reemplazo de "?" en city por pd.NA
Estandarización y validación de columnas de fecha (reg_date, date)
Marcado como pd.NA de 40 fechas futuras (año 2026) en reg_date
Validación de nulos MAR en duration y length mediante cruce con columna type

4. Agregación de Uso por Usuario

Creación de columnas auxiliares is_text e is_call
Agrupación por user_id para calcular: cant_mensajes, cant_llamadas, cant_minutos_llamada
Merge del perfil de uso con el dataset de usuarios → user_profile

5. Visualización y Detección de Outliers

Histogramas por tipo de plan para: age, cant_mensajes, cant_llamadas, cant_minutos_llamada
Boxplots para identificar valores extremos
Cálculo de límites IQR para las variables de uso
Decisión de tratamiento: mantener outliers en mensajes y llamadas (heavy users reales), aplicar capping P99 en cant_minutos_llamada

6. Segmentación de Clientes

Segmentación por nivel de uso: Bajo uso, Uso medio, Alto uso
Segmentación por edad: Joven (< 30), Adulto (30–59), Adulto Mayor (60+)
Visualización de distribución por segmento

7. Insight Ejecutivo

Traducción de hallazgos en conclusiones y recomendaciones accionables para el negocio

## Cómo Ejecutar el Notebook
Opción A — Abrir directamente desde GitHub con Google Colab

En este repositorio, navega hasta el archivo S7_Version-Estudiante-Project-ConnectaTel.ipynb
En la parte superior del archivo, haz clic en el botón "Open in Colab"

Si no aparece el botón, reemplaza github.com por colab.research.google.com/github en la URL


Una vez en Colab, ve a Runtime → Run all para ejecutar todas las celdas

Opción B — Clonar el repositorio localmente
bashgit clone https://github.com/karenaristizabal05-svg/everpeak-analysis.git
cd tu_repositorio
jupyter notebook S7_Version-Estudiante-Project-ConnectaTel.ipynb

## Guía de Reproducción
### Requisitos
bashpip install: pandas, numpy, matplotlib, seaborn
### Estructura de archivos esperada
📁 everpeak-analysis/
├── 📓 S7_Version-Estudiante-Project-ConnectaTel.ipynb
├── 📁 datasets/
│   ├── plans.csv
│   ├── users_latam.csv
│   └── usage.csv
└── 📄 README.md

### Pasos para reproducir

Asegúrate de que los archivos CSV estén en la carpeta /datasets/ tal como se indica arriba
Ejecuta las celdas en orden, de arriba hacia abajo
Cada sección está claramente delimitada con el paso correspondiente (Paso 1 al Paso 7)
Los comentarios en cada celda explican la lógica aplicada


⚠️ Nota: Si ejecutas el notebook en Google Colab, deberás subir los archivos CSV manualmente o montarlos desde Google Drive antes de ejecutar las celdas de carga de datos.


### Tecnologías Utilizadas

Python 3
pandas — manipulación y limpieza de datos
numpy — operaciones numéricas
matplotlib — visualizaciones base
seaborn — visualizaciones estadísticas


### Autor: Karen Aristizabal Linkdn: www.linkedin.com/in/karen-aristizabal
Desarrollado como proyecto final del Sprint 7 — Análisis de Datos con Python.
