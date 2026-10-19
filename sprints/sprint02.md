# ClassGPT: Plan de Trabajo y Reparto de Tareas (Sprint 2)

**Periodo: 14 al 27 de Octubre de 2026 (2 semanas) · Hito Hacia el Core de ClassGPT**

- **Proyecto:** ClassGPT — Asistente Docente RAG Inteligente (Macro-TFG UCM)
- **Director:** Dr. Pablo Cerro Cañizares
- **Titulación:** Grado en Ingeniería del Software (Facultad de Informática, Universidad Complutense de Madrid)
- **Fecha de Planificación:** 19 de octubre de 2026
- **Próxima Revisión de Seguimiento:** Martes 27 de octubre de 2026, 17:00 h
- **Integrantes:** Lucas Sánchez Martín, Alberto Zurita Aguado, Sergio Barbas Vázquez
- **Documento PDF oficial compilado:** [ClassGPT_Plan_Trabajo_Sprint2.pdf](./ClassGPT_Plan_Trabajo_Sprint2.pdf)

---

## 1. Contexto, Decisiones de Dirección y Fuentes de Referencia

El presente plan de trabajo define la hoja de ruta técnica para el Sprint 2 de ClassGPT, articulado a partir de dos fuentes directivas esenciales:

1. **Acta de la Reunión Técnica del 13 de octubre de 2026:**
   - **Consolidación de MongoDB:** Mantener el foco exclusivo en la asignatura de MongoDB antes de generalizar a otras materias (Neo4j o Redis). Ampliar la extracción a todo el conjunto de comandos oficiales e incorporar código fuente.
   - **Integración de Cohere API:** Integrar la clave de API facilitada por el director para implementar Reranking sobre los candidatos recuperados, elevando la precisión del Top-5.
   - **Benchmark sistemático y casos adversariales:** Expandir el banco de pruebas (25–30 preguntas) con taxonomía formal, casos de error real y pruebas adversariales (parámetros invertidos, comandos modificados y contradicciones).
   - **Transparencia en la Web:** Renderizado completo de Markdown, visualización del fragmento recuperado y botón *"Guardar para auditar"* incidencias.
2. **Documento de Diseño "ClassGPT Core" (Entregable 0 de Macro-TFG):**
   - **Principio central:** *"La estructura precede al chunking"*. El chunk es una vista derivada para retrieval, no el objeto fundamental. Adopción del modelo **MIR (Material Intermediate Representation)**.
   - **Priorización arquitectónica:** Abordar el **Nivel 1 (Núcleo Funcional)** estipulado en la Sección 34.2 (Course Space, snapshots, MIR, chunking adaptado, recuperación híbrida y Evidence Bundle).
   - **Exclusión temporal de GraphRAG:** Se pospone la construcción del Knowledge Graph (Sección 13) hasta que el núcleo documental y el benchmark muestren madurez y estabilidad contrastada.

---

## 2. Objetivos Principales del Sprint 2

- **O1 (Ingesta y Corpus):** Completar la extracción de especificaciones IDL de MongoDB (agregaciones, índices y CRUD) y prototipar el conector de código fuente Python.
- **O2 (Recuperación y Reranking):** Implementar el paso de Reranking con Cohere en el pipeline de consulta y evaluar la recuperación híbrida (BM25 léxico + Denso).
- **O3 (Interfaz y Observabilidad):** Añadir renderizado Markdown completo con resaltado de sintaxis, visualización expandida de fragmentos de evidencia y botón de captura para auditoría.
- **O4 (Evaluación y Calibración):** Diseñar y ejecutar el benchmark de 25–30 preguntas con taxonomía formal, midiendo por separado retrieval y generación.

---

## 3. Reparto Detallado de Tareas Individuales

### 3.1. Lucas Sánchez Martín — Ingesta Avanzada, Catálogo IDL y Arquitectura MIR
- **Tarea L1 (Prioridad Máxima - Acta Sec. 10): Catálogo Completo de Comandos IDL de MongoDB.**
  - Ampliar `MongoIDLParser` para procesar las especificaciones formales de los comandos nucleares de la asignatura: `aggregate_command.idl`, `createIndexes_command.idl`, `insert_command.idl`, `update_command.idl`, `delete_command.idl` y `count_command.idl`.
  - Extraer esquemas de parámetros, descripciones y enriquecer con ejemplos sintéticos en Mongo Shell y PyMongo (con fallback canónico de contingencia).
