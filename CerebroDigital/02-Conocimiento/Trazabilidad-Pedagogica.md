---
fecha: 2026-05-12
hora: 19:30
fuente: claude-code (conceptos)
tags: [trazabilidad, arquitectura, educacion, ia, plataforma-educativa, audit, event-sourcing]
proyecto-relacionado: Plataforma-Educativa-IA
---

# Trazabilidad Pedagógica — Modelo Conceptual

Nota de pilar para [[Plataforma-Educativa-IA]]. Esto **no es** logs. Es producto.

## La trampa común

> "Trazabilidad = logs."

**Falso.** Logs son una vista técnica de eventos. Trazabilidad **pedagógica** es un *producto* dirigido a múltiples audiencias. Si la confundís con logging, terminás con un archivo gigante que no le sirve a nadie.

## Las 4 capas (cada una con su audiencia)

| Capa | Audiencia | Qué captura | Granularidad | Retención |
|------|-----------|-------------|--------------|-----------|
| **1. Audit log** | Legal / Operaciones | quién, qué, cuándo, IP, sesión | Por evento | Larga (años) |
| **2. Trayectoria de razonamiento** | Vos (producto) + alumno | El camino de no-saber a saber. Preguntas hechas, movimientos socráticos disparados, momento del insight | Por turno de diálogo | Media |
| **3. Integridad académica** | Profesor | Evidencia exportable: ¿este alumno entendió o solo copió? | Por entregable / consulta | Larga (semestre) |
| **4. Telemetría del tutor** | Vos (mejora del producto) | Qué movimientos [[Tutor-Socratico\|socráticos]] funcionan, qué patrones llevan al insight, qué prompts aburren | Agregada, anonimizada | Larga |

**Punto crítico**: cada capa tiene **formato diferente, retención diferente, control de acceso diferente, exportabilidad diferente.** Si las mezclás → caos.

## Modelo conceptual: el evento como unidad

Pensá la trazabilidad como **stream de eventos** inmutables:

```
EVENT {
  id, timestamp, alumno_id, sesion_id, materia_id,
  tipo: "consulta" | "movimiento_socratico" | "respuesta_alumno" | "insight_detectado" | "abandono",
  payload: { ... datos específicos del tipo },
  contexto: { dispositivo, ubicación si aplica, etc. }
}
```

Cada capa de arriba es **una proyección** sobre este stream:
- Audit log = el stream tal cual + filtros legales
- Trayectoria = stream filtrado por `sesion_id` + ordenado
- Integridad = stream agregado por alumno + entregable
- Telemetría = stream anonimizado + agregaciones

**Esto es event sourcing en su forma natural** para tu dominio.

## La decisión arquitectónica grande

Tenés **12 microservicios**. ¿Dónde vive la trazabilidad?

### Opción A — Servicio centralizado de trazabilidad

Los 11 servicios publican eventos a un servicio dedicado (cola / event bus → servicio que persiste).

**Pros**:
- Una sola fuente de verdad
- Las 4 vistas se construyen sobre el mismo stream
- Más fácil cumplir compliance (un solo lugar audita)

**Contras**:
- Acoplamiento por contrato de eventos (cambiar el shape rompe cosas)
- Punto único de falla en captura
- Si el bus se cae, perdés trazabilidad (¿o blockeás el servicio?)

### Opción B — Cada servicio loggea su propio stream, trazabilidad consume

Cada servicio escribe eventos a su propio store. El servicio de trazabilidad **lee y reproyecta**.

**Pros**:
- Servicios desacoplados
- Si trazabilidad se cae, el sistema sigue
- Cada equipo dueño de sus eventos

**Contras**:
- Reproyección compleja (correlacionar streams de 11 servicios)
- Inconsistencias temporales (eventos llegan tarde, fuera de orden)
- Cada servicio reinventa "cómo logueo"

### Opción C (recomendada) — Híbrida

Bus de eventos compartido para los **eventos pedagógicos** (los que la trazabilidad necesita) + logs locales para lo operacional/debug de cada servicio.

El contrato de eventos pedagógicos es **schema versionado, gobernado**, no libre.

## Privacidad y compliance — no opcional

Estás trabajando con **alumnos universitarios** (mayoría adultos, pero atención a menores en posgrados intensivos / cursos abiertos):

- Ley argentina: **Ley 25.326** de Protección de Datos Personales
- Si alguna facultad tiene convenios europeos: **GDPR**
- Datos sensibles: si registrás "el alumno no entiende X" → eso es **rendimiento académico** = dato sensible
- Derecho de portabilidad: el alumno puede pedir su data
- Derecho al olvido: el alumno puede pedir borrado

**Decisiones que esto fuerza**:
- Anonimización vs pseudonimización: ¿usás `alumno_id` UUID y tabla aparte con PII? Sí, siempre.
- Retención por capa: telemetría puede ser indefinida si está anonimizada; audit log tiene tope legal; trayectoria privada del alumno la debe poder borrar.
- Export en formato estándar (JSON / CSV portable).

## Métricas que se desbloquean

Si la trazabilidad está bien diseñada, gratis te da:

- **Para la facultad**: % de alumnos que llegaron al insight vs. % que abandonaron. Por materia, por profesor, por cohorte.
- **Para el profesor**: ranking de alumnos por **profundidad** de uso (no por volumen). Detección de copy-paste por patrones de diálogo.
- **Para vos (producto)**: qué movimientos socráticos generan más insights. Qué temas requieren más turnos para que el alumno entienda.
- **Para el alumno**: su propia trayectoria de aprendizaje visible.

## Preguntas abiertas para resolver

- [ ] ¿Servicio centralizado o híbrido? Decisión gorda — define cómo escalás
- [ ] ¿Qué event bus? (Kafka / RabbitMQ / NATS / Redis Streams)
- [ ] Schema de eventos: ¿Protobuf, Avro, JSON Schema? Versionado obligatorio.
- [ ] ¿Qué se considera "insight detectado"? Esto es la pregunta más sutil — requiere conversar con pedagogos
- [ ] ¿Cómo se exporta evidencia para el profesor? Formato, frecuencia, qué se incluye
- [ ] Política de retención por capa — definir números concretos
- [ ] ¿Dashboard de telemetría usa qué stack? (Grafana, Metabase, custom)

## Conexión con el resto del cerebro

- [[Plataforma-Educativa-IA]] — proyecto donde esto vive
- [[Tutor-Socratico]] — la otra pata conceptual (a escribir cuando profundicemos)
- [[Stack-Tecnologico-Lenguajes]] — probable Python para pipelines + Go si hay throughput crítico
- [[Filosofia-de-Trabajo]] — esto ES "el humano sigue liderando, no la IA" hecho infraestructura
- [[Workflows-SDD-OPSX]] — antes de tocar código de trazabilidad, hay que tener: especificación técnica, estrategia, trade-offs, roadmap. Justo lo que vos siempre exigís
