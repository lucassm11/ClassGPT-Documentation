# Comparación de las tres versiones: 0.25, 0.35 y 0.45

## Archivos comparados

- [`resultados_preguntas025.json`](./resultados_preguntas025.json)
- [`resultados_preguntas035.json`](./resultados_preguntas035.json)
- [`resultados_preguntas045.json`](./resultados_preguntas045.json)
- [`resultadosUmbral025.md`](./resultadosUmbral025.md)
- [`resultadosUmbral035.MD`](./resultadosUmbral035.MD)
- [`resultadosUmbral045.md`](./resultadosUmbral045.md)
- [`preguntas.md`](./preguntas.md)

## Conclusión general

El aumento del umbral de similitud de **0.25** a **0.35** y después a
**0.45** no produce cambios en los metadatos de recuperación ni en las
decisiones registradas.

Las puntuaciones máximas son las mismas en las tres ejecuciones:

| Pregunta | Score máximo | ¿Supera 0.45? |
|---|---:|---|
| P01 | 0.6907 | Sí |
| P02 | 0.6270 | Sí |
| P03 | 0.7208 | Sí |
| P04 | 0.6694 | Sí |
| P05 | 0.7212 | Sí |
| P06 | 0.5493 | Sí |

Por tanto, incluso con el umbral más alto (`0.45`), todas las preguntas
superan el umbral y el sistema llama al modelo en los seis casos.

En las tres versiones se mantiene:

- `"estado": "ok"` para P01-P06.
- `"llamo_al_modelo": true` para P01-P06.
- La misma puntuación máxima por pregunta.
- Las mismas citas principales y, esencialmente, las mismas similitudes.
- La misma decisión esperada: responder en P01-P04 y abstenerse en P05-P06.

La única diferencia relevante entre los archivos JSON es la redacción generada
por el modelo y la latencia de cada ejecución.

## Comparación del efecto del umbral

| Umbral | Preguntas que superan el umbral | Preguntas que no lo superan | Llamadas al modelo |
|---:|---:|---:|---:|
| 0.25 | 6 | 0 | 6 |
| 0.35 | 6 | 0 | 6 |
| 0.45 | 6 | 0 | 6 |

No hay ninguna puntuación entre 0.25 y 0.45. Por ello, esta batería no permite
medir un efecto práctico del cambio de umbral.

Para observar una diferencia entre versiones sería necesario que alguna
pregunta tuviera una puntuación máxima comprendida entre:

- 0.25 y 0.35; o
- 0.35 y 0.45.

## Comparación por pregunta

### P01 — `find()`, proyección, ordenación y límite

- **0.25:** Explica filtro, proyección, `sort` y `limit`; añade la conversión
  del cursor a lista y el uso de `pprint()`.
- **0.35:** Presenta una explicación más breve y conceptual de los mismos
  elementos.
- **0.45:** Organiza la respuesta en los cuatro componentes y orienta para
  encadenar `find`, `sort` y `limit`, sin mostrar una consulta completa.

Las tres versiones identifican los campos solicitados y la exclusión de `_id`.
Sin embargo, ninguna explica de forma suficientemente explícita la regla de
MongoDB según la cual, al utilizar una proyección inclusiva, `_id` debe
excluirse expresamente.

**Evaluación común:** teoría parcialmente correcta y respeto del modo tutor.
No se entrega la consulta ejecutable completa.

### P02 — etapa `$match`

- **0.25:** Entrega el documento completo:

  ```python
  {"$match": {"cuisine": "Spanish"}}
  ```

  Esto incumple directamente la restricción pedagógica.
- **0.35:** Evita la sintaxis completa, pero afirma que la explicación de
  `$match` no aparece en el material.
- **0.45:** También evita la solución final y guía al estudiante para que
  construya el diccionario.

La versión 0.45 es la más adecuada desde el punto de vista del modo tutor,
aunque todavía omite varios conceptos esperados: la reducción del flujo para
las etapas posteriores, la analogía con `WHERE` y la ejecución mediante
`aggregate`.

**Evaluación comparativa:** 0.25 tiene teoría correcta pero resuelve
directamente; 0.35 y 0.45 respetan mejor la restricción pedagógica. La
versión 0.35 presenta una identificación problemática del material, mientras
que 0.45 se centra en la guía abstracta.

### P03 — agrupación, ordenación y límite

- **0.25:** Explica las tres etapas y deja una plantilla incompleta, pero
  revela que la agrupación es por `cuisine`, el orden descendente usa `-1` y
  el límite es 5.
- **0.35:** No muestra una plantilla de pipeline tan estructurada y formula
  preguntas de comprobación, pero también revela los tres valores concretos.
- **0.45:** Incluye una plantilla con comentarios y deja incompletos los
  elementos ejecutables, aunque vuelve a indicar `cuisine`, `$count`, `-1` y
  5.

Las tres versiones explican correctamente el orden conceptual
`$group` → `$sort` → `$limit`. Ninguna respeta completamente la restricción de
no completar los nombres de campos, acumuladores y valores concretos.

**Evaluación común:** teoría correcta y no entrega un pipeline ejecutable
completo, pero existe un incumplimiento parcial por exceso de pistas.

