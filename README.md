## Tablero de Priorización

### Fila de instrucciones

| Causa | Fase DT origen (E/D/I) | Insight de empatía | Supuesto central | Pregunta analítica | Variables (nombres exactos) | Tipo (Outcome / Explic / Control / Segmento) | Cálculo / Transformación | Métrica (nombre + fórmula) | Periodo / Segmento | Patrón esperado (si cierta) | Condición refutación | Valor esperado para usuario/ciudadano | Riesgo si falsa | Acción si confirma | Acción si refuta | Experimento analítico mínimo (query + visual 1 línea) | Estado (V/A/R) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ¿Qué crees que está pasando "detrás" del síntoma? Debe sonar a mecanismo, no a queja. | ¿De qué momento de Design Thinking salió? | Una observación/cita corta que te inspiró la causa. | ¿Qué crees que une la causa con el resultado? Es la "apuesta". | Forma neutra y medible de la causa. Debe admitir "Sí/No" o "Mayor/Menor". | Columnas que usarás tal cual aparecen en la base (no inventes nombres). |  | Operaciones necesarias antes de la métrica final. | Nombre corto + cómo se calcula. | Rango temporal y cortes. | La condición numérica que esperas ver si tu causa es real. | El rango / comparación que tiraría tu hipótesis. Debe ser diferente (no repetir con "no"). | Beneficio concreto si confirmas la hipótesis (en lenguaje humano). Clavo: debería sonar a mejora perceptible, no a KPI interno. | Qué perderías si sigues insistiendo en esta causa equivocada. Esto ayuda a priorizar falsar rápido. | Movimiento específico inmediato (no "optimizar estrategia"). Debe ser ejecutable en la organización. | Ruta alternativa (no repetir la de confirmar). | Qué harás para probarla en pequeño. | Semáforo: 1. V (Verde) = todos los campos listos (se puede correr hoy). 2. A (Amarillo) = falta 1 cosa (umbral, nombre exacto o acción). 3. R (Rojo) = falta variable clave o patrón; no avanzar. |

### Fila de datos

| Causa | Fase DT origen (E/D/I) | Insight de empatía | Supuesto central | Pregunta analítica | Variables (nombres exactos) | Tipo (Outcome / Explic / Control / Segmento) | Cálculo / Transformación | Métrica (nombre + fórmula) | Periodo / Segmento | Patrón esperado (si cierta) | Condición refutación | Valor esperado para usuario/ciudadano | Riesgo si falsa | Acción si confirma | Acción si refuta | Experimento analítico mínimo (query + visual 1 línea) | Estado (V/A/R) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| La normalización del agotamiento académico incrementa la percepción del sacrificio personal como requisito de éxito | Empático | Los estudiantes de 9.º semestre de la UMNG que cursan opción de grado asocian el agotamiento constante con compromiso académico y normalizan el sacrificio del descanso y el bienestar para graduarse exitosamente. | Si se implementa un programa institucional de bienestar y gestión del tiempo, entonces el porcentaje de estudiantes con burnout_score > 27 disminuirá al menos un 20 % frente a la medición inicial. | ¿Cuál es el impacto del programa institucional de bienestar y gestión del tiempo sobre el Índice de Agotamiento Académico Percibido (IAAP) en comparación con estudiantes que no participan en la intervención? | id_estudiante, fecha_respuesta, programa, semestre, modalidad_estudio, trabaja, horas_trabajo_semana, horas_sueno_promedio, pausas_activas_semana, horas_recreacion_semana, frecuencia_autocuidado, horas_ocio_semana, horas_no_academicas, nivel_estres, nivel_agotamiento, normaliza_agotamiento, burnout_score, agotamiento_alto_flag, grupo_intervencion, ciudad, universidad_tipo | Outcome = burnout_score, agotamiento_alto_flag. Explicativas = pausas_activas_semana, horas_recreacion_semana, frecuencia_autocuidado. Control = horas_trabajo_semana, modalidad_estudio, programa, ciudad, universidad_tipo. Segmento = grupo_intervencion. | Índice de Agotamiento Académico Percibido (IAAP) | **Índice de Agotamiento Académico Percibido (IAAP)** | Mensual o al finalizar cada periodo académico. | Reducción ≥ 20% de estudiantes presentan hábitos compatibles con el equilibrio académico-personal después de la intervención. | **Reducción < 10% de estudiantes presentan hábitos compatibles con el equilibrio académico-personal después de implementar las estrategias de bienestar, descanso y equilibrio personal.** | Mayor equilibrio académico-personal, más descanso y autocuidado, y menor percepción del agotamiento como requisito de éxito. | Inversión en estrategias sin efecto real y falsa sensación de mejora por no respuesta de estudiantes más agotados. | Escalar el programa a otros programas académicos o universidades y mantener monitoreo semestral del IAAP. | RRediseñar la intervención y profundizar en factores alternativos (carga académica, presión económica, cultura del rendimiento, apoyo institucional). |  |  |

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

