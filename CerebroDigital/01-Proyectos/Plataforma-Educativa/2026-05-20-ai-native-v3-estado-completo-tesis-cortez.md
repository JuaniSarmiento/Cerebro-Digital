---
fecha: 2026-05-20
hora: 19:00
fuente: claude-code (juniperra / AI-NativeV3)
tags: [inbox, sin-revisar, proyecto-faculta, tesis-cortez-unsl, ai-native, tutor-socratico, ctr, trazabilidad-criptografica, prog1, sesion-implementacion]
---

# AI-Native V3 — Estado completo del proyecto al 2026-05-20

> **Para el agente que procese esta nota**: esto NO es debug puntual ni log de sesión. Es un snapshot consolidado del proyecto entero + lo que se implementó hoy. Distribuir a `01-Proyectos/Facultad/Tesis-Cortez/` (carpeta nueva si no existe) y de ahí extraer sub-notas si conviene. La parte de "qué es" puede ir a `02-Conocimiento/Plataformas-Educativas-IA/`. Mantener wikilinks resueltos.

## 1. Qué es el proyecto

**[[Plataforma-Educativa-IA|AI-Native V3]]** (también conocido como **juniperra** por el nombre de la carpeta en disco) es la plataforma de la **[[2026-05-18-plataforma-educativa-es-tesis-doctoral-cortez|tesis doctoral]] de [[Alberto|Alberto Alejandro Cortez]] (UNSL)**: *"Modelo AI-Native con Trazabilidad Cognitiva N4 para la Formación en Programación Universitaria"*. Es un piloto académico de la Universidad Nacional de San Luis, **NO un producto comercial**.

### El problema que ataca

La tesis defiende que **un LLM educativo SIN trazabilidad rigurosa no sirve académicamente**. La hipótesis: cuando un alumno usa ChatGPT/Copilot para aprender a programar, no hay forma de saber qué aprendió ni qué copió. Las plataformas educativas existentes con IA (Khan Academy, Coursera, etc.) tampoco resuelven esto — son consumidores pasivos del LLM. La tesis propone una plataforma que firma criptográficamente cada interacción cognitiva del estudiante, las clasifica en niveles de apropiación cognitiva, y permite al docente auditar la actividad sin violar privacidad.

### Lo que aporta de nuevo

1. **CTR (Cadena de Trazabilidad de Registros)**: cada evento del alumno (lectura de enunciado, pregunta al tutor, ejecución de código, etc.) queda firmado con SHA-256 en una cadena append-only. Equivalente a un blockchain pero centralizado en la institución.

2. **Clasificación N1-N4** (modelo derivado de Bloom + SAMR):
   - **N1 Lectura/Comprensión**: el alumno lee el enunciado, comprende lo que se pide
   - **N2 Exploración**: prueba ideas, hace preguntas iniciales
   - **N3 Experimentación/Código**: tipea código, ejecuta, ve resultados
   - **N4 Apropiación/Reflexión**: explica lo que hizo, generaliza, reflexiona

3. **Tutor socrático IA**: no da código, hace preguntas. Implementa los 4 movimientos socráticos clásicos (ironía, mayéutica, elenchos, aporía). Prompt versionado y reproducible.

4. **K-anonymity por diseño**: los alumnos viven en la DB solo como `student_pseudonym` (UUID). NI el docente ve nombres reales. Hash determinístico para export académico anonimizado (salt ≥16 chars).

5. **Reproducibilidad bit-a-bit**: `classifier_config_hash` determinístico. Mismo input → mismo output. Cualquier cambio en la fórmula de hash invalida la tesis.

### Personas detrás

- **Alberto Alejandro Cortez** (UNSL) — doctorando, autor principal
- **Daniela Carbonari** — co-directora del PID
- **Ana Garis** — coautora del paper
- **Bruno Roberti, Carlos Martínez, Claudia Naveda, Juan Robledo** — equipo
- **Juani Sarmiento** (yo) — colaborador técnico de la implementación de la plataforma

### Estado académico

