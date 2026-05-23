---
fecha: 2026-05-14
hora: 12:00
fuente: claude-code (juani4/AI-NativeV3-main)
tags: [proyecto, tesis-cortez, refactor, ai-native-n4, ejercicios, pid-utn, milestone, log-sesion]
---

# Refactor AI-Native N4 — Ejercicios como entidad de primera clase reusable

## El proyecto en una línea

[[Plataforma-Educativa-IA|AI-Native N4]] es la plataforma de la **tesis doctoral de Alberto Cortez (UNSL)** — _"Modelo AI-Native con Trazabilidad Cognitiva N4 para la Formación en Programación Universitaria"_. Yo estoy colaborando en el desarrollo (figuro en agradecimientos junto a Roberti, Martínez, Naveda, Robledo).

Es un piloto académico real corriendo en UNSL: 18 estudiantes, 106 classifications de apropiación cognitiva (delegación pasiva / superficial / reflexiva), tutor socrático con CTR (Cognitive Trace Record) como cadena criptográfica SHA-256 append-only.

**Stack**: monorepo híbrido — 11 servicios Python (FastAPI + SQLAlchemy 2.0 async + Alembic) + 3 frontends React 19 (web-admin / web-teacher / web-student) + 4 DBs Postgres + Redis + Keycloak. Doctrina pedagógica viene del PID Línea 5 (UTN-FRM × UTN-FRSN).

Working dir: `/home/juanisarmiento/ProyectosFacultad/juani4/AI-NativeV3-main/`

## Qué hicimos hoy (sesión larga, 11 batches)

Refactor profundo: **convertir los ejercicios de array JSONB embebido en `TareaPractica.ejercicios` a entidad de primera clase reusable** con UUID propio + tabla intermedia `tp_ejercicios` (N:M).

### Por qué este refactor es importante para la tesis

Antes: un ejercicio "Hola Mundo" en TP1 de Comisión A era literalmente otro objeto que el "Hola Mundo" de TP1 de Comisión B. La tesis quería comparar trayectorias cognitivas N4 entre cohortes sobre el mismo estímulo, pero la unidad de análisis era la TP (variable entre cohortes), no el ejercicio (estable).

Ahora: el `Ejercicio` es la **unidad analítica reproducible**. El piloto del próximo cuatrimestre puede correr exactamente los mismos 25 ejercicios PID-UTN y comparar trayectorias. Eso es **ciencia reproducible** para la defensa doctoral.

### Decisiones arquitectónicas

3 ADRs nuevos en el repo (`docs/adr/`):

- **ADR-047** — Ejercicio como entidad de primera clase reusable (tabla `ejercicios` + intermedia `tp_ejercicios`)
- **ADR-048** — Schema pedagógico PID-UTN (banco socrático N1-N4, misconceptions, anti-patrones, tutor_rules, respuesta_pista, heurística_cierre)
- **ADR-049** — `ejercicio_id` UUID en payload del CTR desde día cero (aprovechando que la BD piloto está limpia para no romper hashes históricos)

### 25 ejercicios canónicos PID-UTN cargados

Extraídos textualmente de los 3 docs del PID:

- `b1.docx` → 10 secuenciales (TP1: Hola Mundo, casting, IMC, área círculo, etc.)
- `condi.docx` → 10 condicionales (TP2: edad, par, contraseña, terremoto, estaciones, etc.)
- `mixtos.docx` → 5 integrador (TP: Caja Kiosco, Login Campus, Agenda Turnos sin listas, Escape Room Bóveda, Arena Gladiador)

Cada ejercicio tiene **TODOS** los campos pedagógicos: banco N1-N4 con señales ✓/✗ por pregunta, misconceptions con probabilidad estimada y pregunta diagnóstica, anti-patrones con mensaje de orientación, respuesta_pista por nivel, heurística de cierre, prerequisitos sintácticos y conceptuales, tests, rubrica, tutor_rules.

El **E3 del integrador** (Agenda de Turnos) tiene la regla especial PID: prohibido listas/dicts/sets/tuplas. El tutor IA enforce esto sin debatir el enunciado.

Path del seed: `scripts/data/ejercicios-piloto.yaml` (4176 líneas YAML, 205KB).

### Wizard IA standalone

Nuevo prompt `ejercicio_generator/v1.0.0` en governance-service. Endpoint `POST /api/v1/ejercicios/generate` que recibe descripción NL + unidad temática + dificultad y devuelve borrador editable con todos los campos pedagógicos. Frontend en `/ejercicios` del web-teacher con wizard "Crear con IA".

### Lo que rompí (decisión consciente)

Opción B en cada decisión: **sin backwards-compat hacks**. La BD piloto se limpió, los frontends y servicios rompen prolijo durante el refactor y se rearman en el batch siguiente. La doctora Cortez aprueba esta postura.

Eliminado: campo `TareaPractica.ejercicios` JSONB legacy, `EjercicioSchema` viejo del contracts package, wizard de IA para TPs inline (queda solo el standalone para Ejercicio).

## Estado al cierre de sesión

- ✅ Backend completo (academic-service / tutor-service / CTR / evaluation-service / ai-gateway / governance-service)
- ✅ Frontends adaptados (web-teacher con vista nueva `EjerciciosView`, web-student con `ejercicio_id` en POST /episodes)
- ✅ Migrations Alembic aplicadas
- ✅ Casbin policies cargadas (184 totales, +14 nuevas para `ejercicio:*`)
- ✅ 25 ejercicios cargados en DB
- ⚠️ Stack levantado pero hubo bug al final con `ERR_CONNECTION_REFUSED` en api-gateway — quedó pendiente verificar logs

## Deuda diferida (anotar para después)

- **Versionado de Ejercicio**: si un docente edita un Ejercicio referenciado por TPs publicadas, la edición se propaga retroactivamente. Habría que agregar `parent_ejercicio_id` para clonar al editar. Por ahora `PATCH /ejercicios/{id}` advierte.
- **`ejercicio_id` autoritativo en CTR sin `ejercicio_orden`**: ahora el payload lleva ambos por compat. En piloto-2 se puede limpiar.
- **Smoke E2E `tests/e2e/smoke/test_ejercicios_e2e.py`**: lo declaré en el plan pero no lo escribí. Pendiente.

## Próximas iteraciones posibles

1. Enriquecer iterativamente los 25 ejercicios via wizard IA (los campos base ya están, los pedagógicos también pero el docente puede mejorar redacciones).
2. Smoke E2E del flow completo: crear ejercicio → asociar a TP → abrir episodio estudiante → verificar `ejercicio_id` en el payload del CTR → marcar completado en entrega.
3. Análisis longitudinal por `ejercicio_id` en analytics-service (querying por UUID estable inter-cohorte).

---

## Pendientes operacionales que vi de paso

- El seed `scripts/seed-3-comisiones.py` tenía un bug pre-existente: usaba `enunciado` cuando la migration de mayo 12 había renombrado a `consigna`. Lo parcheé in-place.
- El stack quedó levantado pero el browser daba `ERR_CONNECTION_REFUSED` al api-gateway. Necesita revisión de logs en `.dev-logs/api-gateway.log` y `.dev-logs/academic-service.log`.

## Personas mencionadas

- **Alberto Cortez**: doctorando, autor de la tesis. UNSL.
- **Daniela Carbonari**: co-directora del PID.
- **Bruno Roberti, Carlos Martínez, Claudia Naveda, Juan Robledo**: colaboradores en agradecimientos.
- **Ana Garis**: coautora del paper consolidado (revisión pendiente).
