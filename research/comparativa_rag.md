# Comparativa de recuperación

El pool denso son los 20 fragmentos de mayor coseno. Cohere reordena ese pool para todas las consultas (sin umbral de similitud, para compararlo en igualdad de condiciones) y se queda con 5. El híbrido fusiona el ranking denso y el BM25 con reciprocal rank (k = 60) porque las dos puntuaciones no comparten escala.

Recall@k es la fracción de citas anotadas que aparecen en el corte. hit@k vale 1 si entra al menos una: es el criterio de las semillas. Precision@k y MRR miran si la evidencia queda arriba, que es lo que debe mejorar un rerank. La abstención se cuenta con el estado del asistente (`abstencion`), no con un párrafo que diga que el tema no está.

Consultas con evidencia anotada (semillas y generados): 7.

| Grupo | Variante | Recall@20 | hit@5 | Precision@5 | MRR | Redund.@5 |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| semillas | denso | 0.750 | 1.000 | 0.350 | 0.833 | 0.145 |
| semillas | hibrido BM25 | 0.750 | 0.750 | 0.300 | 0.750 | 0.102 |
| semillas | Cohere | 0.750 | 1.000 | 0.400 | 0.833 | 0.139 |
| generados | denso | 0.333 | 0.333 | 0.067 | 0.333 | 0.109 |
| generados | hibrido BM25 | 0.333 | 0.333 | 0.067 | 0.333 | 0.125 |
| generados | Cohere | 0.333 | 0.333 | 0.067 | 0.333 | 0.127 |
| categoría: compleja | denso | 0.667 | 1.000 | 0.300 | 1.000 | 0.228 |
| categoría: compleja | hibrido BM25 | 0.667 | 1.000 | 0.400 | 1.000 | 0.154 |
| categoría: compleja | Cohere | 0.667 | 1.000 | 0.400 | 1.000 | 0.232 |
| categoría: directa | denso | 0.833 | 1.000 | 0.400 | 0.667 | 0.123 |
| categoría: directa | hibrido BM25 | 0.833 | 0.500 | 0.200 | 0.500 | 0.065 |
| categoría: directa | Cohere | 0.833 | 1.000 | 0.400 | 0.667 | 0.090 |
| categoría: factual | denso | 1.000 | 1.000 | 0.200 | 1.000 | 0.076 |
| categoría: factual | hibrido BM25 | 1.000 | 1.000 | 0.200 | 1.000 | 0.097 |
| categoría: factual | Cohere | 1.000 | 1.000 | 0.200 | 1.000 | 0.061 |
| categoría: syntax | denso | 0.000 | 0.000 | 0.000 | 0.000 | 0.125 |
| categoría: syntax | hibrido BM25 | 0.000 | 0.000 | 0.000 | 0.000 | 0.140 |
| categoría: syntax | Cohere | 0.000 | 0.000 | 0.000 | 0.000 | 0.160 |

Abstención en casos fuera de temario: 0.000 sobre 2 casos.

Generación, sobre los casos con respuesta guardada. El solape de palabras con el criterio no es esta nota: una respuesta puede compartir vocabulario y aun así entregar la solución o decir lo contrario. Por eso se añade una corrección semántica de 1 a 5 puntuada por el juez.

- Casos con respuesta evaluados: 7
- Acierto de decisión: 0.714
- Fidelidad al fragmento: 1.000 sobre 3 respuestas
- No entrega la solución: 1.000 sobre 3 respuestas
- Corrección semántica (1-5): 5.000 sobre 3 respuestas
- Tasa de citas inventadas: 0.500 sobre 7 respuestas con citas
- Recall de citas anotadas: 0.333
- Errores de infraestructura apartados: 2

Redundancia@5: similitud de Jaccard media entre los fragmentos del top-5 (cuanto más alta, menos diversos son). El overlap del chunking sigue sin medirse porque cambiarlo obliga a volver a calcular los embeddings.

## Latencia (ms)

Codificación de la consulta y búsqueda vectorial se miden por separado. Las filas `e2e` (codificación, búsqueda, rerank, generación del LLM y total) solo aparecen con `--latencia-e2e`.

| Etapa | n | Media | P50 | P95 |
| --- | ---: | ---: | ---: | ---: |
| codificacion consulta | 9 | 25.7 | 25.1 | 33.1 |
| busqueda vectorial | 9 | 87.3 | 50.2 | 433.7 |
| bm25 | 9 | 5.7 | 4.7 | 9.4 |
| rerank Cohere | 9 | 523.0 | 455.8 | 1045.1 |
