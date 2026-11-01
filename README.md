# INF-8239 · Unidad 04 — Gobernanza y DSS responsable

**Asignatura:** Ciencia de Datos II · Universidad Autónoma de Santo Domingo (UASD)  
**Unidad:** 04 — Ética y roles en ciencia de datos  
**Proyecto:** Sistema de apoyo a decisiones (DSS) para revisión de riesgo territorial **sintético**  
**Fecha de actualización:** 1 de noviembre de 2026  
**Alcance:** uso exclusivamente académico y experimental

## 1. Descripción general

Este repositorio integra tres laboratorios de la Unidad 04 para examinar un sistema de apoyo a decisiones (*Decision Support System*, DSS) desde la ética, la equidad algorítmica y la gobernanza. Se trabaja con datos **sintéticos** para priorizar casos simulados que podrían requerir revisión humana.

El proyecto **no representa personas ni territorios reales** y no constituye una herramienta para decisiones administrativas, asignaciones de recursos, intervenciones automáticas, elegibilidad o declaraciones de emergencia. Su propósito es aprender a medir, documentar, auditar y gestionar riesgos de un modelo predictivo.

### Estado de los laboratorios

| Laboratorio | Actividades y validación | Git |
|---|---|---|
| **LAB11 — Impacto ético** | Completado; 8 riesgos documentados; 3 pruebas aprobadas | Commit `5918608` confirmado |
| **LAB12 — Auditoría de equidad** | Completado; modelos Logistic y Tree comparados; 2 pruebas aprobadas | Commit final confirmado por el estudiante |
| **LAB13 — Gobernanza y Streamlit** | Scripts, documentación, simulación y aplicación local ejecutados; batería completa de 10 pruebas aprobada | **Commit final y etiqueta pendientes de confirmación** |

**Decisión de gobernanza observada:** `REVIEW`. El DSS puede utilizarse en el contexto académico controlado, pero **no está autorizado para despliegue real**.

## 2. Objetivos

- **LAB11:** delimitar el propósito y los usos prohibidos, identificar partes interesadas, construir un registro de riesgos y justificar la decisión ética.
- **LAB12:** evaluar dos clasificadores con métricas globales, por grupo e intersección; contrastar desempeño, equidad y recursos; documentar tolerancias y limitaciones.
- **LAB13:** vincular matriz RACI, políticas de gobernanza, controles de despliegue, documentación técnica, respuesta simulada a incidentes y demostración en Streamlit.

## 3. Tecnologías

- **Python 3.12**, con **uv** para dependencias y ejecución reproducible.
- **pandas** y **NumPy** para tratamiento de datos.
- **scikit-learn** para los modelos de regresión logística y árbol de decisión.
- **Fairlearn** para auditoría de métricas de equidad por grupos.
- **PyYAML** para configuraciones y decisiones.
- **pytest** para validaciones automatizadas.
- **Streamlit** y **Plotly** para componentes de la demostración interactiva.
- **Git** para trazabilidad de cambios y evidencias.

El proyecto define las dependencias en `pyproject.toml` y las versiones resueltas en `uv.lock`.

## 4. Estructura del repositorio

```text
.
├── app.py                          # Aplicación demostrativa Streamlit
├── configs/
│   └── governance.yml              # Políticas, usos prohibidos y umbrales
├── data/
│   └── territorial_risk_synthetic.csv
├── docs/
│   ├── AI_USE_DECLARATION.md       # Declaración de asistencia con IA
│   ├── DATASET_CARD.md             # Alcance del dataset
│   ├── MODEL_CARD.md               # Ficha técnica del modelo
│   └── RUNBOOK.md                  # Protocolo de respuesta a incidentes
├── evidencia/                       # Informes académicos del estudiante*
├── models/                          # Modelos locales (.joblib, ignorados por Git)
├── reports/
│   ├── lab11/                      # Riesgos y decisión ética
│   ├── lab12/
│   │   ├── logistic/               # Evaluación de regresión logística
│   │   └── tree/                   # Evaluación del árbol de decisión
│   └── lab13/                      # Decisión de despliegue e incidente simulado
├── scripts/                         # Comandos de los tres laboratorios
├── src/inf8239_u04/               # Lógica de datos, modelos, equidad y reglas
├── student_inputs/                 # Archivos completados por el estudiante
├── tests/                          # Pruebas automatizadas
├── pyproject.toml
├── uv.lock
└── requirements.txt                # Exportación adicional para despliegue*
```

\* `evidencia/` y `requirements.txt` se añadieron durante el trabajo del estudiante; no formaban parte del ZIP inicial. Algunas carpetas de resultados aparecen tras ejecutar los scripts.

