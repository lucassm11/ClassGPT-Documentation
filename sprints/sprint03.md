# ClassGPT: Plan de Trabajo y Reparto de Tareas (Sprint 3)

**Periodo: 10 al 24 de Noviembre de 2026 (2 semanas) · Contratos de Datos, Ingesta por Interfaces, Recuperación Intercambiable y Etiquetado de Versiones**

- **Proyecto:** ClassGPT — Asistente Docente RAG Inteligente (TFG Grado en Ingeniería del Software, UCM)
- **Director:** Dr. Pablo Cerro Cañizares
- **Fecha de Planificación:** 10 de noviembre de 2026
- **Revisión intermedia:** Martes 17 de noviembre de 2026 (hora por confirmar)
- **Cierre del sprint:** Martes 24 de noviembre de 2026
- **Documentos base:** Acta de la reunión del 3 de noviembre de 2026 · *ClassGPT Core – Entregable 1*
- **Integrantes:** Lucas Sánchez Martín, Alberto Zurita Aguado, Sergio Barbas Vázquez
- **Documento PDF oficial compilado:** [ClassGPT_Plan_Trabajo_Sprint3.pdf](./ClassGPT_Plan_Trabajo_Sprint3.pdf)

---

## 1. Contexto y Decisiones de Dirección (Acta 3 de Noviembre)

El tutor considera que la base funcional está bastante avanzada y fija como **decisión prioritaria** adaptar progresivamente lo desarrollado a los **contratos de datos** del documento *ClassGPT Core – Entregable 1*, comprobando cada cambio con tests. El grafo de conocimiento y el benchmark quedan para después de esa formalización.

| Acuerdo / acción del acta | Qué significa para el Sprint 3 |
| :--- | :--- |
| **Contratos de datos** (Sec. 09 · Decisión) | Pasar de `DocumentChunk` (texto + metadatos sueltos) a `MIRDocument`/`MIRElement` (Sec. 11) y `Chunk` (Sec. 16), con tipos docentes (definición, ejemplo, contraejemplo, ejercicio, solución). |
| **Claves de fragmentos y versiones** (Sec. 09 · Acción) | Cada fragmento guarda `course_id`, `edition_id`, `source_id`, snapshot y versión de la tecnología (p. ej. MongoDB 8.0 frente a 8.11). |
| **Ingesta con clases o interfaces** (Sec. 01 · Acción) | Conectores (de dónde se obtiene el material) y adaptadores por formato (cómo se interpreta): `SourceConnector` y `FormatAdapter` (Sec. 9). |
| **Recuperación sin reranking e intercambiable** (Sec. 04 · Acción) | Modo sin Cohere para clase, selección por configuración, comportamiento definido si falla el reordenador y transformación de puntuaciones documentada. |
| **Ejercicios y ejemplos vinculados** (Sec. 03 y 12) | Relación enunciado–solución en notebooks y varios ejemplos por definición (caso `find`). |
| **Criterios de auditoría** (Sugerencias) | Comprobar correspondencia afirmación–fuente, localización, versión y suficiencia de la evidencia. |

## 2. Objetivos Principales del Sprint 3

- **O1 (Contratos de datos):** modelo tipado y validado de `MIRDocument`, `MIRElement`, `Chunk` y registro de fuentes, con tests.
- **O2 (Ingesta por interfaces):** conectores y adaptadores (PDF, notebook, código Python, IDL) que producen MIR.
- **O3 (Fragmentación y versiones):** chunks conformes al contrato, sin truncado de embeddings y con etiquetas de versión filtrables en Atlas.
- **O4 (Recuperación intercambiable):** modos con y sin reranking, reranker local de respaldo y abstención corregida y calibrada.
- **O5 (Unidades docentes y calidad):** ejercicios, soluciones y ejemplos identificados; auditoría con criterios; tests ejecutados en la CI.

> **Principio de trabajo:** migración progresiva. El corpus nuevo se indexa en `knowledge_chunks_v2` y la web sigue con la colección actual hasta que la nueva supere los criterios de aceptación.

---

## 3. Punto de Partida: Fallos Detectados en el Código (10-Nov)

