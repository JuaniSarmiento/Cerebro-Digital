---
fecha: 2026-05-12
hora: 18:50
fuente: usuario
tags: [proyecto, blend, padel, red-social, elo, matchmaking]
estado: pausado
empresa: Blend-Software
---

# BlendPadel

> [!warning] En pausa
> Foco actual está en [[Plataforma-Educativa-IA]].

Red social y sistema de **ranking deportivo** con matchmaking basado en **ELO** para pádel.

## Contexto

Producto de [[Blend-Software]].

## Núcleo del problema

- Ranking dinámico por jugador
- Matchmaking: emparejar jugadores con habilidad similar (ELO)
- Capa social: perfiles, partidos, comunidad

## Decisiones técnicas relevantes

- Algoritmo ELO (versión clásica vs. Glicko-2 — pendiente decidir)
- Backend: probable [[Stack-Tecnologico-Lenguajes|Go]] por performance del cálculo y concurrencia
- Frontend / mobile: pendiente definir

## Notas abiertas

- Mercado objetivo (Mendoza local primero / regional / nacional)
- Modelo de monetización
- Integración con clubes (reserva de canchas)
