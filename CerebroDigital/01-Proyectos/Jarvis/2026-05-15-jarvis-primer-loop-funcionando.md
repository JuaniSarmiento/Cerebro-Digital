---
fecha: 2026-05-15
hora: 14:30
fuente: claude-code (sesion-conversacional)
tags: [inbox, sin-revisar, proyecto, jarvis, hito, primera-conversacion-por-voz]
---

# Jarvis — Primer loop end-to-end funcionando

Hoy hablé por voz con Jarvis en español por primera vez. El spike `scripts/spike_loop.py` ejecutó las cuatro piezas en cadena: mic → Whisper → JarvisSession (tmux) → XTTS → parlantes.

## Lo que se confirmó

- **El loop completo funciona.** Voice in, voice out, Claude en el medio respetando la personalidad de Jarvis.
- **Whisper transcribió aceptable** pero falla en palabras inglesas dentro de oraciones españolas (transcribió "Yaruiz" en vez de "Jarvis"). Posible mitigación: usar `initial_prompt` en Whisper para bias hacia "Jarvis", o pasar a un modelo más grande.
- **Claude respondió con persona Jarvis correctamente**: "Muy bien, acá listo. ¿Qué necesitás?" — corto, voice-friendly, sin markdown. El override de `.claude/CLAUDE.md` funciona.
- **Claude reconoció sus límites**: cuando pedí partido de River, dijo "no tengo acceso a la web, abrí un navegador". Comportamiento correcto.
- **Ana Florence en español sonó humana** (a confirmar opinión final de Juani).

## Latencias medidas

| Etapa | Turno 1 | Turno 2 |
|-------|---------|---------|
| Grabación (fijo) | 5.1s | 5.0s |
| STT (Whisper CPU) | 1.5s | 1.3s |
| Claude (tmux + parser) | 7.1s | 9.1s |
| TTS síntesis batch | 3.1s | 3.5s |
| Audio sonando | 6.1s | 8.1s |
| **LATENCIA PERCEPTIDA** | **11.7s** | **13.9s** |

## Diagnóstico de la latencia

Total perceptido (sin contar grabación ni audio sonando) ronda los 12-14s. Esto es ALTO comparado con el target de 2s. El grueso viene de:
- **Claude (~7-9s)**: dominado por (a) el modelo pensando y (b) los 2.5s del parser esperando "estabilidad" del pane antes de devolver.
- **TTS síntesis (~3s)**: batch synthesis del response completo antes de empezar a reproducir.
- **Whisper (~1.4s)**: en CPU. En GPU baja a ~0.3s.

## Plan de optimización (en orden de impacto)

1. **Detectar fin de respuesta en parser**: el TUI muestra `✻ Cogitated for Xs` cuando termina. Detectar eso → return inmediato. Estimado: **-2 a -3s**.
2. **TTS streaming con `inference_stream`** de XTTS: empezar audio mientras sigue generando. Estimado: **-2s perceptidos**.
3. **Whisper en GPU**: `WhisperModel(device="cuda")`. Estimado: **-1s**.
4. **Suma**: bajaríamos de ~13s a ~7s perceptidos. No es 2s, pero brutalmente mejor.

## Lecciones técnicas

- **Whisper en español falla con palabras inglesas** dentro de oraciones. Considerar `initial_prompt="Conversación con Jarvis."` para bias.
- **Detección de end-of-response por marker `✻`** es más rápida que esperar estabilidad temporal del output.
- **Parser de respuestas asistente** funciona limpio extrayendo lineas con `● ` y siguientes indentadas.
- **No vale la pena pelearle a hooks de Engram/skills** del nivel global de Claude Code — quedan filtradas por el parser.

## Estructura actual del código de producción

```
src/jarvis/
├── stt/whisper.py       ← WhisperSTT (clase)
├── tts/xtts.py          ← XTTSEngine (clase)
└── tmux_ipc/session.py  ← JarvisSession + _parse_responses

scripts/spike_loop.py    ← orquestador del loop completo
```

## Pendientes en el horizonte

- Optimizaciones de latencia (orden indicado arriba)
- VAD con Silero para que detecte fin de habla, en vez de los 5s fijos
- Hotkey global (GNOME custom shortcut → trigger script)
- systemd user service para que arranque en cada login
- Daemon que mantenga la sesión Jarvis viva 24/7 (en vez de spike interactivo)
- Eventualmente: control de ventanas (v2) y orquestación de Claude Codes en paralelo (v3)

## Cuando esto se vuelva proyecto activo

Mover todas las notas inbox de Jarvis a `01-Proyectos/Jarvis/`.
