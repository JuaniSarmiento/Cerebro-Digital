---
fecha: 2026-05-12
hora: 18:50
fuente: usuario
tags: [proyecto, ia, asistente-personal, langgraph, mcp, agentes]
estado: pausado
---

# Jarvis

> [!warning] En pausa
> Foco actual está en [[Plataforma-Educativa-IA]]. Patrones de agentes que armaste acá pueden reusarse en el tutor socrático.

**Ecosistema de asistente de IA personal.**

## Stack

- **LangGraph** — para máquinas de estado de agentes
- **Protocolo MCP** (Model Context Protocol) — para integración con herramientas
- Arquitectura: **máquinas de estado + agentes con tool-calling**

## Por qué importa

Es la pieza que conecta todo: el "orchestrator" personal que puede tocar el resto del stack. Encaja con el rol [[Perfil-Profesional|Project Orchestrator]] llevado al extremo personal.

## Notas abiertas

- Lista canónica de herramientas que expone vía MCP
- Modelo de memoria propia (¿usa [[Sistema-Memoria-Engram]] como backend?)
- Surface de interfaz: CLI, voz, móvil
- Costos vs valor — relación con suscripciones [[Stack-Herramientas|Google AI Pro / Anthropic Max]]

## Relacionado

- [[Sistema-Memoria-Engram]] — patrón de memoria persistente afín
- [[PreBot]] — comparten conceptos de agente con tool-calling
