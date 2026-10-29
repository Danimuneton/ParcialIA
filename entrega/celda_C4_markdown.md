**C.4 — Tabla de fidelidad y análisis.**

La **Faithfulness** = (afirmaciones respaldadas por el contexto) / (afirmaciones totales). Se descompone cada respuesta real generada por `nvidia/nemotron-3.5-bolt-e4b` frente al contexto recuperado con `retrieve` (3 chunks).

**Caso 1 — fuera del corpus:** *"¿Cuál es el costo mensual del parqueadero para motocicletas?"*
Respuesta del modelo: **"no la tengo"**.

| # | Afirmación | ¿Respaldada por el contexto? |
|---|---|---|
| 1 | No tiene la información en el contexto (abstención) | ✅ Sí — no afirma ningún hecho externo |

**Faithfulness = 1/1 = 1.0.** El modelo **se abstuvo** (comportamiento correcto): no inventó tarifa alguna. Contrasta con el caso de `faithfulness_cases.json`, donde un modelo alucinaba "$45.000 en Tesorería" (Faithfulness 0).

**Caso 2 — sigla:** *"¿Qué es el SIA?"* (contexto: doc R-02).
Respuesta: *"El Sistema de Información Académica (SIA) es la plataforma oficial donde el estudiante consulta su horario, inscribe asignaturas, revisa sus calificaciones y descarga certificados."*

| # | Afirmación | ¿Respaldada? |
|---|---|---|
| 1 | SIA = Sistema de Información Académica | ✅ Sí |
| 2 | Es la plataforma oficial | ✅ Sí |
| 3 | El estudiante consulta su horario | ✅ Sí |
| 4 | Inscribe asignaturas | ✅ Sí |
| 5 | Revisa calificaciones | ✅ Sí |
| 6 | Descarga certificados | ✅ Sí |

**Faithfulness = 6/6 = 1.0** (prácticamente una cita del documento R-02).

**Caso 3 — paráfrasis:** *"¿Hay una pausa de varios días en medio del curso?"* (contexto: doc R-12).
Respuesta: *"A mitad de cada período se programa una semana de receso académico de cinco días hábiles sin actividades académicas ni evaluaciones."*

| # | Afirmación | ¿Respaldada? |
|---|---|---|
| 1 | A mitad de cada período hay una semana de receso académico | ✅ Sí |
| 2 | Dura cinco días hábiles | ✅ Sí |
| 3 | Sin actividades académicas ni evaluaciones | ✅ Sí |

**Faithfulness = 3/3 = 1.0.** Nótese que la pregunta es una **paráfrasis** ("pausa de varios días" ≠ "receso académico") y, aun así, la búsqueda vectorial recuperó el documento correcto (R-12) y el modelo respondió fiel al contexto.

**Análisis del caso fuera del corpus (2 pts).** El modelo **se abstuvo** en lugar de alucinar. La parte decisiva del prompt es la instrucción de abstención explícita: *"Responde ÚNICAMENTE con el contexto… Si la información no está en el contexto, di explícitamente que no la tienes y no inventes."* Esto confirma que un prompt con restricción estricta al contexto previene alucinaciones (Faithfulness alta), a diferencia del caso del respaldo donde la respuesta rellenaba el hueco con datos inventados.
