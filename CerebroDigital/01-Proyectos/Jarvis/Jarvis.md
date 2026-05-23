---
fecha: 2026-05-15
hora: 15:30
fuente: claude-code (sesion-conversacional)
tags: [proyecto, jarvis, ia-personal, voz, claude-code, control-gui, activo]
estado: activo
prioridad: alta
---

# Jarvis — IA personal por voz

**Capa de voz + control GUI alrededor de Claude Code.** Push-to-talk, conversación continua, memoria delegada a Claude (Engram + vault Obsidian). No reemplaza a Claude Code: lo amplifica y le da boca y oídos.

## Qué NO es

- No es un proyecto desde cero — es una capa sobre Claude Code que ya tiene acceso a archivos, sistema y vault.
- No es wake-word continuo en v1 — push-to-talk con hotkey global.
- No es memoria propia — usa las 3 capas que ya están en `~/.claude/CLAUDE.md` (sesión Claude + Engram + vault).

## Stack confirmado (v1)

| Pieza | Implementación | Latencia |
|-------|----------------|----------|
| STT | `faster-whisper` small, GPU CUDA | ~0.4s |
| Claude persistente | `tmux` + `JarvisSession` | ~2.4s |
| TTS | `coqui-tts` XTTS v2 (Ana Florence, es, speed=1.15) GPU | ~1.6s |
| Audio I/O | `sounddevice` con `pipewire` | <0.1s |
| Hotkey | GNOME custom shortcut (X11) → trigger | — |

**Latencia perceptida actual:** ~4.4s end-to-end. Target idealizado era 2s pero 4.4s ya es **3x mejor** que el primer loop (11-14s).

## Arquitectura para v3, envío v1

Estructura de carpetas pensada como si fuera v3 desde el día uno. v2 y v3 suman código sin refactorizar.

- **v1** (semanas 1-3): loop voz completo + Claude en tmux
- **v2** (semanas 4-5): skills GUI (`abrir_app`, `tile`, `mover_a_workspace`, `mover_a_monitor`) con `xdotool` + `wmctrl`
- **v3** (semanas 6-8): orquestación de múltiples Claude Codes via `tmux send-keys` + `capture-pane`, escenas tipo "modo programación proyecto X"

## Repo y código

- **Path**: `~/ProyectosPersonales/Jarvis/`
- **Lenguaje**: Python (manejado con `uv`)
- **Override de config para `jarvis-core`**: `.claude/CLAUDE.md` con persona Jarvis + Engram OFF + Obsidian ON + respuestas voice-friendly cortas sin markdown

### Estructura actual

```
src/jarvis/
├── stt/whisper.py       ← WhisperSTT (GPU + initial_prompt)
├── tts/xtts.py          ← XTTSEngine
├── tmux_ipc/session.py  ← JarvisSession + parser optimizado con ✻
├── audio/               ← vacío, pendiente v1
├── skills/              ← v2
└── orchestration/       ← v3

scripts/
├── spike_loop.py            ← orquestador del loop completo
├── benchmark_latency.py     ← benchmark sin voz, determinístico
└── (otros spikes históricos)
```

## Por qué este proyecto cierra con la identidad

| Pieza | Cómo aparece en Jarvis |
|-------|------------------------|
| [[Filosofia-de-Trabajo\|IA como herramienta dirigida]] | Jarvis no programa — es intermediario. Claude ejecuta, Juani dirige por voz |
| [[Filosofia-de-Trabajo\|Conceptos > Código]] | Arquitectura para v3 desde día uno, sin refactor entre versiones |
| [[Filosofia-de-Trabajo\|Contra el inmediatismo]] | Spikes para validar latencia ANTES de escribir el daemon final |
| [[Sistema-Memoria-Engram\|3 capas de memoria]] | Sesión Claude + Engram + Vault — Jarvis no tiene memoria propia |
| [[Stack-Tecnologico-Lenguajes\|Python para IA]] | Daemon en Python, máxima librería para audio/STT/TTS |

## Estado actual (2026-05-18)

**Hitos logrados (todos el 2026-05-15):**

1. Visión inicial reframeada → es UNA capa sobre Claude Code, no veinte proyectos
2. Arquitectura validada: las 4 piezas (STT, Claude tmux, TTS, audio I/O) funcionan
3. **Primer loop end-to-end andando** — Juani habló por voz con Jarvis en español
4. Optimización autónoma: latencia de Claude bajó 65%, perceptida total bajó 67%

**Próximos pasos sugeridos (en orden):**

1. Re-correr `spike_loop.py` con voz para sentir el cambio post-optimización
2. **TTS streaming** (`inference_stream` de XTTS) — la última gran palanca de latencia (-1 a -2s)
3. **VAD con Silero** para reemplazar el grabar-5s-fijo con auto-detect de silencio
4. **Hotkey global GNOME + systemd user service** → daemon que vive 24/7
5. v2: control de ventanas (`xdotool` + `wmctrl`)

## Log cronológico

Orden de lectura recomendado:

1. [[2026-05-15-proyecto-jarvis-vision-inicial]] — scope completo y devolución técnica (12 capas, 6 personas / 2 años de scope, reframe pendiente)
2. [[2026-05-15-jarvis-arquitectura-y-plan-de-construccion]] — reframe a "capa sobre Claude Code", decisiones de stack, plan v1-v3
3. [[2026-05-15-jarvis-arquitectura-validada-piezas-listas]] — las 4 piezas validadas con números, gotcha de hooks/plugins de Claude Code
4. [[2026-05-15-jarvis-primer-loop-funcionando]] — primer voice-in / voice-out funcionando, latencia honesta medida
5. [[2026-05-15-jarvis-optimizaciones-resultados]] — sesión autónoma con resultados antes/después, latencia -67%

## Gotchas importantes (no perder)

- **Hooks y plugins de Claude Code se cargan ANTES de los flags `--settings/--plugin-dir`** → no se pueden apagar del todo. Solución: parser regex que extrae solo líneas con marker `●`, todo lo demás queda contenido en tmux.
- **Detección de fin-de-respuesta por marker `✻`** (e.g. "✻ Cogitated for 2s") es mucho más rápida que esperar 2.5s de "estabilidad" del pane.
- **Whisper en español falla con palabras inglesas** dentro de oraciones (transcribió "Yaruiz" por "Jarvis"). Mitigación: `initial_prompt` con bias a "Jarvis" y rioplatense.
- **GPU disponible**: RTX 3050 6GB, torch 2.12 + CUDA 13.0. Con XTTS cargado quedan ~3.9GB libres → margen para Whisper en GPU también.
- **"Fusión Claude + Copilot transparente" NO existe.** Son servicios distintos sin estado compartido. Lo que sí podés armar es un router. v5+ si alguna vez — en v1 todo va a Claude.
