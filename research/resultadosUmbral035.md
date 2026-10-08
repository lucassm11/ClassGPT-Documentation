# PREGUNTA 1
**Respuesta** 

¡Hola! Estaré encantado de guiarte para que construyas tu consulta en PyMongo paso a paso.

Para resolver tu ejercicio, debes combinar varios métodos y parámetros que ya has visto en los materiales:

1. **Filtro de búsqueda (`find`):** Debes filtrar los documentos donde el tipo de cocina (`cuisine`) sea `"Spanish"`.
2. **Proyección:** Para indicar qué campos mostrar y cuáles ocultar (como el `_id`), puedes pasar un segundo argumento al método `find` con diccionarios de exclusión/inclusión, similar a cómo se hace en `[Notebook - CRUD.ipynb - Celda 45]`.
3. **Ordenamiento (`sort`):** Para ordenar los resultados de forma ascendente por el campo `name`, puedes utilizar la función `ASCENDING` importada desde `pymongo`, aplicando una estructura de lista de tuplas similar a la que se muestra en `[Notebook - CRUD.ipynb - Celda 37]`.
4. **Límite (`limit`):** Para restringir el número máximo de documentos devueltos, puedes encadenar el método `.limit()` al final de la consulta, tal como se hace en `[Notebook - CRUD.ipynb - Celda 45]`.

Piensa en cómo estructurarías estos elementos en una sola línea de código asignada a una variable (por ejemplo, `resultados = list(restaurants.find(...))`). 

Intenta proponer tu código basado en estas pistas y en las celdas de referencia, y lo revisamos juntos. ¡Tú puedes!
Fragmentos recuperados
Celda[Notebook - CRUD.ipynb - Celda 37] — similitud 0.69
Celda[Notebook - CRUD.ipynb - Celda 49] — similitud 0.65
Celda[Notebook - CRUD.ipynb - Celda 45] — similitud 0.64

**anotaciones**
**Evaluación:** Cumple en general la política de tutor: distingue `find`, filtro,
proyección, `sort` y `limit`, orienta sin entregar una consulta ejecutable y cita
fragmentos recuperados. La teoría es correcta, pero queda incompleta porque no
explicita que la proyección solicitada debe incluir `name` y `borough`, ni que,
al usar una proyección inclusiva, `_id` debe excluirse expresamente.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.


# PREGUNTA 2
**Respuesta**
Hola. Como asistente docente, analizaremos tu duda basándonos estrictamente en el material proporcionado.

Revisando el material de referencia actual (**Tema 2 - Mongodb.pdf**), debo indicarte honestamente que **dicho tema o la explicación detallada de la etapa `$match` no aparece en las diapositivas proporcionadas**. 

No obstante, basándonos en la estructura lógica de los pipelines de agregación que sí se estudian en el material (como `$bucket` o `$project`), una etapa en el pipeline de MongoDB opera transformando o filtrando flujos de datos. Si necesitas aplicar un filtro para conservar documentos específicos (como filtrar por un campo `cuisine`), la teoría general de bases de datos y agregación indica que se deben utilizar operadores de consulta dentro de las etapas del pipeline para evaluar condiciones.

Si tienes más fragmentos del material o notas de clase sobre `$match`, compártelos aquí y con gusto lo revisaremos juntos aplicando el rigor técnico necesario.
Fragmentos recuperados
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 78] — similitud 0.65
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 9] — similitud 0.61
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 70] — similitud 0.61

**anotaciones**
**Evaluación:** Explica de forma válida la función general de una etapa de
filtrado y su efecto sobre las etapas posteriores, y no entrega un documento
`$match` completo. Sin embargo, afirma que el tema no aparece en el material
aunque la pregunta exige localizarlo en el material objetivo; tampoco establece
la analogía con `WHERE` ni ofrece un pseudopaso suficientemente concreto.
Los fragmentos citados no respaldan de forma clara la explicación específica
de `$match`.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.

# PREGUNTA 3
**Respuesta**
¡Hola! Como tu asistente docente, estaré encantado de guiarte para que diseñes este pipeline tú mismo aplicando el rigor técnico necesario.

Si analizamos el material de referencia, para resolver tu consulta necesitamos encadenar tres etapas (operadores) dentro de una lista de agregación en MongoDB (`pipeline`). 