| Fallo | Prueba que lo demuestra | Se resuelve en |
| :--- | :--- | :---: |
| Abstención inoperante | Atlas devuelve (1+cos)/2 y el umbral 0,35 equivale a coseno −0,3. "¿Capital de Australia?" llama al LLM. Atlas no separa "time series" (0,82, fuera) de "replica set" (0,76, dentro); Cohere sí (máx. 0,35 fuera frente a mín. 0,75 dentro). | A2 |
| Embeddings truncados | MiniLM lee 128 tokens; 304 de 1.057 fragmentos son más largos. "$setUnion" y "currentOp" responden "no aparece". | S1 |
| Citas IDL no trazables | Ejemplos escritos por el equipo ("límite de 100 MB") citados como `[aggregate_command.idl]`; Atlas guarda una versión del texto distinta a la del código. | L4, A3 |
| Datos de ejemplo como teoría | "¿Receta de la paella?" devuelve datos de prueba del notebook de índices. | S2 |
| Sin modo sin reranking ni respaldo | Si Cohere falla se pierde el único filtro que separa bien. | A1, A2 |
| Proveedor LLM opaco | Groq cae a Gemini en silencio; la auditoría guarda `LLM_MODEL`, no el modelo real. | A4 |
| Tests fuera de la CI | 41 tests en local (más 19 no descubiertos); el workflow no ejecuta ninguno. | S4 |
| Benchmark no comparable | 3 casos regenerados al azar en cada release. | S3 (mínimo) |

## 4. Calendario y Dependencias

La tarea **L1 (contratos)** bloquea al resto: debe estar fusionada como muy tarde el **jueves 12**.

| Fechas | Lucas | Alberto | Sergio |
| :--- | :--- | :--- | :--- |
| **10–12 nov** · Arranque | L1 contratos + validador + tests; informe de diferencias | A1 interfaces `Retriever`/`Reranker` y modo sin reranking | S4 tests en la CI; S2 análisis de notebooks de ejercicios |
| **13–16 nov** · Semana 1 | L2 conectores y adaptadores PDF y notebook | A2 abstención: escala, reranker local, calibración | S3 conjunto de calibración (20/20); S2 reglas de unidades docentes |
| **17 nov** · Revisión tutor | Demo: contratos y MIR de un PDF y un notebook | Demo: con/sin reranking y abstención | Demo: CI con tests y tabla de calibración |
| **18–22 nov** · Semana 2 | L2 código e IDL; L3 fuentes y versiones; L4 IDL trazable | A3 chunk store v2; A4 auditoría | S1 chunking sobre MIR; S2 integración de tipos docentes |
| **23–24 nov** · Cierre | Reindexación completa en v2 | Cambio de la web a v2 | Comparativa con/sin reranking |

---

## 5. Reparto Detallado de Tareas (Sprint 3)

### 5.1. Lucas Sánchez Martín — Contratos de Datos, Framework de Ingesta y Versiones
- **L1 (MÁXIMA · bloqueante): Modelo tipado de los contratos.** `src/contracts/` con `CourseSpace`, `SourceRecord`, `SourceSnapshot`, `MIRDocument`, `MIRElement` y `Chunk`; enumeraciones cerradas (tipos MIR, autoridad C0–C4, visibilidad, tipo de fuente); `MIRValidator` mínimo; informe `docs/diferencias_contratos.md`. *Hecho cuando* un MIR de ejemplo se valida y uno inválido produce incidencias.
- **L2 (MÁXIMA): Framework de conectores y adaptadores.** `SourceConnector` (`discover`, `snapshot`, `list_artifacts`, `fetch`) y `FormatAdapter` (`parse` → `MIRDocument`); `LocalFolderConnector`, `GitConnector`; adaptadores PDF, notebook, Python e IDL. *Hecho cuando* los 18 materiales de MongoDB y los 7 IDL producen MIR válido.
- **L3 (ALTA): Registro de fuentes, snapshots y versiones.** `data/sources.yaml`, `content_hash` SHA-256, `course_id`/`edition_id` (propuesta `nsq-ucm` / `2026-2027`), `technology_version`; IDL fijado a una rama o etiqueta, no a `master`.
- **L4 (ALTA): IDL trazable.** Solo el contenido oficial con autoridad `OFFICIAL_EXTERNAL`; los ejemplos del equipo pasan a material `GENERATED` con su propia cita y revisados a mano.

