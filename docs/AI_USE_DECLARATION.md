# Declaración de uso de inteligencia artificial

**Proyecto:** INF-8239 · Unidad 04 — Ética, gobernanza y DSS responsable  
**Laboratorios incluidos:** LAB11, LAB12 y LAB13  
**Herramienta:** ChatGPT (OpenAI)  
**Fecha de actualización:** 1 de noviembre de 2026

## 1. Herramienta y finalidad

Se utilizó ChatGPT como asistente académico y técnico para interpretar las instrucciones del Manual de Laboratorio, comprender conceptos de ética y gobernanza de sistemas de ciencia de datos, proponer contenido documental, interpretar las salidas de scripts y pruebas, y detectar errores de sintaxis. Su participación consistió en **orientación, revisión y propuestas de código y redacción**; no sustituyó la ejecución local ni la revisión crítica del estudiante.

## 2. Fragmentos y actividades asistidos

| Etapa | Apoyo de IA | Archivos o evidencias relacionados |
|---|---|---|
| **LAB11 — Impacto ético** | Propuesta de propósito, usos excluidos, responsabilidades, ocho escenarios de riesgo, controles y decisión `REVIEW`; explicación de los resultados. | `student_inputs/lab11_impact.yml`, `student_inputs/stakeholders.csv`, `student_inputs/risk_register.csv`, `reports/lab11/DECISION_RECORD.md` |
| **LAB11 — Código** | Identificación y propuesta de corrección de una cadena multilínea inválida. | `scripts/lab11_impact.py` |
| **LAB12 — Equidad** | Interpretación de métricas globales, por grupos e intersecciones; comparación de regresión logística y árbol de decisión; propuesta de criterio de tolerancia, limitaciones y recomendación `REVIEW`. | `student_inputs/lab12_decision.yml`, `reports/lab12/` y los resultados generados por la auditoría |
| **LAB13 — Gobernanza** | Revisión de la matriz RACI; propuesta de ajuste del umbral de FPR; interpretación de las puertas de despliegue; redacción de la ficha del modelo, procedimiento de incidentes e informe de simulación. | `student_inputs/raci.csv` (revisado, sin cambios propuestos), `configs/governance.yml`, `docs/MODEL_CARD.md`, `docs/RUNBOOK.md`, `reports/lab13/INCIDENT_POSTMORTEM.md` |
| **LAB13 — Código** | Identificación y propuesta de corrección de una cadena `f` multilínea inválida en el simulador de incidentes. | `scripts/simulate_incident.py` |
| **Documentación transversal** | Redacción y actualización de la documentación general y de esta declaración de uso de IA. | `README.md`, `docs/AI_USE_DECLARATION.md` |

## 3. Verificaciones realizadas por el estudiante

El estudiante trabajó en VS Code y ejecutó los comandos en su propio entorno, compartiendo los resultados para su interpretación:

- **LAB11:** `scripts/lab11_impact.py` se ejecutó tras corregir su sintaxis; **3 pruebas aprobadas** con `pytest`. Se verificó el commit `5918608` mediante comandos de Git.
- **LAB12:** se ejecutaron los modelos `logistic` y `tree`; se revisaron las métricas y archivos de auditoría, se obtuvo `decision_status: documented` y **2 pruebas aprobadas**. El estudiante confirmó el commit final de este laboratorio.
- **LAB13:** `scripts/lab13_governance.py` devolvió **`REVIEW`** por incumplimiento de la puerta `fpr_gap`; se corrigió y ejecutó `scripts/simulate_incident.py`; el estudiante informó que la aplicación Streamlit se ejecutó y obtuvo **10 pruebas aprobadas** mediante `uv run pytest -q`.

Estas comprobaciones validan la ejecución informada y el comportamiento de las pruebas programadas; **no certifican** equidad, seguridad operativa, eficacia de mitigaciones ni aptitud para desplegar el sistema en un entorno real. Al momento de esta actualización, **no se ha confirmado un commit final del LAB13**.

## 4. Errores, correcciones y decisiones metodológicas

1. **Errores de sintaxis en scripts del proyecto base:** se detectaron cadenas de texto abiertas incorrectamente en `scripts/lab11_impact.py` y `scripts/simulate_incident.py`. Se propusieron correcciones utilizando saltos de línea escapados (`\n`) o una cadena multilínea válida (`f"""..."""`), respectivamente; el estudiante volvió a ejecutar ambos scripts con éxito.
2. **Nombre del archivo de decisiones:** se aclaró que la ruta correcta es `student_inputs/lab12_decision.yml`, en singular.
3. **Umbral de equidad:** el valor original `max_fpr_gap: 0.16` de `configs/governance.yml` fue reemplazado por **`0.05`**, para mantener consistencia con la tolerancia propuesta en LAB12. Se trata de **una elección metodológica para el ejercicio**, no de una exigencia del manual ni de un estándar universal. Con una brecha FPR observada de **`0.125`**, esta puerta no se cumplió y el sistema recomendó **`REVIEW`**.
4. **Incidente simulado frente a hallazgo real:** el simulador plantea la señal hipotética `recall gap exceeded`; el incumplimiento observado en la ejecución real de LAB13 fue **`fpr_gap`**, no `recall_gap`. La redacción del informe diferencia expresamente ambos escenarios y no presenta la simulación como un incidente real.

## 5. Límites del uso de IA y responsabilidad académica

- Las propuestas de ChatGPT fueron materiales de apoyo sujetos a aceptación, corrección y comprobación por parte del estudiante.
- El proyecto trabaja con **datos sintéticos**; no se ha demostrado validez para personas, instituciones o territorios reales.
- Las diferencias estadísticas por grupo requieren análisis contextual: **no prueban por sí solas discriminación causal**.
- La IA no accedió al entorno local del estudiante, no ejecutó allí comandos, no realizó el despliegue real, no aprobó decisiones institucionales y no verificó directamente la ejecución de medidas correctivas.
- Los controles, responsables y plazos documentados en `MODEL_CARD.md`, `RUNBOOK.md` y `INCIDENT_POSTMORTEM.md` incluyen **propuestas académicas**, no garantías de implementación operativa.
- La revisión del contenido, las decisiones metodológicas, el uso de los resultados y la **responsabilidad académica final** corresponden al estudiante.

## 6. Referencias de orientación

- Manual de Laboratorio de la Unidad 04, LAB11–LAB13, facilitado en la asignatura.
- NIST, *AI Risk Management Framework*: https://www.nist.gov/itl/ai-risk-management-framework
- Fairlearn, *Assessment*: https://fairlearn.org/main/user_guide/assessment/index.html
- Python, *String and bytes literals*: https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals
