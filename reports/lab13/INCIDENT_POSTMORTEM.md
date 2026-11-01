# LAB13 — Incident Postmortem

## 1. Detección y clasificación

- **Fecha de detección:** 2026-11-01T21:50:57.666803+00:00
- **Tipo:** Simulación académica de incidente algorítmico.
- **Señal:** `recall gap exceeded`.
- **Descripción:** Se simula una situación en la que la diferencia de recall entre grupos protegidos supera el límite de equidad, lo que podría ocasionar una identificación desigual de casos positivos.

## 2. Contención

- **Estado:** REVIEW.
- **Medida propuesta:** Suspender cualquier autorización de despliegue y mantener la revisión humana como alternativa segura.
- **Responsable operativo:** ML Engineer.
- **Responsable final:** Institutional Owner.

## 3. Preservación de evidencias

Se establece la conservación de los reportes de desempeño y equidad, la configuración de gobernanza, la versión del modelo y los registros de decisiones.

Estas evidencias permitirían investigar las causas del incidente y verificar las acciones correctivas. Su preservación efectiva deberá comprobarse antes del cierre.

## 4. Comunicación

El ML Engineer deberá notificar el incidente al Institutional Owner y consultar al Data Scientist y al área Legal para evaluar sus posibles consecuencias.

Los representantes de las personas potencialmente afectadas deberán ser informados cuando corresponda. Toda comunicación deberá documentarse, manteniendo la confidencialidad de la información.

## 5. Acciones correctivas, responsables y fechas

| Acción correctiva | Responsable | Fecha límite propuesta |
|---|---|---|
| Investigar las causas de la brecha de recall | Data Scientist | 03/11/2026 |
| Revisar calidad de datos y variables utilizadas | Data Engineer | 04/11/2026 |
| Evaluar alternativas de mitigación y repetir la auditoría | Data Scientist | 05/11/2026 |
| Verificar los controles y condiciones de recuperación | ML Engineer | 06/11/2026 |
| Revisar evidencias y decidir sobre la autorización | Institutional Owner | 06/11/2026 |

Las fechas son propuestas para el ejercicio y no representan compromisos institucionales aprobados.

## 6. Reversión y análisis posterior

Ante un incumplimiento de los límites de equidad, se recomienda mantener el sistema en estado REVIEW y utilizar procedimientos de revisión manual hasta comprobar la efectividad de los controles.

La recuperación deberá condicionarse a una nueva evaluación de desempeño y equidad, la documentación de los resultados y la aprobación del responsable institucional.

## 7. Conclusión

La simulación permitió documentar un procedimiento de respuesta ante posibles incidentes algorítmicos, incluyendo detección, contención, conservación de evidencias, comunicación y acciones correctivas.

**Decisión: REVIEW.**

Se recomienda mantener la supervisión humana y no autorizar un despliegue real sin verificar previamente el cumplimiento de los controles de gobernanza.

Este documento corresponde a un ejercicio simulado y no constituye evidencia de un incidente real ni de la ejecución efectiva de las acciones correctivas.