### 5.2. Alberto Zurita Aguado — Recuperación Intercambiable, Abstención y Chunk Store
- **A1 (MÁXIMA): Recuperación intercambiable.** `Retriever` y `Reranker` (`CohereReranker`, `LocalCrossEncoderReranker`, `NoReranker`), `RETRIEVAL_MODE=rerank/direct`, modo visible en la web y respaldo local si Cohere falla.
- **A2 (MÁXIMA): Abstención corregida y calibrada.** Escala documentada (coseno = 2·puntuación − 1), decisión con la relevancia del reranker, umbrales calibrados con S3, campo "respondible" del LLM y una única función `debe_abstenerse` para web y benchmark.
- **A3 (ALTA): Chunk store v2.** `knowledge_chunks_v2` con el contrato `Chunk`, `display_content` separado de `retrieval_content`, `token_count`, autoridad, versión, visibilidad, filtros en el índice vectorial y limpieza de huérfanos.
- **A4 (MEDIA): Auditoría con criterios.** Traza con `corpus_snapshot_id`, `chunk_id`, modo, reranker y proveedor/modelo real; comprobación automática de que las citas existen en el contexto.

### 5.3. Sergio Barbas Vázquez — Fragmentación, Unidades Docentes y Calidad
- **S1 (MÁXIMA): Chunking sobre MIR sin truncado.** Políticas por tipo de material, división con `mir_spans` respetando el límite del tokenizador y `retrieval_content` con contexto. *Hecho cuando* "$setUnion" y "currentOp" recuperan su diapositiva en el top-5.
- **S2 (ALTA): Unidades docentes.** Detección de `EXERCISE`, `SOLUTION` y `EXAMPLE`, enlaces enunciado–solución, datos de prueba marcados como no-teoría y soluciones con visibilidad de profesor.
- **S3 (MEDIA): Conjunto de calibración.** 20 preguntas dentro y 20 fuera (incluidas las "cercanas"), revisadas y congeladas; comparativa de calidad, latencia y dependencia con y sin reranking. No es el benchmark ClassGPT-0.
- **S4 (MEDIA): Tests en la CI.** Job de `unittest` en cada push y pull request, incluir `Test_metricas.py` y tests herméticos sin Atlas real.

---

## 6. Fuera de Alcance (Decisiones del Acta)

| Tema | Motivo |
| :--- | :--- |
| Grafo de conocimiento | Acta Sec. 10: después de formalizar los datos. |
| Benchmark ClassGPT-0 y adversariales | Acta Sec. 09 y 10: pospuestos (solo el conjunto mínimo S3). |
| Vídeos, subtítulos y enriquecimiento | Acta Sec. 02: posibilidad, no compromiso. |
| Historial, valoraciones y FAQ | Acta Sec. 06: exploración futura. |
| Consultas desde varios clientes | Acta Sec. 07: pendiente de definir permisos y privacidad. |
| Webs y Neo4j/Redis | Fuera del foco en MongoDB acordado el 13 de octubre. |

## 7. Criterios de Aceptación (24 de Noviembre de 2026)

1. **Contratos operativos:** todo el corpus de MongoDB como `MIRDocument` válido y chunks conformes al contrato `Chunk`, con curso, edición, fuente, snapshot y versión.
2. **Ingesta por interfaces:** PDF, notebook, Python e IDL entran por `SourceConnector` + `FormatAdapter`.
3. **Recuperación en dos modos:** la web funciona con y sin reranking y sin conexión a Cohere.
4. **Abstención real:** las preguntas de fuera del conjunto de calibración se abstienen sin llamar al LLM.
5. **Trazabilidad:** ninguna cita atribuye a una fuente oficial un texto que no contiene; las soluciones no son recuperables por un alumno.
6. **Calidad verificable:** todos los cambios tienen tests y la CI los ejecuta en cada push.

## 8. Riesgos y Medidas

| Riesgo | Medida |
| :--- | :--- |
| Querer implementar el documento completo | Solo el "núcleo funcional" de la Sec. 34.2. |
| L1 se retrasa y bloquea al resto | Sesión conjunta el día 11 y fusión el 12 aunque sean contratos mínimos. |
| Romper la web durante la migración | Colección `knowledge_chunks_v2` separada. |
| Umbrales ajustados a ojo | Fijarlos con el conjunto S3 antes de la comparativa final. |
| Disponibilidad en diciembre | Cerrar el Sprint 3 completo el 24 de noviembre. |

---

### Firmas y Responsables

- **Lucas Sánchez Martín** — Contratos, Ingesta y Versiones
- **Alberto Zurita Aguado** — Recuperación, Abstención y Auditoría
- **Sergio Barbas Vázquez** — Fragmentación, Unidades Docentes y Calidad
