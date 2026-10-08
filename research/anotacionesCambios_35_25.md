# Anotaciones de cambios entre los resultados 0.35 y 0.25

## Archivos comparados

- [`resultados_preguntas025.json`](./resultados_preguntas025.json)
- [`resultados_preguntas035.json`](./resultados_preguntas035.json)
- [`preguntas.md`](./preguntas.md)

## Conclusión general

El cambio del umbral de similitud de **0.25** a **0.35** no produce un cambio
observable en la decisión del sistema en esta ejecución.

Las puntuaciones máximas de las seis preguntas son superiores a 0.35:

| Pregunta | Score máximo |
|---|---:|
| P01 | 0.6907 |
| P02 | 0.6270 |
| P03 | 0.7208 |
| P04 | 0.6694 |
| P05 | 0.7212 |
| P06 | 0.5493 |

En consecuencia:

- P01-P04 siguen siendo preguntas respondidas.
- P05-P06 siguen recibiendo una respuesta de abstención de contenido.
- En las seis preguntas se mantiene `"estado": "ok"`.
- En las seis preguntas se mantiene `"llamo_al_modelo": true`.
- Las citas y sus puntuaciones de similitud son las mismas en ambos JSON.

Por tanto, el aumento del umbral no fue suficiente para excluir ningún
resultado recuperado. En particular, P05 y P06 no se abstuvieron por la lógica
de umbral, porque sus puntuaciones máximas son 0.7212 y 0.5493,
respectivamente.

## Diferencias en el texto generado

Aunque los metadatos de recuperación y decisión son prácticamente iguales, el
texto generado por el modelo cambia entre ambas ejecuciones.

### P01 — `find()`, proyección, ordenación y límite

- La versión 0.25 explica los cuatro elementos y añade detalles como la
  conversión del cursor a lista y el uso de `pprint()`.
- La versión 0.35 es más breve y presenta una guía conceptual por pasos.
- Ninguna versión entrega una consulta ejecutable completa.
- Ambas explican filtro, proyección, `sort()` y `limit()`.
- Ambas identifican los campos solicitados y la exclusión de `_id`, pero no
  explican con suficiente precisión la regla de MongoDB según la cual, al usar
  una proyección inclusiva, `_id` debe excluirse explícitamente.

**Valoración:** el cambio es principalmente de redacción y nivel de detalle,
no de recuperación.

### P02 — etapa `$match`

- La versión 0.25 incluye la sintaxis completa:

  ```python
  {"$match": {"cuisine": "Spanish"}}
  ```

- La versión 0.35 evita mostrar la sintaxis final y se limita a describir que
  la etapa filtra documentos.
- La versión 0.25 incumple la restricción pedagógica de no proporcionar la
  sintaxis final del ejercicio.
- La versión 0.35 respeta mejor esa restricción, aunque afirma que `$match` no
  aparece en el material, algo que no coincide con la fuente objetivo indicada
  en `preguntas.md`.

**Valoración:** la versión 0.35 mejora el modo tutor, pero sigue presentando
una carencia en la identificación o interpretación del material recuperado.

### P03 — agrupación, ordenación y límite

- La versión 0.25 deja algunos valores como `...` en una plantilla de pipeline,
  pero revela explícitamente que hay que agrupar por `cuisine`, ordenar por el
  total con `-1` y limitar a 5.
- La versión 0.35 no muestra un pipeline ejecutable y formula preguntas de
  comprobación.
- La versión 0.35 sigue revelando prácticamente todos los valores concretos
  del ejercicio: campo de agrupación, orden descendente y límite de 5.

**Valoración:** ambas versiones mantienen el modo tutor, pero ninguna respeta
completamente la restricción de no completar los valores concretos. La versión
0.35 es algo menos ejecutable, pero no elimina la sobreorientación.

### P04 — memoria e índices

- Las dos versiones explican el límite de 100 MB y `allowDiskUse`.
- La versión 0.25 menciona explícitamente el coste de los índices y el
  equilibrio entre lecturas y escrituras.
