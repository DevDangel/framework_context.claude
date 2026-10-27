# Protocolo de Arquitectura: Context Engine (Claude CLI)

Este documento define el protocolo operativo estricto para la interacción entre el desarrollador y Claude. El objetivo es minimizar el consumo de tokens en consola, forzar el diseño técnico previo al código y mantener un histórico consolidado.

---

## 1. Topología del Framework

```text
context.claude/ (Ruta del Framework de Contexto)
├── prompt.init.md          # Bootstrap para pegar en el CLI de tu proyecto
├── architecture.md         # Este protocolo operativo
├── DOC.MD                  # Documentación humana del flujo
├── context/
│   ├── draft_context.md    # Requerimiento inicial en borrador
│   └── 00X_context.md      # Plantillas de feedback y ajustes del dev (temporales)
├── response/
│   └── 00X_response.md     # Propuesta técnica acumulada generada por Claude (temporales)
└── librarie_sessions/
    └── session_DD_MM_YYYY.md # Resumen técnico consolidado post-desarrollo (permanente)
```

---

## 2. Regla de Ubicación y Ámbitos de Operación

Claude trabaja con dos ubicaciones naturales:

1. **Directorio del Proyecto Actual (Working Directory):**
   - Es la carpeta donde el desarrollador tiene abierta la terminal de Claude CLI.
   - Claude no necesita que le indiquen esta ruta porque ya está ejecutándose en ella.
   - Aquí se inspecciona el código base, se ejecutan comandos y se implementan los cambios tras el `Accept`.

2. **Directorio del Framework de Contexto (`context.claude`):**
   - Es la ruta absoluta configurada en `prompt.init.md` por quien clone el framework.
   - Claude acude a esta ruta para leer `context/draft_context.md`, generar `response/00X_response.md`, recibir feedback en `context/00X_context.md` y guardar el resumen final en `librarie_sessions/`.

---

## 3. Regla de Oro de la Consola (CLI Output)

> [!IMPORTANT]
> Claude **NUNCA** debe redactar explicaciones, código, análisis ni respuestas conversacionales en el chat/CLI.
> La consola se reserva exclusivamente para señales de control de **una sola línea** con prefijo: `[STATUS]`, `[OK]`, `[WORKING]`, `[ERROR]` o `[DONE]`.
> Todo el intercambio técnico ocurre a través de los archivos Markdown en el directorio de contexto.

---

## 4. Máquina de Estados y Ciclo de Vida

### Fase 0: Inicialización
- El desarrollador abre Claude CLI en la carpeta de su proyecto real.
- Pega el contenido de `prompt.init.md`.
- Claude lee `architecture.md` en la ruta de contexto indicada y se prepara.
- **Salida CLI obligatoria:**
  `[OK] Framework context cargado exitosamente. Llena context/draft_context.md y escribe 'start' para comenzar.`

---

### Fase 1: Análisis del Requerimiento Inicial (`start` o `continue`)
1. El desarrollador escribe `start` (o `continue`) en el CLI tras redactar su requerimiento.
2. Claude va a la carpeta de contexto y lee `context/draft_context.md`.
3. Claude analiza el proyecto actual donde está abierta la terminal para entender código existente y dependencias.
4. Claude genera en la carpeta del framework:
   - `response/001_response.md`: 
     - Diagnóstico del código actual.
     - Propuesta de arquitectura y lista de archivos a tocar en el proyecto.
     - Plan de ejecución paso a paso.
     - Dudas técnicas o decisiones abiertas.
   - `context/001_context.md`: Plantilla lista para feedback:
     ```markdown
     # Feedback sobre 001_response
     - [ ] Puntos aprobados:
     - [ ] Cambios solicitados:
     - [ ] Respuestas a dudas técnicas:
     ```
5. **Salida CLI obligatoria:**
   `[OK] 001_response.md generado. Completa context/001_context.md y escribe 'continue' o 'Accept'.`

---

### Fase 2: Iteración y Refinamiento (`continue`)
1. El desarrollador completa `context/00N_context.md` y escribe `continue` en el CLI.
2. Claude identifica el índice consecutivo más alto ($N$) en la carpeta de contexto.
3. Claude lee `context/00N_context.md` y genera:
   - `response/00(N+1)_response.md`: **Especificación técnica acumulada y consolidada (SSOT)**.
   - `context/00(N+1)_context.md`: Nueva plantilla para feedback de esa versión.
4. **Salida CLI obligatoria:**
   `[OK] 00(N+1)_response.md generado. Esperando en context/00(N+1)_context.md. Escribe 'continue' o 'Accept'.`

---

### Fase 3: Aprobación y Desarrollo (`Accept`)
1. El desarrollador escribe `Accept` en el CLI.
2. Claude toma el último `response/00X_response.md` como contrato final inmutable.
3. Claude notifica inicio de trabajo en CLI:
   `[WORKING] Requerimiento aceptado. Iniciando desarrollo en el proyecto actual...`
4. Claude implementa los cambios directamente en los archivos del proyecto donde está corriendo el CLI.

---

### Fase 4: Finalización y Limpieza Automática
Una vez concluido y verificado el desarrollo en el proyecto actual:
1. **Crear resumen permanente:** Genera `librarie_sessions/session_DD_MM_YYYY.md` en la carpeta del framework con:
   - **Requerimiento:** Nombre y objetivo del cambio.
   - **Proyecto:** Nombre de la carpeta del proyecto actual.
   - **Resumen Técnico:** Síntesis concisa de la solución (máximo 10 líneas).
   - **Archivos Impactados:** Lista de archivos creados, modificados o eliminados.
   - **Decisiones Relevantes:** Cambios arquitectónicos o advertencias para el futuro.
2. **Limpieza de transitorios en el framework:**
   - Eliminar todos los archivos `context/00*_context.md`.
   - Eliminar todos los archivos `response/00*_response.md`.
   - Restaurar `context/draft_context.md` a su plantilla original vacía para el siguiente ciclo.
3. **Salida CLI obligatoria:**
   `[DONE] Desarrollo finalizado. Resumen guardado en librarie_sessions/session_DD_MM_YYYY.md. Entorno limpio.`
