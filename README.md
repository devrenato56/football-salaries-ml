# ⚽ Football Salaries Predictor

**Autor:** Renato Gutierrez

Proyecto final de módulo del curso **Data Science for Business**, dictado por **Futura, Data y Analítica Avanzada**, dentro del bloque de **Regresión Supervisada**.

---

## 📌 Descripción del Proyecto

Este proyecto aborda un problema de **Machine Learning aplicado a Sports Analytics**: estimar el salario semanal (`wage_eur`) de jugadores profesionales de fútbol a partir de sus atributos técnicos, físicos y demográficos, utilizando el dataset **FIFA 22 Players**.

El caso simula una situación real de negocio dentro del mundo del fútbol profesional: clubes, agentes y analistas deportivos necesitan **valorizar talento de forma objetiva** para tomar decisiones de fichajes, renovaciones y negociación salarial. Este proyecto construye una solución de regresión que sustenta ese tipo de decisiones con evidencia basada en datos, en lugar de solo criterio subjetivo.

### Contexto de Negocio

La decisión que se busca soportar es transformar el análisis exploratorio y el modelo predictivo en una **recomendación accionable** para sports analytics, valorización de talento y negociación salarial — dejando claro qué variables importan más, cuándo confiar en la predicción del modelo, y en qué segmentos de jugadores el error esperado es mayor.

### Objetivo Principal

> Estimar el salario semanal de jugadores profesionales a partir de atributos técnicos, físicos y demográficos.

### Objetivos Específicos

- Auditar la calidad de los datos: valores atípicos, codificaciones, variables con riesgo de *leakage*.
- Preparar variables numéricas, categóricas y estructurales según corresponda (jugadores de campo vs. arqueros).
- Construir una solución reproducible de regresión.
- Interpretar resultados en lenguaje de negocio, con recomendaciones accionables para el stakeholder.
- Documentar restricciones éticas, operativas y de despliegue del modelo.

### Preguntas que el Proyecto busca Responder

- ¿En qué unidad se interpreta `wage_eur` y qué margen de error es aceptable para el negocio?
- ¿Qué variables podrían generar *leakage* o no estar disponibles antes de predecir?
- ¿Qué modelo se recomienda y cómo mejora frente a una línea base simple?
- ¿Dónde se concentran los errores más grandes y qué segmentos requieren mayor cautela?
- ¿Cómo se usaría la predicción en la práctica: pricing, priorización, planeación o simulación de fichajes?

---

## 📊 Dataset

