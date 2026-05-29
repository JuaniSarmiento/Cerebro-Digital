---
fecha: 2026-05-18
hora: 12:10
fuente: claude-code (sesion-conversacional)
tags: [inbox, sin-revisar, proyecto, plataforma-educativa, tesis, reframe, correccion-vault]
---

# Plataforma-Educativa-IA = AI-Native N4 = tesis doctoral Cortez (UNSL)

Juani confirmó: **son el mismo sistema, dos nombres distintos en el [[CerebroDigital|vault]].**

## Identidad real del proyecto

- **Nombres usados**: "[[Plataforma-Educativa-IA|Plataforma Educativa con IA]]" (en `Plataforma-Educativa-IA.md`) y "AI-Native N4" (en log del 14)
- **Naturaleza real**: materialización tecnológica de la **tesis doctoral** de [[Alberto|Alberto Cortez]] en UNSL
- **Título tesis**: _"Modelo AI-Native con Trazabilidad Cognitiva N4 para la Formación en Programación Universitaria"_
- **Stack real (no 12 microservicios genéricos)**: 11 servicios Python (FastAPI + SQLAlchemy 2.0 async + Alembic) + 3 frontends React 19 (web-admin / web-teacher / web-student) + 4 DBs Postgres + Redis + Keycloak
- **Working dir**: `/home/juanisarmiento/ProyectosFacultad/juani4/AI-NativeV3-main/`
- **Piloto académico real**: UNSL, 18 estudiantes, 106 classifications N4 (delegación pasiva / superficial / reflexiva), tutor socrático con CTR (Cognitive Trace Record SHA-256 append-only)
- **Doctrina pedagógica**: PID Línea 5 (UTN-FRM × UTN-FRSN)

## Implicancias para reframe del vault

### 1. El driver real no es comercial

El proyecto se está construyendo porque:
- Alberto Cortez tiene que **defender una tesis doctoral**
- Hay un **PID con financiamiento académico** detrás (Línea 5, UTN-FRM × UTN-FRSN)
- El **piloto en UNSL** es ciencia reproducible, no go-to-market

El framing comercial ("escalar plataforma a 2da facultad para llegar a $2.5M") es **fork posible**, no la finalidad declarada del proyecto. Eso debilita uno de los pilares del [[Objetivos-12-Meses|plan económico]].

### 2. Hay TRES instituciones en juego, no una

| Institución | Rol |
|-------------|-----|
| **UTN Mendoza (FRM)** | Carrera de Juani como alumno + bloque grande del Día Tipo + UTN-FRM del PID |
| **UNSL** | Doctorado de Cortez + piloto académico real corre acá |
| **[[Formacion-Academica-UTN|TUPAD]] (UTN a distancia)** | Donde Juani enseña (Prog 1, Prog 4 desde agosto) |

El vault solo menciona UTN. UNSL **no aparece en `Sobre-Mi.md` ni en ninguna otra parte hasta hoy**.

### 3. Rol de Juani

Según el log del 14: "Yo estoy colaborando en el desarrollo (figuro en **agradecimientos** junto a Roberti, Martínez, Naveda, Robledo)".

Esto significa:
- Implementador técnico clave, **no co-autor**
- Crédito académico = agradecimientos (no autoría, no paper firmado)
- Trabajo a cambio de promoción académica + posible pago monetario (no aclarado)

A confirmar: si Juani tiene algún paper / publicación firmada propia derivada del trabajo en N4.

### 4. Reconciliación de archivos pendiente

- `Plataforma-Educativa-IA.md` y `2026-05-14-refactor-ai-native-ejercicios-reusables.md` describen el mismo proyecto **sin linkearse**
- Hay que fusionar la identidad y agregar los datos reales del stack (11 servicios + 3 frontends + 4 DBs, no "12 microservicios genéricos")
- El nombre canónico a decidir: "Plataforma Educativa IA" (PARA/comercial) vs "AI-Native N4" (PARA/académico) — probablemente el archivo debería llamarse según el contexto real (tesis)

## Pendiente de confirmar con Juani

- ¿Tu rol académico es solo "agradecimientos" o tenés algún paper / autoría firmada?
- ¿Existe un fork comercial planificado o todo apunta a la defensa académica?
- ¿Qué pasa cuando Cortez defienda y/o vos te recibas? — pregunta marcada como crítica en `Alberto.md`
