# Informe Final — Predicción de Salarios de Futbolistas (FIFA 22)

**Tipo de problema:** Regresión supervisada · **Variable objetivo:** `wage_eur` (salario semanal en euros)
**Dataset:** FIFA 22 Players — 19,178 jugadores, 29 variables (OpenML #45012)

---

## 1. Resumen ejecutivo

El objetivo del proyecto fue **estimar el salario semanal de un futbolista profesional a partir de sus características** deportivas, físicas y de contexto. Es un problema relevante para clubes y agencias: contar con una referencia objetiva de sueldo permite detectar oportunidades de fichaje, ordenar negociaciones y auditar la coherencia de una plantilla.

Tras un ciclo completo de análisis (exploración, inferencia, preparación y modelado), se construyó un modelo de **Gradient Boosting** que predice el sueldo con buena precisión:

- Reduce el error de estimación **a menos de la mitad** respecto de una referencia ingenua (MAE de ~7,650 € a ~3,765 €).
- Explica alrededor del **73% de la variabilidad** del salario (R² ≈ 0.73).
- Sus decisiones se apoyan, sobre todo, en la **calidad global del jugador** (`overall`), con la **nacionalidad** como contexto de mercado secundario.

El modelo es fiable para el grueso de los futbolistas (nivel bajo, medio y alto) y tiene un punto débil conocido: **subestima a las superestrellas**, cuyo sueldo depende de factores extra-deportivos (marketing, prestigio del club, cláusulas) ausentes en los datos.

**Recomendación:** usarlo como **herramienta de referencia salarial (benchmarking)** para scouting y dirección deportiva —no como veredicto exacto, sino como rango orientativo que enmarca decisiones y señala desviaciones a explicar.

---

## 2. Hallazgos principales (en lenguaje de negocio)

1. **El sueldo es un juego de élites, no de promedios.** La mitad de los jugadores gana ≤3,000 €, mientras una minoría llega a 350,000 €. El análisis debe (y el modelo logra) explicar los extremos, donde se mueve el presupuesto real.
2. **La calidad global manda.** El `overall` es el impulsor dominante del sueldo, validado estadísticamente (Mann-Whitney, p < 0.001, efecto grande). Las habilidades técnicas sueltas pesan poco por separado.
3. **La relación nivel–sueldo es explosiva, no proporcional.** Subir de bueno a excelente multiplica el sueldo en lugar de sumarle un poco; por eso los modelos no lineales (árboles/boosting) superan claramente a los lineales.
4. **La nacionalidad pesa como contexto de mercado, no como causa.** Diferencia sueldos de forma significativa (Kruskal-Wallis, p ≈ 10⁻²⁰⁶) pero con efecto moderado (~11%): refleja el poder económico de la liga donde se desarrolla el jugador.

---

## 3. Tabla de variables

### Variables usadas (predictoras)

| Grupo | Variables | Tratamiento |
|---|---|---|
| Calidad / veteranía | `overall`, `potential`, `movement_reactions`, `age` | Escaladas (StandardScaler) |
| Técnica ofensiva | `attacking_*`, `skill_*` | Escaladas |
| Físico / movimiento | `height_cm`, `weight_kg`, `movement_*` | Escaladas |
| Defensa | `defending_standing_tackle`, `defending_sliding_tackle` | Escaladas |
| Portería | `goalkeeping_*` (5) | Escaladas (además resumidas en `gk_index`) |
| Contexto | `nationality_name` | Target encoding suavizado (`nat_target_enc`) + frequency encoding (`nat_freq_enc`) |

### Variables derivadas (feature engineering)

| Variable | Definición | Aporte |
|---|---|---|
| `growth` | `potential − overall` | Etapa de carrera (promesa vs. consolidado) |
| `gk_index` | Promedio de los 5 atributos de portería | Comprime la familia de portería / detecta porteros |
| `att_index` | Promedio de atributos técnico-ofensivos | Resume la familia ofensiva |
| `def_index` | Promedio de atributos defensivos | Resume la familia defensiva |
| `imc` | Índice de masa corporal (peso/estatura²) | Constitución física |

### Variables excluidas

| Variable / tipo | Motivo |
|---|---|
| Identificadores (nombre, ID) | **No existen** en el dataset — nada que excluir |
| Variables con leakage | **Ninguna**: `overall`/`potential` son valoraciones deportivas, no derivadas del sueldo |
| — | La única "poda" prevista es por **redundancia** (atributos muy correlacionados), no por leakage |

### Transformación del target

`wage_eur` → **`log(1 + wage_eur)`** para el entrenamiento (normaliza la asimetría de 6.47 a 0.47 y optimiza el error relativo). Las predicciones se reconvierten a euros con `expm1` para reportar.

---

## 4. Evidencia de validación

### Comparación de modelos (test, en euros)

| Modelo | MAE (€) | RMSE (€) | R² | MAPE (%) |
|---|--:|--:|--:|--:|
| Línea base (mediana) | 7,650 | 20,409 | −0.09 | 143 |
| Ridge (lineal) | 4,544 | 11,741 | 0.64 | 86 |
| Árbol de decisión | 5,238 | 12,411 | 0.60 | 101 |
| Random Forest | 3,808 | 9,475 | 0.77 | 65 |
| **Gradient Boosting (final)** | **3,765** | 10,170 | 0.73 | **64** |

*Modelo final:* Gradient Boosting afinado (`max_depth=4`, `learning_rate=0.08`, `subsample=0.8`, `min_samples_leaf=5`, `n_estimators=300`). Elegido por su mejor error típico (MAE/MAPE) y su validación cruzada, asumiendo una leve desventaja en RMSE ligada a los sueldos extremos.

### Análisis de residuales

- Residual **mediano ≈ 0 €** (bien calibrado en el caso típico); residual medio positivo (~+1,300 €) por subestimación de estrellas.
- **Heterocedasticidad** esperada: el error crece con el nivel salarial.

### Error por segmento salarial

| Tramo | MAE (€) | MAPE (%) |
|---|--:|--:|
| Bajo (<2k €) | ~860 | ~92 |
| Medio (2k–5k €) | ~1,390 | ~36 |
| Alto (5k–20k €) | ~4,310 | ~40 |
| Élite (>20k €) | ~19,370 | ~40 |

El MAPE es estable (~40%) salvo en el tramo bajo, donde se dispara por puro efecto de escala sobre cifras pequeñas.

### Drivers del modelo (importancia de variables)

`overall` (dominante) ≫ `nat_target_enc` > `nat_freq_enc` > `age` > `potential`. Coincide con lo que el EDA y la inferencia habían señalado.

---

## 5. Recomendación y cautelas

**Recomendación accionable:** desplegar el modelo como **referencia de sueldo de mercado** para el área de scouting/dirección deportiva —detección de infravalorados, auditoría de plantilla, cifra de partida en negociaciones—, con el área financiera validando rangos.

**Cautelas de implementación:**
- No confiar en la predicción puntual de **superestrellas** (piso orientativo, no cifra).
- Error relativo alto en **sueldos muy bajos** (útil para orden de magnitud).
- La variable de **nacionalidad** es contexto de mercado; no usarla para justificar pagar menos a jugadores de ciertos países (riesgo de sesgo).
- **Reentrenar cada temporada**: el mercado del fútbol cambia rápido (el modelo es una foto de FIFA 22).
- El modelo **predice, no explica causas** (correlación ≠ causalidad).

---

## 6. Checklist final de entrega

- [x] El notebook corre de inicio a fin sin rutas absolutas personales (rutas relativas a `data/`).
- [x] El dataset y target coinciden con la metadata (19,178 filas, `wage_eur`).
- [x] Variables con leakage/identificadores revisadas y justificadas (no hay que excluir).
- [x] Decisiones de limpieza, imputación, codificación y escalamiento documentadas.
- [x] MAE/RMSE reportados en unidades interpretables (euros).
- [x] Residuales analizados y explicados.
- [x] Al menos 3 hallazgos de negocio sustentados por datos.
- [x] Recomendación final clara y accionable.
- [x] Resumen ejecutivo legible sin revisar el código.

---

## 7. Estructura del proyecto (reproducibilidad)

| Notebook | Fase(s) | Contenido |
|---|---|---|
| `notebooks/01_eda.ipynb` | 2–3 | Calidad de datos + análisis exploratorio |
| `notebooks/02_inferencial.ipynb` | 4 | Estadística inferencial (hipótesis, pruebas) |
| `notebooks/03_multivariante.ipynb` | 5 | Correlaciones, PCA, agrupamiento de variables |
| `notebooks/04_feature_engineering.ipynb` | 6 | Preparación, codificación, escalado, pipeline → `data/processed/` |
| `notebooks/05_modelado.ipynb` | 7–9 | Modelado, evaluación, análisis de errores e interpretación de negocio |

El pipeline de preparación (Fase 6) exporta `data/processed/train.csv` y `test.csv`, que el notebook de modelado consume directamente, garantizando reproducibilidad de punta a punta.