| Campo | Detalle |
|---|---|
| **Nombre** | FIFA 22 Players |
| **Fuente** | [OpenML](https://www.openml.org/d/45012) |
| **Filas** | 19,178 jugadores |
| **Variables** | 29 |
| **Variable objetivo** | `wage_eur` (salario semanal, valor continuo) |
| **Tipo de problema** | Regresión supervisada |

El dataset incluye atributos demográficos (edad, altura, peso, nacionalidad), atributos generales de valoración (`overall`, `potential`), y atributos técnicos específicos de FIFA 22 agrupados en categorías: ataque (`attacking_*`), habilidad (`skill_*`), movimiento (`movement_*`), defensa (`defending_*`) y portería (`goalkeeping_*`).

Un desafío particular del dataset es que combina **jugadores de campo y arqueros** en las mismas columnas, por lo que buena parte de la limpieza y preparación de datos requiere tratar ambos grupos de forma diferenciada para evitar conclusiones erróneas (por ejemplo, al analizar outliers o correlaciones).

---

## 🧭 Plan de Análisis

El proyecto sigue una metodología estándar de ciencia de datos aplicada a negocio, dividida en las siguientes fases:

1. **Análisis Exploratorio de Datos (EDA)** — calidad de datos, distribución del target, análisis de variables relevantes y hallazgos en lenguaje de negocio.
2. **Estadística Inferencial** — formulación y contraste de hipótesis de negocio mediante pruebas estadísticas (t-test, ANOVA, correlación, pruebas no paramétricas, según corresponda).
3. **Análisis Multivariante** — relaciones simultáneas entre variables, redundancias y reducción dimensional para apoyar la selección de features.
4. **Preparación de Datos y Feature Engineering** — codificación de categóricas, escalamiento, control de *leakage* y exclusión justificada de variables.
5. **Modelado Predictivo** — comparación de algoritmos de regresión, validación, análisis de residuales y explicación de errores en términos de negocio.

### Algoritmos Considerados

El proyecto compara distintas familias de modelos de regresión para seleccionar el más adecuado según desempeño, interpretabilidad y comportamiento de residuales:

- **Regresión Lineal Regularizada** (Ridge / Lasso) — como línea base interpretable.
- **Árbol de Regresión (Decision Tree Regressor)** — para capturar relaciones no lineales simples.
- **Random Forest Regressor** — ensamble robusto frente a outliers y relaciones no lineales complejas.
- **Gradient Boosting** (p. ej. XGBoost / LightGBM / GradientBoostingRegressor) — para maximizar desempeño predictivo mediante boosting secuencial.

La selección final del modelo se basa en la comparación de métricas de error, no únicamente en R², e incluye análisis de residuales y desempeño por segmentos.

---

## 📈 Métricas de Éxito

| Métrica | Uso |
|---|---|
| **MAE** | Error promedio interpretable en unidades del negocio (euros) |
| **RMSE** | Penaliza errores grandes con mayor severidad |
| **R² (test)** | Varianza explicada fuera de muestra |
| **MAPE / SMAPE** | Error relativo, cuando el target lo permite |
| **Error por segmento** | Detección de sesgos por grupo, posición o geografía |

---

## 🗂️ Estructura del Proyecto

```
football-salaries-ml/
├── data/
│   ├── raw/              # Dataset original sin modificar
│   └── processed/        # Datasets limpios/transformados
├── notebooks/
│   ├── 01_eda.ipynb                 # Análisis Exploratorio de Datos
│   ├── 02_inferencial.ipynb         # Estadística Inferencial
│   ├── 03_multivariante.ipynb       # Análisis Multivariante
│   ├── 04_feature_engineering.ipynb # Preparación del dataset
│   └── 05_modelado.ipynb            # Modelado predictivo
├── reports/
│   └── figures/           # Gráficos exportados del análisis
├── docs/
│   └── resumen_ejecutivo.md
├── src/                    # Funciones reutilizables (si aplica)
├── requirements.txt
└── README.md
```

---

## 🛠️ Stack Tecnológico

**Lenguaje:** Python 3

**Librerías principales:**

| Librería | Uso |
|---|---|
| `pandas` | Manipulación y limpieza de datos |
| `numpy` | Operaciones numéricas |
| `matplotlib` | Visualización de datos |
| `seaborn` | Visualización estadística (EDA) |
| `scipy` | Pruebas estadísticas inferenciales |
| `scikit-learn` | Preprocesamiento, modelado y validación |

**Entorno de desarrollo:** Jupyter Notebook (`.ipynb`), trabajado localmente y versionado en Git/GitHub.

---

## ✅ Entregables del Proyecto

- Notebooks reproducibles con EDA, limpieza, preparación, modelado y conclusiones.
- Pipeline de preparación de datos documentado (tratamiento de outliers, codificación, exclusión de variables).
- Comparación de al menos tres modelos de regresión con justificación del modelo recomendado.
- Análisis de errores: residuales, casos con mayor error y error por segmento.
- Recomendación de negocio: cómo usar la predicción, con qué límites y cautelas.

---

## 📄 Licencia y Fuente de Datos

Dataset obtenido de [OpenML — FIFA 22 Players (ID 45012)](https://www.openml.org/d/45012). Uso exclusivamente académico en el marco del curso Data Science for Business de Futura, Data y Analítica Avanzada.
