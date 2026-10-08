# PREGUNTA 1
**Respuesta** 
¡Hola! Estaré encantado de ayudarte a construir tu consulta en PyMongo paso a
paso. 

Para lograr lo que pides, necesitamos combinar varios elementos que ya has
visto en tus materiales: la función `find()` para filtrar y proyectar campos,
el método `sort()` para el ordenamiento, y `limit()` para restringir el número
de documentos.

Aquí te explico la estructura teórica que debes seguir:

1. **Filtro y proyección en `find()`:** El primer diccionario dentro de
   `find()` define el filtro: buscar los restaurantes cuyo tipo de cocina
   (`cuisine`) sea `"Spanish"`. El segundo diccionario define la proyección,
   es decir, qué campos mostrar y cuáles ocultar. Para ocultar `_id` y mostrar
   solo `name` y `borough`, debes asignarles `1` o `0` según corresponda
   [Notebook - CRUD.ipynb - Celda 45].
2. **Ordenamiento (`sort`):** Puedes aplicar un orden ascendente utilizando
   `ASCENDING` de `pymongo`, tal como se muestra en la celda 37 [Notebook -
   CRUD.ipynb - Celda 37].
3. **Límite (`limit`):** Al igual que en la celda 45, puedes encadenar el
   método `.limit(n)` al final para restringir el número de documentos.
4. **Conversión a lista y visualización:** `find()` devuelve un cursor, por lo
   que puedes convertirlo en una lista usando `list()` y utilizar `pprint()`
   para mostrar los resultados [Notebook - CRUD.ipynb - Celda 45].

Intenta armar tu código combinando estos métodos y recuerda importar
`ASCENDING` desde `pymongo` [Notebook - CRUD.ipynb - Celda 37]. Si te surge
alguna duda con la sintaxis exacta, dime y lo revisamos.

Fragmentos recuperados  
Celda[Notebook - CRUD.ipynb - Celda 37] — similitud 0.69  
Celda[Notebook - CRUD.ipynb - Celda 49] — similitud 0.63  
Celda[Notebook - CRUD.ipynb - Celda 45] — similitud 0.62

**anotaciones**
**Evaluación:** Cumple en general la política de tutor: distingue `find`,
filtro, proyección, `sort` y `limit`, orienta sin entregar una consulta
ejecutable y cita fragmentos recuperados. La teoría es parcialmente correcta,
pero no explicita con suficiente precisión que la proyección solicitada debe
incluir `name` y `borough`, ni que, al usar una proyección inclusiva, `_id`
debe excluirse expresamente. Añadir `pprint()` y la conversión a lista es
correcto, aunque no era necesario para resolver el ejercicio.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.


# PREGUNTA 2
**Respuesta**
Hola. Como asistente docente, estaré encantado de explicarte el funcionamiento
de esta etapa fundamental en MongoDB.

Basándonos en el material de la asignatura, la etapa `$match` tiene como
propósito **filtrar documentos** utilizando una consulta similar a las que
empleamos en el comando `find()` [Notebook - Teoría agregaciones.ipynb -
Celda 26]. En un pipeline de agregación, actúa normalmente al principio para
reducir el conjunto de datos sobre el que trabajarán las siguientes etapas.

Para escribir esta etapa y conservar únicamente los documentos cuyo campo
`cuisine` sea igual a `"Spanish"`, debes utilizar un documento JSON o
diccionario en Python con el operador de igualdad implícito. La estructura
propuesta es:

```python
{"$match": {"cuisine": "Spanish"}}
```

Piensa en cómo estructurabas las consultas `find`: el campo por el que filtras
va como clave y el valor deseado como valor. ¿Cómo encajaría esta etapa dentro
de una lista de un pipeline si quisieras combinarla con otras operaciones?

Fragmentos recuperados  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 78] — similitud 0.63  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 51] — similitud 0.62  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 26] — similitud 0.62