# MAD UMNG — Reporte de Auditoría, Diccionario y Transformaciones

## 1. Reporte de auditoría (trazabilidad)

| Validación | Resultado |
|---|---|
| Filas iniciales | 384 |
| Filas eliminadas por nulos en variable clave (`burnout_score`) | 0 |
| Filas eliminadas por duplicados técnicos exactos | 0 |
| Filas eliminadas por duplicado de `id_estudiante` (una fila por usuario) | 0 |
| **Filas finales del MAD** | **384** |
| Estudiantes con `semestre = 9` (numerador/denominador solicitados) | 384 de 384 (100%) |
| Revisión de 5 usuarios de muestra | OK — ver sección 5 |
| Estudiantes con "opción de grado" | No aplica — ver Advertencias |

**Nota crítica:** la base ya venía sin nulos ni duplicados en las columnas evaluadas, por lo que no hubo pérdida de filas en ningún filtro. El archivo original se llama "...384_registros_limpios.xlsx", lo cual es consistente con este resultado.

## 2. Tabla resumen de la métrica

Indicador: `Estudiantes con burnout_score > 27 / Total estudiantes evaluados`

| Segmento | Total evaluados | Con burnout_score > 27 | Proporción |
|---|---|---|---|
| Semestre 9 (numerador y denominador solicitados) | 384 | 372 | **96.9%** |
| Total MAD (todos los registros) | 384 | 372 | 96.9% |

> Como el 100% de la base corresponde a semestre 9, el resultado "semestre 9" y el resultado "total MAD" son idénticos. Ver advertencia de calidad #1.

## 3. Diccionario de datos

| Columna | Tipo final | Rol | Descripción |
|---|---|---|---|
| `id_estudiante` | texto (string) | Identificador único | Código de estudiante, sin espacios al inicio/fin |
| `fecha_respuesta` | fecha (YYYY-MM-DD) | Control temporal | Fecha de diligenciamiento de la encuesta |
| `programa` | texto categórico (minúsculas, sin tildes) | Segmento | Programa académico del estudiante |
| `semestre` | numérico (entero) | Segmento/Control | Semestre que cursa el estudiante |
| `modalidad_estudio` | texto categórico (minúsculas, sin tildes) | Segmento | Presencial / Virtual |
| `trabaja` | texto categórico (minúsculas, sin tildes) | Explicativa | Sí / No trabaja actualmente |
| `horas_trabajo_semana` | numérico (entero) | Explicativa | Horas de trabajo remunerado por semana |
| `horas_sueno_promedio` | numérico (decimal) | Explicativa | Horas promedio de sueño diario |
| `pausas_activas_semana` | numérico (entero) | Explicativa | Número de pausas activas realizadas por semana |
| `horas_recreacion_semana` | numérico (decimal) | Explicativa | Horas semanales dedicadas a recreación |
| `frecuencia_autocuidado` | numérico (entero) | Explicativa | Frecuencia semanal de prácticas de autocuidado |
| `burnout_score` | numérico (entero) | Outcome / llave de cálculo | Puntaje de burnout (rango observado: 18–74). Se conservó únicamente para calcular el indicador solicitado; no formaba parte del "diseño experimental" enumerado, pero es indispensable matemáticamente para el numerador/denominador |

