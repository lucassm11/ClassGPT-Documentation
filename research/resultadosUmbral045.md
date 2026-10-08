# PREGUNTA 1
**Respuesta**

¡Hola! Estaré encantado de ayudarte a construir tu consulta en PyMongo paso a
paso.

Para resolverla, debes combinar varios elementos que ya has visto en los
materiales:

1. **Filtro de búsqueda (`filter`):** Selecciona los restaurantes cuyo tipo de
   cocina sea `"Spanish"`. Puedes revisar el ejemplo de filtrado por campos
   específicos en [Notebook - CRUD.ipynb - Celda 45].
2. **Proyección (`projection`):** Muestra únicamente `name` y `borough` y
   excluye `_id`. La estructura con valores `1` y `0` aparece en
   [Notebook - CRUD.ipynb - Celda 45].
3. **Ordenamiento (`sort`):** Ordena por `name` de forma ascendente utilizando
   `ASCENDING`, como se muestra en [Notebook - CRUD.ipynb - Celda 37].
4. **Límite (`limit`):** Restringe el resultado a 5 documentos encadenando
   `.limit()` al final de la consulta, según [Notebook - CRUD.ipynb - Celda
   45].

Intenta estructurar el código combinando `restaurants.find(...)`, el filtro y
la proyección, y encadenando después `.sort(...)` y `.limit(5)`. Si te atascas
con la sintaxis, comparte tu propuesta y la revisamos.

Fragmentos recuperados  
Celda[Notebook - CRUD.ipynb - Celda 37] — similitud 0.69  
Celda[Notebook - CRUD.ipynb - Celda 49] — similitud 0.63  
Celda[Notebook - CRUD.ipynb - Celda 45] — similitud 0.62

**anotaciones**

**Evaluación:** Distingue correctamente filtro, proyección, ordenamiento y
límite; identifica los campos solicitados y la exclusión de `_id`, cita
material pertinente y no entrega una consulta `find()` completa. La explicación
es adecuada, aunque no explicita la regla de MongoDB según la cual, cuando la
proyección es inclusiva, `_id` debe excluirse expresamente.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.

# PREGUNTA 2
**Respuesta**

Hola. Como asistente docente, estaré encantado de guiarte con tu duda sobre las
agregaciones en MongoDB.

La etapa `$match` sirve para **filtrar documentos** utilizando una consulta
similar a la que se emplea con `find()` [Notebook - Teoría agregaciones.ipynb -
Celda 26]. Para conservar únicamente los documentos que cumplen una condición
sobre `cuisine`, debes colocar `$match` dentro del pipeline y acompañarlo de un
documento que indique el campo que se comprueba y el valor que debe tener.

Intenta redactar la estructura del diccionario para esta etapa sin completar la
solución. Si quieres, comparte tu propuesta y la revisamos.

Fuente: [Notebook - Teoría agregaciones.ipynb - Celda 26]

Fragmentos recuperados  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 78] — similitud 0.63  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 51] — similitud 0.62  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 26] — similitud 0.62

**anotaciones**

**Evaluación:** Explica correctamente que `$match` filtra documentos y lo
relaciona con `find()`. También evita proporcionar el documento `$match`
completo, por lo que respeta la restricción pedagógica principal. Sin embargo,
no explica que el filtrado afecta a las etapas posteriores, no establece la
analogía con `WHERE` y no menciona de forma explícita que el pipeline se ejecuta
mediante `aggregate`.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.

# PREGUNTA 3
**Respuesta**

Hola. Como asistente docente, estaré encantado de guiarte para que diseñes este
pipeline por ti mismo utilizando el material oficial de la asignatura.

Un pipeline se ejecuta de manera secuencial, pasando los documentos de una
etapa a la siguiente:

1. **Agrupación:** Cuenta cuántos restaurantes hay por cada tipo de cocina
   (`cuisine`) mediante `$group` y el acumulador `$count` [Notebook - Teoría
   agregaciones.ipynb - Celda 36].
2. **Ordenación:** Ordena los resultados de mayor a menor mediante `$sort` y
   el valor `-1` para el sentido descendente [Notebook - Teoría agregaciones
   .ipynb - Celda 36].
3. **Limitación:** Utiliza `$limit` para devolver únicamente las 5 primeras
   cocinas [Notebook - Teoría agregaciones.ipynb - Celda 36].

Puedes completar esta estructura:

```python
pipeline = [
    {
        "$group": {
            # Define aquí cómo agrupar y contar
        }
    },
    {
        "$sort": {
            # Define aquí el campo de conteo y el orden descendente
        }
    },
    {
        "$limit": # Indica el número máximo de resultados
    }
]
```

Fragmentos recuperados  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 36] — similitud 0.72  
Celda[Notebook - CRUD.ipynb - Celda 43] — similitud 0.61  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 11] — similitud 0.61

**anotaciones**

**Evaluación:** Explica el orden lógico de `$group`, `$sort` y `$limit`, el
papel general de cada etapa y cita el fragmento principal. La plantilla no es
ejecutable porque deja los elementos esenciales incompletos. No obstante,
revela valores concretos del ejercicio (`cuisine`, `$count`, `-1` y 5), pese a
que el criterio pide que el estudiante complete los nombres de campos,
acumuladores y valores.  
**teoria_correcta:** sí. **no_resuelve_directamente:** sí, con incumplimiento
parcial de la restricción pedagógica.

# PREGUNTA 4
**Respuesta**

