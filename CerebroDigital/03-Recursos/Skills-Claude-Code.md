---
fecha: 2026-05-12
hora: 18:30
fuente: claude-code (sintesis CLAUDE.md)
tags: [claude-code, skills, automatizacion]
---

# Skills de Claude Code — Auto-carga

Skills que Claude Code DEBE leer **antes** de escribir código, según el contexto detectado.

## Tabla de carga automática

| Contexto detectado | Archivo a leer |
|--------------------|----------------|
| Tests en Go, testing de Bubbletea TUI | `~/.claude/skills/go-testing/SKILL.md` |
| Creación de skills nuevas para IA | `~/.claude/skills/skill-creator/SKILL.md` |
| **Cualquier trabajo de UI** (diseño frontend, redesign, review UX/UI, polish, audit, animaciones, layout, tipografía, color, componentes, dashboards, landing pages) | `~/.claude/skills/impeccable/SKILL.md` |

## Reglas de aplicación

- Las skills se leen **antes** de escribir código, no después
- Se aplican **TODOS** los patrones de la skill
- Múltiples skills pueden aplicar simultáneamente
- La carga es **automática** por contexto — no hay que pedirla

## Skill especial: Impeccable

Tiene **gates obligatorios**:
- `PRODUCT.md`
- `DESIGN.md`
- Shape brief

**No se toca código** hasta que los gates pasen. Si los archivos de contexto no existen → correr `impeccable teach` PRIMERO. **No se saltea.**

---

Ver [[Reglas-Colaboracion-IA]] para el marco general de cómo trabajar con la IA.