- 11/20 capabilities funcionales al 100% (sobre los 4 criterios estrictos: código+tests+docs / invariantes / producción piloto / aprobación académica)
- Paper consolidado en `paper-draft.md` con 10/10 decisiones académicas resueltas (Camino 1 + protocolo dual para κ ≥ 0.70)
- Submisión a **CONAIISI 2025**
- 2 riesgos académicos vigentes:
  1. 106 classifications con hash legacy pre-LABELER_VERSION 1.2.0 (necesita re-clasificación masiva con DB real)
  2. Validación intercoder κ ≥ 0.70 sobre protocolo dual: 200 eventos estratificados (50 por nivel N1-N4) + 50 episodios cerrados en 3 categorías de apropiación — requiere coordinar con 2 docentes UNSL (~25-30h/docente). **Cuello de botella académico más grande.**

## 2. Stack técnico (snapshot)

### Layout

- Disco local: `/home/juanisarmiento/ProyectosFacultad/juniperra/`
  - Wrapper externo con docs de gobierno (paper, ADRs, auditorías)
  - `AI-NativeV3-main/` ← el monorepo real (ojo: wrapper se llama V4, subdirectorio se llama V3 por herencia)
- Remoto: `git@github.com:JuaniSarmiento/AI-Native-V5.git` (rama `main`)
- Backend Python: workspace `uv` (Python 3.12, FastAPI 0.100+, SQLAlchemy 2.0 async, Alembic, structlog, OpenTelemetry)
- Frontend TypeScript: workspace `pnpm` + `turbo` (Node 20+, React 19, Vite 6, TanStack Router/Query, Tailwind v4, Biome)

### 11 servicios Python activos (con puertos)

| Puerto | Servicio | Qué hace |
|---|---|---|
| 8000 | api-gateway | Único punto de auth (JWT RS256). Inyecta headers `X-Tenant-Id`, `X-User-Id`, `X-User-Roles` a servicios internos |
| 8002 | academic-service | CRUDs académicos (facultades, materias, comisiones, TPs, ejercicios, inscripciones). Bulk-import |
| 8004 | evaluation-service | Entregas de TP + calificaciones |
| 8005 | analytics-service | Kappa inter-rater, progresión cohorte, alertas k-anonymity, export académico anonimizado, integrity-events |
| 8006 | tutor-service | Orquestador socrático con SSE + guardrails + worker abandono + worker distracción (nuevo hoy) |
| 8007 | ctr-service | Cadena criptográfica SHA-256 append-only (8 particiones Redis) |
| 8008 | classifier-service | Clasificador N1-N4 reproducible bit-a-bit |
| 8009 | content-service | RAG con pgvector + chunker estratificado |
| 8010 | governance-service | Prompts versionados internos + TP-generator IA |
| 8011 | ai-gateway | LLM proxy BYOK multi-provider (Mistral / OpenAI / Anthropic / mock + nuevo OpenAIProvider para copilot-api) |
| 8012 | integrity-attestation | Firmas Ed25519 externas post-cierre episodio (vive en VPS UNSL en piloto real) |

Más 8 ctr-workers (single-writer por partición Redis Streams).

### 3 frontends React 19

| Puerto | App | Para quién |
|---|---|---|
| 5173 | web-admin | Gestión institucional, auditoría CTR, BYOK |
| 5174 | web-teacher | Docentes — vistas analíticas (progresión, kappa, niveles N1-N4, intentos adversos, integridad) |
| 5175 | web-student | Alumnos — IDE Monaco + Pyodide + chat tutor SSE |

### Bases de datos

4 bases Postgres lógicas separadas (sin joins cross-base):
- `academic_main` — CRUDs operacionales (alumnos viven como `student_pseudonym` UUID)
- `ctr_store` — eventos CTR firmados, append-only
- `classifier_db` — clasificaciones N1-N4
- `content_db` — material de cátedra para RAG (chunks + embeddings pgvector)

Multi-tenant con **Row-Level Security forzado** (ADR-001). Cada tabla con `tenant_id` tiene policy RLS, driver entra con `SET LOCAL app.current_tenant = ...`.

### LLM provider en uso

**copilot-api** (proxy local en `:4141` que expone GitHub Copilot como OpenAI-compatible API). Permite usar **gpt-4o-mini gratis** con cuenta de GitHub. Setup:

```bash
# Primera vez (auth):
npx -y copilot-api@latest auth
# Cada vez:
npx -y copilot-api@latest start --port 4141
```

Alternativa: API keys reales (OpenAI / Anthropic / Mistral) en `.env`.

## 3. Lo que se implementó hoy (2026-05-20)

