---
fecha: 2026-05-12
hora: 18:30
fuente: claude-code (sintesis CLAUDE.md)
tags: [engram, memoria, contexto, ia]
---

# Sistema de Memoria — Engram

Engram es el sistema de memoria persistente que sobrevive sesiones y compactaciones.

## Filosofía: razonar antes de guardar

**No** guardar después de cada tarea. **No** seguir checklists de triggers. Antes de cada `mem_save`:

1. ¿El yo-futuro arrancando frío realmente lo va a necesitar? Si la próxima sesión no sería medible peor sin esto → skip
2. ¿Se puede derivar del repo? Si `git log`, `git blame`, el código o `CLAUDE.md` ya lo dicen → skip. Memoria es para contexto que el repo NO captura
3. ¿Es estable? Estado volátil (rama actual, qué estoy debuggeando ahora mismo) va en tasks/plan, no en memoria. Guardar cosas que sigan siendo verdad la semana que viene
4. ¿Vale el costo de ruido? Cada observación guardada diluye búsquedas futuras. **Ante duda → no guardar**

**Calibración de frecuencia**: en una sesión normal esperar **0, 1 o tal vez 2** `mem_save`. Si guardo más, probablemente estoy guardando ruido.

## Buenos candidatos para guardar

- Gotchas no obvios que costaron debug real y volverían a morder
- Decisiones arquitectónicas donde lo importante es el **por qué** (el código muestra qué; la memoria guarda por qué)
- Preferencias y constraints duros no visibles en código ("nunca hagas X", "siempre Y")
- Continuidad entre sesiones para trabajo multi-día que genuinamente pausa
- Descubrimientos que el código activamente esconde ("parece X pero se comporta como Y por Z histórico")

## Qué NO guardar

- Cualquier cosa derivable de `git log` / `git blame` / leer el archivo
- Fixes rutinarios, refactors mecánicos, features normales
- "Lo que acabo de hacer este turno" — eso es el diff
- Cualquier cosa ya en `CLAUDE.md` o docs del proyecto
- Estado volátil

## Buscar es barato — usar libremente

Asimetría: buscar es bajo costo y alto valor. Guardar tiene costo de ruido. **Generoso con búsqueda, mezquino con guardado.**

Flujo:
1. `mem_context` primero (historia reciente, barato)
2. `mem_search` con keywords si no aparece
3. `mem_get_observation` para contenido completo (los resultados de search vienen truncados)

## Formato cuando se guarda

- **title**: Verbo + qué (ej: "Fixed N+1 query in UserList")
- **type**: `bugfix | decision | architecture | discovery | pattern | config | preference`
- **scope**: `project` (default) | `personal`
- **topic_key**: clave estable para tópicos que evolucionan (ej: `architecture/auth-model`)
- **content**: What / Why / Where / Learned

Mismo tópico evolucionando → reusar `topic_key` (upsert). Tópicos distintos NO deben sobreescribirse.

## Después de compactación

Si aparece marcador de compactación o "FIRST ACTION REQUIRED" → escribir `mem_session_summary` del contenido compactado **antes de seguir**. Reconstruir contexto cuesta mucho más que persistirlo una vez.

## Override por proyecto

El `CLAUDE.md` de un proyecto puede sobreescribir este protocolo. Siempre chequear `CLAUDE.md` del proyecto antes de guardar.

---

Conecta con [[Workflows-SDD-OPSX]] que usan Engram como artifact store por default.
