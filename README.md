## Tablero de Priorización

### Fila de instrucciones

| Causa | Fase DT origen (E/D/I) | Insight de empatía | Supuesto central | Pregunta analítica | Variables (nombres exactos) | Tipo (Outcome / Explic / Control / Segmento) | Cálculo / Transformación | Métrica (nombre + fórmula) | Periodo / Segmento | Patrón esperado (si cierta) | Condición refutación | Valor esperado para usuario/ciudadano | Riesgo si falsa | Acción si confirma | Acción si refuta | Experimento analítico mínimo (query + visual 1 línea) | Estado (V/A/R) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ¿Qué crees que está pasando "detrás" del síntoma? Debe sonar a mecanismo, no a queja. | ¿De qué momento de Design Thinking salió? | Una observación/cita corta que te inspiró la causa. | ¿Qué crees que une la causa con el resultado? Es la "apuesta". | Forma neutra y medible de la causa. Debe admitir "Sí/No" o "Mayor/Menor". | Columnas que usarás tal cual aparecen en la base (no inventes nombres). |  | Operaciones necesarias antes de la métrica final. | Nombre corto + cómo se calcula. | Rango temporal y cortes. | La condición numérica que esperas ver si tu causa es real. | El rango / comparación que tiraría tu hipótesis. Debe ser diferente (no repetir con "no"). | Beneficio concreto si confirmas la hipótesis (en lenguaje humano). Clavo: debería sonar a mejora perceptible, no a KPI interno. | Qué perderías si sigues insistiendo en esta causa equivocada. Esto ayuda a priorizar falsar rápido. | Movimiento específico inmediato (no "optimizar estrategia"). Debe ser ejecutable en la organización. | Ruta alternativa (no repetir la de confirmar). | Qué harás para probarla en pequeño. | Semáforo: 1. V (Verde) = todos los campos listos (se puede correr hoy). 2. A (Amarillo) = falta 1 cosa (umbral, nombre exacto o acción). 3. R (Rojo) = falta variable clave o patrón; no avanzar. |

### Fila de datos

| Causa | Fase DT origen (E/D/I) | Insight de empatía | Supuesto central | Pregunta analítica | Variables (nombres exactos) | Tipo (Outcome / Explic / Control / Segmento) | Cálculo / Transformación | Métrica (nombre + fórmula) | Periodo / Segmento | Patrón esperado (si cierta) | Condición refutación | Valor esperado para usuario/ciudadano | Riesgo si falsa | Acción si confirma | Acción si refuta | Experimento analítico mínimo (query + visual 1 línea) | Estado (V/A/R) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| La normalización del agotamiento académico incrementa la percepción del sacrificio personal como requisito de éxito | Empático | Se identificó que los estudiantes universitarios de últimos semestres asocian el agotamiento constante con compromiso académico, normalizando la idea de que sacrificar descanso y bienestar es necesario para graduarse exitosamente. | Si se implementan estrategias que promuevan hábitos de bienestar, descanso y equilibrio personal en estudiantes universitarios, entonces aumentará su percepción de equilibrio entre la vida académica y personal, reduciendo la creencia de que el agotamiento es un requisito para alcanzar el éxito académico. | ¿Cuál es el impacto del programa institucional de bienestar y gestión del tiempo sobre el Índice de Agotamiento Académico Percibido (IAAP) en comparación con estudiantes que no participan en la intervención? | `pausas_activas_semana`, `horas_recreacion_semana`, `frecuencia_autocuidado`, `horas_sueno_promedio`, `horas_ocio_semana`, `horas_no_academicas`, `burnout_score`, `agotamiento_alto_flag`, `semestre`, `materias_inscritas`, `modalidad_estudio`, `horas_trabajo_semana`, `grupo_intervencion` | `pausas_activas_semana` = Explicativa. `horas_recreacion_semana` = Explicativa. `frecuencia_autocuidado` = Explicativa. `burnout_score` = Outcome. `agotamiento_alto_flag` = Outcome. `semestre` = Control. `materias_inscritas` = Control. `modalidad_estudio` = Control. `horas_trabajo_semana` = Control. `grupo_intervencion` = Segmento. | Índice de Agotamiento Académico Percibido (IAAP) | **Índice de Agotamiento Académico Percibido (IAAP)** | Mensual o al finalizar cada periodo académico. | Reducción ≥ 20% de estudiantes presentan hábitos compatibles con el equilibrio académico-personal después de la intervención. | **Reducción < 10% de estudiantes presentan hábitos compatibles con el equilibrio académico-personal después de implementar las estrategias de bienestar, descanso y equilibrio personal.** | Mejora del equilibrio entre la vida académica y personal, reducción del agotamiento percibido, incremento de espacios de descanso, ocio y autocuidado, fortalecimiento del bienestar emocional y una experiencia universitaria más saludable y sostenible. | La universidad o los responsables del proyecto podrían invertir recursos en estrategias de bienestar que no generen cambios reales en la percepción del estudiante. El agotamiento continuaría siendo normalizado como requisito para el éxito académico, manteniendo niveles elevados de estrés, desmotivación y afectación del bienestar estudiantil. | Implementar de manera permanente programas de bienestar estudiantil enfocados en autocuidado, gestión equilibrada de responsabilidades, pausas activas, promoción del descanso y acompañamiento psicoeducativo. Escalar la intervención a otros programas académicos y monitorear periódicamente el Índice de Equilibrio Académico-Personal (IEAP). | Rediseñar la hipótesis y profundizar en la investigación del usuario para identificar otros factores que puedan estar influyendo en la normalización del agotamiento, tales como presión académica, cultura del rendimiento, exigencias familiares, factores económicos o dinámicas institucionales. Diseñar nuevas intervenciones y realizar pruebas piloto adicionales. |  |  |

---

**Bloque de código 1** — Cálculo / Transformación

```
Filtrar el segmento de análisis, conservando únicamente registros de estudiantes universitarios
colombianos que cursen los últimos semestres académicos. Se excluirán registros de estudiantes
de semestres inferiores o con información demográfica incompleta.

Eliminar registros duplicados, utilizando el identificador único del estudiante para garantizar
que cada participante sea contabilizado una sola vez dentro del análisis.

Validar y estandarizar los tipos de datos, convirtiendo todas las variables numéricas a formato
entero o decimal según corresponda:
  - Número de pausas activas por semana → entero.
  - Frecuencia de actividades de autocuidado → entero.
  - Horas de sueño promedio por noche → decimal.
  - Horas destinadas a actividades recreativas → decimal.
  - Horas semanales de ocio → decimal.
  - Tiempo dedicado a actividades no académicas → decimal.

Tratamiento de valores nulos o vacíos:
  - Si una variable presenta valores nulos en menos del 10% de los registros, se reemplazará
    por la mediana de la muestra para evitar distorsiones.
  - Si un registro presenta más del 50% de sus variables clave vacías, será excluido del análisis.
  - Todos los reemplazos deberán quedar registrados en un log de auditoría.

Detección y exclusión de datos atípicos o inconsistentes, considerando rangos máximos razonables:
  - Horas de sueño promedio por noche: entre 0 y 24 horas.
  - Horas semanales de ocio: entre 0 y 168 horas.
  - Horas destinadas a actividades recreativas: entre 0 y 168 horas.
  - Número de pausas activas por semana: valores positivos.
  - Frecuencia de actividades de autocuidado: valores positivos.
  - Tiempo dedicado a actividades no académicas: entre 0 y 168 horas semanales.
```
