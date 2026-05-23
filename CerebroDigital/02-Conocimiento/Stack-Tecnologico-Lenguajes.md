---
fecha: 2026-05-12
hora: 18:50
fuente: usuario
tags: [stack, lenguajes, go, python, backend]
---

# Stack Tecnológico — Lenguajes

Decisión deliberada de **dos lenguajes**, cada uno por su rol.

## Go (Golang)

**Para qué**: alto rendimiento y concurrencia.

Encaja con:
- Backend de [[BlendPOS-V2]] (multi-tenant a escala)
- Cálculo intensivo de [[BlendPadel]] (ELO, ranking)
- Ingesta de [[Aerotec]] (throughput de sensores)

## Python (FastAPI)

**Para qué**: pegamento para el ecosistema de **Inteligencia Artificial**.

Encaja con:
- [[PreBot]] (RAG, embeddings, LLM)
- [[Jarvis]] (LangGraph, MCP, agentes)
- Cualquier integración con librerías ML/AI maduras del ecosistema Python

## Por qué dos y no uno

Go es excelente para sistemas pero el ecosistema de IA en Python tiene **años de ventaja** en librerías. Forzar Python a hacer high-perf concurrente es subóptimo; forzar Go a hacer IA es reinventar la rueda.

División de responsabilidades:
- **Go** = servicios de plataforma, APIs públicas, motores de cálculo
- **Python (FastAPI)** = capa de IA, agentes, integraciones ML

---

Conecta con [[Expertise-Tecnica]] (arquitecturas) y [[Workflows-SDD-OPSX]] (metodología).
