# Model Card — Territorial Review DSS (Synthetic)

**Unidad:** 04 — Ética y roles en Ciencia de Datos | **Laboratorio:** LAB13  
**Estado de la evaluación:** `REVIEW` — uso académico permitido; despliegue real no autorizado.

## Intended use

Sistema de apoyo a decisiones (DSS) **exclusivamente académico** para priorizar casos territoriales **sintéticos** que requieren revisión humana. Utiliza un modelo de **regresión logística** para producir predicciones binarias sobre casos simulados. Su salida orienta el análisis, pero **no sustituye la decisión de una persona responsable**.

Usuarios previstos: estudiantes, docentes y analistas que realizan ejercicios de evaluación de modelos, equidad y gobernanza. El propósito es estudiar riesgos, documentar evidencia y ensayar controles de despliegue, no tomar decisiones sobre la población de República Dominicana.

## Out-of-scope use

Quedan prohibidos los usos siguientes, establecidos en `configs/governance.yml`:

- Ejecutar intervenciones automáticas a partir de una predicción.
- Decidir elegibilidad para beneficios, servicios o derechos.
- Declarar emergencias o activar medidas reales.
- Perfilar personas, comunidades o territorios reales, o utilizar los resultados como estadísticas oficiales.
- Incorporar identificadores personales directos (`name`, `document_number`, `email`, `phone`).

## Data and split

- **Fuente:** `data/territorial_risk_synthetic.csv`; datos generados para fines educativos, sin personas ni territorios reales.
- **Tamaño:** 640 registros y 9 columnas.
- **Variable objetivo:** `target` binaria; distribución total aproximada: 55.2 % clase 1 y 44.8 % clase 0.
- **Separación temporal:** 480 registros de entrenamiento (75 %) y 160 de prueba (25 %); entrenamiento hasta el **24/09/2026** y prueba desde el **25/09/2026**.
- **Variables de entrada:** `zone`, `age_band`, `previous_incidents`, `infrastructure_score` y `exposure`.
- **Campos excluidos de las entradas del modelo:** `case_id`, `received_date`, `protected_group` y `target`. El atributo `protected_group` se conserva para auditar diferencias, no para entrenar el clasificador. **`age_band` sí se utiliza como variable de entrada**, aunque también se reconoce como atributo sensible para la auditoría; su uso amerita revisión ética.
- **Grupos en prueba:** G1 = 101 registros; G2 = 59 registros. Las intersecciones presentan tamaños aún menores.

## Global and subgroup evaluation

**Modelo evaluado:** regresión logística del pipeline LAB13. Las cifras corresponden a la ejecución documentada en `reports/lab13/deployment_decision.json` y a la auditoría previa del LAB12.

| Métrica | Global | G1 (n=101) | G2 (n=59) |
|---|---:|---:|---:|
| Accuracy | 71.88 % | 70.30 % | 74.58 % |
| Recall | 71.74 % | 65.31 % | 79.07 % |
| Precision | 77.65 % | 71.11 % | 85.00 % |
| Selection rate | 53.13 % | 44.55 % | 67.80 % |
| False positive rate (FPR) | 27.94 % | 25.00 % | 37.50 % |

**Brechas absolutas entre G1 y G2:** recall = **13.76 puntos porcentuales**; FPR = **12.50 puntos porcentuales**. La brecha de selección es de **23.24 puntos porcentuales**.

**Puertas de gobernanza (LAB13):** se aprobaron los controles de recall mínimo (0.68), brecha máxima de recall (0.16), latencia máxima (500 ms) y tamaño máximo (1024 KB). **No se aprobó la brecha FPR**: el valor observado (**0.125**) supera el límite configurado (**0.05**).

**Justificación del cambio:** el proyecto original proporcionaba `max_fpr_gap: 0.16`. Se cambió a **0.05** como criterio académico más estricto y para mantener consistencia con `student_inputs/lab12_decision.yml`. **No es un umbral universal ni una exigencia original del manual.** La decisión automática fue **`REVIEW`**, con revisión y decisión final humanas pendientes.

## Green AI evidence

En la ejecución registrada en `reports/lab13/deployment_decision.json` se midieron:

| Indicador | Resultado |
|---|---:|
| Tiempo de entrenamiento | 0.0792 segundos |
| Tamaño del modelo serializado | 3.56 KB |
| Latencia promedio de predicción por registro | 0.0343 ms |

Estas son **mediciones puntuales de recursos**, no una evaluación integral de sostenibilidad. No se midieron consumo energético, emisiones de CO₂, uso acumulado de infraestructura ni variación entre equipos; tampoco se ejecutó un estudio repetido de rendimiento. Por ello, no puede afirmarse una reducción ambiental demostrada.

## Limitations, human oversight and appeal

- Los datos son **sintéticos** y no permiten generalizar las métricas a poblaciones reales.
- Los tamaños por grupo e intersección son limitados; las diferencias observadas pueden ser inestables. **Una brecha estadística no prueba por sí sola discriminación causal.**
- Excluir `protected_group` del entrenamiento no elimina posibles variables indirectas relacionadas con el grupo; además, `age_band` participa explícitamente como entrada del modelo.
- El incumplimiento de la puerta FPR requiere investigar calidad de datos, errores por subgrupo, variables relacionadas y posibles mitigaciones antes de reconsiderar un despliegue.
- La salida es una **recomendación**: el **Operator** debe revisarla y dejar constancia de la decisión; el **Product Owner** responde por la autorización de despliegue y el **Institutional Owner** por la gobernanza y respuesta a incidentes, conforme a `student_inputs/raci.csv`.
- Toda persona potencialmente afectada por una aplicación futura debe contar con un mecanismo accesible para solicitar **revisión humana, corrección o impugnación**. En este laboratorio dicho mecanismo está **documentado como requisito**, no probado con usuarios reales.
- Se debe detener cualquier uso fuera del alcance académico y preservar registros de evaluación, versiones y decisiones para auditoría.

## Owner and review date

- **Propietario funcional / responsable de la autorización de despliegue:** `Product Owner` (rol definido en RACI).
- **Responsable institucional de gobernanza e incidentes:** `Institutional Owner` (rol definido en RACI).
- **Fecha de elaboración de esta ficha:** 01/11/2026.
- **Estado de aprobación:** pendiente de revisión y decisión humana formal; `REVIEW` **no autoriza despliegue real**.
- **Próxima revisión requerida:** antes de cualquier despliegue y siempre que cambien el conjunto de datos, el modelo, los umbrales o los resultados de equidad. La fecha de aprobación formal deberá registrarla el responsable correspondiente.
