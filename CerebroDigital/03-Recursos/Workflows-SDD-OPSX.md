---
fecha: 2026-05-12
hora: 18:30
fuente: claude-code (sintesis CLAUDE.md)
tags: [workflow, sdd, opsx, claude-code, agentes]
---

# Workflows — SDD y OPSX

Dos flujos de **desarrollo dirigido por especificación** que tengo configurados.

## SDD (Spec-Driven Development)

Capa de planificación estructurada para cambios sustanciales. Usa sub-agentes para delegar trabajo y proteger contexto.

### Grafo de dependencias

```
proposal → specs → tasks → apply → verify → archive
            ^
            |
          design
```

### Comandos como skills

- `/sdd-init` — inicializa contexto SDD; detecta stack, bootstrappea persistencia
- `/sdd-explore <topic>` — investiga una idea; no crea archivos
- `/sdd-apply [change]` — implementa tareas en batches
- `/sdd-verify [change]` — valida implementación contra specs
- `/sdd-archive [change]` — cierra el cambio
- `/sdd-onboard` — walkthrough end-to-end

### Meta-comandos

- `/sdd-new <change>` — arranca cambio nuevo (delega exploration + proposal)
- `/sdd-continue [change]` — corre la siguiente fase dependency-ready
- `/sdd-ff <name>` — fast-forward: proposal → specs → design → tasks

### Modos de ejecución

- **Automatic** (`auto`): todas las fases corren back-to-back
- **Interactive** (`interactive`): después de cada fase, mostrar resumen y preguntar antes de continuar

Default → **Interactive** (más seguro, da control).

### Artifact stores

- `engram` (default si disponible) — rápido, sin archivos
- `openspec` — archivos en `openspec/`, committeable, history en git
- `hybrid` — ambos, más costo de tokens
- `none` — inline only

## OPSX (alternativa liviana a OpenSpec)

Ejecución **inline**, sin sub-agentes, generación de artefactos en una pasada. Para cambios chicos/medianos donde quiero velocidad sobre orquestación.

### Comandos

- `/opsx:explore <topic>` — partner de pensamiento, no escribe código
- `/opsx:propose <name>` — `openspec new change` + genera todos los artefactos
- `/opsx:apply [name]` — implementa tareas pendientes una a una, marca `- [ ]` → `- [x]`
- `/opsx:archive [name]` — mueve cambio a `openspec/changes/archive/YYYY-MM-DD-<name>/`

### Reglas operativas

1. CLI es la fuente de verdad: `openspec status --change <name> --json`
2. `context` y `rules` de `openspec instructions` son **constraints para vos**, no contenido del artefacto
3. Leer artefactos dependientes **antes** de escribir uno nuevo
4. Si el nombre ya existe → preguntar, no pisar
5. **Nunca** auto-archivar — siempre pedir confirmación
6. `/opsx:apply` **pausa**, no adivina ante ambigüedad

## Cuándo SDD vs OPSX

| Elegí | Por qué |
|-------|---------|
| **OPSX** | Cambio chico/mediano, contexto único, prefiero velocidad sobre review per-fase |
| **SDD**  | Cambio grande, multi-batch, necesito sub-agentes para comprimir contexto, TDD estricto |

---

Conecta con [[Sistema-Memoria-Engram]] (artifact store default) y [[Reglas-Colaboracion-IA]] (cómo se comunica el agente durante estas fases).
