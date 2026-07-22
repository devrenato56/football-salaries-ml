# Plan de Trabajo — Caso 11: Salario de Futbolistas (FIFA 22 Players)

**Tipo de problema:** Regresión supervisada
**Variable objetivo:** `wage_eur`
**Dataset:** FIFA 22 Players — 19,178 filas, 29 variables (OpenML #45012)

---

## Fase 0 — Preparación del Entorno y Entendimiento del Caso

- [ ] Leer el enunciado completo (`proyecto.md` / `proyecto.html`) y la metadata (`metadata.json`).
- [ ] Configurar el entorno de trabajo (notebook, librerías: pandas, numpy, scikit-learn, matplotlib/seaborn).
- [ ] Cargar `dataset.csv` y validar que coincide con la metadata (19,178 filas, 29 columnas, hash MD5 si aplica).
- [ ] Confirmar que el notebook no dependerá de rutas absolutas personales.

---

## Fase 1 — Entendimiento del Negocio

- [ ] Redactar el problema de negocio en una frase: estimar el salario semanal (`wage_eur`) de jugadores profesionales.
- [ ] Identificar stakeholders y su interés (sponsor, equipo operativo, analytics, riesgo/compliance, dirección).
- [ ] Responder explícitamente:
  - [ ] ¿En qué unidad se interpreta `wage_eur` y qué error sería aceptable?
  - [ ] ¿Qué variables podrían generar leakage o no estar disponibles antes de predecir?
  - [ ] ¿Cómo se usaría la predicción (pricing, priorización, planeación, alerta, simulación)?
- [ ] Documentar la decisión que se espera soportar con el análisis.

---

## Fase 2 — Auditoría y Calidad de Datos

- [ ] Revisar tipos de datos, valores nulos/faltantes y cardinalidad por variable.
- [ ] Detectar outliers en variables numéricas (edad, atributos físicos/técnicos, `wage_eur`).
- [ ] Revisar consistencia de codificación en `nationality_name` (categórica).
- [ ] Identificar variables candidatas a leakage o baja utilidad (ej. atributos de portero para jugadores de campo).
- [ ] Documentar qué columnas se excluirán y por qué (identificador, leakage, privacidad, baja utilidad).
- [ ] Definir estrategia de partición train/test (asegurando que imputación/escalado se aprenda solo con train).

---

## Fase 3 — Análisis Exploratorio de Datos (EDA)

- [ ] Analizar la distribución de `wage_eur` (asimetría, posible transformación log).
- [ ] Analizar distribución de al menos 5 variables relevantes (`age`, `overall`, `potential`, `height_cm`, `movement_reactions`, etc.).
- [ ] Explorar correlaciones entre variables numéricas y el target.
- [ ] Explorar relación entre `nationality_name` y `wage_eur` (top países, agrupaciones).
- [ ] Redactar al menos 3 hallazgos en lenguaje de negocio (no solo gráficos).

---

## Fase 4 — Estadística Inferencial

- [ ] Formular al menos 2 hipótesis de negocio verificables (ej. "jugadores con mayor `overall` tienen salario significativamente mayor").
- [ ] Seleccionar y aplicar pruebas estadísticas apropiadas (t-test, ANOVA, correlación, no paramétricas según corresponda).
- [ ] Reportar resultados (p-valores, intervalos de confianza) e interpretarlos.
- [ ] Explicar si los resultados cambian alguna decisión de negocio.

---

## Fase 5 — Análisis Multivariante

- [ ] Construir matriz de correlación completa y detectar redundancias entre variables técnicas.
- [ ] Aplicar reducción dimensional (PCA) o análisis de agrupamiento de variables si aporta valor.
- [ ] Identificar grupos de variables correlacionadas (ej. atributos de ataque vs. defensa vs. portería).
- [ ] Usar estos hallazgos para apoyar la selección de features en el modelado.

---

## Fase 6 — Preparación del Dataset y Feature Engineering

- [ ] Excluir identificadores y variables con leakage confirmadas en Fase 2.
- [ ] Codificar `nationality_name` (ej. target encoding, agrupación por frecuencia, o one-hot si la cardinalidad lo permite).
- [ ] Escalar/normalizar variables numéricas según el modelo a usar.
- [ ] Evaluar transformación del target (`log(wage_eur)`) si la distribución lo justifica.
- [ ] Derivar variables adicionales si aportan valor (ej. ratio potencial-overall, índice de forma física).
- [ ] Documentar el pipeline final de transformación de forma reproducible.

---

## Fase 7 — Modelado Predictivo

- [ ] Definir línea base simple (ej. media/mediana de `wage_eur`, o regresión lineal simple).
- [ ] Entrenar y comparar al menos 3 modelos:
  - [ ] Regresión lineal regularizada (Ridge/Lasso)
  - [ ] Árbol de regresión
  - [ ] Random Forest
  - [ ] Gradient Boosting (opcional, recomendado para mejorar el ranking)
- [ ] Realizar validación cruzada o partición train/validation/test correctamente separada.
- [ ] Ajustar hiperparámetros del modelo(s) más prometedor(es).
- [ ] Justificar la configuración final elegida.

---

## Fase 8 — Evaluación y Análisis de Errores

- [ ] Calcular métricas: MAE, RMSE, R² test, MAPE/SMAPE.
- [ ] Comparar desempeño del modelo final contra la línea base.
- [ ] Analizar residuales (distribución, heterocedasticidad, patrones).
- [ ] Identificar casos con mayor error y posibles causas.
- [ ] Calcular error por segmento relevante (ej. por posición, rango de edad, nacionalidad, nivel salarial).

---

## Fase 9 — Interpretación y Recomendación de Negocio

- [ ] Identificar los principales drivers del salario según el modelo (importancia de variables / coeficientes).
- [ ] Traducir hallazgos técnicos a lenguaje de negocio.
- [ ] Redactar recomendación accionable: qué hacer, con quién, por qué y con qué cautelas.
- [ ] Documentar límites del modelo y riesgos de implementación (sesgos, generalización, actualización de datos).

---

## Fase 10 — Documentación y Entrega Final

- [ ] Consolidar notebook reproducible (EDA → limpieza → preparación → modelado → evaluación → conclusiones).
- [ ] Redactar resumen ejecutivo (problema, hallazgos, recomendación, impacto esperado, límites).
- [ ] Elaborar tabla de variables usadas, excluidas y motivo de exclusión.
- [ ] Adjuntar evidencia de validación (métricas, gráficos, residuales, perfiles de segmento).
- [ ] Pasar el checklist final antes de entregar:
  - [ ] El notebook corre de inicio a fin sin rutas absolutas personales.
  - [ ] El dataset y target coinciden con la metadata.
  - [ ] Variables con leakage/identificadores excluidas y justificadas.
  - [ ] Decisiones de limpieza, imputación, codificación y escalamiento documentadas.
  - [ ] MAE/RMSE reportados en unidades interpretables.
  - [ ] Residuales analizados y explicados.
  - [ ] Al menos 3 hallazgos de negocio sustentados por datos.
  - [ ] Recomendación final clara y accionable.
  - [ ] Resumen ejecutivo legible sin revisar el código.

---

## Referencia — Pesos de Evaluación

| Criterio | Peso |
|---|---:|
| Entendimiento del negocio | 15% |
| Calidad y preparación de datos | 20% |
| Análisis exploratorio e inferencial | 15% |
| Modelado o segmentación | 25% |
| Interpretación y recomendación | 20% |
| Presentación reproducible | 5% |
