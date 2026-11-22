# Informe de tests de ClassGPT

- **Fecha:** 2026-11-22 19:59 UTC
- **Rama y commit:** Sergio @ 009917c
- **Resultado:** TODOS LOS TESTS PASAN: 137 de 137 correctos en 3.2 s

Los tests son herméticos: no se conectan a MongoDB Atlas ni a las APIs de los modelos.

| Tarea | Qué se comprueba | Referencia | Tests | Fallan | Estado |
| --- | --- | --- | ---: | ---: | --- |
| A1 | Recuperación con y sin reranking | Core Sec. 18.7 · Acta Sec. 04 | 8 | 0 | OK |
| A2 | Abstención ante preguntas fuera del temario | Core Sec. 22 · Acta Sec. 04 | 6 | 0 | OK |
| A3 | Colección de chunks v2 con filtros | Core Sec. 16 y 17 · Acta Sec. 09 | 4 | 0 | OK |
| A4 | Auditoría de respuestas | Core Sec. 27.8 · Acta Sec. 05 | 6 | 0 | OK |
| Base | Errores de la API del LLM | Sprints 1 y 2 | 6 | 0 | OK |
| Base | IDL, crawler, parser de código y caché | Sprints 1 y 2 | 9 | 0 | OK |
| Base | Ingesta y citas del sistema actual | Sprints 1 y 2 | 3 | 0 | OK |
| Base | Paso de Markdown a HTML | Sprints 1 y 2 | 2 | 0 | OK |
| Evaluación | Generación y métricas del benchmark | Core Sec. 30 y 32 · Acta Sec. 08 | 15 | 0 | OK |
| Evaluación | Métricas de recuperación y generación | Core Sec. 30 · Acta Sec. 08 | 19 | 0 | OK |
| L1 | Contratos de datos y validador MIR | Core Sec. 11 y 16 · Acta Sec. 09 | 14 | 0 | OK |
| L2 | Conectores y adaptadores a MIR | Core Sec. 9 · Acta Sec. 01 | 9 | 0 | OK |
| L3 · L4 | Registro de fuentes, versiones e IDL oficial | Core Sec. 5, 7 y 8 · Acta Sec. 09 | 7 | 0 | OK |
| S1 | Fragmentación a partir del MIR | Core Sec. 14 | 29 | 0 | OK |
