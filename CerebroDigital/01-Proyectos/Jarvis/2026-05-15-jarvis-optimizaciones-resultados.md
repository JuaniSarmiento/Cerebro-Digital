---
fecha: 2026-05-15
hora: 15:15
fuente: claude-code (sesion-autonoma)
tags: [inbox, sin-revisar, proyecto, jarvis, optimizacion, latencia]
---

# Jarvis — Resultados de optimización (sesión autónoma)

Juani me dejó tiempo autónomo mientras se bañaba para "probar, mejorar y bajar mucho el tiempo". Acá van los resultados concretos.

## Antes / Después

| Métrica | Antes | Después | Cambio |
|---------|-------|---------|--------|
| Claude `ask()` promedio | 7-9s | **2.44s** | **-65%** |
| Latencia perceptida promedio (Claude + TTS) | 11-14s | **4.03s** | **-67%** |
| Respuestas cortas (ej: "Juani.", "140.") | 7-9s | **2.7-3.0s** | dentro del rango aceptable |
| Respuestas medias (1 oración) | 11-14s | **3.0-5.6s** | aceptable |

Nota: estos números son sin contar STT (Whisper) ni grabación. Con STT en GPU agregás ~0.3-0.5s.

## Cambios aplicados

### 1. Parser end-of-response por marker `✻` — IMPACTO ALTO

Antes: `JarvisSession.ask()` esperaba 2.5s de output estable del pane tmux. Eso era un floor de 2.5s sumado a cada turno.

Ahora: detectamos cuando aparece una línea con marker `✻` (que Claude escribe al terminar: "✻ Cogitated for 2s", "✻ Worked for 1s", etc.). Apenas la vemos, devolvemos. Polling más rápido (0.15s).

Implementación: `src/jarvis/tmux_ipc/session.py` — agregamos `_count_completions()` y reescribimos `ask()` para usar count diff en vez de wait_stable.

**Ahorro medido: -2 a -3 segundos por turno.**

### 2. Whisper en GPU + initial_prompt — IMPACTO BAJO-MEDIO

Antes: `WhisperSTT` corría en CPU con int8, 1.4s para transcribir 5s de audio.

Ahora:
- Default a `device="cuda"` con `compute_type="float16"` (más rápido en GPU)
- Auto-fallback a CPU si CUDA no está disponible
- `initial_prompt` por default que sesga la transcripción hacia "Jarvis" y rioplatense argentino

**Ahorro estimado: -1s** (no medido directamente porque requiere mic, pero la carga del modelo en GPU ya es 1.15s y la transcripción será proporcionalmente más rápida)

### 3. XTTS deprecation fix — IMPACTO CERO (cosmético)

Reemplazamos `TTS(gpu=True)` (deprecated) por `TTS().to("cuda")`. No cambia performance, elimina warning.

## Estado de uso de VRAM

Con XTTS cargado: **1.78 GB de 5.67 GB**. Tenemos margen para meter Whisper en GPU también sin problemas (~500MB adicionales). Todavía sobra VRAM si quisiéramos un modelo más grande.

## Lo que NO se hizo (esperando a Juani)

1. **TTS streaming real (`inference_stream`)** — requeriría usar la API de streaming de XTTS que es más invasiva. Lo prefiero hacer con Juani presente para validar audio en vivo.
2. **VAD con Silero** — bueno para el daemon final (detectar fin de habla en vez de grabar 5s fijos), pero hay decisiones de UX que requieren su input.
3. **Hotkey + systemd daemon** — son cambios del sistema que necesitan su confirmación.
4. **v2: control de ventanas** — explícitamente fuera de scope hasta charlarlo.

## Archivos modificados/creados

```
src/jarvis/tmux_ipc/session.py     ← parser optimizado con ✻ detection
src/jarvis/stt/whisper.py          ← GPU + initial_prompt
src/jarvis/tts/xtts.py             ← .to(device) en vez de gpu=True
scripts/benchmark_latency.py       ← benchmark sin voz, determinístico
```

## Latencia honesta proyectada cuando Juani vuelva

Si vuelve a correr `scripts/spike_loop.py` (loop completo con su voz):

```
Grabación:        5.0s   ← fijo, no cuenta como latencia
STT (GPU):        ~0.4s
Claude:           ~2.4s
TTS síntesis:     ~1.6s
─────────────────
LATENCIA PERCEPTIDA: ~4.4s
```

Eso es **3x mejor que antes** (era 11-14s). No llegamos al target idealizado de 2s pero **4.4s para una sesión de voz local sin GPU dedicada de inferencia es muy decente**.

Si quisiéramos bajar más: TTS streaming inferencia (-1 a -2s) sería el siguiente paso real.

## Próximos pasos sugeridos al volver

1. **Re-correr `scripts/spike_loop.py`** con su voz para sentir el cambio
2. Si le cierra el feel → decidir entre:
   - Atacar TTS streaming (la última gran palanca de latencia)
   - Empezar a integrar VAD (para reemplazar el grabar-5s-fijo con auto-detect de silencio)
   - Empezar el daemon real (systemd + hotkey)
3. Si NO le cierra → reevaluar antes de seguir
