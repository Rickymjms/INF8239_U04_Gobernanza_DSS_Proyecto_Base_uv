# LAB11 - Decision Record
## Evaluacion de impacto etico del DSS

### 1. Proposito del sistema

Evaluar academicamente un sistema de apoyo a decisiones
(DSS) que identifica patrones de riesgo territorial mediante
datos sinteticos. Su finalidad es apoyar la revision de casos
simulados, sin sustituir el criterio humano ni autorizar
intervenciones sobre personas o territorios reales.

### 2. Evidencias disponibles

Se completaron los siguientes registros:

- lab11_impact.yml: proposito, usuarios, limites y controles.
- stakeholders.csv: cuatro partes interesadas.
- risk_register.csv: ocho riesgos eticos documentados.
- risk_register_scored.csv: riesgos priorizados.
- rmf_summary.json: resumen de gobernanza de riesgos.

El analisis identifico los siguientes riesgos prioritarios:

| Riesgo | Prioridad | Responsable |
|---|---:|---|
| R02 - Calidad de datos | 9 | Data team |
| R01 - Sesgo territorial | 8 | Data team |
| R07 - Falta de apelacion | 8 | Institutional owner |

Ademas, las pruebas automatizadas finalizaron correctamente:

Comando:
uv run pytest -q tests/test_impact.py tests/test_privacy.py

Resultado: 3 passed in 0.68s.

### 3. Interpretacion de los riesgos prioritarios

R02 - Calidad de datos:
Los datos incompletos o inconsistentes pueden generar
recomendaciones incorrectas. Se requiere validar su
integridad y consistencia antes de utilizar los resultados.

R01 - Sesgo territorial:
El sistema podria producir diferencias injustificadas
entre grupos territoriales. Se requiere evaluar resultados
por subgrupos y documentar posibles desigualdades.

R07 - Falta de apelacion:
Las recomendaciones podrian afectar a personas sin ofrecer
mecanismos adecuados de revision. Se requiere garantizar
supervision humana y procedimientos para cuestionar
y corregir decisiones.

### 4. Incertidumbres y limitaciones

- El conjunto de datos es sintetico y no representa
  necesariamente condiciones de territorios reales.
- No se ha comprobado empiricamente la efectividad
  de todos los controles propuestos.
- Las pruebas automatizadas verifican condiciones
  tecnicas, pero no garantizan ausencia de sesgos.
- La evaluacion de equidad y robustez esta pendiente
  para las siguientes actividades del laboratorio.

### 5. Responsables y acciones requeridas

Data team:
Validar calidad de datos, evaluar sesgos y documentar
resultados y limitaciones del sistema.

Institutional owner:
Supervisar la aplicacion de controles, garantizar
mecanismos de apelacion y autorizar cualquier cambio
en el alcance del proyecto.

Operator:
Revisar las recomendaciones y documentar las decisiones
humanas antes de cualquier accion.

### 6. Decision final

DECISION: REVIEW

Se recomienda continuar exclusivamente con la evaluacion
academica controlada, manteniendo suspendido cualquier
uso operativo o aplicacion sobre personas y territorios
reales.

La autorizacion para avanzar hacia una aplicacion real
queda condicionada a:

1. Validar la calidad y consistencia de los datos.
2. Evaluar posibles sesgos entre grupos.
3. Implementar mecanismos de supervision y apelacion.
4. Documentar la efectividad de los controles.
5. Obtener autorizacion del responsable institucional.

### 7. Justificacion

La decision REVIEW se fundamenta en que los riesgos
R02, R01 y R07 presentan las mayores prioridades.

Aunque las tres pruebas automatizadas fueron superadas,
no existe evidencia suficiente para concluir que los
riesgos eticos identificados estan controlados.

Por tanto, se permite continuar con las actividades
academicas de evaluacion, pero no se recomienda
desplegar el sistema en un entorno real.