- **Tarea L2 (Prioridad Alta - Acta Sec. 10 y Core Sec. 9.6): Conector de Código Fuente.**
  - Diseñar el conector y adaptador de código (`CodePolicy`) para ingerir scripts Python (`.py`) y repositorios de prácticas, preservando firmas, docstrings y contexto estructural sin fragmentaciones arbitrarias.
- **Tarea L3 (Prioridad Media - Core Sec. 11): Prototipo de Representación Intermedia (MIR).**
  - Refactorizar la jerarquía de parseadores hacia un esquema inicial de `MIRDocument` y `MIRElement` (Sec. 11.2), desacoplando la estructura del material original de la generación de fragmentos derivados.
- **Tarea L4 (Prioridad Media - Core Sec. 15): Estructuración Jerárquica Parent-Child.**
  - Modelar relaciones padre-hijo en chunks para permitir indexar partes atómicas (hijo) y recuperar contexto envolvente (padre) durante la consulta.

### 3.2. Alberto Zurita Aguado — Reranking con Cohere, Frontend Markdown y Resiliencia
- **Tarea A1 (Prioridad Máxima - Correo Dirección y Core Sec. 18.7): Integración de Cohere Rerank.**
  - Implementar el componente `CohereReranker` en `src/retrieval/rerank.py` usando la API key oficial de Cohere. Reordenar los candidatos recuperados por el motor vectorial antes de la composición del contexto, midiendo el impacto en precisión.
- **Tarea A2 (Prioridad Alta - Acta Sec. 05 y 07): Renderizado Markdown y Visualización de Evidencias.**
  - Actualizar `templates/index.html` para soportar Markdown completo con resaltado de sintaxis y desplegable interactivo que permita al estudiante inspeccionar el texto exacto del fragmento recuperado (`display_content`).
- **Tarea A3 (Prioridad Alta - Acta Sec. 08): Botón 'Guardar para Auditar'.**
  - Añadir en la interfaz un control para capturar preguntas problemáticas, respuestas incompletas o fallos de recuperación, persistiendo los datos (consulta, candidatos y respuesta) en `data/auditoria/` para revisión posterior.
- **Tarea A4 (Prioridad Media - Acta Sec. 04 y 07): Resiliencia y Fallback Multi-Proveedor.**
  - Reforzar el cliente LLM en `src/assistant.py` con política de reintentos exponenciales y conmutación automática entre modelos o proveedores de respaldo para blindar la app ante errores de cuota (429) o indisponibilidad (503).

### 3.3. Sergio Barbas Vázquez — Benchmark Sistemático, Casos Adversariales y Evaluación
- **Tarea S1 (Prioridad Máxima - Acta Sec. 01/08 y Core Sec. 32): Ampliación del Benchmark (25–30 Preguntas).**
  - Construir y etiquetar formalmente en `data/preguntas.md` y `.json` una batería de 25–30 casos siguiendo la taxonomía del Core: Factuales, Relacionales, Dependencia técnica, Ejemplos, Código, Comparativas y Fuera de temario (Out-of-scope).
- **Tarea S2 (Prioridad Alta - Acta Sec. 08 y 11): Casos Adversariales y Errores Habituales de Prácticas.**
  - Incorporar preguntas con fallos sintácticos intencionados, nombres de comandos ligeramente modificados y parámetros invertidos para evaluar la robustez y capacidad correctiva/docente del sistema ante errores reales de estudiantes.
- **Tarea S3 (Prioridad Alta - Acta Sec. 10 y Core Sec. 17.3): Evaluación de Recuperación Híbrida (Dense + BM25).**
  - Prototipar e indexar un índice léxico BM25 para evaluar la recuperación combinada frente a consultas de símbolos exactos (ej. nombres de métodos), comparando el desempeño frente a la búsqueda exclusivamente densa.
- **Tarea S4 (Prioridad Media - Acta Sugerencias): Desacoplamiento de Métricas de Evaluación.**
  - Instrumentar la suite de pruebas para medir y reportar por separado: (i) métricas de retrieval (Recall@k, Precision@k, MRR); y (ii) métricas de generación (Groundedness, respeto del modo tutor y abstención ante casos ajenos).

---

## 4. Matriz de Tareas, Entregables y Prioridades del Sprint 2

