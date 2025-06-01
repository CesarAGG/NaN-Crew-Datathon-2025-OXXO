# Proyecto Datathon OXXO: Modelo Predictivo de Éxito de Tiendas por NaN Crew

## Resumen del Proyecto

Este proyecto fue desarrollado por el equipo **NaN Crew** para el **DSC Datathon 4ta Edición (2025)**, centrado en la cadena de tiendas de conveniencia OXXO de FEMSA. El objetivo principal fue **desarrollar un modelo de Machine Learning capaz de predecir el potencial de éxito (definido como el cumplimiento de su meta de ventas) de una nueva tienda OXXO, basándose en su ubicación geográfica y un conjunto enriquecido de características del entorno.**

El proyecto abarcó desde la recolección y procesamiento de datos públicos externos hasta la ingeniería avanzada de características geoespaciales y el desarrollo de modelos predictivos. Se logró una **Exactitud (Asertividad) del 91.4%** en el conjunto de evaluación oficial, superando el objetivo del reto. 

**Nota Importante sobre los Datos:** Este repositorio contiene el código y la estructura metodológica del proyecto. Sin embargo, los datasets originales proporcionados por OXXO/FEMSA y algunos archivos de datos externos de gran tamaño no están incluidos debido a su naturaleza confidencial y/o volumen. Por lo tanto, el proyecto, tal como se presenta aquí, no es directamente ejecutable sin acceso a dichos datos. Sirve como una demostración detallada del enfoque analítico y el trabajo realizado. Los datos confidenciales se pondrían en `./datos/` y sus respectivos subdirectorios para la ejecución.

## Objetivos del Reto Cumplidos

1.  **Modelo Predictivo de Éxito:** Se desarrolló un modelo que recibe una ubicación (latitud/longitud) y características del proyecto, generando una predicción sobre el alto potencial de éxito de la tienda.
2.  **Asertividad del Modelo:** Se obtuvo una **Exactitud (Asertividad) del 91.4%** en el conjunto de tiendas de evaluación oficial. Otras métricas clave logradas fueron:
    *   **Precisión:** 97.9%
    *   **Sensibilidad (Recall):** 93.1%
    *   **F1-Score:** 95.4%
3.  **Sugerencias para Otros Negocios:** Se propusieron consideraciones para adaptar este modelo a otros formatos de negocio de FEMSA (tiendas Bara, Caffenio, Farmacias Yza).

## Metodología Aplicada

El proyecto siguió un riguroso pipeline de Data Science:

1.  **Comprensión del Negocio y Definición del Problema:** Se analizó el reto para definir "Éxito" como el cumplimiento de la meta de ventas, utilizando datos históricos de ventas y metas proporcionados.
2.  **Recolección y Preparación de Datos:**
    *   **Datos Internos OXXO:** Catálogo de tiendas (+900) y ventas mensuales de los últimos 2 años.
    *   **Datos Externos (Investigación y Obtención):**
        *   INEGI: Marco Geoestadístico Nacional (MGN) para AGEBs, Censo de Población y Vivienda 2020 (a nivel AGEB vía SCITEL para Nuevo León y Tamaulipas).
        *   INEGI: Directorio Estadístico Nacional de Unidades Económicas (DENUE), filtrado por códigos SCIAN 2023 para competencia y generadores de tráfico.
        *   CONAPO: Índice de Marginación Urbana (IMU) 2020 a nivel AGEB.
        *   OpenStreetMap (OSM): Para PDIs complementarios y red vial.
    *   **Procesamiento:** Limpieza, creación de variable objetivo `EXITO` (utilizando ventas promedio del periodo Enero 2023 - Julio 2024), y uniones geoespaciales (GeoPandas) para enriquecer los datos de tiendas con información censal y del entorno. Se utilizó **Polars** para el manejo eficiente de grandes volúmenes de datos de ventas.

3.  **Ingeniería de Características (Feature Engineering):**
    *   Creación de un amplio conjunto de variables predictivas, incluyendo:
        *   Perfil demográfico y socioeconómico del AGEB (densidad poblacional, estructura de edad, escolaridad, IMU).
        *   Entorno comercial: Densidad y proximidad a competidores directos/indirectos (DENUE).
        *   Generadores de tráfico: Densidad y proximidad a escuelas, oficinas, bancos, hospitales (DENUE, OSM).
        *   Accesibilidad: Proximidad a vías principales, densidad de paradas de transporte (OSM).
        *   Características intrínsecas de la tienda (tamaño, estacionamiento, tipo de segmento).

4.  **Modelado Predictivo:**
    *   Preprocesamiento final: Manejo de nulos, codificación de variables categóricas (One-Hot Encoding) y escalado de numéricas.
    *   Entrenamiento y evaluación de múltiples algoritmos de clasificación (ej. Random Forest, XGBoost, LightGBM).
    *   Optimización de hiperparámetros y validación cruzada robusta.
    *   Selección del modelo con mejor desempeño general en el conjunto de validación interno.

5.  **Evaluación y Presentación:**
    *   Validación del modelo final en el conjunto de prueba interno (`7b_Test_Data.ipynb`) y en el conjunto de evaluación oficial (`7c_Oficial_Test.ipynb`).
    *   Elaboración de la presentación final.

