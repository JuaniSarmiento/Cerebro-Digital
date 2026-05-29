---
fecha: 2026-05-29
hora: "00:43"
fuente: claude-code
tags: [proyecto-ai-native, auditoria, repos, port-mejoras, baseline, maty-vs-juani]
estado: baseline-congelado
proximo-paso: clonar-juani-actualizado-y-diff
---

# AI-Native-V5 — Baseline, auditoria y plan de port (Maty <- Juani)

## Contexto del problema

Hay dos repos AI-Native-V5 en GitHub:

- `MatyAlts/AI-Native-V5` — **fork** de Juani, hecho por Matías Torres Altamirano. Este es el que **esta deployado en el VPS** (produccion).
- `JuaniSarmiento/AI-Native-V5` — repo **origen**, propiedad de Juani.

**Historia real:** el proyecto base era el de Juani. Lo replicaron en el repo de Maty, se le hicieron mejoras encima, y se subio al VPS desde ahi. El de Juani quedo desactualizado.

**Lo que esta pasando ahora:** por una confusion, **alguien esta trabajando otra vez sobre el repo de Juani** (que esta viejo). Va a meter mejoras nuevas ahi.

**Lo que queremos:** cuando esten esas mejoras nuevas en el repo de Juani, **aislarlas** (separarlas del estado actual del repo de Juani) y **portarlas al repo de Maty** (el deployado), sin pisar las mejoras que Maty ya tiene encima.

## Formula del flujo

```
delta_mejoras = Juani_NUEVO − Juani_HOY (baseline congelado el 2026-05-29)
Maty_FINAL    = Maty_ACTUAL + delta_mejoras (con resolucion de conflictos)
```

El `Juani_HOY` ya esta auditado y va a ser congelado como baseline inmutable.

## Hallazgos clave de la auditoria (29-may-2026)

### Estado git real

- Maty hizo **fork formal git** del de Juani (no copia manual). Comparten SHAs identicos hasta el merge-base `a9eb547` del 25-may.
- Maty tiene **60 commits propios** sobre el merge-base. De esos:
  - **32 firmados por "Juani Sarmiento"** (Juani trabajo directo sobre el clon de Maty entre 26 y 28 de mayo).
  - **27 firmados por Maty** (dockerizacion EasyPanel, runtime deps, VITE_API_URL, etc).
  - **1 del bot Copilot**.
- El repo de Juani esta **congelado en `60ac9c6`** (literalmente "Merge pull request #1 from MatyAlts"). 0 commits propios despues de ese merge.

### Diff numerico Maty vs Juani (estado al 29-may)

- **63 archivos difieren**:
  - 45 modificados
  - 18 solo en Maty (agrego cosas)
  - 0 solo en Juani (Juani no tiene nada que Maty no tenga)
- **Tamaños**: ambos repos pesan 27 MB.

### Las 5 decisiones macro que Maty (o Juani sobre el clon de Maty) metio encima

1. **Clerk + `student_profile`** — identidad real del alumno con UUID determinista derivado del Clerk ID, sin romper anonimato del CTR. Privacy-by-design.
2. **Poda EasyPanel** — saco Keycloak, Otel Collector, Jaeger, Prometheus, Loki, Grafana del `compose.prod`. Agrego migraciones alembic en ENTRYPOINT y `.env.prod.example` por servicio.
3. **Embeddings → Gemini API** — chau torch/GPU, hola REST a Gemini. `GeminiEmbedder` nuevo, `device=cpu` forzado.
4. **Tutor anti-distraccion endurecido** — 0s tolerancia + sweep 1s (antes 30s/5s). Si salis de la pestaña, perdes el episodio.
5. **Prompts del tutor IA INTACTOS** — las 4 versiones byte-a-byte iguales. Decision consciente: coautoria con Ana Garis no se toca.

### Zonas calientes para conflicto en el port

Cuando porteemos las mejoras nuevas del de Juani al de Maty, los **archivos modificados por Maty (45)** son zona caliente. Si las mejoras nuevas tocan alguno de esos 45, va a haber merge conflict real. Hay que mirarlos uno por uno.