| Tarea / Entregable | Responsable | Módulo Afectado | Prioridad | Criterio de Éxito |
| :--- | :---: | :--- | :---: | :--- |
| **Ampliación comandos IDL** | Lucas | `src/parsers/idl_parser.py` | **MÁXIMA** | 6+ comandos oficiales con ejemplos indexados |
| **Conector de código fuente** | Lucas | `src/parsers/code_parser.py` | **ALTA** | Ingesta de scripts `.py` con preservación AST |
| **Prototipo de clases MIR** | Lucas | `src/parsers/mir.py` | **MEDIA** | Contrato común `MIRDocument` / `MIRElement` |
| **Integración Cohere Rerank** | Alberto | `src/retrieval/rerank.py` | **MÁXIMA** | Reordenación eficaz de candidatos en el flujo |
| **Frontend Markdown y evidencias** | Alberto | `templates/index.html` | **ALTA** | Renderizado Markdown + vista de fragmentos |
| **Captura para auditoría** | Alberto | `app.py` / `assistant.py` | **ALTA** | Botón "Guardar para auditar" en la web |
| **Benchmark ampliado (25–30)** | Sergio | `data/preguntas.md / .json` | **MÁXIMA** | Batería categorizada según taxonomía Core |
| **Casos adversariales y de error** | Sergio | `data/preguntas_adversariales` | **ALTA** | Pruebas con errores reales de alumnos |
| **Recuperación híbrida (BM25)** | Sergio | `src/retrieval/lexical.py` | **ALTA** | Comparativa empírica Dense vs Dense+BM25 |

---

## 5. Mapeo Arquitectónico hacia ClassGPT Core

El trabajo del Sprint 2 implementa los componentes prioritarios del **Nivel 1 (Núcleo Funcional)** estipulado en la Sección 34.2 del documento ClassGPT Core:

1. **Representación MIR (Sección 11, Págs. 30–42):** Transición hacia `MIRElement` tipados (`CODE_BLOCK`, `API_OPERATION`, `NOTEBOOK`, `SECTION`), asegurando que el chunking sea una vista derivada y preservando procedencia inmutable.
2. **Recuperación Híbrida y Reranking (Secciones 17 y 18, Págs. 93–127):** Fusión de señales de búsqueda densa y léxica (BM25) complementada con **Cohere Rerank** para filtrar y ordenar el conjunto de candidatos antes de componer el `EvidenceBundle`.
3. **Parent-Child Retrieval (Sección 15, Págs. 76–85):** Indexación de chunks hijos específicos con referencia al chunk padre contenedor para proveer contexto ampliado al LLM cuando la pregunta lo requiera sin desbordar el presupuesto de tokens.
4. **Benchmark Reproducible (Sección 32, Págs. 288–297):** Adopción formal del contrato de evaluación por intención (Factual, Relacional, Código, Out-of-scope), separando estrictamente la calidad de recuperación de la precisión y fidelidad de la respuesta generada.

> **Alineamiento Estratégico sobre GraphRAG (Sección 13):** En cumplimiento estricto del acuerdo del acta (Sec. 12) y de las instrucciones del director, se pospone la construcción del Knowledge Graph hasta que el núcleo documental de MongoDB y el benchmark muestren estabilidad y calidad contrastada. El Sprint 2 sienta las bases estructuradas necesarias para que el grafo posterior sea fiable.

---

## 6. Criterios de Aceptación para la Reunión del 27 de Octubre de 2026

1. **Catálogo IDL Funcional:** El sistema debe resolver dudas sobre operaciones de agregación (`aggregate`), índices y CRUD recuperando y citando los archivos IDL oficiales con sus correspondientes ejemplos canónicos.
2. **Reranking Operativo con Cohere:** Comprobación en vivo del reordenamiento de evidencias mediante Cohere, demostrando una mejora cualitativa y cuantitativa en la selección de los fragmentos más relevantes.
3. **Frontend Enriquecido y Auditoría:** Demostración de la interfaz con soporte completo de Markdown, visualización del fragmento recuperado y registro funcional de una consulta problemática mediante el botón de auditoría.
4. **Informe Experimental del Benchmark:** Presentación de los resultados de evaluación sobre la batería ampliada de 25–30 preguntas, desglosando métricas de recuperación (Recall@k, MRR) y generación (groundedness, abstención y respeto del modo tutor).

---

### Firmas y Responsables

- **Lucas Sánchez Martín** — Ingeniería de Ingesta, IDL & MIR
- **Alberto Zurita Aguado** — Reranking, Web & Resiliencia
- **Sergio Barbas Vázquez** — Benchmark, QA & RAG Híbrido