**anotaciones**
**Evaluación:** Explica correctamente la función de `$match`, su posición
habitual al principio del pipeline y su efecto de reducir los documentos que
procesarán las etapas posteriores. También relaciona la etapa con `find` y
cita material pertinente. Sin embargo, entrega la sintaxis completa con los
campos y valores concretos del ejercicio, incumpliendo la restricción
pedagógica. Tampoco establece explícitamente la analogía con `WHERE`.  
**teoria_correcta:** sí. **no_resuelve_directamente:** no.

# PREGUNTA 3
**Respuesta**
¡Hola! Como asistente docente, estaré encantado de guiarte para que diseñes tu
propio pipeline de agregación en MongoDB basándote en los materiales de la
asignatura.

Para resolver este ejercicio, debes encadenar tres etapas dentro de una lista,
en el orden lógico en que se procesan los datos:

1. **Agrupar y contar:** Primero necesitas agrupar los documentos por el tipo
   de cocina (`cuisine`) y contar cuántos restaurantes pertenecen a cada
   categoría. Revisa cómo se define el operador acumulador para contar en una
   fase de agrupación [Notebook - Teoría agregaciones.ipynb - Celda 36].
2. **Ordenar:** Una vez obtenido el conteo por cocina, debes ordenar los
   resultados. Como buscas de mayor a menor, necesitarás usar el valor `-1`
   [Notebook - Teoría agregaciones.ipynb - Celda 36].
3. **Limitar:** Finalmente, restringe el resultado a las 5 primeras cocinas.
   El operador correspondiente limita el número de documentos que pasan a la
   salida del pipeline [Notebook - Teoría agregaciones.ipynb - Celda 36].

Intenta estructurar el pipeline en una lista de diccionarios y dime qué código
has pensado si te atascas.

Fragmentos recuperados  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 36] — similitud 0.72  
Celda[Notebook - CRUD.ipynb - Celda 43] — similitud 0.61  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 11] — similitud 0.61

**anotaciones**
**Evaluación:** Explica correctamente el orden conceptual de `$group`, `$sort`
y `$limit`, cita el material y evita presentar un pipeline ejecutable completo.
No obstante, revela los valores concretos del ejercicio: agrupar por
`cuisine`, ordenar de forma descendente con `-1` y limitar a 5. La plantilla
mental es útil, pero la respuesta no respeta completamente la restricción de
no completar los nombres de campos, acumuladores y valores.  
**teoria_correcta:** sí. **no_resuelve_directamente:** sí, con incumplimiento
parcial de la restricción pedagógica.
# PREGUNTA 4
**Respuesta**
¡Hola! Estudiar el rendimiento de las agregaciones es fundamental para
optimizar bases de datos. Vamos a analizar tu duda paso a paso utilizando el
material de referencia.

### 1. El problema de la ordenación en memoria
Según el material, la etapa `sort` ordena los documentos por una o más claves.
Por defecto, esta operación se realiza en memoria y está limitada a 100 MB
[Notebook - Teoría agregaciones.ipynb - Celda 35]. Si el volumen de datos a
ordenar tras la agrupación supera ese límite, la operación fallará.

### 2. Alternativas para evitarlo o mitigarlo
El material ofrece la posibilidad de permitir que la operación utilice el
disco configurando `allowDiskUse=True` dentro de `aggregate()` [Notebook -
Teoría agregaciones.ipynb - Celda 35]. Esto permite manejar colecciones grandes
superando el límite de memoria, aunque puede ser más lento.

### 3. Relación con el diseño de índices
Un uso correcto de los índices puede evitar que `sort` tenga que ordenar desde
cero grandes volúmenes, ya que los índices almacenan los datos ordenados según
las claves especificadas. Sin embargo, un exceso de índices o muchas escrituras
puede encarecer las operaciones, por lo que deben evaluarse las métricas de
rendimiento y la proporción entre lecturas y escrituras [Notebook - Indexacion
y Rendimiento.ipynb - Celda 89].

Reflexiona sobre si compensaría crear un índice para evitar `allowDiskUse`.

Fragmentos recuperados  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 35] — similitud 0.67  
Diapositiva[DiseñoPatrones.pdf - Diapositiva 46] — similitud 0.64  
Celda[Notebook - Indexacion y Rendimiento.ipynb - Celda 89] — similitud 0.63

