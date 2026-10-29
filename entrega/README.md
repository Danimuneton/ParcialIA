# SI3003 · Evaluación 2 · Parte II (aplicada) — Entrega

Solución completa de la Parte II (aplicada) del parcial de Inteligencia Artificial (SI3003, EAFIT,
Prof. Jorge Iván Padilla Buriticá). Resuelve las cuatro secciones del enunciado (100 pts):

| Sección | Tema | Puntos |
|---|---|---|
| A | CNN y detección de objetos con YOLO (conv_out, red de la Parte I, comparación scratch/pretrained/freeze, IoU/NMS, métricas) | 33 |
| B | Transformers y NLP (atención causal desde cero, generación autorregresiva con GPT-2, tokenización) | 30 |
| C | RAG (compatibilidad de embeddings, búsqueda vectorial, BM25 + híbrida RRF, HNSW/FAISS, RAG end-to-end + faithfulness) | 32 |
| D | Declaración de uso de IA y reproducibilidad | 5 |

## Archivos

- **`Eval2_Apellido_Nombre.ipynb`** — Notebook plantilla, listo para ejecutar. **Antes de entregar,
  reemplace** `STUDENT_NAME`, `STUDENT_ID` (su documento, solo dígitos) y `GROUP` en la celda de
  *Datos*. De ahí se derivan `SEED` (últimos 4 dígitos) y `D` (último dígito), que fijan la semilla
  global y el subconjunto de 10 imágenes de la Sección A.3.
- **`Eval2_Apellido_Nombre_EJECUTADO.ipynb`** — El mismo notebook ya **ejecutado de arriba hacia
  abajo** con las salidas visibles (gráficas, tablas, asserts), usando un documento de prueba
  (`1037123456` → SEED=3456, D=6). Sirve como referencia de que todo corre sin errores; para la
  entrega real, ejecute la plantilla con *su* documento.
- **`datos_examen/`** — Archivos de datos del examen (YOLO, corpus, embeddings oficiales, etc.).

## Cómo ejecutar

1. Abra `Eval2_Apellido_Nombre.ipynb` (Colab o Jupyter local).
2. Edite la celda de *Datos* con su nombre, documento y grupo.
3. (Opcional) Para la Sección C.4 con un LLM real, configure una clave de proveedor
   (`NVIDIA_API_KEY`, `GROQ_API_KEY` o `HF_TOKEN`) en Colab Secrets o variable de entorno, y ajuste
   `PROVEEDOR`/`MODELO_CHAT`. Si no hay clave, C.4 usa automáticamente el respaldo
   `faithfulness_cases.json` (el ítem vale máx. 3/5, según el enunciado).
4. *Reiniciar y ejecutar todo.*

## Resultados verificados (con el documento de prueba)

- **A.1** `conv_out` coincide con `torch.nn.Conv2d` en 3 configuraciones; red de la Parte I = 1562
  parámetros (igual al cálculo a mano).
- **A.2** Orden de desempeño: **pretrained (mAP50≈0.66) > freeze (≈0.56) ≫ scratch (≈0.01)**;
  freeze es el más rápido.
- **A.3** IoU((2,2,6,6),(4,4,8,8)) = 0.1429; NMS por clase reduce las cajas; al subir conf de 0.25
  a 0.50 la **precision sube** (≈0.80→0.87) y el **recall baja** (≈0.78→0.59).
- **B.1** La atención causal coincide con `F.scaled_dot_product_attention(..., is_causal=True)`; el
  ejemplo de la Parte I da pesos [0.665, 0.245, 0.090] y salida ≈[0.755, 0.335].
- **B.2** La temperatura baja (0.2) agudiza la distribución (prob_top1≈0.99, entropía≈0.11); la alta
  (2.0) la aplana (prob_top1≈0.01, entropía≈13.7).
- **B.3** tokens/carácter EN (0.17) < ES (0.23) < código (0.44); un emoji ocupa 3 tokens.
- **C.0** Recálculo con el modelo oficial: coseno = 1.00 (compatible); otro modelo 384-d: coseno ≈
  0.42 (incompatible, R1/R2).
- **C.1** Recall@3 = 0.917, MRR = 0.836.
- **C.2** Paráfrasis: la vectorial supera a BM25; siglas: empatan o la vectorial gana (se explica
  por qué).
- **C.3** El recall@10 de HNSW sube con `efSearch` (vectores aleatorios = caso difícil).
- **C.4** Analiza faithfulness y el caso fuera del corpus (abstención vs. alucinación).

> **Importante:** ninguna celda ni salida muestra una API key; las claves se leen con `get_secret()`
> desde Colab Secrets o variables de entorno.
