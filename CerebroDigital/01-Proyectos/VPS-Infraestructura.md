---
fecha: 2026-05-24
hora: 12:00
fuente: claude-code (sesion-investigacion)
tags: [proyecto, vps, infraestructura, activo, wa-claude, zepp, taskflow]
estado: activo
prioridad: media
---

# VPS — Infraestructura personal

Servidor personal de Juani corriendo 24/7. Funciona como la versión cloud del ecosistema Jarvis: asistente WhatsApp, datos de salud, vault sincronizado y workspace remoto.

## Acceso

- **IP**: 2.24.122.56
- **Usuario**: claude-bot
- **OS**: Ubuntu (kernel 6.8.0-111-generic), x86_64
- **SSH desde local**: `ssh claude-bot@2.24.122.56` (clave ed25519 de juanisarmiento)
- **Hostname interno**: srv1697196
- **Home**: `/home/claude-bot`

## Claude Code en la VPS

Claude Code instalado con config propia adaptada al contexto servidor:

- **CLAUDE.md**: `/home/claude-bot/.claude/CLAUDE.md` — personalizado para VPS (modelo Sonnet por defecto, sugiere Opus para tareas pesadas)
- **Output style**: Gentleman (mismo que local)
- **Plugin**: Engram activo
- **Permisos**: `bypassPermissions` (la VPS es entorno controlado)
- **Skills transferidas** (2026-05-24): 17 esenciales + commands opsx/SDD + agents SDD

## Proyectos activos

### wa-claude — Bot WhatsApp 24/7

Asistente personal vía WhatsApp usando Claude Agent SDK + Baileys.

- **Path**: `~/proyectos/wa-claude/`
- **Stack**: Node.js, Baileys (WhatsApp Web), Claude Agent SDK
- **Función**: socio/secretario 24/7. Acceso completo al VPS, puede ejecutar comandos, editar archivos, consultar el vault Obsidian y pushear a GitHub.
- **Anti-ban**: delays aleatorios, chunking de mensajes, rate limiting (8 msg/min max)
- **Owners**: configurados en `OWNERS` array de `index.js` (LID + número WA de Juani)
- **Workdir**: `~/proyectos/workdir/` — ahí ejecuta comandos y crea archivos cuando Juani lo pide por WhatsApp
- **[[CerebroDigital|Cerebro Digital]]**: puede guardar conversaciones significativas en `00-Inbox/` del vault y pushear a GitHub
- **System prompt**: mismo estilo rioplatense, contexto completo de Juani, reglas de conventional commits

### zepp-health-cli — Datos de salud Amazfit

CLI para consultar datos de salud desde la API de Zepp (Amazfit).

- **Path**: `~/proyectos/zepp-health-cli/`
- **Script**: `zepp_health.py`
- **Config**: `config.json` (app_token, user_id, host)
- **Subcomandos**: summary, heart-rate, temperature, events (hrv, stress, body-battery, readiness, respiratory), sport-load, vo2, weight, band-data (sleep/steps), blood-pressure, run-history
- **Presets de events**: hrv, hrv-rmssd, stress, body-battery, readiness, respiratory, blood-pressure, emotion, daily-health
- **Gotcha**: el token expira cada ~30 días, hay que recapturar con proxy HTTPS (Charles) desde la app Zepp en el celular

### taskflow — Task manager demo

Aplicación demo de gestión de tareas.

- **Path**: `~/proyectos/taskflow/`
- **Stack**: FastAPI + React/TypeScript (Vite)
- **Backend**: Python 3.12, FastAPI, Pydantic v2 — puerto 8000
- **Frontend**: React 19, TypeScript, Vite — puerto 5173
- **API**: REST CRUD en `/api/v1/tasks`

### Cerebro-Digital — Vault Obsidian sincronizado

Clon del vault de Obsidian para que wa-claude pueda consultarlo y escribir en él.

- **Path**: `~/proyectos/Cerebro-Digital/`
- **Repo**: `git@github.com:JuaniSarmiento/Cerebro-Digital.git`
- **Git config**: user=JuaniSarmiento, email=juanisarmientoomartinez@gmail.com
- **Push**: `cd ~/proyectos/Cerebro-Digital && git add . && git commit -m "msg" && PATH=$HOME/bin:$PATH gh auth setup-git && git push`

## Servicios del sistema

- **Docker**: instalado y corriendo (containerd + docker.service), sin contenedores activos al 2026-05-24
- **SSH**: OpenBSD Secure Shell server
- **Seguridad**: Monarx Agent (security scanner) activo
- **Cron**: activo para tareas programadas

## Relación con [[Jarvis|Jarvis (local)]]

La VPS y Jarvis son complementarios:

| Aspecto | Jarvis (local) | VPS |
|---------|---------------|-----|
| Interfaz | Voz (push-to-talk) | WhatsApp + SSH |
| Claude | tmux persistente, Sonnet | Claude Code standalone |
| Disponibilidad | Cuando la PC está encendida | 24/7 |
| GPU | RTX 3050 6GB (STT/TTS) | Sin GPU |
| Casos de uso | Programación, control GUI, pensamiento en voz alta | Mensajería, salud, tareas remotas |

A futuro, Jarvis podría orquestar la VPS como worker remoto (v3+: múltiples Claude Codes coordinados).

## Historial de config

- **2026-05-24**: SSH configurado desde máquina local (clave ed25519). Skills esenciales transferidas (17 skills + opsx + SDD).