## 5. Instalación y puesta en marcha

Desde la **raíz del proyecto**, con `uv` instalado:

```powershell
uv sync --frozen
uv run python scripts/check_environment.py
```

Para ejecutar las pruebas de todo el proyecto:

```powershell
uv run pytest -q
```

Las rutas utilizadas por los scripts son relativas; ejecutarlos desde otra ubicación puede provocar errores de lectura o escritura.

## 6. Dataset y estrategia de evaluación

El conjunto `data/territorial_risk_synthetic.csv` contiene **640 registros y 9 columnas**. La distribución de la variable objetivo binaria es `1`: **55.2 %** y `0`: **44.8 %**. Los grupos protegidos son **G1 (396 observaciones)** y **G2 (244 observaciones)**.

Se utiliza una **partición temporal**: 480 registros de entrenamiento, cuyo período termina el **24/09/2026**, y 160 registros de prueba, que empiezan el **25/09/2026**.

Las entradas de los modelos incluyen `zone`, `age_band`, `previous_incidents`, `infrastructure_score` y `exposure`. Se excluyen de los predictores `case_id`, `received_date`, `protected_group` y `target`.

> La exclusión de `protected_group` no demuestra ausencia de sesgo: otras variables pueden funcionar como indicadores indirectos. Además, `age_band` es una variable utilizada como predictor y señalada como sensible en `configs/governance.yml`.

Para inspeccionar el conjunto de datos:

```powershell
uv run python scripts/describe_dataset.py
```

## 7. LAB11 — Evaluación de impacto ético

**Estado:** completado y registrado en Git.

Se completaron `student_inputs/lab11_impact.yml`, `student_inputs/stakeholders.csv` y `student_inputs/risk_register.csv`. Se documentaron cuatro partes interesadas y ocho riesgos éticos. Los riesgos con mayor puntuación de prioridad fueron:

| Riesgo | Escenario | Prioridad | Responsable |
|---|---|---:|---|
| R02 | Calidad de datos | 9 | Data team |
| R01 | Sesgo territorial | 8 | Data team |
| R07 | Falta de mecanismos de apelación | 8 | Institutional owner |

**Decisión:** `REVIEW`, con continuidad académica controlada y sin autorización de uso real.

**Principales salidas:** `reports/lab11/risk_register_scored.csv`, `reports/lab11/stakeholders.csv`, `reports/lab11/rmf_summary.json` y `reports/lab11/DECISION_RECORD.md`.

```powershell
uv run python scripts/lab11_impact.py
uv run pytest -q tests/test_impact.py tests/test_privacy.py
```

**Evidencia de validación:** `3 passed in 0.68s`. **Commit verificado:** `5918608` (`docs: complete ethical impact assessment`).

**Precaución:** al volver a ejecutar `lab11_impact.py`, se puede **sobrescribir** `reports/lab11/DECISION_RECORD.md`. Antes de repetirlo, guardar una copia o verificar los cambios en Git.

## 8. LAB12 — Auditoría de equidad con Fairlearn

**Estado:** auditoría, documentación, pruebas y commit final completados.

Se entrenaron y compararon los modelos **Logistic (regresión logística)** y **Tree (árbol de decisión)** sobre las mismas 160 observaciones de prueba.

### Desempeño global

| Indicador | Logistic | Tree |
|---|---:|---:|
| Accuracy | **71.88 %** | 70.63 % |
| Recall | 71.74 % | 71.74 % |
| Precision | **77.65 %** | 75.86 % |
| FPR | **27.94 %** | 30.88 % |
| Tiempo de entrenamiento* | **0.0829 s** | 0.0898 s |
| Tamaño del modelo | **3.56 KB** | 7.87 KB |

### Brechas absolutas entre G1 y G2

| Indicador | Logistic | Tree |
|---|---:|---:|
| Selection rate | **23.24 pp** | 26.63 pp |
| Recall | **13.76 pp** | 18.13 pp |
| FPR | **12.50 pp** | 16.83 pp |
| Accuracy | **4.28 pp** | 6.26 pp |

Se priorizó la **brecha FPR** por su relación con el posible daño de clasificaciones positivas incorrectas. La tolerancia definida fue **0.05 (5 puntos porcentuales)**. Esta cifra es una **decisión metodológica del ejercicio**, no un estándar universal de Fairlearn ni una exigencia del manual. Ninguno de los dos modelos cumple la tolerancia.

La regresión logística se mantuvo como **referencia académica provisional**, porque obtuvo una exactitud ligeramente mayor, FPR global menor, brechas inferiores y un modelo más pequeño. No se ha demostrado que sea equitativa para un contexto real. La auditoría interseccional mostró subgrupos pequeños y diferencias que necesitan investigación adicional.