### Sesión completa con dos features grandes

#### Feature 1 — Activación del prompt tutor v1.2.0

El proyecto tenía dos versiones del prompt del tutor:
- **v1.1.0** (activa hasta hoy): mayéutica débil ("hacer preguntas en vez de afirmar"). 121 líneas. Cubre 4/10 guardrails formales.
- **v1.2.0** (draft, no activa): **4 movimientos socráticos platónicos completos** (ironía, mayéutica, elenchos, aporía). 268 líneas. Cubre 9/10 guardrails formales.

**Activé v1.2.0** modificando:
1. `ai-native-prompts/manifest.yaml` (`tutor: v1.1.0` → `v1.2.0`)
2. `apps/tutor-service/src/tutor_service/config.py` (`default_prompt_version: "v1.2.0"`)
3. `packages/ui/src/components/AuditFooter.tsx` (`PROMPT_VERSION = "tutor/v1.2.0"`)
4. `apps/web-student/src/components/OpeningStage.tsx` (detail label)

**Verificado** que el tutor responde con mayéutica formal — ej. ante "dame el código, no quiero pensarlo" responde con paso 1 de la mayéutica escalonada ("¿qué te haría pensar que es mejor recibir el código?").

#### Feature 2 — Integridad del episodio (foco + clipboard)

Nueva feature que registra (y bloquea donde se puede) intentos del alumno de evadir el contexto del episodio. Cobertura académica del Cap. 8 de la tesis sobre integridad pedagógica.

**4 nuevos event types CTR firmados SHA-256:**

| Event type | Cuándo se dispara | Bloqueable |
|---|---|---|
| `pestana_perdida` | `document.visibilitychange === "hidden"` o `window.blur` | NO (limitación del browser) — solo se detecta |
| `pestana_recuperada` | El alumno vuelve a la pestaña | NO — se mide `tiempo_fuera_segundos` |
| `copia_intentada` | Ctrl+C / Ctrl+X / paste del menú contextual en Monaco | **SÍ** — bloqueado |
| `pega_intentada` | Ctrl+V / paste DOM / drag&drop en Monaco | **SÍ** — bloqueado, registra `contenido_preview` (200 chars) |

**Comportamiento del cambio de pestaña:**
- Overlay rojo full-screen al perder foco con mensaje "ATENCIÓN: te saliste del episodio. Quedó registrado criptográficamente."
- Banner amarillo al volver con `Estuviste fuera N.Ns. Quedó registrado.`
- **Worker server-side `distraction_worker.py`**: cada 5 segundos barre marcas en Redis (`tutor:distraction:{episode_id}`). Si supera **30 segundos**, emite `EpisodioAbandonado(reason="distraccion_pestana")` y cierra el episodio.

**Comportamiento del clipboard en Monaco:**
- `editor.addCommand` sobrescribe Ctrl+V / Ctrl+C / Ctrl+X
- DOM listeners en el container del editor capturan `paste`/`copy`/`cut` con capture phase
- Toast naranja "🚫 Pegar está bloqueado. Quedó registrado." auto-oculta en 4s
- Cada intento queda firmado en CTR

**Dashboard docente — nuevo tab "Distracción y trampa":**

En `web-teacher` → sidebar → Análisis → **Intentos adversos** → tab **"Distracción y trampa"**.

Muestra:
- Contador total de eventos
- Lista "Alumnos con más intentos de evasión" (top con `Badge`)
- Lista de eventos recientes con etiquetas de color por tipo (azul, amarillo, naranja, rojo)
- Drill-down a `/episode-n-level?episodeId=...` para ver timeline N1-N4 completo

**Endpoint backend**: `GET /api/v1/analytics/cohort/{id}/integrity-events` (analytics-service).

### Feature 3 — Ejercicio canónico "edad → categoría" en el banco

Para grabación de video, creé en la DB:
- **Unidad "test"** (id `aaaaaaaa-1111-1111-1111-000000000099`) en la materia "Programación I" comisión 1
- **TP "TP-EDAD-01" "Clasificación por edad"** (id `cccccccc-1111-1111-1111-000000000099`), estado `published`
- **Ejercicio "Categoría por edad"** (id `bbbbbbbb-eeee-dddd-dddd-000000000099`) con:
  - Enunciado canónico: niño <12, adolescente [12,18), adulto joven [18,30), adulto ≥30
  - Casos borde explícitos (11, 12, 17, 18, 29, 30)
  - **Banco socrático N1-N4** con 12 preguntas escalonadas
  - **3 misconceptions** con probabilidad y pregunta diagnóstica
  - **4 pistas escalonadas** (nivel 1-4)
  - **3 anti-patrones** de código
  - **Heurística de cierre** (6 tests pasados + verbalización)
  - **Tutor rules** (`prohibido_dar_solucion=true`, `forzar_pregunta_antes_de_hint=true`)