¡Hola! Soy **ClassGPT**, tu asistente docente. Estudiemos este problema de
rendimiento en las agregaciones paso a paso.

### 1. El problema de la ordenación en memoria

Cuando se realiza una etapa `sort` sobre una colección grande, la operación se
ejecuta en memoria por defecto y está limitada a **100 MB** [Notebook - Teoría
agregaciones.ipynb - Celda 35]. Si se supera ese umbral, la operación puede
fallar o detenerse por falta de memoria.

### 2. Alternativa de uso de disco

Para permitir que el motor maneje volúmenes superiores a 100 MB, el material
indica que se puede habilitar `allowDiskUse=True` dentro de `aggregate()`
[Notebook - Teoría agregaciones.ipynb - Celda 35].

### 3. Relación con el diseño de índices

Un índice adecuado puede mantener los datos ordenados según las claves
requeridas y evitar una ordenación pesada sobre todo el conjunto de resultados.
La conveniencia depende de que el índice sea compatible con la ordenación.

También debe considerarse el coste de los índices en las operaciones de
agregación y escritura [Notebook - Indexacion y Rendimiento.ipynb - Celda 89].
Reflexiona sobre si crear un índice compensaría el uso de
`allowDiskUse`.

Fragmentos recuperados  
Celda[Notebook - Teoría agregaciones.ipynb - Celda 35] — similitud 0.67  
Diapositiva[DiseñoPatrones.pdf - Diapositiva 46] — similitud 0.64  
Celda[Notebook - Indexacion y Rendimiento.ipynb - Celda 89] — similitud 0.63

**anotaciones**

**Evaluación:** Explica correctamente el límite de 100 MB y la alternativa de
usar disco mediante `allowDiskUse`. Relaciona los índices con la posibilidad
de evitar una ordenación costosa y menciona el coste de las escrituras. No
entrega una llamada `aggregate()` completa ni un índice concreto. Sin embargo,
no indica expresamente que debe comprobarse si aparece `SORT` en el plan de
ejecución, y la relación entre índice y eliminación de `sort` debe
condicionarse a que el índice sea realmente utilizable por el orden del
pipeline.  
**teoria_correcta:** parcial. **no_resuelve_directamente:** sí.

# PREGUNTA 5
**Respuesta**

Hola. Analizando la pregunta sobre el diseño y la configuración de colecciones
*time series*, sus opciones de `granularity` y la optimización de *buckets*,
debo indicarte honestamente que **dicho tema no aparece en las diapositivas
proporcionadas** en el material de referencia actual.

El material disponible cubre aspectos generales de modelización y agregaciones
[Tema 2 - Mongodb.pdf - Diapositiva 51 y DiseñoPatrones.pdf - Diapositiva 2],
pero no profundiza en las características específicas de las colecciones
*time series*.

Si tienes material adicional sobre este tema, compártelo para analizarlo
basándonos estrictamente en él.

Fragmentos recuperados  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 51] — similitud 0.72  
Diapositiva[DiseñoPatrones.pdf - Diapositiva 2] — similitud 0.72  
Diapositiva[Tema 2 - Mongodb.pdf - Diapositiva 18] — similitud 0.71

**anotaciones**

**Evaluación:** Se abstiene de inventar una explicación sobre `time series`,
`granularity` y buckets, lo que es correcto en contenido. Sin embargo, no
devuelve literalmente la frase canónica exigida:
`Ese tema no aparece en el material de la asignatura.` Además, las citas son
generales y no respaldan directamente la consulta. El score máximo es 0.7212,
superior al umbral 0.45; por tanto, el sistema llamó al LLM porque la
recuperación superó el umbral, pero la recuperación irrelevante impidió la
abstención automática esperada.  
**decisión:** abstención de contenido correcta, recuperación y decisión
esperada incorrectas.

# PREGUNTA 6
**Respuesta**

Hola. Como asistente docente, debo indicarte honestamente que **dicho tema (la
configuración, los eventos y la reanudación mediante *resume tokens* de un
Change Stream) no aparece en las diapositivas o materiales de referencia
proporcionados**.

El material actual solo cubre los métodos básicos de modificación mediante
CRUD, como `update_one` y `update_many`, y el funcionamiento de las
modificaciones en un conjunto de réplicas mediante el *oplog* [Notebook -
CRUD.ipynb - Celda 104, Notebook_-_Teoria_Replicas.ipynb - Celda 53].

Si dispones de material adicional sobre Change Streams, compártelo para
resolver la duda de forma rigurosa y basada en las fuentes.

Fragmentos recuperados  
Celda[Notebook - CRUD.ipynb - Celda 104] — similitud 0.55  
Celda[Notebook_-_Teoria_Replicas.ipynb - Celda 53] — similitud 0.55  
Celda[Notebook - CRUD.ipynb - Celda 119] — similitud 0.46

**anotaciones**

**Evaluación:** Evita inventar la API de Change Streams y reconoce que el
material recuperado no cubre `watch()`, sus eventos ni los `resume tokens`.
No devuelve literalmente la frase canónica de abstención y las citas solo son
indirectamente relacionadas. El score máximo es 0.5493, superior al umbral
0.45; por tanto, el sistema llamó al LLM porque la recuperación superó el
umbral, pero la recuperación irrelevante impidió la abstención automática
esperada.  
**decisión:** abstención de contenido correcta, recuperación y decisión
esperada incorrectas.