**anotaciones**
**Evaluación:** Explica correctamente el límite de 100 MB, el posible fallo al
superarlo y el uso de `allowDiskUse` para permitir ordenación en disco.
Relaciona la mitigación con los índices y menciona su coste en las
operaciones de escritura. No presenta una llamada `aggregate()` completa ni
un índice completo, por lo que no resuelve directamente el ejercicio. Sin
embargo, no indica que debe comprobarse si aparece `SORT` en el plan de
ejecución. La relación entre un índice y la eliminación de `sort` debería
condicionarse al orden del pipeline y a que el índice sea utilizable.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.
# PREGUNTA 5
**Respuesta**
Hola. Como asistente docente, debo indicarte honestamente que **el tema de las
colecciones *time series* (series temporales), sus opciones de *granularity* y
la optimización de *buckets* no aparece** en el material de referencia
proporcionado (*Tema 2 - Mongodb.pdf* y *DiseñoPatrones.pdf*).

El material actual se centra en aspectos generales de la modelización de datos,
proyecciones y una introducción básica a las agregaciones [Tema 2 - Mongodb.pdf
- Diapositiva 51, DiseñoPatrones.pdf - Diapositiva 2, Tema 2 - Mongodb.pdf -
Diapositiva 18]. Si dispones de más documentación sobre este tema, compártela
para analizarla basándonos en el material oficial.

Fragmentos recuperados  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 51] — similitud 0.72  
Diapositiva[DiseñoPatrones.pdf - Diapositiva 2] — similitud 0.72  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 18] — similitud 0.71

**anotaciones**
**Evaluación:** La respuesta se abstiene de inventar una explicación sobre un
tema fuera del temario y reconoce que los fragmentos no lo cubren. Sin embargo,
no devuelve literalmente la respuesta canónica exigida: `Ese tema no aparece
en el material de la asignatura.` Además, incluye información y citas generales
que no respaldan directamente la consulta. El score máximo es 0.7212, superior
al umbral 0.25; por tanto, el sistema siguió coherentemente la regla de llamar
al LLM cuando la recuperación supera el umbral, pero la recuperación
irrelevante impidió la abstención automática esperada.  
**decisión:** abstención de contenido correcta, gestión de recuperación y
decisión esperada incorrectas.
# PREGUNTA 6
**Respuesta**
Hola. Analizando el material de referencia proporcionado sobre operaciones CRUD
y teoría de réplicas en MongoDB [Notebook - CRUD.ipynb - Celda 104,
Notebook_-_Teoria_Replicas.ipynb - Celda 53], debo informarte que **dicho tema
no aparece en las diapositivas o notebooks proporcionados**.

El material actual cubre los métodos de modificación básicos (`update_one`,
`update_many`, etc.), operadores como `$set` y `$unset`, y el registro de
operaciones (*oplog*) a nivel de réplicas, pero no detalla la configuración,
eventos ni el uso de *resume tokens* de Change Streams.

Si subes el material correspondiente, estaré encantado de ayudarte a resolver
la duda de forma rigurosa y guiada.

Fragmentos recuperados  
Celda[Notebook - CRUD.ipynb - Celda 104] — similitud 0.55  
Celda[Notebook_-_Teoria_Replicas.ipynb - Celda 53] — similitud 0.55  
Celda[Notebook - CRUD.ipynb - Celda 119] — similitud 0.46

**anotaciones**
**Evaluación:** La respuesta evita inventar la API de Change Streams y reconoce
que el material recuperado no cubre `watch()`, sus eventos ni los resume
tokens. No devuelve literalmente la frase canónica de abstención y las citas
son solo indirectamente relacionadas. El score máximo es 0.5493, superior al
umbral 0.25; por tanto, el sistema siguió coherentemente la regla de llamar al
LLM cuando la recuperación supera el umbral, pero la recuperación irrelevante
impidió la abstención automática esperada.  
**decisión:** abstención de contenido correcta, recuperación y decisión
esperada incorrectas.