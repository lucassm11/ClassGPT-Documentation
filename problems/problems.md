# A tener en cuenta 

El servidor con flask no es concurrente, es decir las solicitudes se procesan de una en una, si varios le pedimos respuestas al RAG tardara en generarlas todas.