# Banco de preguntas de evaluación de MongoDB

## Propósito

Esta batería es la calibración inicial del asistente RAG. Contiene seis
preguntas etiquetadas para poder medir por separado la recuperación, la
generación y la abstención:

- **P01-P02:** sintaxis directa, con la respuesta localizada principalmente
  en un fragmento.
- **P03-P04:** preguntas complejas que combinan agregaciones, índices y
  gestión de memoria.
- **P05-P06:** preguntas deliberadamente fuera del temario indexado; el
  resultado esperado es la abstención.

La etiqueta `fuente_objetivo` identifica el material que debe aparecer entre
los fragmentos recuperados. No es una respuesta literal ni sustituye a la
revisión de la cita concreta devuelta por el sistema.

## Política pedagógica evaluada

El objetivo no es que el asistente resuelva los ejercicios por el estudiante.
Para P01-P04, una respuesta adecuada debe:

- explicar la teoría y el propósito de cada operación;
- orientar sobre el orden de razonamiento y los conceptos que debe aplicar el
  estudiante;
- señalar errores habituales o preguntas de comprobación;
- citar el material recuperado cuando corresponda.

No debe entregar la consulta, el pipeline o el código final completo, ni
rellenar todos los valores concretos del ejercicio. Si el estudiante pide
explícitamente la solución, debe mantener el modo tutor y ofrecer una pista
progresiva o pedirle que comparta su intento. Una respuesta que da una
solución ejecutable completa se considera un incumplimiento pedagógico,
aunque técnicamente sea correcta.

## Criterios comunes

- Una pregunta **en temario** se considera correctamente recuperada si el
  top-3 contiene al menos un fragmento que respalde los conceptos indicados.
- Una pregunta **en temario** se considera correctamente generada si la
  respuesta explica los conceptos esperados sin introducir afirmaciones no
  respaldadas por el contexto y respeta la política pedagógica.
- Para P01-P04 se registran por separado `teoria_correcta` y
  `no_resuelve_directamente`. La respuesta solo cumple completamente si
  ambas son verdaderas.
- Una pregunta **fuera de temario** se considera correctamente gestionada si
  la similitud máxima es inferior al umbral configurado y el sistema devuelve
  `Ese tema no aparece en el material de la asignatura.` sin llamar al LLM.
- Los campos `fallo`, `diagnostico` y `carencias` se completan después de
  ejecutar la batería; inicialmente deben permanecer como `pendiente`.

## P01 — Consulta `find()`: filtro, proyección y ordenación

- **Pregunta:** En PyMongo, escribe una consulta `find()` que seleccione los
  restaurantes de tipo `Spanish`, muestre solo `name` y `borough` (sin `_id`),
  los ordene por `name` ascendente y limite el resultado a 5 documentos.
- **Tipo:** directa
- **En temario:** sí
- **Respuesta esperada:** Debe distinguir filtro, proyección, ordenación y
  límite; explicar que la proyección selecciona campos y que `_id` requiere
  una exclusión explícita cuando se usa una proyección inclusiva; y orientar
  sobre la cadena de operaciones sin completar el código.
- **Conceptos esperados:** `find`, filtro, proyección, `sort`, `limit`,
  exclusión de `_id`.
- **Restricción pedagógica:** No debe mostrar una llamada `find()` completa ni
  sustituir los valores del enunciado por una solución ejecutable.
- **Fuente objetivo:** `Notebook - CRUD.ipynb`, secciones sobre `find()`,
  proyección, `sort()` y `limit()`.
- **Decisión esperada:** responder.
- **Fallo:** pendiente
- **Diagnóstico:** pendiente
- **Carencias:** pendiente

## P02 — Etapa `$match` en agregaciones

- **Pregunta:** ¿Qué hace la etapa `$match` en un pipeline de agregación de
  MongoDB y cómo se escribiría para conservar únicamente documentos cuyo
  campo `cuisine` sea `Spanish`?
- **Tipo:** directa
- **En temario:** sí
- **Respuesta esperada:** Debe explicar el filtrado, su posición en el flujo
  del pipeline, su efecto sobre las etapas posteriores y la analogía con
  `WHERE`, sin proporcionar la sintaxis final de una consulta.
- **Conceptos esperados:** `aggregate`, pipeline, `$match`, filtrado,
  correspondencia con `WHERE`.
- **Restricción pedagógica:** Se permiten pseudopasos o una descripción
  abstracta, pero no un documento `$match` completo con el campo y valor del
  enunciado.
- **Fuente objetivo:** `Notebook - Teoría agregaciones.ipynb`, introducción
  al framework y ejemplos de `$match`.
- **Decisión esperada:** responder.
- **Fallo:** pendiente
- **Diagnóstico:** pendiente
- **Carencias:** pendiente

## P03 — Agrupación, ordenación y límite en una agregación

- **Pregunta:** Diseña un pipeline que cuente cuántos restaurantes hay por
  `cuisine`, ordene las cocinas de mayor a menor número de restaurantes y
  devuelva solo las 5 primeras. Explica el papel y el orden de cada etapa.