## 4. Reporte de transformaciones aplicadas

1. **Perímetro:** se descartaron todas las columnas fuera de la lista de diseño experimental, más `Columna1`…`Columna16363` (columnas vacías/artefacto de formato de Excel) y otras variables del set original (`horas_ocio_semana`, `nivel_estres`, `nivel_agotamiento`, `normaliza_agotamiento`, `agotamiento_alto_flag`, `grupo_intervencion`, `Sede`, `universidad_tipo`).
2. **Nulos críticos:** se aplicó `dropna` sobre `burnout_score` (la única columna con nulos potenciales relevante al indicador). Resultado: 0 filas eliminadas.
3. **Estandarización de texto:** en `programa`, `modalidad_estudio` y `trabaja` se aplicó minúsculas, eliminación de tildes/diacríticos y recorte de espacios. `id_estudiante` se recortó de espacios pero se mantuvo su capitalización original por ser un identificador.
4. **Formatos operables:** columnas numéricas convertidas con `pd.to_numeric`; `fecha_respuesta` convertida a `datetime` y formateada como `YYYY-MM-DD`.
5. **Duplicados técnicos:** se aplicó `drop_duplicates()` sobre el set completo de columnas → 0 eliminados.
6. **Una fila por usuario:** se verificó unicidad de `id_estudiante` → 0 duplicados encontrados.

No se imputó, infirió ni inventó ningún dato. Todo valor faltante habría resultado en eliminación de fila, nunca en relleno artificial.

## 5. Revisión de 5 usuarios de muestra

| id_estudiante | fecha_respuesta | programa | modalidad | trabaja | burnout_score |
|---|---|---|---|---|---|
| EST269 | 2026-06-29 | ingenieria industrial | virtual | si | 62 |
| EST251 | 2026-07-02 | ingenieria mecatronica | presencial | no | 53 |
| EST356 | 2026-06-29 | derecho | virtual | si | 52 |
| EST333 | 2026-06-30 | ingenieria de sistemas | virtual | no | 26 |
| EST057 | 2026-07-03 | derecho | presencial | si | 47 |

Valores consistentes con el resto del dataset; sin anomalías de formato.

## 6. Supuestos aplicados

- Se asumió que `burnout_score` es la variable correcta para evaluar el numerador del indicador (`burnout_score > 27`), dado que no existe una columna llamada literalmente "burnout_score categórico" adicional.
- Se asumió que "Estudiantes últimos semestre" se refiere a `semestre = 9`, ya que es el valor máximo y único presente en la base.
- El identificador de estudiante (`id_estudiante`) se tomó como clave de unicidad para garantizar "una fila por usuario".
- No se imputaron datos faltantes bajo ninguna circunstancia, conforme a instrucción explícita.

## 7. Advertencias de calidad

1. **Sin variabilidad en `semestre`:** el 100% de los 384 registros corresponde a semestre 9. Esto hace que el indicador "semestre 9" sea matemáticamente idéntico al indicador global, y que `semestre` no aporte poder de segmentación en este archivo. Verificar si se recibió solo un subconjunto de la base original.
2. **"Opción de grado":** el punto 8 del brief solicita el resultado de "estudiantes que presentan alternamente opción de grado", pero **no existe ninguna columna relacionada con opción de grado** en el archivo fuente ni en la lista de perímetro definida. No fue posible calcular este resultado — se requiere la columna correspondiente o aclaración de a qué variable se refiere.
3. **Columnas vacías masivas:** el archivo original traía miles de columnas (`Columna1` a `Columna16363`) completamente vacías, probablemente por un rango de formato de Excel extendido accidentalmente. Fueron excluidas del MAD por estar fuera del perímetro y no aportar información.
4. **Sin pérdida de datos:** al no encontrarse nulos ni duplicados, no fue posible validar el comportamiento del pipeline de limpieza ante casos reales de estos tipos de error; se recomienda una prueba con datos sintéticos si se requiere certificar la robustez del proceso.