## Herramientas y Tecnologías Utilizadas

*   **Lenguaje de Programación:** Python 3.x
*   **Bibliotecas Principales:**
    *   Pandas, Polars (Manejo y procesamiento de datos)
    *   GeoPandas, Shapely (Análisis geoespacial)
    *   NumPy (Computación numérica)
    *   Scikit-learn (Modelado, preprocesamiento, métricas)
    *   XGBoost / LightGBM (Algoritmos de Gradient Boosting)
    *   Matplotlib, Seaborn (Visualización)
*   **Entorno de Desarrollo:** Jupyter Notebooks
*   **Control de Versiones:** Git, GitHub
*   **Gestión de Entorno Virtual:** `uv` (definido en `pyproject.toml`)

## Estructura del Repositorio

*   `1_Creacion_Variable_Objetivo.ipynb`: Carga de datos internos, procesamiento de ventas y metas, y creación de la variable objetivo `EXITO`.
*   `2_Exploracion_Shapefile_AGEB.ipynb`: Carga e inspección del shapefile de AGEBs del Marco Geoestadístico Nacional.
*   `3_Procesamiento_Censo_Union_Geoespacial.ipynb`: Procesamiento de datos censales de SCITEL y unión geoespacial con las tiendas.
*   `4_Procesar_Catalogo_SCIAN.ipynb`: Parseo del catálogo SCIAN para generar un diccionario de categorías de PDI.
*   `5_Integracion_DENUE.ipynb`: Incorporación de datos del DENUE y creación de características de proximidad/densidad.
*   `6_Integracion_OSM.ipynb`: Integración de datos de OpenStreetMap.
*   `7_Integracion_IMU.ipynb`: Añadir datos del Índice de Marginación Urbana.
*   `7b_Test_Data.ipynb`: Aplicación del pipeline y modelo al conjunto de prueba interno (split de datos de entrenamiento).
*   `7c_Oficial_Test.ipynb`: Aplicación del pipeline y modelo al conjunto de datos de evaluación oficial proporcionado.
*   `8_Preprocesamiento.ipynb`: Preprocesamiento final del dataset enriquecido antes del modelado.
*   `9_Modelado.ipynb`: Entrenamiento, validación, optimización y selección final de los modelos.
*   `./datos/`: Directorio destinado a los datos. **Los datos confidenciales se pondrían en `./datos/` y sus subdirectorios para la ejecución.**
    *   `./datos/output/`: Para los datasets procesados y generados por los scripts.
    *   **Instrucciones para Datos Faltantes (Confidenciales/Grandes):**
        *   Los archivos originales de OXXO (ej. `DIM_TIENDA.csv`, `venta.csv`, `Meta_venta.csv`) se colocarían en `./datos/`.
        *   Crear y poblar los siguientes subdirectorios con los datos correspondientes:
            *   `./datos/censo/`: Archivos CSV del Censo INEGI (para Nuevo León y Tamaulipas, nivel AGEB).
            *   `./datos/denue_raw/`: Archivos CSV del DENUE.
            *   `./datos/imu_data/`: Archivo CSV del Índice de Marginación Urbana.
            *   `./datos/mgn/conjunto_de_datos/`: Shapefiles del Marco Geoestadístico Nacional (especialmente `00a.shp`).
            *   `./datos/osm_data/`: Shapefiles de OpenStreetMap.
            *   `./datos/scian_catalogos/`: Archivo Excel del catálogo SCIAN.
*   `Presentación Reto Femsa.pdf` (Ejemplo de nombre para la presentación)
*   `README.md`: Este archivo.
*   `pyproject.toml`: Define las dependencias del proyecto para el entorno `uv`.

## Resultados Clave y Conclusiones

*   Se desarrolló un modelo predictivo que alcanzó una **Exactitud (Asertividad) del 91.4%** en el conjunto de evaluación oficial, acompañado de una **Precisión del 97.9%**, una **Sensibilidad (Recall) del 93.1%**, y un **F1-Score del 95.4%**.
*   Si bien múltiples factores contribuyeron al rendimiento del modelo, variables relacionadas con la **proximidad a competidores, el Índice de Marginación Urbana del AGEB, y diversas características demográficas y de entorno comercial** demostraron ser significativas.
*   Se delinearon estrategias para adaptar la metodología a otros negocios de FEMSA, enfatizando la personalización de fuentes de datos y la redefinición de "éxito" para cada caso.

## Desafíos y Aprendizajes

*   La integración y armonización de diversas fuentes de datos geoespaciales con diferentes granularidades y calidades fue un desafío técnico central.
*   La ingeniería de características, especialmente la creación de variables espaciales significativas (densidades, distancias, conteos en radios), fue fundamental para el desempeño del modelo.
*   La colaboración efectiva y la gestión ágil del tiempo fueron cruciales para abordar un problema complejo en el formato intensivo de un Datathon.

## Posibles Mejoras Futuras

*   Incorporación de datos de movilidad más detallados (flujo peatonal/vehicular).
*   Análisis de series temporales para capturar tendencias de ventas y del entorno.
*   Modelado de la canibalización entre tiendas OXXO cercanas.
*   Desarrollo de una herramienta interactiva para la exploración de ubicaciones potenciales.

---
**Equipo:** NaN Crew
---
