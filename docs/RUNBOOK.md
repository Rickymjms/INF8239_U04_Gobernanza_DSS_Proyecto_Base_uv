# Incident Runbook — Territorial Review DSS (Synthetic)

**Unidad:** 04 — Ética y roles en Ciencia de Datos | **Laboratorio:** LAB13  
**Alcance:** demostración académica con datos sintéticos. No autoriza decisiones automáticas ni uso operativo sobre personas o territorios reales.  
**Estado al 01/11/2026:** `REVIEW` por incumplimiento de la puerta de equidad `fpr_gap`; **no se ha documentado un incidente real ni un despliegue en producción**.

## 1. Detection and classification

El **ML Engineer** revisará la salida del DSS, las métricas por grupo, los controles de `configs/governance.yml` y cualquier reporte de uso fuera del alcance, errores del modelo o exposición de información.

- **Señal observada en LAB13:** `fpr_gap = 0.125`, superior al máximo configurado de `0.05`; los demás cuatro controles automáticos aprobaron. El sistema produjo `REVIEW`, lo que exige revisión y decisión humana antes de cualquier despliegue.
- **Incidente de equidad (alto):** una brecha que supera el umbral de gobernanza, especialmente si persiste o afecta a un subgrupo. Requiere investigación y revisión del modelo; no demuestra por sí sola discriminación causal.
- **Incidente crítico:** uso prohibido, intervención automática no autorizada, filtración de datos personales o exposición de personas reales. Requiere suspender el uso y escalar inmediatamente a la autoridad institucional.

La clasificación crítica y la eventual orden de **detener** son decisiones humanas: la lógica automatizada actual del proyecto solo devuelve `GO` o `REVIEW`, no un estado `STOP` implementado.

## 2. Containment authority

Según `student_inputs/raci.csv`, para **Incident response** el **ML Engineer** es **Responsible (R)**, el **Institutional Owner** es **Accountable (A)**, **Legal and Data Scientist** son **Consulted (C)** y **Affected representatives** son **Informed (I)**.

Cuando se detecte una señal de incumplimiento, el ML Engineer deberá impedir que la recomendación se interprete como autorización de despliegue, informar al Institutional Owner y preparar la revisión técnica. El Institutional Owner decidirá formalmente la suspensión, revisión o reanudación; el **Product Owner**, responsable del despliegue en la matriz RACI, no podrá autorizarlo mientras persistan incumplimientos sin resolución documentada.

Para este ejercicio, se mantiene la **continuidad de pruebas académicas controladas**, pero **no se autoriza ningún despliegue real**.

## 3. Evidence preservation

Se conservarán, sin alterar sus originales, los artefactos que permiten reconstruir el resultado y la decisión:

- `reports/lab13/deployment_decision.json` y `reports/lab13/fairness_by_group.csv`.
- `configs/governance.yml` (incluido el cambio justificado de `max_fpr_gap: 0.16` a `0.05`).
- `student_inputs/lab12_decision.yml`, informes de LAB12 y registros de riesgos y decisiones de LAB11.
- `student_inputs/raci.csv`, `docs/MODEL_CARD.md`, la versión del código y el identificador del commit de Git.
- Registro del incidente, fecha de detección, hallazgos, decisiones humanas y acciones correctivas en `reports/lab13/INCIDENT_POSTMORTEM.md`, **cuando se ejecute la simulación correspondiente**.

No se incluirán nombres, documentos, correos ni teléfonos reales en evidencias públicas. La mera existencia de una plantilla o archivo no constituye prueba de un control ejecutado.

## 4. Internal and affected-party communication

El **ML Engineer** notificará el hallazgo al **Institutional Owner** y al **Product Owner**; consultará a **Data Scientist** sobre las diferencias entre grupos y a **Legal** cuando exista riesgo normativo o de privacidad. El mensaje debe indicar: señal detectada, métricas y umbrales, alcance, incertidumbre, estado `REVIEW`, medidas de contención, responsable y siguiente revisión.

Los **Affected representatives** recibirán información pertinente y comprensible cuando corresponda, respetando confidencialidad y minimización de datos. En este proyecto **no hay personas afectadas reales a quienes se haya notificado**: cualquier comunicación del LAB13 será parte de una simulación documentada. Las solicitudes de revisión o apelación se trasladarán a un responsable humano y quedarán registradas.

## 5. Rollback or safe baseline

Ante un incumplimiento o incidente crítico, se detendrá cualquier intento de despliegue y se conservará la versión y configuración que produjeron la señal. Como alternativa segura se utilizará **revisión manual de casos sintéticos con criterios explícitos**, sin decisión predictiva automática, conforme a la alternativa no predictiva planteada en LAB11.

Si existiera una versión previamente autorizada en un entorno futuro, la reversión a dicha versión requeriría aprobación y verificación documentadas. **En este laboratorio no existe evidencia de un despliegue real ni de una reversión ejecutada**; por tanto, el *rollback* se documenta como procedimiento previsto, no como acción realizada.

El retorno a una eventual evaluación de despliegue exige repetir pruebas de desempeño y equidad, justificar cualquier ajuste de umbrales y registrar la autorización humana. No se debe subir el límite `max_fpr_gap` solo para conseguir un resultado `GO`.

## 6. Postmortem, owner and due date

**Responsable operativo:** ML Engineer. **Responsable final:** Institutional Owner. **Apoyo técnico:** Data Scientist; consulta a Legal según la naturaleza del incidente.

El informe posterior al incidente debe incluir: fecha y señal inicial, clasificación, impacto potencial, evidencias conservadas, causa o hipótesis investigada, decisiones humanas, medidas de contención, acciones correctivas, responsables individuales por rol y verificación del cierre.

**Plazos propuestos para el ejercicio (no establecidos como SLA institucional):** registrar la alerta y la contención el mismo día de su detección; preparar el análisis inicial en un plazo máximo de **2 días hábiles**; y documentar las acciones correctivas con sus responsables y fechas acordadas en un máximo de **5 días hábiles**. La fecha concreta se fijará al realizar `scripts/simulate_incident.py` y completar `reports/lab13/INCIDENT_POSTMORTEM.md`.

**Estado actual:** pendiente de simulación y de aprobación humana del cierre. El resultado `REVIEW` observado en LAB13 **no equivale a un incidente operativo confirmado ni a un permiso de despliegue**.

---