Archivos especialmente sensibles:
- `infrastructure/docker-compose.prod.yml` (Maty −Keycloak, Jaeger, Loki, Grafana, Prometheus, Otel; +174/−16)
- `tutor-service/config.py` (umbrales anti-distraccion)
- `nginx-frontends.conf` (UUIDs/emails hardcodeados distintos por entorno)
- Sistema `student_profiles` completo
- `EpisodePage.tsx` (UX cierre de episodio)
- Frontends con Clerk integrado

## Rutas en el VPS

### Repos clonados (baseline al 29-may-2026)
- `/home/claude-bot/proyectos/comparativa-ai-native/matyalts-ai-native-v5/` — repo Maty (27 MB)
- `/home/claude-bot/proyectos/comparativa-ai-native/juani-ai-native-v5/` — repo Juani BASELINE (27 MB)
- Ambos contienen el proyecto real en subcarpeta `AI-NativeV3-main/`.

### Reportes de auditoria
- `/home/claude-bot/proyectos/comparativa-ai-native/audit/01-matyalts-repo-audit.md`
- `/home/claude-bot/proyectos/comparativa-ai-native/audit/02-juani-repo-audit.md`
- `/home/claude-bot/proyectos/comparativa-ai-native/audit/03-diff-baseline.md`
- `/home/claude-bot/proyectos/comparativa-ai-native/audit/04-git-history-divergence.md`
- `/home/claude-bot/proyectos/comparativa-ai-native/audit/05-semantic-diff-critical.md`
- `/home/claude-bot/proyectos/comparativa-ai-native/audit/diffs/` — 63 diffs unified individuales

## Plan para mañana (30-may o cuando lleguen las mejoras)

1. **Clonar el repo de Juani actualizado** a una carpeta nueva: `/home/claude-bot/proyectos/comparativa-ai-native/juani-ai-native-v5-NUEVO/`
2. **Diff baseline vs nuevo**: `diff -r juani-ai-native-v5/ juani-ai-native-v5-NUEVO/` → eso da el **delta puro de mejoras nuevas** (sin contaminacion del estado base).
3. **Catalogar el delta**: para cada archivo modificado, decidir si es mejora portable o cambio especifico que no aplica al de Maty.
4. **Cruzar con zona caliente**: marcar cuales archivos del delta caen sobre los 45 ya modificados por Maty → ahi va a haber conflicto.
5. **Aplicar patches al de Maty**, uno por uno, con review. Para conflictos: consultar antes de resolver.
6. **Validar** que el de Maty siga buildeando y el test suite siga verde.
7. **Commit con mensaje claro** explicando el origen de cada cambio.

## Acciones pendientes (TODO antes de mañana)

- [ ] Crear tag git en el repo de Juani local: `baseline-2026-05-29`
- [ ] Crear tarball del repo de Juani: `juani-baseline-2026-05-29.tar.gz`
- [ ] Crear tag git en el repo de Maty local: `pre-port-2026-05-29`
- [ ] Crear tarball del repo de Maty: `maty-pre-port-2026-05-29.tar.gz`
- [ ] Documentar el playbook de port en `/home/claude-bot/proyectos/comparativa-ai-native/audit/PLAYBOOK-PORT.md`

## Preguntas abiertas

- ¿Quien esta haciendo las mejoras nuevas sobre el repo de Juani?
- ¿Va a abrir PR o committea directo a `main`?
- ¿Sabe que el repo deployado es el de Maty? (Para evitar que la confusion se repita)

## Referencias

- GitHub Maty: https://github.com/MatyAlts/AI-Native-V5
- GitHub Juani: https://github.com/JuaniSarmiento/AI-Native-V5
- Merge-base comun: `a9eb547` (2026-05-25)
- HEAD Juani al 29-may: `60ac9c6` (Merge PR #1 from MatyAlts)

## Notas relacionadas

- [[2026-05-20-ai-native-v3-estado-completo-tesis-cortez]] — snapshot V3, el estado previo del que parte esta historia
- [[2026-05-18-plataforma-educativa-es-tesis-doctoral-cortez]] — reframe: la plataforma es la tesis doctoral de Cortez
- [[Plataforma-Educativa-IA]] — nota raíz del proyecto
- [[Alberto]] — Cortez, jefe/doctorando, dueño de la tesis
- [[VPS-Infraestructura]] — el repo de Maty es el que está deployado en el VPS