**Decisión:** `REVIEW`. Las brechas observadas **no son, por sí mismas, prueba de discriminación causal**.

```powershell
uv run python scripts/lab12_audit.py --model logistic
uv run python scripts/lab12_audit.py --model tree
uv run python scripts/validate_lab12_decision.py
uv run pytest -q tests/test_fairness.py tests/test_model.py
```

**Evidencias:** `reports/lab12/logistic/`, `reports/lab12/tree/` y **`student_inputs/lab12_decision.yml`** (singular). El validador mostró `decision_status: documented` y las pruebas `2 passed in 4.07s`.

\* Los tiempos corresponden a una ejecución de entrenamiento y no a un estudio estadístico de eficiencia.

## 9. LAB13 — Gobernanza, DSS y simulación de incidentes

**Estado:** actividades técnicas y documentales realizadas; **cierre en Git pendiente de confirmación**.

### 9.1. Responsabilidades y políticas

Se revisó `student_inputs/raci.csv`, que contempla calidad de datos, validación del modelo, despliegue y respuesta a incidentes. La matriz identifica responsables de ejecución (**R**) y rendición final de cuentas (**A**), además de roles consultados (**C**) e informados (**I**).

En `configs/governance.yml` se conservaron los usos prohibidos y cuatro umbrales del proyecto base, y se **modificó solamente `max_fpr_gap` de `0.16` a `0.05`**, para mantener coherencia con la evaluación del LAB12. El cambio fue decidido y documentado para esta práctica; no constituye una exigencia original del manual.

### 9.2. Evaluación de las puertas de gobernanza

La ejecución del modelo Logistic con las reglas configuradas produjo:

| Control | Resultado observado | Criterio | Evaluación |
|---|---:|---:|---|
| Recall global | 71.74 % | Mínimo 68 % | Cumple |
| Brecha Recall | 13.76 pp | Máximo 16 pp | Cumple |
| **Brecha FPR** | **12.50 pp** | **Máximo 5 pp** | **No cumple** |
| Latencia promedio por registro | 0.0343 ms | Máximo 500 ms | Cumple |
| Tamaño del modelo | 3.56 KB | Máximo 1024 KB | Cumple |

**Resultado generado:** `REVIEW` — 4 verificaciones aprobadas y 1 incumplida (`fpr_gap`). La acción requerida es documentar la revisión y la decisión humana final. Las evidencias se encuentran en `reports/lab13/deployment_decision.json` y `reports/lab13/fairness_by_group.csv`.

```powershell
uv run python scripts/lab13_governance.py
```

**Precisión técnica:** la función actual `deployment_decision()` retorna **`GO` o `REVIEW`** según los controles numéricos. No implementa una tercera salida automática `STOP`. Una suspensión o rechazo operativo debe documentarse como decisión de gobernanza humana; `GO` tampoco equivaldría por sí solo a autorización institucional.

### 9.3. Documentación e incidente simulado

Se prepararon `docs/MODEL_CARD.md` y `docs/RUNBOOK.md`, con usos, límites, métricas, responsables, supervisión, revisión y respuesta ante incidentes.

También se corrigió un error de sintaxis del proyecto base en `scripts/simulate_incident.py`. El script generó `reports/lab13/INCIDENT_POSTMORTEM.md` y se completaron los apartados de comunicación, acciones correctivas, responsables y fechas propuestas.

**Diferencia importante:** el incidente del simulador se basa en la señal **hipotética** `recall gap exceeded`. En la ejecución observada del LAB13, **recall_gap sí cumplió**; el incumplimiento medido fue **fpr_gap**. No se ha constatado ningún incidente operativo real, notificación efectiva a personas afectadas ni implementación real de mitigaciones.

```powershell
uv run python scripts/simulate_incident.py
```

**Precaución:** el simulador puede sobrescribir el informe `INCIDENT_POSTMORTEM.md` ya completado; conservar una copia antes de volver a ejecutarlo.

### 9.4. Aplicación local en Streamlit

La aplicación `app.py` fue **ejecutada localmente por el estudiante**. Muestra el propósito y usos prohibidos, exactitud y recall, métricas por grupo, resultado de las puertas de calidad y el recordatorio de acción humana.

```powershell
uv run streamlit run app.py
```

El uso local de Streamlit **no constituye publicación en Internet ni despliegue productivo**. La publicación en Streamlit Community Cloud fue descrita como **opcional** y no está confirmada.

## 10. Pruebas y evidencia de reproducibilidad

Se confirmó la ejecución satisfactoria de la batería completa del proyecto:

```powershell
uv run pytest -q
```

