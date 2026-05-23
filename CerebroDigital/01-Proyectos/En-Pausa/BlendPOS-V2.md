---
fecha: 2026-05-12
hora: 18:50
fuente: usuario
tags: [proyecto, blend, pos, saas, multi-tenant]
estado: pausado
empresa: Blend-Software
---

# BlendPOS V2

> [!warning] En pausa
> Foco actual está en [[Plataforma-Educativa-IA]]. [[BlendPOS-V1]] sigue siendo el que genera revenue.

Evolución del sistema POS original hacia un modelo **SaaS multi-tenant**.

## Contexto

Producto core de [[Blend-Software]] junto a [[Franco-y-Lautaro]].

## Decisiones técnicas relevantes

- Modelo SaaS multi-tenant (definir: shared DB / DB por tenant / hybrid)
- Stack alineado con [[Stack-Tecnologico-Lenguajes|Go + Python/FastAPI]]
- Workflow [[Workflows-SDD-OPSX|SDD/OPSX]]

## Notas abiertas

- Estado actual: ¿discovery, design, build, beta?
- Tenancy: aislamiento de datos, billing, onboarding
- Roadmap de migración desde V1