Script SQL: `scripts/seed-video-ejercicio.sql` (idempotente, en el repo).

### Feature 4 — Script todo-en-uno para grabación de video

`scripts/start-video-ready.sh` (4.9 KB, ejecutable). Hace 8 cosas en orden:

1. Verifica infra Docker (postgres, redis, keycloak); la levanta si no está
2. Verifica `copilot-api` en `:4141` (si no, falla con instrucciones)
3. Apaga backends previos (idempotente)
4. Levanta 11 backends + 8 ctr-workers
5. Reinicia api-gateway con `DEV_TRUST_HEADERS=true`
6. Switchea ai-gateway + tutor-service a copilot-api
7. Aplica `seed-video-ejercicio.sql` si el ejercicio no existe
8. Levanta los 3 frontends Vite

Tiempo: ~30s la primera vez, ~10s en sucesivas.

### Feature 5 — README actualizado

- Nueva sección "Demo rápida (grabación de video o presentación)" con prerequisitos, comandos y limitaciones conocidas
- Nuevo workflow "Levantar el stack para una demo o grabación" en "Workflows comunes"
- Mención del prompt v1.2.0 en la tabla de estado
- Caveat sobre `DEV_TRUST_HEADERS=true` que no es default y rompe el dev mode si no se setea

## 4. Limitaciones conocidas (importante para usar el proyecto)

### Del demo

1. **`input()` no funciona en el editor Monaco** del browser. Pyodide no tiene stdin interactivo. Workaround: usar `edad = 15` hardcodeado.
2. **El editor muestra placeholder genérico** (`def factorial(n): pass`) en lugar del `inicial_codigo` del ejercicio del banco. Workaround: borrar y tipear.
3. **El chip "Mistral"** en el panel del tutor es hardcoded — el modelo real es `gpt-4o-mini` vía copilot-api.
4. **El stream SSE a veces se corta** después de 1-2 oraciones. Cortar en edición del video.
5. **El menú contextual del browser** (right-click → Pegar) puede no estar 100% bloqueado en todos los browsers. Mi listener DOM debería capturarlo pero NO lo verifiqué manualmente.

### Del proyecto en general

1. **DB en estado de prueba**: las 106 classifications con hash legacy esperan re-clasificación masiva (A1 del plan de acción).
2. **Validación intercoder κ ≥ 0.70** pendiente — bloquea la activación de `socratic_compliance` (ADR-027/044) y `lexical_anotacion_override` (ADR-023 G8b / ADR-045).
3. **Keycloak no onboardeado en dev**: se usa `dev_trust_headers=True` con headers X-* inyectados por Vite.
4. **Gap B.2 abierto**: `GET /api/v1/comisiones/mis` no funciona para estudiantes reales (JOINea `usuarios_comision` que es para docentes). Workaround: vite.config hardcodea X-User-Id del alumno seedeado.
5. **integrity-attestation-service:8012 es 503 by design** en dev local. Vive en VPS UNSL en piloto real.

## 5. Verificación criptográfica end-to-end (lo que se grabó hoy)

Para el video, hicimos pruebas con Playwright y verificamos en DB:

| Acción del alumno | Evento CTR registrado | Verificación |
|---|---|---|
| Cambiar pestaña (visibilitychange=hidden) | `pestana_perdida{trigger="visibilitychange"}` | ✓ DB seq=1 |
| Volver a la pestaña | `pestana_recuperada{tiempo_fuera_segundos: 27.579}` | ✓ DB seq=3 |
| Ctrl+V intentando pegar | `pega_intentada{metodo="shortcut"}` | ✓ DB seq=8 |
| Paste vía DOM event | `pega_intentada{metodo="menu_contextual", preview="edad = 25\nprint(adulto joven)"}` | ✓ DB seq=5 |
| Ctrl+C | `copia_intentada{metodo="shortcut"}` | ✓ DB seq=10 |
| Ctrl+X | `copia_intentada{metodo="shortcut"}` | ✓ DB seq=11 |
| Tab hidden >30s | `episodio_abandonado{reason="distraccion_pestana", last_activity_seconds_ago: 34.17}` | ✓ DB seq=5 (otro episodio) |

