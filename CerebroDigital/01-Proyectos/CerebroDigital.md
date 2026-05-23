---
fecha: 2026-05-12
hora: 18:30
fuente: claude-code
tags: [proyecto, pkm, obsidian, sistema-personal]
estado: activo
---

# CerebroDigital

Sistema personal de gestión de conocimiento. Bóveda Obsidian con estructura PARA + script de captura automática.

## Estructura

```
CerebroDigital/
├── 00-Inbox/         # Captura sin procesar
├── 01-Proyectos/     # Trabajo con outcome y deadline
├── 02-Conocimiento/  # Conceptos estables, evergreen
├── 03-Recursos/      # Herramientas, configs, referencias
└── 04-Plantillas/    # Templates reutilizables
```

## Captura

Hay un **script externo** (estado: "creo que funciona jaja") que escribe automáticamente en la bóveda. Ejemplo de output: `01-Proyectos/prueba.md` con frontmatter `fecha/hora/fuente/tags`.

## Convenciones

- Frontmatter YAML obligatorio: `fecha`, `hora`, `fuente`, `tags`
- Nombres de archivo en kebab-case o PascalCase descriptivo
- Wikilinks `[[Nota]]` para conectar conceptos
- Inbox → revisar y mover a la carpeta PARA correcta

## Notas seed

Ver [[Sobre-Mi]] como punto de entrada al mapa mental personal.

## Proyectos hermanos (paralelos en mi vida)

- [[Blend-Software]] y derivados ([[BlendPOS-V2]], [[BlendPadel]])
- [[Jarvis]] · [[PreBot]] · [[Aerotec]] · [[Buen-Bocado-Empanadas]]

## Próximos pasos

- [ ] Verificar que el script de captura escribe en `00-Inbox/` por default
- [ ] Definir ritual semanal de revisión de Inbox
- [ ] Decidir si agregar plugin Dataview para vistas dinámicas
- [ ] Mover `Sobre-Mi.md` desde `00-Inbox` a `02-Conocimiento` (es índice estable, no captura cruda)