Aquí tienes la estructura conceptual y el propósito de cada una de las etapas que debes utilizar:

1. **Agrupación (`$group`):** Esta es la etapa inicial que te permite agrupar los documentos según un campo específico (en este caso, la cocina) y realizar una operación de conteo sobre ellos [Notebook - Teoría agregaciones.ipynb - Celda 8 y Celda 36].
   * *Pregunta clave para reflexionar:* ¿Qué valor debes asignar a `_id` para agrupar por el tipo de cocina, y qué operador de acumulación se menciona en el material para contar los elementos de cada grupo [Notebook - Teoría agregaciones.ipynb - Celda 8 y Celda 36]?

2. **Ordenación (`$sort`):** Una vez que tienes el conteo total por cada cocina, necesitas ordenar los resultados [Notebook - Teoría agregaciones.ipynb - Celda 36].
   * *Pregunta clave para reflexionar:* El enunciado te pide ordenar de **mayor a menor** (orden descendente). Según el código de ejemplo en las fuentes, ¿qué valor numérico (`1` o `-1`) debes asociar al campo del total para lograr este orden [Notebook - Teoría agregaciones.ipynb - Celda 36]?

3. **Límite (`$limit`):** Por último, para restringir la cantidad de documentos que salen de la tubería [Notebook - Teoría agregaciones.ipynb - Celda 36].
   * *Pregunta clave para reflexionar:* Si solo quieres que se devuelvan las 5 primeras cocinas tras el ordenamiento, ¿qué número entero debes pasarle a este operador [Notebook - Teoría agregaciones.ipynb - Celda 36]?

Intenta armar el fragmento de código JSON (la lista `pipeline`) combinando estas tres etapas basándote en la estructura que aparece en `[Notebook - Teoría agregaciones.ipynb - Celda 36]`. Si te atascas en alguna parte, dime qué llevas escrito y lo revisamos juntos.
Fragmentos recuperados
Celda[Notebook - Teoría agregaciones.ipynb - Celda 36] — similitud 0.71
Celda[Notebook - CRUD.ipynb - Celda 43] — similitud 0.63
Celda[Notebook - Teoría agregaciones.ipynb - Celda 8] — similitud 0.62
**anotaciones**
**Evaluación:** Explica correctamente la función y el orden conceptual de
`$group`, `$sort` y `$limit`, incluye preguntas de comprobación, cita el
material y no presenta un pipeline ejecutable. No obstante, revela prácticamente
todos los valores concretos del ejercicio (agrupación por `cuisine`, orden
descendente y límite de 5), pese a que el criterio pide que el estudiante los
complete. Por tanto, mantiene el modo tutor, pero ofrece una pista demasiado
resolutiva.  
**teoria_correcta:** sí. **no_resuelve_directamente:** sí, con incumplimiento
parcial de la restricción pedagógica.
# PREGUNTA 4
**Respuesta**
Hola. Como tu asistente docente, estoy aquí para guiarte en la resolución de tu duda basándome estrictamente en el material oficial de la asignatura.

Analicemos los puntos que mencionas:

1. **El problema de la ordenación en memoria:**
   Según el material, la etapa `sort` en las agregaciones realiza el ordenamiento por defecto en memoria. El problema principal es que este método **está limitado a 100 MB** [Notebook - Teoría agregaciones.ipynb - Celda 35]. Si el volumen de datos o el resultado intermedio de tu agregación supera este umbral, la operación fallará.

2. **Alternativas para evitarlo o mitigarlo:**
   Para colecciones grandes que superan este límite, la alternativa proporcionada por el motor es realizar la ordenación **en disco** [Notebook - Teoría agregaciones.ipynb - Celda 35]. Esto se consigue habilitando el parámetro correspondiente en la función de agregación mediante la opción `allowDiskUse=True` [Notebook - Teoría agregaciones.ipynb - Celda 35].

3. **Relación con el diseño de índices:**
   Piénsalo desde una perspectiva de rendimiento y diseño: si una etapa de agregación necesita ordenar una gran cantidad de datos, ¿cómo interviene la indexación previa? Te sugiero reflexionar sobre cómo un índice adecuado en las claves de ordenación puede evitar que MongoDB tenga que realizar un `sort` pesado en memoria o disco al final de la canalización (*pipeline*), entregando los documentos ya ordenados o facilitando su acceso. 

   *Pregunta para ti:* Si tuvieras que diseñar un índice para una consulta de agregación que filtra y luego ordena por un campo específico, ¿cómo estructurarías ese índice para optimizar la etapa `sort`? 

