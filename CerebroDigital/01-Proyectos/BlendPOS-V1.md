---
fecha: 2026-05-12
hora: 19:10
fuente: usuario
tags: [proyecto, blend, pos, produccion, go, fastapi, afip]
estado: produccion
empresa: Blend-Software
---

# BlendPOS V1

**Producto en producción.** Punto de orgullo técnico de [[Blend-Software]].

## Qué es

POS (Point of Sale) **completo**.

## Arquitectura (confirma la dualidad de lenguajes)

- **Core**: [[Stack-Tecnologico-Lenguajes|Go]] — performance, concurrencia
- **Worker AFIP**: Python — porque las librerías del fisco argentino viven en Python

Esto **valida la decisión** de stack: Go donde manda performance, Python donde el ecosistema gana.

## Estado

En **producción real**, con clientes corriendo. No es prototipo.

## Relación con [[BlendPOS-V2]]

V2 era la evolución hacia SaaS multi-tenant. **Hoy está pausada** — V1 sigue siendo la fuente de revenue.

## Notas abiertas

- ¿Cuántos clientes en producción?
- ¿Modelo de cobro?
- ¿Soporte técnico — cómo está organizado?
- ¿Qué lecciones del V1 quedaron documentadas para cuando se retome V2?
- Worker AFIP: ¿qué librerías Python? (`afip.py`, `pyafipws`, custom?)
- ¿Hay dolor operativo recurrente?