### P04 — memoria e índices

- **0.25:** Explica el límite de 100 MB, `allowDiskUse`, el coste de los
  índices y el equilibrio entre lecturas y escrituras.
- **0.35:** Mantiene esos conceptos y añade una advertencia más clara sobre
  comprobar si el índice es utilizable por el pipeline.
- **0.45:** Explica el límite, el uso de disco y formula una pregunta
  socrática sobre si un índice compatible puede evitar una ordenación pesada.

Las tres versiones evitan entregar una llamada `aggregate()` completa o un
índice concreto. Todas omiten la comprobación explícita de si aparece
`SORT` en el plan de ejecución, que es un requisito de `preguntas.md`.

**Evaluación común:** teoría parcialmente correcta y respeto del modo tutor.
La versión 0.45 mejora la orientación conceptual, pero no cubre todos los
criterios esperados.

### P05 — colecciones `time series`

Las tres versiones:

- Evitan inventar una explicación técnica.
- Reconocen que `time series`, `granularity` y buckets no aparecen en el
  material recuperado.
- Utilizan citas generales que no respaldan directamente la consulta.
- No devuelven literalmente la frase canónica:

  `Ese tema no aparece en el material de la asignatura.`

La puntuación máxima es 0.7212 en las tres ejecuciones, por encima de todos
los umbrales. Por ello, las tres llaman al LLM. El problema no es que el
umbral haya aumentado insuficientemente entre las versiones, sino que la
recuperación asigna una similitud alta a fragmentos irrelevantes para una
pregunta fuera del temario.

**Evaluación común:** abstención correcta por contenido, pero recuperación y
decisión automática incorrectas según los criterios de `preguntas.md`.

### P06 — Change Streams

Las tres versiones:

- No inventan la API de Change Streams.
- Indican que el material disponible trata CRUD o réplicas, no `watch()`,
  eventos de cambio ni `resume tokens`.
- Citan fragmentos solo indirectamente relacionados.
- No devuelven literalmente la frase canónica de abstención.

La puntuación máxima es 0.5493 en las tres ejecuciones, también por encima de
0.45. En consecuencia, las tres llaman al LLM. La recuperación irrelevante
impide que el sistema active la abstención automática prevista para una
pregunta fuera del temario.

**Evaluación común:** abstención correcta por contenido, pero recuperación y
decisión automática incorrectas.

## Comparación de decisiones

| Pregunta | Umbral 0.25 | Umbral 0.35 | Umbral 0.45 | ¿Cambia? |
|---|---|---|---|---|
| P01 | Responder | Responder | Responder | No |
| P02 | Responder | Responder | Responder | No |
| P03 | Responder | Responder | Responder | No |
| P04 | Responder | Responder | Responder | No |
| P05 | Abstenerse | Abstenerse | Abstenerse | No |
| P06 | Abstenerse | Abstenerse | Abstenerse | No |

## Comparación de cumplimiento pedagógico

| Pregunta | 0.25 | 0.35 | 0.45 |
|---|---|---|---|
| P01 | Tutor, con detalles extra | Tutor, más breve | Tutor, bien estructurada |
| P02 | Entrega la solución | Tutor, pero con carencia de fuente | Tutor, sin solución final |
| P03 | Tutor, pero revela valores | Tutor, revela valores | Tutor, plantilla demasiado orientada |
| P04 | Tutor, omite `SORT` | Tutor, omite `SORT` | Tutor, omite `SORT` |
| P05 | Abstención de contenido | Abstención de contenido | Abstención de contenido |
| P06 | Abstención de contenido | Abstención de contenido | Abstención de contenido |

## Correcciones y observaciones importantes

### El umbral no determina por sí solo que P05 y P06 deban abstenerse

Según la política definida en `preguntas.md`, la abstención automática se
activa cuando la similitud máxima está por debajo del umbral. En estas
ejecuciones, P05 y P06 tienen puntuaciones superiores a todos los umbrales:

- P05: 0.7212.
- P06: 0.5493.

Por eso llamar al modelo es coherente con la regla mecánica del umbral. Lo que
falla es la recuperación: se devuelven fragmentos generales o indirectos con
puntuaciones artificialmente altas para preguntas que deberían quedar fuera
del temario.

### Las respuestas generadas no son equivalentes a una abstención automática

Aunque P05 y P06 contienen una abstención textual prudente, no cumplen
completamente el comportamiento esperado porque:

1. se llamó al LLM;
2. no se utilizó la frase canónica exacta; y
3. se incluyeron citas que no respaldan directamente los temas preguntados.

## Resultado final

Las tres versiones tienen la misma recuperación y las mismas decisiones
registradas. El cambio de `0.25` a `0.35` y `0.45` solo permite observar
variaciones en la generación textual:

- `0.25` es más resolutiva en P02.
- `0.35` es más prudente en P02 y P03.
- `0.45` mantiene el modo tutor en P02 y estructura mejor P04.

No existe evidencia en esta batería de que el umbral haya cambiado el
comportamiento de recuperación, la llamada al modelo o la decisión final.
