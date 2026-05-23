---
fecha: 2026-05-12
hora: 19:10
fuente: usuario
tags: [proyecto, ia, educacion, microservicios, fastapi, foco-actual]
estado: activo
prioridad: maxima
---

# Plataforma Educativa con IA

**Foco actual de trabajo.** Todo lo demás está en pausa.

## Origen y modelo de colaboración

> **Alberto te mandó a hacerla**. La desarrollan **juntos**.

Esto no es proyecto personal — es **encargo institucional** del [[Personas/Alberto|jefe de la carrera de programación de UTN]]. Sos el implementador técnico; él es el sponsor/co-creator y representa al cliente institucional natural (UTN).

**Implicancia gigante**: UTN como primer adopter de la plataforma **no es venta a cerrar** — es la realidad operativa actual. La pregunta no es "¿cómo conseguimos el primer cliente?" sino **"¿cómo replicamos a otras facultades?"**.

## Qué es

Plataforma para **facultades** que permite a los alumnos usar IA **de forma buena**.

Núcleo del producto:
- **Tutor socrático** — la IA no da respuestas, hace preguntas que llevan al alumno a pensar
- **Trazabilidad** — registro auditable de cómo el alumno usó la IA

## Arquitectura

- **Microservicios**: 12 servicios
- **Stack**: Python + FastAPI (todo el ecosistema)
- Encaja con [[Stack-Tecnologico-Lenguajes|Python para el lado IA]]

## Por qué este proyecto cristaliza tu identidad

Mirá cómo se alinea con todo el cerebro digital:

| Pieza | Cómo aparece en la plataforma |
|-------|-------------------------------|
| [[Filosofia-de-Trabajo|Conceptos > Código]] | Tutor socrático = obligar a entender antes de copiar |
| [[Filosofia-de-Trabajo\|Contra el inmediatismo]] | El alumno no obtiene atajos, obtiene preguntas |
| [[Filosofia-de-Trabajo\|IA como herramienta dirigida]] | Trazabilidad = el humano sigue liderando, no la IA |
| [[Formacion-Academica-UTN\|UTN + tutor de Python/FastAPI]] | Dominio del problema desde adentro |
| [[Perfil-Profesional\|Project Orchestrator]] | 12 microservicios → orquestación pura |

Este no es "un proyecto más". Es **el proyecto donde toda tu filosofía cristaliza en código**.

## Cliente institucional natural — UTN (CONFIRMADO)

[[Personas/Alberto|Alberto]] = jefe de la carrera + co-creator. UTN es **adopter natural confirmado** del producto.

Implicancias:
- Validación pedagógica desde adentro (Alberto)
- [[Trazabilidad-Pedagogica]] aceptada de origen por el dominio
- Cero fricción de venta para el primer caso

**Esta es la palanca grande para el [[Objetivos-12-Meses|objetivo de $2.5M/mes a 12 meses]]** — pero la palanca ahora es **escala a otras facultades**, no "cerrar el primero".

## Equipo

**Dos personas**: Juani + [[Personas/Alberto|Alberto]].

División de código: **fluida**. *"A veces lo desarrolla él, a veces lo desarrollo yo."* No hay separación rígida frontend/backend ni "tu zona / mi zona" — ambos tocan el código del producto.

Esto significa:
- Velocidad alta cuando los dos están alineados
- Riesgo cuando uno toca un servicio que el otro no documentó
- Bus factor = 2 (riesgo bajo pero existe)
- Sin gestión de equipo formal — comunicación directa

## Notas abiertas (preguntas pendientes)

- ¿Estado actual exacto? (discovery / build / piloto / producción)
- ¿Qué facultades planeadas además de UTN?
- ¿Convenciones de código compartidas? ¿Code review entre vos y Alberto?
- ¿Modelo de negocio definido para escalar fuera de UTN?
- ¿Por qué 12 microservicios? Decisión arquitectónica grande — ¿separación por dominio, por equipo, por escalado?
- ¿Modelo de monetización? (B2B con la facultad, B2C alumno, freemium)
- ¿Cuál es el servicio que más te complica hoy?
- ¿Qué LLM(s) en backend? ¿Tema costos por uso?
- ¿Cómo se materializa la "trazabilidad"? (logs, dashboard, exportable para profesores)
- ¿Tema de privacidad / datos de menores?
