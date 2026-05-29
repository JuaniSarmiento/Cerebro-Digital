---
fecha: 2026-05-15
hora: 14:00
fuente: claude-code (sesion-conversacional)
tags: [inbox, sin-revisar, proyecto, jarvis, hito, arquitectura-validada]
---

# [[Jarvis]] — Arquitectura validada, las 4 piezas funcionan

Hito importante: terminamos de validar todas las piezas técnicas de v1. Ya tenemos confirmado que la arquitectura elegida funciona end-to-end, falta solo integrarlas en un loop completo.

## Estado actual (post-spikes)

| Pieza | Implementación | Latencia medida | Estado |
|-------|----------------|-----------------|--------|
| STT (transcripción de voz) | `faster-whisper`, modelo `small`, CPU | 1.42s | ✅ OK |
| Claude persistente | `tmux` + clase `JarvisSession` en `src/jarvis/tmux_ipc/` | 5-8s por turno | ✅ OK |
| TTS (síntesis de voz) | `coqui-tts` (XTTS v2), GPU CUDA en RTX 3050 | 2.5x realtime | ✅ OK |
| Audio I/O | `sounddevice` con device `pipewire` | <100ms | ✅ OK |

## Decisiones técnicas confirmadas

- **STT**: `faster-whisper small` en CPU. Whisper `base` si necesitamos más velocidad. GPU disponible si hace falta.
- **TTS**: descartamos Kokoro (sonaba robótico en español). Vamos con XTTS v2 + speaker `Ana Florence` + `speed=1.15` + `language=es`. Suena más humano, factor 2.5x realtime en GPU.
- **GPU encontrada**: RTX 3050 6GB. `torch 2.12 + CUDA 13.0`. Cambia el panorama, da margen para modelos pesados.
- **Sesión Claude**: tmux nombrado `jarvis-core`, lanzado con `claude --model sonnet`, mantenido vivo todo el día. Cero overhead per-turn después del startup.
- **Continuidad de memoria**: confirmada **sin `--resume`** — es la misma sesión todo el tiempo, recuerda intra-sesión naturalmente.

## Override de config para Jarvis-core

Creamos `.claude/` dentro del proyecto con:
- `CLAUDE.md`: persona de Jarvis + override del CLAUDE.md global (Engram OFF, Obsidian ON, respuestas voice-friendly cortas sin markdown)
- `settings.json`: hooks vacíos (intento de apagar hook de [[Sistema-Memoria-Engram|Engram]] — parcialmente exitoso)
- `jarvis-mcp.json`: `{"mcpServers": {}}` (apaga MCPs)

Lanzamos con: `--model sonnet --settings .claude/settings.json --mcp-config .claude/jarvis-mcp.json --plugin-dir /tmp/jarvis-no-plugins`

## Gotcha importante encontrada

**Los hooks y plugins de Claude Code se cargan ANTES de que los flags `--settings/--plugin-dir` puedan apagarlos.** El hook `UserPromptSubmit` del plugin Engram firea sí o sí. Las skills del usuario (1461) se cargan igual.

**Solución**: NO pelearle a la config. El daemon NO interactúa con el TUI directamente — lee el output con `tmux capture-pane` y un parser regex extrae solo las líneas con marker `●` (la respuesta del asistente). El ruido (hook output, cajas ASCII, skills dropped) queda contenido dentro de tmux y nunca llega al usuario.

El parser está en: `src/jarvis/tmux_ipc/session.py::_parse_responses()`.

## Estructura de código actual

```
~/ProyectosPersonales/Jarvis/
├── .claude/                    ← override config para jarvis-core
│   ├── CLAUDE.md
│   ├── settings.json
│   └── jarvis-mcp.json
├── .git/
├── pyproject.toml              ← uv-managed, deps: faster-whisper, coqui-tts, sounddevice, torch+CUDA
├── src/jarvis/
│   ├── tmux_ipc/               ← PRIMERA pieza de código de producción
│   │   ├── __init__.py
│   │   └── session.py          ← JarvisSession + _parse_responses
│   ├── audio/                  ← vacío, pendiente v1
│   ├── stt/                    ← vacío, pendiente v1
│   ├── tts/                    ← vacío, pendiente v1
│   ├── llm/                    ← vacío (lo cubre tmux_ipc)
│   ├── skills/                 ← v2
│   └── orchestration/          ← v3
├── scripts/                    ← todos los spikes
│   ├── check_mic.py
│   ├── spike_stt_latency.py
│   ├── spike_tts_latency.py
│   ├── spike_claude.py
│   ├── spike_tmux_claude.py
│   └── spike_jarvis_session.py
└── models/                     ← Whisper y XTTS cacheados
```

## Latencia honesta del loop completo (proyección)

Con las piezas como están:

- STT: 1.4s
- Claude `ask()`: 5-8s
- TTS arranque: ~2s
- **Total perceptido: 9-12s por turno**

Por encima del budget de 2s. Palancas pendientes:

1. **Optimizar fin-de-respuesta del parser**: actualmente espera 2.5s de output estable. Si detectamos el marker `✻ Cogitated` (que aparece cuando Claude terminó), cortamos al instante. Estimo -2s.
2. **TTS streaming**: empezar a reproducir audio mientras XTTS sigue generando. Estimo -1.5s percibidos.
3. **Whisper en GPU**: bajaría de 1.4s a ~0.3s.
4. **Eventualmente**: si el `claude` CLI sigue siendo lento, podríamos pasar al Anthropic SDK directo. Pero eso es v2.

## Próximo paso inmediato

Loop integrado: micrófono → Whisper → JarvisSession.ask() → XTTS → parlantes, midiendo cada etapa. Es el momento de escuchar a Jarvis respondiendo por voz por primera vez.

## Cuando esto se vuelva proyecto activo

Mover estas notas a `01-Proyectos/Jarvis/` con sub-notas por capa. Por ahora se queda en inbox.