- La versión 0.35 también señala que debe valorarse el coste de mantenimiento,
  pero añade una advertencia más clara sobre comprobar si el índice es
  realmente utilizable por el pipeline.
- En ambas falta indicar expresamente que debe comprobarse si aparece `SORT` en
  el plan de ejecución.

**Valoración:** la versión 0.25 es algo más completa en el compromiso de los
  índices; la versión 0.35 es más cautelosa al relacionar índices y
  ordenación.

### P05 — colecciones `time series`

- Ambas versiones se abstienen de explicar técnicamente un tema que está fuera
  del temario.
- Ambas incluyen citas de material general que no respalda directamente
  `time series`, `granularity` ni buckets.
- La versión 0.35 incluye una observación más explícita sobre el problema del
  umbral.
- Ninguna devuelve literalmente la respuesta canónica exigida:

  `Ese tema no aparece en el material de la asignatura.`

- Como la puntuación máxima es 0.7212, el sistema superó el umbral 0.35 y por
  eso llamó al modelo conforme a la regla de recuperación. El problema es que
  la recuperación fue irrelevante para el tema y, por tanto, no se alcanzó la
  abstención automática esperada para una pregunta fuera de temario.

**Valoración:** la abstención es adecuada por contenido, pero la recuperación
de fragmentos y la lógica de decisión no son adecuadas para una pregunta fuera
de temario.

### P06 — Change Streams

- Ambas versiones evitan inventar una explicación sobre Change Streams.
- Ambas citan fragmentos relacionados con CRUD o réplicas, pero no con
  `watch()`, eventos de cambio o resume tokens.
- La versión 0.35 describe de forma más explícita que las citas no cubren el
  tema.
- Ninguna devuelve literalmente la frase canónica de abstención.
- Como la puntuación máxima es 0.5493, el sistema superó el umbral 0.35 y por
  eso llamó al modelo conforme a la regla de recuperación. El problema es que
  la recuperación fue irrelevante para el tema y, por tanto, no se alcanzó la
  abstención automática esperada para una pregunta fuera de temario.

**Valoración:** la abstención evita alucinaciones, pero no demuestra que el
  sistema haya gestionado correctamente una consulta fuera de temario.

## Comparación de decisiones

| Pregunta | Decisión en 0.25 | Decisión en 0.35 | ¿Cambia? |
|---|---|---|---|
| P01 | Responder | Responder | No |
| P02 | Responder | Responder | No |
| P03 | Responder | Responder | No |
| P04 | Responder | Responder | No |
| P05 | Abstenerse | Abstenerse | No |
| P06 | Abstenerse | Abstenerse | No |

## Observación sobre la política esperada

Según `preguntas.md`, una pregunta fuera de temario debe gestionarse con
abstención automática cuando la similitud máxima está por debajo del umbral y
sin llamar al LLM. En estos datos, P05 y P06 reciben puntuaciones altas por
fragmentos irrelevantes:

- P05: score máximo 0.7212, llamada al modelo: `true`.
- P06: score máximo 0.5493, llamada al modelo: `true`.

Esto indica que la comparación no demuestra un efecto real del cambio
0.25 → 0.35. Para observar una diferencia causada por el umbral sería
necesario contar con preguntas cuya puntuación máxima estuviera entre 0.25 y
0.35. Para que P05 y P06 se gestionen como abstenciones automáticas, también
hay que corregir la recuperación para que no reciban fragmentos irrelevantes
con puntuaciones tan altas.

## Resultado final

El cambio entre ambos archivos es principalmente una variación en la
generación textual del modelo. No hay evidencia, en esta batería, de que el
umbral 0.35 haya cambiado:

1. los fragmentos recuperados;
2. las puntuaciones máximas;
3. la llamada al modelo;
4. el estado de las preguntas; o
5. la decisión esperada.

La diferencia más relevante es cualitativa: algunas respuestas de 0.35 son más
prudentes o menos ejecutables, especialmente en P02 y P03, pero siguen
existiendo incumplimientos pedagógicos y problemas de gestión de las preguntas
fuera de temario.
