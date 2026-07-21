# Análisis de Siniestros e Incidentes de Tránsito en Ecuador

Solución integral de análisis de datos (**Python + KNIME + Power BI + SQL/NoSQL**) para el estudio de incidentes y siniestros de tránsito a nivel nacional, con enfoque en la identificación de zonas, causas, temporalidad y niveles de riesgo.

**Proyecto de Fin de Semestre — Análisis de Datos**  
Escuela de Formación de Tecnólogos (ESFOT) — Escuela Politécnica Nacional (EPN)

---

## 📋 Contenidos

- [Objetivo](#-objetivo)
- [Arquitectura de la solución](#-arquitectura-de-la-solución)
- [Fuentes de datos](#-fuentes-de-datos)
- [Estructura del repositorio](#-estructura-del-repositorio)
- [Cómo ejecutar el proyecto](#-cómo-ejecutar-el-proyecto)
- [Fase 1 — Python y Jupyter Notebooks](#-fase-1--python-y-jupyter-notebooks)
- [Fase 2 — KNIME: proceso ETL](#-fase-2--knime-proceso-etl)
- [Fase 3 — Power BI: modelo y dashboard](#-fase-3--power-bi-modelo-y-dashboard)
- [Bases de datos SQL y NoSQL](#-bases-de-datos-sql-y-nosql)
- [Problemas encontrados y decisiones documentadas](#-problemas-encontrados-y-decisiones-documentadas)
- [Autores](#-autores)

---

## 🎯 Objetivo

Diseñar, implementar y defender una solución de análisis de datos a escala real que integre la ingesta, limpieza, procesamiento ETL, modelado relacional/dimensional y visualización interactiva de registros de incidentes de tránsito en Ecuador, facilitando la toma de decisiones informadas sobre la seguridad vial.

---

## 🏗️ Arquitectura de la solución

Fuentes de Datos (INEC, SPPAT, APIs REST)
│
▼
Python (Jupyter Notebooks)
· Ingesta de archivos CSV/XLSX y consumo de API
· Limpieza, tratamiento de nulos y normalización
· Unificación y escalado masivo de datos
· Agrupamiento y análisis (K-Means)
│
▼
Bases de Datos Intermedias (SQL / NoSQL / JSON)
· SQLite (Bases locales de feriados y registros)
· MongoDB (Colecciones de catálogo y clasificaciones)
│
▼
KNIME Analytics Platform
· Workflow ETL integral
· Cruce de dimensiones y tablas de hechos
· Generación de esquema estrella consolidado (dw_siniestralidad.db)
│
▼
Power BI Desktop
· Modelo dimensional relacional
· Métricas y medidas DAX complejas
· Reporte interactivo multinivel

## 🗂️ Fuentes de datos

| # | Fuente / Descripción | Tipo | Formato / Origen |
|---|---|---|---|
| 1 | Registros de Siniestros e Incidentes de Tránsito | Open Data (INEC) | CSV / XLSX |
| 2 | Coberturas y Atenciones de Gastos Funerarios y Fallecidos | Open Data (SPPAT) | CSV / Base SQL |
| 3 | Calendario Oficial de Feriados Nacionales | API REST | JSON / HTTP |
| 4 | Diccionarios y Catálogos DPA (Provincia, Cantón, Parroquia) | Referencia Oficial | CSV |

---

## 📁 Estructura del repositorio

Veliz_Sanchez_ProyectoFinalAnalisis/
├── datos/
│   └── parte_1.ipynb                    # Notebook inicial de ingesta, calidad y preparación de datos
├── documentos/                           # Documentación metodológica, dictámenes y reportes
│   └── ...
├── knime/                                # Flujos de trabajo de KNIME Analytics Platform
│   └── ...                              # Workflows (.knwf) de extracción, transformación y carga
├── powerbi/                              # Reportes y tableros interactivos
│   └── ...                              # Archivo ejecutable de Power BI (.pbix)
├── scripts/
│   └── Total_incidentes/                # Scripts de automatización y subprocesos ETL
│       └── Total_incidentes/
│           ├── indicentes_etl.csv       # Datasets procesados intermediarios
│           └── ...
├── .gitignore                            # Exclusión de archivos pesados de datos (>100MB)
└── README.md                             # Documentación general del repositorio

> **Nota sobre el manejo de datasets pesados:** Los archivos con extensión `.csv`, `.db` y datasets procesados de gran volumen (como los consolidados de incidentes) superan los límites de rastreo convencional de Git y se encuentran configurados mediante `.gitignore` y/o **Git LFS** para garantizar la integridad y ligereza del repositorio.

---

## ⚙️ Cómo ejecutar el proyecto

### 1. Requisitos previos
- **Python 3.10+** (Jupyter Notebook, Pandas, NumPy, Scikit-Learn)
- **KNIME Analytics Platform** (con extensiones SQLite / MongoDB habilitadas)
- **Power BI Desktop** (edición actualizada)
- **Git LFS** (si se requiere sincronización local de los datasets comprimidos)

### 2. Ejecutar el pipeline de procesamiento

1. **Fase Python:** Abre y ejecuta los notebooks ubicados en `datos/` (`parte_1.ipynb`). Esto realiza la ingesta inicial, genera las consultas a la API de feriados, limpia los registros de incidentes y exporta las bases parciales SQLite/MongoDB.
2. **Fase KNIME:** Inicia KNIME y abre los flujos ubicados en la carpeta `knime/`. Ejecuta el workflow completo para transformar las dimensiones, calcular cruces y estructurar el data warehouse relacional.
3. **Fase Power BI:** Abre el tablero interactivo en `powerbi/`. Actualiza el origen de datos apuntando al archivo de base de datos generado por KNIME para visualizar los indicadores en tiempo real.

---

## Fase 1 — Python y Jupyter Notebooks

- Unificación de múltiples archivos con diferentes codificaciones de texto (UTF-8, Latin-1) y delimitadores.
- Validación estricta de totales de víctimas y homologación de variables categóricas (cantones, causas, tipos de vehículos).
- Procesamiento de clustering (K-Means) para la segmentación automática de cantones según su índice de severidad y riesgo vial.
- Exportación estructurada hacia bases de datos SQLite y colecciones en MongoDB.

## Fase 2 — KNIME: proceso ETL

- Lectura multiorigen (archivos locales, SQLite y NoSQL).
- Aplicación de nodos de limpieza, enriquecimiento de datos por fechas/feriados y reglas de negocio.
- Estructuración final de las tablas de dimensión y la tabla de hechos en esquema estrella.

## Fase 3 — Power BI: modelo y dashboard

- Diseño de modelo en estrella optimizado para alto rendimiento de consultas.
- Implementación de medidas DAX (totales acumulados, tasa de siniestralidad, variaciones interanuales y rankings dinámicos).
- Tablero navegable estructurado con análisis ejecutivo, vistas geográficas por provincia/cantón y conclusiones operativas.

---

## Problemas encontrados y decisiones documentadas

- **Gestión de volumen de datos:** La presencia de datasets con millones de filas obligó a fragmentar el procesamiento y utilizar SQLite como motor intermedio para evitar desbordamientos de memoria RAM.
- **Inconsistencia de nombres y códigos geográficos:** Se implementaron tablas cruzadas de homologación DPA para mapear cantones homónimos entre distintas provincias.
- **Exclusión de archivos pesados en Git:** Se adoptó una política estricta con `.gitignore` y **Git LFS** para gestionar archivos `.csv` de más de 100 MB, permitiendo mantener un repositorio de código ordenado.

---

## 👥 Integrantes

- **Cristhian Veliz** ([@Chesss92](https://github.com/Chesss92))
- **Lizbeth Sánchez** ([@Lizbeth593])

**Escuela de Formación de Tecnólogos (ESFOT)**  
**Escuela Politécnica Nacional (EPN)**  
Quito, Ecuador
