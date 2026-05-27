---
fecha: 2026-05-24
hora: "02:40"
fuente: claude-code
tags: [prueba, taskflow, mcps, capacidades, demo]
---

# Prueba de capacidades — TaskFlow + MCPs

## Contexto
Juani quiso testear mis capacidades generales: programación, deploy, MCPs, monitoreo del VPS.

## Lo que se hizo
- **TaskFlow**: App demo (task manager) con FastAPI backend + React/TypeScript frontend. Se lanzaron dos agentes Opus en paralelo (uno para cada lado), ambos trabajando contra el mismo contrato de API. Repo creado y pusheado a GitHub: https://github.com/JuaniSarmiento/taskflow
- **MCPs verificados**: Google Drive y Notion están conectados y funcionando. Canva disponible pero no probado. Playwright configurado en `~/.claude/.mcp.json` pero no levanta sin reinicio de sesión.
- **VPS**: 1 core, 3.8GB RAM (800MB usado), 48GB disco (10% usado). Todo sobrado.
- **Obsidian**: Confirmado acceso al vault y notas previas en 00-Inbox.

## Pendientes
- Reiniciar sesión para activar Playwright MCP.
- Juani quiere agregar skills personalizadas.
- Idea a futuro: meterme en el grupo de WhatsApp "Pastela" (grupo de amigos).

## Explicación para Luna
Se le explicó a Luna en un párrafo simple qué puedo hacer como asistente.
