<div align="center">

# ⚽ Football Salaries Predictor

### Predicción de salarios de futbolistas profesionales mediante Machine Learning

**Renato Gutierrez** · Proyecto final de módulo — *Data Science for Business*
**Futura, Data y Analítica Avanzada**

<br>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=plotly&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

</div>

---

## 📌 Sobre el Proyecto

Este proyecto aborda un problema de **Machine Learning aplicado a Sports Analytics**: estimar el salario semanal (`wage_eur`) de jugadores profesionales de fútbol a partir de sus atributos técnicos, físicos y demográficos, utilizando el dataset **FIFA 22 Players**.

El caso simula una situación real de negocio dentro del mundo del fútbol profesional: clubes, agentes y analistas deportivos necesitan **valorizar talento de forma objetiva** para tomar decisiones de fichajes, renovaciones y negociación salarial. Este proyecto construye una solución de regresión que sustenta ese tipo de decisiones con evidencia basada en datos, en lugar de solo criterio subjetivo.

> 💼 **Decisión de negocio que soporta:** transformar el análisis exploratorio y el modelo predictivo en una recomendación accionable para sports analytics, valorización de talento y negociación salarial — dejando claro qué variables importan más, cuándo confiar en la predicción del modelo, y en qué segmentos de jugadores el error esperado es mayor.

<br>

## 🎯 Objetivos

**Objetivo principal**
Estimar el salario semanal de jugadores profesionales a partir de atributos técnicos, físicos y demográficos.

**Objetivos específicos**
- 🔍 Auditar calidad de datos: valores atípicos, codificaciones y variables con riesgo de *leakage*.
- 🛠️ Preparar variables numéricas, categóricas y estructurales (jugadores de campo vs. arqueros).
- 📈 Construir una solución reproducible de regresión.
- 💬 Interpretar resultados en lenguaje de negocio, con recomendaciones accionables.
- ⚖️ Documentar restricciones éticas, operativas y de despliegue del modelo.

<br>

## ❓ Preguntas que el Proyecto Responde

- ¿En qué unidad se interpreta `wage_eur` y qué margen de error es aceptable para el negocio?
- ¿Qué variables podrían generar *leakage* o no estar disponibles antes de predecir?
- ¿Qué modelo se recomienda y cómo mejora frente a una línea base simple?
- ¿Dónde se concentran los errores más grandes y qué segmentos requieren mayor cautela?
- ¿Cómo se usaría la predicción: pricing, priorización, planeación o simulación de fichajes?

<br>

---

## 📊 Dataset

<div align="center">

| Campo | Detalle |
|:---|:---|
| **Nombre** | FIFA 22 Players |
| **Fuente** | [OpenML](https://www.openml.org/d/45012) |
| **Filas** | 19,178 jugadores |
| **Variables** | 29 |
| **Variable objetivo** | `wage_eur` (salario semanal) |
| **Tipo de problema** | Regresión supervisada |

</div>

El dataset incluye atributos demográficos (edad, altura, peso, nacionalidad), atributos generales (`overall`, `potential`) y atributos técnicos agrupados en: ataque (`attacking_*`), habilidad (`skill_*`), movimiento (`movement_*`), defensa (`defending_*`) y portería (`goalkeeping_*`).

> ⚠️ **Reto particular:** el dataset combina jugadores de campo y arqueros en las mismas columnas, por lo que gran parte de la limpieza y preparación requiere tratar ambos grupos de forma diferenciada — evitando así conclusiones erróneas al analizar outliers o correlaciones.

<br>

---

## 🧭 Plan de Análisis

<div align="center">

| Fase | Contenido |
|:---|:---|
| 🔎 **1. EDA** | Calidad de datos, distribución del target, variables relevantes, hallazgos de negocio |
| 📐 **2. Inferencial** | Hipótesis de negocio contrastadas con pruebas estadísticas |
| 🧩 **3. Multivariante** | Relaciones simultáneas, redundancias, reducción dimensional |
| ⚙️ **4. Feature Engineering** | Codificación, escalamiento, control de *leakage* |
| 🤖 **5. Modelado** | Comparación de algoritmos, validación, análisis de residuales |

</div>

### Algoritmos Evaluados

| Modelo | Rol en la comparación |
|:---|:---|
| **Regresión Lineal Regularizada** (Ridge / Lasso) | Línea base interpretable |
| **Árbol de Regresión** | Captura de relaciones no lineales simples |
| **Random Forest Regressor** | Ensamble robusto ante outliers y no linealidad |
| **Gradient Boosting** (XGBoost / LightGBM) | Máximo desempeño predictivo mediante boosting |

La selección final se basa en comparación de métricas de error (no solo R²), análisis de residuales y desempeño por segmentos.

<br>

---

## 📈 Métricas de Éxito

<div align="center">

| Métrica | Uso |
|:---|:---|
| **MAE** | Error promedio interpretable en euros |
| **RMSE** | Penaliza errores grandes con mayor severidad |
| **R² (test)** | Varianza explicada fuera de muestra |
| **MAPE / SMAPE** | Error relativo del target |
| **Error por segmento** | Detección de sesgos por grupo o posición |

</div>

<br>

---

## 🗂️ Estructura del Proyecto

```
football-salaries-ml/
├── data/
│   ├── raw/                          # Dataset original sin modificar
│   └── processed/                    # Datasets limpios/transformados
├── notebooks/
│   ├── 01_eda.ipynb                  # Análisis Exploratorio de Datos
│   ├── 02_inferencial.ipynb          # Estadística Inferencial
│   ├── 03_multivariante.ipynb        # Análisis Multivariante
│   ├── 04_feature_engineering.ipynb  # Preparación del dataset
│   └── 05_modelado.ipynb             # Modelado predictivo
├── reports/
│   └── figures/                      # Gráficos exportados
├── docs/
│   └── resumen_ejecutivo.md
├── src/                               # Funciones reutilizables
├── requirements.txt
└── README.md
```

<br>

---

## 🛠️ Tech Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=plotly&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-3776AB?style=for-the-badge&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

</div>

| Componente | Herramienta |
|:---|:---|
| Lenguaje | Python 3 |
| Manipulación de datos | `pandas`, `numpy` |
| Visualización | `matplotlib`, `seaborn` |
| Estadística inferencial | `scipy` |
| Modelado y validación | `scikit-learn` |
| Entorno | Jupyter Notebook (local, versionado en Git/GitHub) |

<br>

---

## ✅ Entregables

- 📓 Notebooks reproducibles con EDA, limpieza, preparación, modelado y conclusiones.
- 🔧 Pipeline de preparación de datos documentado.
- 📊 Comparación de al menos tres modelos de regresión con justificación del elegido.
- 🎯 Análisis de errores: residuales, casos con mayor error y error por segmento.
- 💼 Recomendación de negocio: cómo usar la predicción, con qué límites y cautelas.

<br>

---

<div align="center">

### 📄 Fuente de Datos

Dataset obtenido de [OpenML — FIFA 22 Players (ID 45012)](https://www.openml.org/d/45012)
Uso exclusivamente académico — curso *Data Science for Business*, Futura, Data y Analítica Avanzada.

</div>