- **Tipo:** compleja
- **En temario:** sí
- **Respuesta esperada:** Debe explicar la relación entre clave de agrupación,
  acumulador, ordenación por el resultado calculado y recorte final; debe
  justificar el orden conceptual de esas operaciones y proponer preguntas de
  comprobación para que el estudiante complete el pipeline.
- **Conceptos esperados:** pipeline, `$group`, clave `_id`, `$count` o
  `$sum`, `$sort: -1`, `$limit`, orden de ejecución.
- **Restricción pedagógica:** No debe incluir un pipeline ejecutable ni
  completar los nombres de campos, acumuladores y valores concretos del
  ejercicio.
- **Fuente objetivo:** `Notebook - Teoría agregaciones.ipynb`, etapas
  `$group`, `$sort` y `$limit`; `Notebook - Agregacion_Festivales.ipynb`,
  ejercicios de agrupación y ordenación.
- **Decisión esperada:** responder.
- **Fallo:** pendiente
- **Diagnóstico:** pendiente
- **Carencias:** pendiente

## P04 — Memoria e índices en agregaciones

- **Pregunta:** Una agregación agrupa una colección grande y después ordena
  los resultados. ¿Qué problema puede producir una ordenación en memoria de
  más de 100 MB y qué alternativas del material permiten evitarlo o
  mitigarlo? Relaciona la respuesta con el diseño de índices.
- **Tipo:** compleja
- **En temario:** sí
- **Respuesta esperada:** Debe explicar que una ordenación en memoria está
  limitada a 100 MB según el material, que una colección grande puede
  superar ese límite, y comparar conceptualmente el uso de disco y el de un
  índice adecuado. Debe indicar que hay que comprobar si aparece `SORT` en el
  plan y valorar el coste de los índices, sin dar una receta completa.
- **Conceptos esperados:** límite de 100 MB, `allowDiskUse`, `$sort`, `SORT`
  en el plan, índice que cubre la ordenación, coste de los índices.
- **Restricción pedagógica:** No debe escribir una llamada `aggregate()`
  completa, un índice completo ni una solución específica para el escenario.
- **Fuente objetivo:** `Notebook - Teoría agregaciones.ipynb`, ordenación en
  memoria y `allowDiskUse`; `Notebook - Indexacion y Rendimiento.ipynb`,
  relación entre índices y `SORT`; `Notebook - Indices.ipynb`, rendimiento y
  memoria de índices.
- **Decisión esperada:** responder.
- **Fallo:** pendiente
- **Diagnóstico:** pendiente
- **Carencias:** pendiente

## P05 — Fuera de temario: colecciones time series

- **Pregunta:** ¿Cómo se diseña y configura una colección time series de
  MongoDB, qué opciones de `granularity` existen y cómo se optimizan sus
  buckets?
- **Tipo:** fuera_de_temario
- **En temario:** no
- **Respuesta esperada:** Abstención. La respuesta no debe presentar una
  explicación técnica como si estuviera respaldada por los materiales
  indexados.
- **Conceptos ausentes buscados:** colecciones time series, buckets y
  `granularity`.
- **Fuente objetivo:** ninguna.
- **Decisión esperada:** abstenerse.
- **Fallo:** pendiente
- **Diagnóstico:** pendiente
- **Carencias:** confirmar tras la ejecución que el material no cubre este
  tema.

## P06 — Fuera de temario: Change Streams

- **Pregunta:** ¿Cómo se configura un Change Stream para recibir cambios de
  una colección, qué eventos genera y cómo se reanuda desde un resume token?
- **Tipo:** fuera_de_temario
- **En temario:** no
- **Respuesta esperada:** Abstención. No debe inventar una API ni completar la
  respuesta con conocimiento externo al material.
- **Conceptos ausentes buscados:** Change Streams, `watch()`, eventos de
  cambio y resume tokens.
- **Fuente objetivo:** ninguna.
- **Decisión esperada:** abstenerse.
- **Fallo:** pendiente
- **Diagnóstico:** pendiente
- **Carencias:** confirmar tras la ejecución que el material no cubre este
  tema.

## Registro posterior de resultados

Para P01-P04, `teoria_correcta` y `no_resuelve_directamente` deben
registrarse por separado. Una respuesta que entrega la solución completa no
debe recibir la puntuación máxima de generación, aunque su código funcione.

| ID | Score máximo | Top-3 relevante | Decisión del umbral | Teoría correcta | No resuelve directamente | Resultado | Fallo | Diagnóstico |
|---|---:|---|---|---|---|---|---|---|
| P01 | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente |
| P02 | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente |
| P03 | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente |
| P04 | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente | pendiente |
| P05 | pendiente | pendiente | pendiente | no aplica | no aplica | pendiente | pendiente | pendiente |
| P06 | pendiente | pendiente | pendiente | no aplica | no aplica | pendiente | pendiente | pendiente |