Cada evento firmado con `self_hash` SHA-256 sobre payload canonical + `chain_hash` = SHA-256(self_hash + prev_chain_hash). Cadena verificable end-to-end.

## 6. Commit y push del 2026-05-20

- **Hash**: `7ab4c64`
- **Branch**: `main` → `origin/main` (`github.com/JuaniSarmiento/AI-Native-V5`)
- **Cambios**: 25 archivos · 2116 inserciones · 16 deleciones · 5 archivos nuevos
- **Mensaje**: `feat(integridad): foco + clipboard tracking + activacion tutor v1.2.0`

## 7. Pendientes técnicos del próximo sprint

- **Tests unitarios** de los nuevos endpoints + worker `distraction_worker`
- **ADR formal** para la tesis sobre la feature de integridad (sugerido: `docs/adr/056-tab-focus-and-clipboard-integrity.md`)
- **HelpButton content** para la vista nueva del docente
- **Verificación manual** del bloqueo via menú contextual (right-click → Pegar) en distintos browsers
- **Fix del editor**: que cargue `inicial_codigo` del ejercicio en lugar del placeholder genérico
- **Vista del estudiante en el docente** (drill-down): que aparezca timeline de integridad en la página `/student-longitudinal`

## 8. Pendientes académicos (no técnicos)

- Coordinar etiquetadores UNSL para κ ≥ 0.70 con protocolo dual (200 eventos + 50 episodios)
- Ejecutar A1 del plan-acción: re-clasificación masiva con DB real
- Revisión coautoral del paper-draft con Ana Garis previa a submisión CONAIISI 2025
- Coordinar Keycloak claim `comisiones_activas` con DI UNSL (gap B.2)
- Marcar `Smoke E2E API` como Required check en branch protection (A8)

## 9. Cómo retomar el proyecto

```bash
# 1. Levantar copilot-api (terminal aparte)
npx -y copilot-api@latest start --port 4141

# 2. Ir al repo
cd /home/juanisarmiento/ProyectosFacultad/juniperra/AI-NativeV3-main

# 3. Levantar el stack completo (incluye seed video)
bash scripts/start-video-ready.sh

# URLs:
# Alumno:  http://localhost:5175
# Docente: http://localhost:5174
# Admin:   http://localhost:5173
```

Flujo del demo:
1. Abrir `http://localhost:5175` (alumno)
2. Click en "Programación I" → "Entrar"
3. Click en unidad **"test #99"**
4. Click en **"TP-EDAD-01"** → "Empezar"
5. Click en **"Ejercicio 1: Categoría por edad"** → "Empezar"
6. Los 3 paneles cargan (Consigna · Editor + Pyodide · Tutor SSE)
7. Mandar mensaje al tutor pidiendo el código directo → ver mayéutica v1.2.0
8. Intentar Ctrl+V → ver toast naranja + evento firmado
9. Cambiar pestaña → ver overlay rojo + evento firmado
10. Cambiar al docente (`:5174`) → Análisis → Intentos adversos → tab "Distracción y trampa" → ver todos los eventos del alumno

## 10. Wikilinks que dejé para procesar después

- [[tesis-cortez-resumen]] — resumen ejecutivo de la tesis (si existe)
- [[plataforma-ai-native-arquitectura]] — diagrama de arquitectura más visual
- [[guion-video-pantalla-alumno]] — el guion del video que armamos hoy (lo dejé sin guardar en el inbox como nota separada, considerar moverlo)
- [[unsl-equipo-pid]] — gente involucrada (Cortez, Carbonari, Garis, Roberti, etc.)
- [[copilot-api-setup]] — setup del proxy local para LLM gratis
- [[prompt-engineering-socratico]] — referencias al prompt v1.2.0 con los 4 movimientos platónicos
- [[ctr-cadena-trazabilidad]] — concepto técnico de la cadena criptográfica
- [[clasificacion-n1-n4-bloom-samr]] — modelo de niveles de apropiación cognitiva
