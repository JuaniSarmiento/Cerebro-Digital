---
fecha: 2026-05-15
hora: 12:30
fuente: claude-code (sesion-conversacional)
tags: [inbox, sin-revisar, proyecto, jarvis, arquitectura, plan]
---

# Jarvis — Decisiones arquitectónicas + plan de construcción

Refinamos la visión inicial (ver `[[2026-05-15-proyecto-jarvis-vision-inicial]]`) hasta llegar a un plan concreto. Estas son las decisiones tomadas hasta acá.

## Reframe del scope

[[Jarvis]] NO es un proyecto desde cero. Es una **capa de voz + control GUI alrededor de Claude Code**, que ya tiene acceso a archivos, sistema, y al [[CerebroDigital|vault]]. Dos de las tres piezas grandes ya están funcionando:

- **Obsidian / segundo cerebro** → ya configurado en `CLAUDE.md` global (protocolo CerebroDigital + [[Sistema-Memoria-Engram|Engram]])
- **Acceso al sistema** → Claude Code en Linux lo hace nativamente
- **Voz + control GUI más allá de terminal** → ESTO es lo que falta y es el proyecto real

## Setup del usuario (verificado)

- Display server: **X11**
- Desktop: **GNOME (Ubuntu)**
- Window manager: `ubuntu`
- Tools de control GUI: ninguno instalado todavía (xdotool, wmctrl, ydotool)

Buena noticia: X11 simplifica todo el control de ventanas dramáticamente vs Wayland.

## Decisiones arquitectónicas tomadas

### Activación
- **Push-to-talk con hotkey global** (no wake word continuo en v1)
- **Re-apretar el hotkey** cierra la conversación (la opción más simétrica y limpia)
- Wake word continuo se considera para v4+, no antes

### Modo de conversación
- **Conversación con continuidad** (no one-shot por hotkey)
- Una vez abierto el canal, podés hablar varios turnos sin re-apretar
- VAD detecta cuándo termina cada turno (Silero VAD)

### Memoria
- **3 capas, todas YA configuradas en `~/.claude/CLAUDE.md`**:
  1. Conversación actual → sesión de Claude Code (`--resume <sid>`)
  2. Largo plazo semántica → Engram (`mem_save`, `mem_search`)
  3. Conocimiento humano → vault Obsidian + captura al `00-Inbox/`
- Jarvis NO tiene memoria propia. Usa la de Claude.

### Stack técnico
- **STT**: faster-whisper o whisper.cpp (local, modelo `small`)
- **TTS**: Piper (local, voz natural, <1s latencia). XTTS para voice cloning en v4+
- **VAD**: Silero VAD (local, pequeño)
- **Hotkey**: GNOME custom shortcut → dispara comando que pega al daemon
- **Daemon**: systemd user service (`~/.config/systemd/user/jarvis.service`)
- **Lenguaje**: probablemente Python para el daemon (más biblioteca disponible para audio + STT + TTS)
- **Claude**: invocación headless `claude -p --resume <sid>`
- **Control GUI** (v2+): xdotool + wmctrl
- **Control de Claude Code corriendo** (v3+): tmux send-keys + capture-pane

### Caso de uso meta (v3 target)
Que Juani pueda decir:
> "Jarvis, abrime en un escritorio Claude Code para programar, en la otra mitad de la pantalla Google con Gemini, en mi segunda pantalla Spotify, y en otro escritorio buscá qué partidos de fútbol hay esta tarde."

Y después:
> "Decile al chat de Claude Code que quiero seguir con tal proyecto, que dónde me recomienda continuar."

Y que Jarvis ejecute todo, lea la respuesta de Claude Code en voz alta.

## Plan de construcción acordado: "Arquitectura para v3, envío v1"

Diseñar las carpetas, módulos e interfaces como si fuera v3 desde el día uno. Implementar v1 primero. v2 y v3 suman código sin refactorizar.

### Semana 1 — Esqueleto + spike de latencia
- Estructura de carpetas pensada para v3
- Loop de voz con skill trivial
- **MÉTRICA CRÍTICA**: latencia end-to-end <2s. Si no, parar y optimizar antes de continuar.

### Semanas 2-3 — v1 funcional
- Loop voz completo + conversación con re-press para cerrar
- Engram y vault funcionando vía Claude

### Semanas 4-5 — v2: skills GUI
- `abrir_app`, `tile`, `mover_a_workspace`, `mover_a_monitor`

### Semanas 6-8 — v3: orquestación + tmux
- Escenas ("modo programación proyecto X")
- Mgmt de instancias Claude Code via tmux
- Detección de end-of-response (la pieza no trivial)

**Total**: ~8 semanas full focus, 3-4 meses con dedicación de fin de semana.

## Conceptos clave para no olvidar

### "Ver la PC" son TRES capas distintas
1. **Metadata de ventanas** (wmctrl, xdotool) → 80% de los casos, instantáneo, gratis
2. **OCR de screenshot** (tesseract) → casi nunca necesario
3. **Multimodal a Claude** (screenshot → API) → cuando hace falta entender contenido visual real

### tmux es la llave para hablarle a un Claude Code vivo
- Cada Claude Code se lanza dentro de `tmux new-session -d -s <nombre> 'claude'`
- Jarvis envía input con `tmux send-keys`
- Lee output con `tmux capture-pane -p`
- El humano y Jarvis comparten el control de la misma sesión. Hermoso.

### "Fusión Claude + Copilot internamente" NO existe como tal
- Son servicios distintos con APIs distintas, sin estado compartido
- Lo que sí podés armar es un **router** que decide a quién mandar cada pedido
- En v1 nada de esto importa: todo va a Claude. El router es v5+ si alguna vez.

## Estado actual de la conversación

Quedó pendiente la confirmación del plan. Si Juani da OK, el próximo paso concreto es **diseñar la estructura de carpetas y módulos** del repositorio Jarvis.

## Pendiente de capturar cuando avancemos

- Nombre final del proyecto (probablemente "jarvis", confirmar)
- Path donde vive el repo
- Estructura de carpetas final
- Resultados del spike de latencia (semana 1)
- Cuando el proyecto se vuelva activo → mover este conjunto de notas a `01-Proyectos/Jarvis/`
