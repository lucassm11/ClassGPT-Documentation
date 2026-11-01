# Comparativa de recuperación

El pool denso son los 20 fragmentos de mayor coseno. Cohere reordena ese pool para todas las consultas (sin umbral de similitud, para compararlo en igualdad de condiciones) y se queda con 5. El híbrido fusiona el ranking denso y el BM25 con reciprocal rank (k = 60) porque las dos puntuaciones no comparten escala.

Recall@k es la fracción de citas anotadas que aparecen en el corte. hit@k vale 1 si entra al menos una: es el criterio de las semillas. Precision@k y MRR miran si la evidencia queda arriba, que es lo que debe mejorar un rerank. La abstención se cuenta con el estado del asistente (`abstencion`), no con un párrafo que diga que el tema no está.

Consultas con evidencia anotada (semillas y generados): 7.

| Grupo | Variante | Recall@20 | hit@5 | Precision@5 | MRR | Redund.@5 |
| --- | --- | ---: | ---: | ---: | ---: | ---: |
| semillas | denso | 0.750 | 1.000 | 0.350 | 0.833 | 0.145 |
| semillas | hibrido BM25 | 0.750 | 0.750 | 0.300 | 0.750 | 0.102 |
| semillas | Cohere | 0.750 | 1.000 | 0.400 | 0.833 | 0.139 |
| generados | denso | 0.667 | 0.667 | 0.133 | 0.500 | 0.118 |
| generados | hibrido BM25 | 1.000 | 1.000 | 0.200 | 0.733 | 0.126 |
| generados | Cohere | 0.667 | 0.667 | 0.133 | 0.667 | 0.119 |
| adversarial | denso | 1.000 | 1.000 | 0.200 | 0.250 | 0.072 |
| adversarial | hibrido BM25 | 1.000 | 1.000 | 0.200 | 1.000 | 0.086 |
| adversarial | Cohere | 1.000 | 1.000 | 0.200 | 1.000 | 0.084 |
| categoría: compleja | denso | 0.667 | 1.000 | 0.300 | 1.000 | 0.228 |
| categoría: compleja | hibrido BM25 | 0.667 | 1.000 | 0.400 | 1.000 | 0.154 |
| categoría: compleja | Cohere | 0.667 | 1.000 | 0.400 | 1.000 | 0.232 |
| categoría: directa | denso | 0.833 | 1.000 | 0.400 | 0.667 | 0.123 |
| categoría: directa | hibrido BM25 | 0.833 | 0.500 | 0.200 | 0.500 | 0.065 |
| categoría: directa | Cohere | 0.833 | 1.000 | 0.400 | 0.667 | 0.090 |
| categoría: factual | denso | 1.000 | 1.000 | 0.200 | 0.500 | 0.128 |
| categoría: factual | hibrido BM25 | 1.000 | 1.000 | 0.200 | 1.000 | 0.133 |
| categoría: factual | Cohere | 1.000 | 1.000 | 0.200 | 1.000 | 0.131 |
| categoría: syntax | denso | 0.500 | 0.500 | 0.100 | 0.500 | 0.113 |
| categoría: syntax | hibrido BM25 | 1.000 | 1.000 | 0.200 | 0.600 | 0.122 |
| categoría: syntax | Cohere | 0.500 | 0.500 | 0.100 | 0.500 | 0.112 |

Abstención en casos fuera de temario: 0.000 sobre 2 casos.

Generación, sobre los casos con respuesta guardada. El solape de palabras con el criterio no es esta nota: una respuesta puede compartir vocabulario y aun así entregar la solución o decir lo contrario. Por eso se añade una corrección semántica de 1 a 5 puntuada por el juez.

- Casos con respuesta evaluados: 4
- Acierto de decisión: 1.000
- Fidelidad al fragmento: 0.750 sobre 4 respuestas
- No entrega la solución: 1.000 sobre 4 respuestas
- Corrección semántica (1-5): 4.000 sobre 4 respuestas
- Tasa de citas inventadas: 0.000 sobre 4 respuestas con citas
- Recall de citas anotadas: 0.500
- Errores de infraestructura apartados: 0

Redundancia@5: similitud de Jaccard media entre los fragmentos del top-5 (cuanto más alta, menos diversos son). El overlap del chunking sigue sin medirse porque cambiarlo obliga a volver a calcular los embeddings.

## Latencia (ms)

Codificación de la consulta y búsqueda vectorial se miden por separado. Las filas `e2e` (codificación, búsqueda, rerank, generación del LLM y total) solo aparecen con `--latencia-e2e`.

| Etapa | n | Media | P50 | P95 |
| --- | ---: | ---: | ---: | ---: |
| codificacion consulta | 10 | 25.0 | 24.6 | 30.3 |
| busqueda vectorial | 10 | 40.5 | 32.4 | 66.0 |
| bm25 | 10 | 6.4 | 5.6 | 11.7 |
| rerank Cohere | 10 | 736.4 | 469.5 | 1754.4 |