¡Intenta plantear una propuesta y la discutimos!
Fragmentos recuperados
Celda[Notebook - Teoría agregaciones.ipynb - Celda 35] — similitud 0.67
Diapositiva[DiseñoPatrones.pdf - Diapositiva 46] — similitud 0.64
Celda[Notebook - Indexacion y Rendimiento.ipynb - Celda 89] — similitud 0.63
**anotaciones**
**Evaluación:** Explica correctamente el límite de 100 MB y el uso conceptual de
`allowDiskUse`, relacionándolo con la ordenación y los índices, sin escribir una
llamada `aggregate()` ni un índice completo. Faltan dos elementos exigidos:
indicar que debe comprobarse si aparece `SORT` en el plan y valorar el coste de
mantener índices. Además, la afirmación de que un índice puede evitar el `sort`
debe condicionarse al orden del pipeline y a la posibilidad de que el índice
sea utilizable.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.
# PREGUNTA 5
**Respuesta**
Hola. Como asistente docente, debo indicarte honestamente que el tema sobre **cómo diseñar y configurar una colección de tipo *time series* en MongoDB, las opciones de `granularity` y la optimización de sus *buckets*** no aparece en las diapositivas proporcionadas en el material de referencia actual. 

El material disponible menciona aspectos generales de la modelización de datos en MongoDB (como el uso de documentos embebidos y esquemas flexibles) [DiseñoPatrones.pdf - Diapositiva 18], pero no profundiza en colecciones de series temporales (*time series*). 

Si tienes apuntes adicionales, fragmentos de código o diapositivas específicas sobre este tema, por favor compártelos aquí y con gusto te ayudaré a analizar la teoría y resolver tus dudas aplicando un enfoque pedagógico.
Fragmentos recuperados
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 18] — similitud 0.72
Diapositiva[DiseñoPatrones.pdf - Diapositiva 18] — similitud 0.72
Diapositiva[DiseñoPatrones.pdf - Diapositiva 2] — similitud 0.71
**anotaciones**
**Evaluación:** Se abstiene de inventar una explicación sobre `time series`,
`granularity` y buckets, por lo que respeta la intención principal de la
abstención. Sin embargo, añade información general sobre modelización y citas
irrelevantes, en lugar de devolver únicamente la abstención esperada. Además,
el máximo recuperado es 0.72, superior al umbral 0.35; por tanto, el sistema
llamó al LLM de acuerdo con la regla de superar el umbral, pero la recuperación
irrelevante impidió la abstención automática esperada.  
**decisión:** abstención de contenido correcta, gestión de recuperación y
decisión esperada incorrectas.
# PREGUNTA 6
**Respuesta**
Hola. Como tu asistente docente, debo indicarte honestamente que **dicho tema (la configuración de Change Streams, los eventos que generan y los resume tokens) no aparece en las diapositivas o notebooks proporcionados** en el material de referencia actual. 

Si subes el material correspondiente a esa unidad, estaré encantado de ayudarte a resolver tu duda basándome estrictamente en él. ¡Mucho ánimo con el estudio!
Fragmentos recuperados
Celda[Notebook - CRUD.ipynb - Celda 104] — similitud 0.55
Celda[Notebook_-_Teoria_Replicas.ipynb - Celda 53] — similitud 0.55
Celda[Notebook - CRUD.ipynb - Celda 119] — similitud 0.46
**anotaciones**
**Evaluación:** No inventa la API de Change Streams y declara que el tema no
aparece en el material, lo que es correcto como contenido de abstención. La
respuesta no usa la frase canónica exigida y los fragmentos recuperados no son
pertinentes. El máximo recuperado es 0.55, superior al umbral 0.35; por ello,
el sistema llamó al LLM de acuerdo con la regla de superar el umbral, pero la
recuperación irrelevante impidió la abstención automática esperada.  
**decisión:** abstención de contenido correcta, recuperación y decisión
esperada incorrectas.