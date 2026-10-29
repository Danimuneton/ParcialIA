## SECCIÓN D — Declaración de uso de IA y reproducibilidad (5 pts)

**1. Herramienta(s) de IA usadas y 2 prompts clave.**
- Herramienta: asistente de IA (modelo de lenguaje) para redactar y depurar el código del notebook.
- Prompt clave 1: *"Implementa IoU y NMS greedy en Python puro y valida con assert que IoU((2,2,6,6),(4,4,8,8)) ≈ 0.143."*
- Prompt clave 2: *"Escribe la atención causal de una cabeza desde cero y verifica que coincide con F.scaled_dot_product_attention(..., is_causal=True)."*

**2. Proveedor, modelo de chat y modelo de embeddings efectivamente usados.**
- Proveedor de chat: **NVIDIA NIM** (`https://integrate.api.nvidia.com/v1`, SDK `openai`), modelo **`nvidia/nemotron-3.5-bolt-e4b`** (confirmado con un ping real; la celda C.0.1 lo seleccionó automáticamente). En C.4 este modelo se abstuvo correctamente ante la pregunta fuera del corpus (no alucinó).
- Modelo de embeddings: **`sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`** (oficial, según `embedding_info.json`), 384 dimensiones, vectores normalizados. El LLM de chat es independiente del modelo de embeddings (regla R3).

**3. Un caso en que la IA produjo algo incorrecto y cómo se corrigió (2 pts).**
Al configurar el proveedor, el nombre de modelo inicial (`meta/llama-3.1-8b-instruct`) ya no existía en el catálogo de NVIDIA y la llamada devolvía **error 404 ("not found for account")**. Se detectó al ejecutar la celda de *ping* de C.0.1, y se corrigió haciendo que el notebook **pruebe varios modelos con una llamada real** y se quede con el primero que responde (`nvidia/nemotron-3.5-bolt-e4b`), con caída automática al respaldo `faithfulness_cases.json` si ninguno funciona.

**4. Reproducibilidad.**
- Semilla: **SEED = 1357** (últimos 4 dígitos del documento), con `D = 7` (define el subconjunto de 10 imágenes de la Sección A.3). Fijada en `random`, `numpy` y `torch`.
- Versiones de librerías clave: ejecute la celda de versiones (`torch`, `transformers`, `sentence-transformers`, `faiss-cpu`, `openai`, `rank-bm25`, `tiktoken`) y pegue aquí la salida.
- El notebook corre de principio a fin sin errores (*Entorno de ejecución → Reiniciar y ejecutar todo*): **sí**.