**Resultado compartido:** `10 passed in 5.21s`.

Este resultado demuestra que **las diez pruebas programadas pasaron** en el entorno reportado. No certifica ausencia de sesgos, seguridad para casos reales, cumplimiento normativo ni idoneidad de despliegue.

Los artefactos utilizados para analizar el LAB13 son `configs/governance.yml`, `student_inputs/raci.csv`, `docs/MODEL_CARD.md`, `docs/RUNBOOK.md` y `reports/lab13/`.

## 11. Control de versiones y preparación del cierre

Los LAB11 y LAB12 cuentan con registro final en Git. En la última comprobación reportada para LAB13, `git status` mostraba cambios **sin preparar** y archivos nuevos. Por tanto, aún no se considera confirmado su commit de cierre ni la etiqueta `u04-lab13`.

Antes del commit final:

1. Revisar `git status`, `git diff` y `git diff --cached --stat` para confirmar las modificaciones.
2. Incluir documentación, configuración, scripts corregidos y resultados correspondientes a LAB13.
3. Evitar incluir archivos temporales de Word (`~$*.docx`), secretos, entornos virtuales o datos personales. Los cambios pendientes en documentos de LAB12 deben revisarse por separado.
4. Comprobar el `requirements.txt` exportado por `uv` antes de usarlo para un despliegue público; su generación **no prueba** compatibilidad con Streamlit Community Cloud.
5. Registrar el commit y, si corresponde al procedimiento de entrega, crear y verificar la etiqueta `u04-lab13`.

Para examinar los cambios sin alterar el repositorio:

```powershell
git status
git diff --stat
git diff --cached --stat
```

No se incluyen aquí comandos de publicación remota ni se presume que el repositorio público contiene ya el resultado final de LAB13.

## 12. Limitaciones y medidas de uso responsable

- **Datos sintéticos:** las métricas no son generalizables a personas o territorios reales.
- **Equidad:** las brechas requieren análisis de tamaños muestrales, prevalencias, errores e intersecciones; no prueban causalidad.
- **Privacidad:** no agregar identificadores personales o expedientes reales a un repositorio público.
- **Supervisión humana:** los controles técnicos no sustituyen autorización institucional, apelación y trazabilidad.
- **Green AI:** se midieron tiempo de entrenamiento, tamaño y latencia, pero **no** consumo energético ni emisiones de carbono; no puede afirmarse sostenibilidad demostrada.
- **Simulaciones:** un protocolo documentado o un incidente simulado no equivale a un control operacional verificado.
- **Interpretación de estados:** `REVIEW` impide considerar aprobado el despliegue real aunque la suite de pruebas haya terminado correctamente.

## 13. Declaración de uso de inteligencia artificial

Durante **LAB11, LAB12 y LAB13** se utilizó **ChatGPT (OpenAI)** como apoyo académico y técnico para comprender instrucciones, proponer textos, interpretar las métricas, contrastar modelos y revisar errores de programación.

El uso incluyó asistencia en la elaboración de evaluaciones éticas, registros de riesgos, decisiones de equidad, documentación de gobernanza, `MODEL_CARD.md`, `RUNBOOK.md`, el informe del incidente simulado y este README. También se propusieron correcciones de sintaxis en `scripts/lab11_impact.py` y `scripts/simulate_incident.py`.

**La ejecución de los scripts y pruebas, la revisión de sus salidas y el manejo de Git fueron realizados por el estudiante en su entorno.** Las redacciones y propuestas de IA no sustituyen validación empírica, criterio académico ni autorización institucional. El resultado `REVIEW` y el carácter sintético del proyecto permanecen expresamente documentados.

La declaración detallada —herramienta, finalidad, fragmentos asistidos, verificaciones, errores, correcciones y limitaciones— está disponible en [`docs/AI_USE_DECLARATION.md`](docs/AI_USE_DECLARATION.md).

## 14. Fuentes y referencias

- *U04 Libro.pdf*, *U04 Ruta Maestra.pdf* y *U04 Manual de Laboratorio.pdf*, materiales de la asignatura.
- [Fairlearn — Assessment](https://fairlearn.org/main/user_guide/assessment/index.html).
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework).
- [uv — Documentation](https://docs.astral.sh/uv/).
- [Streamlit — Documentation](https://docs.streamlit.io/).
- [pytest — Documentation](https://docs.pytest.org/).

---

**Nota de mantenimiento:** al finalizar el commit y la etiqueta de LAB13, actualizar el estado de Git en este README con el identificador verificado. No registrar como realizadas las mitigaciones, comunicaciones, autorizaciones o publicaciones que todavía no tienen evidencia.
