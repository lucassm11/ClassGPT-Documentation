# Informes Ejecutivos de Rendimiento y Benchmark RAG

En este directorio se almacenan los informes PDF periódicos generados automáticamente en cada release de **ClassGPT**.

Estos informes recopilan la evolución cuantitativa y cualitativa del sistema de cara a la defensa del **Trabajo de Fin de Grado (TFG)**:

- **Evolución del pipeline RAG**: Denso (Atlas `$vectorSearch`), Híbrido (BM25 + Vector) y Reranking con Cohere.
- **Métricas de Recuperación**: Hit Rate@k, Recall@k, MRR y Redundancia@k.
- **Métricas Pedagógicas del LLM**:
  - Precisión de Decisión / Abstención (responder solo temario docente).
  - Fidelidad Contextual (*Faithfulness* - sin alucinaciones).
  - Restricción Pedagógica / Método Socrático (no resolver ejercicios directamente).
  - Corrección Semántica (1 - 5).
  - Tasa de Citas Alucinadas (auditoría de corchetes contra el corpus).
- **Latencias**: Percentiles P50 y P95 desglosados por fase operativa.

## Archivos destacados:
- `ClassGPT_Ultimo_Informe_Rendimiento_RAG.pdf`: Último informe consolidado de la rama `release`.
- `ClassGPT_Informe_YYYYMMDD_HHMMSS.pdf`: Histórico inmutable para trazabilidad temporal entre sprints.
