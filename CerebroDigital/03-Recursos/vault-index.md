---
fecha: 2026-05-27
fuente: claude-code
tags: [indice, vault, busqueda, sistema]
auto_generated: true
---

# Índice del Vault — Cerebro Digital

> **Para el agente**: LEÉ ESTE ARCHIVO PRIMERO cuando Juani pida buscar algo. Cada nota tiene keywords de búsqueda. Encontrá la nota correcta acá y después leé el archivo completo.
> **Última actualización**: 2026-05-27

---

## Sobre-Mi.md
**Mapa mental / índice raíz del vault.** Punto de entrada que linkea a todo: identidad, proyectos, personas, brújula, intereses.
`keywords: indice, mapa, quien soy, sobre mi, entrada, navegacion`

---

## 01-Proyectos/

### Plataforma-Educativa-IA.md
**Foco actual. Tutor socrático + trazabilidad pedagógica para alumnos universitarios.** 12 microservicios Python/FastAPI. Co-creada con Alberto Cortez (jefe UTN). Es la materialización de su tesis doctoral UNSL. Piloto real en UNSL con 18 estudiantes. Palanca económica para escalar a otras facultades.
`keywords: plataforma, educativa, ia, tutor, socratico, trazabilidad, alberto, cortez, tesis, unsl, utn, microservicios, fastapi, foco, proyecto principal`

### Blend-Software.md
**Empresa co-fundada con Franco y Lautaro.** Paraguas de productos comerciales (BlendPOS, BlendPadel). Juani = Project Orchestrator + Backend Architect.
`keywords: blend, empresa, franco, lautaro, socios, cofundador`

### BlendPOS-V1.md
**POS en producción.** Core en Go + worker Python AFIP. Clientes reales corriendo. Fuente de revenue de Blend Software.
`keywords: blendpos, pos, punto de venta, produccion, go, afip, python, revenue, clientes`

### CerebroDigital.md
**Este vault de Obsidian.** Sistema PKM con estructura PARA. Script de captura automática. Convenciones de frontmatter y wikilinks.
`keywords: cerebro, digital, obsidian, vault, pkm, notas, sistema, captura`

### 2026-05-14-refactor-ai-native-ejercicios-reusables.md
**Sesión larga de refactor en AI-Native N4.** Ejercicios como entidad de primera clase reusable (UUID propio + tabla N:M). 25 ejercicios canónicos PID-UTN cargados. 3 ADRs nuevos (047, 048, 049). Wizard IA standalone para generar ejercicios.
`keywords: refactor, ejercicios, ai-native, n4, adr, pid, wizard, seed, yaml, academic-service`

### En-Pausa/Jarvis.md
**Placeholder corto del proyecto Jarvis en pausa.** La versión activa está en `01-Proyectos/Jarvis/Jarvis.md`.
`keywords: jarvis, pausa`

### En-Pausa/BlendPOS-V2.md
**Evolución de BlendPOS a SaaS multi-tenant.** Pausado. Stack Go + FastAPI.
`keywords: blendpos, v2, saas, multi-tenant, pausa`

### En-Pausa/BlendPadel.md
**Red social + ranking ELO para pádel.** Pausado. Matchmaking por habilidad. Backend probable Go.
`keywords: padel, elo, ranking, matchmaking, red social, deporte, pausa`

### En-Pausa/Aerotec.md
**Dashboards + sensores para aeronaves.** Humedad y temperatura. Discovery. Pausado.
`keywords: aerotec, sensores, aeronaves, iot, dashboards, humedad, temperatura, pausa`

### En-Pausa/PreBot.md
**RAG para estudiantes de medicina.** Roadmap e ingesta completados. Embeddings + vector store. Pausado.
`keywords: prebot, rag, medicina, estudiantes, embeddings, vector, ia, pausa`

### En-Pausa/Buen-Bocado-Empanadas.md
**Negocio gastronómico.** Juani gestiona perfil Google Business. Mantenimiento mínimo. Posible conexión con Luna.
`keywords: buen bocado, empanadas, gastronomia, google business, luna, comida, negocio`

### Jarvis/Jarvis.md
**IA personal por voz — VERSIÓN ACTIVA.** Capa de voz + control GUI sobre Claude Code. Push-to-talk con hotkey. Stack: faster-whisper (STT) + coqui XTTS (TTS) + tmux IPC. Latencia ~4.4s end-to-end. Primer loop funcionando 2026-05-15. Python con uv. Repo: ~/ProyectosPersonales/Jarvis/
`keywords: jarvis, voz, ia personal, stt, tts, whisper, xtts, tmux, push to talk, hotkey, latencia, audio, gpu, cuda`

### Jarvis/2026-05-15-proyecto-jarvis-vision-inicial.md
**Visión completa de Jarvis — 13 capas.** Wake word, voz clonada, OCR, vault indexado, control de apps, WhatsApp, programación delegada, esfera Ultron. Scope de 6 personas / 2 años. Reframeado a "capa sobre Claude Code".
`keywords: jarvis, vision, scope, capas, ultron, esfera, wake word, voz clonada`

### Jarvis/2026-05-15-jarvis-arquitectura-y-plan-de-construccion.md
**Decisiones arquitectónicas + plan v1-v3 en 8 semanas.** Push-to-talk, Silero VAD, memoria = 3 capas Claude, tmux para control de Claude Codes. X11 + GNOME. "Arquitectura para v3, envío v1".
`keywords: jarvis, arquitectura, plan, vad, silero, tmux, x11, gnome, semanas`

### Jarvis/2026-05-15-jarvis-arquitectura-validada-piezas-listas.md
**4 piezas validadas con números.** STT, Claude tmux, TTS, audio I/O. Gotcha de hooks/plugins.
`keywords: jarvis, validacion, piezas, benchmark, hooks`

### Jarvis/2026-05-15-jarvis-primer-loop-funcionando.md
**Primer voice-in / voice-out.** Latencia honesta medida.
`keywords: jarvis, primer loop, voz, latencia, funcionando`

### Jarvis/2026-05-15-jarvis-optimizaciones-resultados.md
**Sesión de optimización autónoma.** Latencia -67% (de 11-14s a ~4.4s).
`keywords: jarvis, optimizacion, latencia, mejora, rendimiento`

### Plataforma-Educativa/2026-05-18-plataforma-educativa-es-tesis-doctoral-cortez.md
**REFRAME CRÍTICO: la plataforma = tesis doctoral de Alberto Cortez en UNSL.** No es proyecto comercial puro. Driver = defensa de tesis. UNSL como institución del piloto. UTN sponsor del PID. Rol de Juani = colaborador técnico (agradecimientos, no autoría). Personas: Carbonari, Garis, Roberti, Martínez, Naveda, Robledo.
`keywords: tesis, cortez, unsl, reframe, academico, pid, doctorado, comercial, plataforma, piloto`

### Plataforma-Educativa/2026-05-20-ai-native-v3-estado-completo-tesis-cortez.md
**Snapshot completo del proyecto AI-Native V3 al 20/05.** 11 servicios Python con puertos. 3 frontends React 19. 4 DBs Postgres. CTR SHA-256. Clasificación N1-N4. Tutor v1.2.0 activado (4 movimientos socráticos). Feature integridad (pestaña + clipboard). Ejercicio canónico edad→categoría. Script start-video-ready.sh. Commit 7ab4c64. Copilot-api como LLM gratis. Cómo retomar el proyecto.
`keywords: ai-native, v3, snapshot, servicios, puertos, react, ctr, sha256, n1-n4, tutor, socratico, integridad, clipboard, pestana, ejercicio, edad, video, copilot-api, retomar, levantar stack`

### TUPAD/2026-05-20-tup-marzo-2026-parcial1-alumnos-sin-entrega.md
**15 alumnos sin entrega del 1er parcial tras 3 instancias.** C1-23: 5 de 19. C1-25: 10 de 21. Alerta: recuperatorio (assign 11381) posiblemente mal configurado. Próxima instancia 25/06/2026. Lista de nombres completa.
`keywords: tupad, parcial, alumnos, entrega, c1-23, c1-25, seguimiento, assign, recuperatorio, marzo 2026`

---

## 02-Conocimiento/

### Perfil-Profesional.md
**Rol: Project Orchestrator + Backend Architect.** 20 años, Mendoza. Delega código a IA. Aspiracional: Senior Architect. Profesor apasionado. Rechaza inmediatismo.
`keywords: perfil, profesional, rol, orchestrator, architect, identidad, profesor`

### Formacion-Academica-UTN.md
**TUP UTN Mendoza, último semestre, legajo 52760.** Tutor TUPAD: Prog 1 (activo) + Prog 4 (agosto). Arreglo con Alberto = promoción sin cursar. Escuela primaria/secundaria Casa de María.
`keywords: utn, tup, carrera, legajo, tupad, programacion, profesor, tutor, alberto, casa de maria, ultimo semestre`

### Filosofia-de-Trabajo.md
**4 pilares: conceptos > código, IA como herramienta, fundaciones sólidas, contra inmediatismo.** Aplica a cómo enseña y cómo trabaja con IA.
`keywords: filosofia, principios, conceptos, codigo, inmediatismo, fundaciones, pilares`

### Expertise-Tecnica.md
**Dominios profundos: Clean/Hexagonal/Screaming Architecture, Atomic Design, TDD, SDD.** Exige documentación exhaustiva. Testing obligatorio.
`keywords: expertise, arquitectura, clean, hexagonal, screaming, atomic, tdd, testing, patrones`

### Stack-Tecnologico-Lenguajes.md
**Go (performance/concurrencia) + Python/FastAPI (ecosistema IA).** División deliberada. Go = plataforma/APIs, Python = IA/agentes/ML.
`keywords: stack, go, golang, python, fastapi, lenguajes, ia, ml, backend`

### Objetivos-12-Meses.md
**3 ejes brújula hasta mayo 2027:** cambiar auto (Corsa Classic 2011), superar $2.5M ARS/mes (hoy $1.5M, agosto >$2M con Prog 4), salud familiar. Palanca: escalar plataforma a 2da facultad. Gap de $500K sobre proyección agosto.
`keywords: objetivos, brujula, auto, corsa, plata, ingresos, 2.5 millones, salud, familia, 12 meses, metas`

### Trazabilidad-Pedagogica.md
**Modelo conceptual profundo.** 4 capas (audit log, trayectoria razonamiento, integridad académica, telemetría tutor). Event sourcing. Privacidad (Ley 25.326, GDPR). Decisión arquitectónica: servicio centralizado vs híbrido. Métricas desbloqueadas.
`keywords: trazabilidad, pedagogica, audit, eventos, event sourcing, privacidad, compliance, metricas, capas`

### Area-Analisis-Datos-Futbol.md
**Pasión declarada: IA no-generativa para fútbol.** Scouting, análisis táctico, predicción, xG. Stack: Go ingesta + Python ML. Fuentes: FBref, StatsBomb, Understat. Vertical elegida: formación óptima para River.
`keywords: futbol, datos, analytics, ml, scouting, xg, river, formacion, ia no generativa, statsbomb, fbref`

### Concepto-Dominio-Futbol.md
**Qué significa "dominio" en analytics moderno.** 4 dimensiones: territorio (Field Tilt), presión (PPDA), volumen ofensivo (xG/touches in box), privación (xG concedido). Guardiola, Bielsa, Klopp, Sarri. Función objetivo del modelo River.
`keywords: dominio, futbol, tactica, field tilt, ppda, xg, guardiola, bielsa, klopp, presion, territorio`

### Pregunta-Tactica-Optima-River.md
**¿Qué formación maximiza rendimiento de River?** Función objetivo definida: max xG vía dominio absoluto. Pesos: xG 0.40 + Field Tilt 0.20 + PPDA 0.20 + touches 0.20. Modelo: roster + formación → Hungarian algorithm → ranking. MVP 4 semanas. Stack Python (soccerdata, scipy). Formaciones probables: 4-3-3, 4-2-3-1, 3-4-3, 3-1-4-2.
`keywords: river, formacion, tactica, xg, modelo, mvp, hungarian, ranking, jugadores, plantel`

### 2026-05-18-reframe-tupad-asincronico-y-trayectoria-2024-2026.md
**Trayectoria real corregida:** 2024-verano 2026 = estudiante (gym ok), verano 2026-hoy = programador + profesor TUPAD (gym cayó). TUPAD es asincrónico → trampa de expansión del trabajo. Probable que TUPAD = misma tecnicatura que estudió.
`keywords: tupad, asincronico, trayectoria, gym, cansancio, transicion, estudiante, profesor, energia`

### 2026-05-18-trabajo-doble-tupad-programador-ingreso.md
**Causa raíz del cansancio: sumó TUPAD al laburo de programador.** Ingreso $1.5M pero "más cargado". El frío era proxy del agotamiento real. Doble trabajo = mismo día.
`keywords: trabajo, doble, tupad, programador, ingreso, 1.5 millones, cansancio, causa raiz, gym`

### Personas/Alberto.md
**Alberto Cortez. Jefe carrera programación UTN Mendoza. Doctorando UNSL.** Co-creator plataforma educativa. Arreglo: trabajo a cambio de promoción académica. Nodo más denso de la red. Pregunta abierta: ¿qué pasa post-graduación?
`keywords: alberto, cortez, jefe, utn, unsl, doctorando, tesis, arreglo, promocion, mentor`

### Personas/2026-05-18-aclaracion-alberto-cortez-utn-unsl.md
**Alberto Cortez = UNA sola persona.** Consolidación de identidad. Plataforma = tesis doctoral UNSL. UNSL como piloto real. Personas del PID: Carbonari, Garis, Roberti, etc. Target $2.5M tiene supuesto débil si Cortez prioriza publicar sobre comercializar.
`keywords: alberto, cortez, consolidacion, unsl, tesis, pid, carbonari, garis, supuesto economico`

### Personas/Luna.md
**Pareja sentimental.** Colaboración en emprendimientos gastronómicos. Posible overlap con Buen Bocado.
`keywords: luna, pareja, novia, gastronomia`

### Personas/Franco-y-Lautaro.md
**Socios co-fundadores de Blend Software.**
`keywords: franco, lautaro, socios, blend, cofundadores`

### Personas/Familia.md
**Núcleo familiar.** Claudio (papá), Viviana (mamá), Thiago (hermano), Nona (abuela), Nono (abuelo). Restricción de optimización: familia siempre gana.
`keywords: familia, claudio, viviana, thiago, nona, nono, papa, mama, hermano, abuelos`

### Personas/Claudio.md
**Papá.** Placeholder, datos pendientes.
`keywords: claudio, papa, padre, familia`

### Personas/Viviana.md
**Mamá.** Placeholder, datos pendientes.
`keywords: viviana, mama, madre, familia`

### Personas/Thiago.md
**Hermano.** Placeholder, datos pendientes.
`keywords: thiago, hermano, familia`

### Personas/Nona.md
**Abuela.** Así la llama. Placeholder.
`keywords: nona, abuela, familia`

### Personas/Nono.md
**Abuelo.** Así lo llama. Placeholder.
`keywords: nono, abuelo, familia`

### Personas/Amigos-Infancia.md
**Grupo Pastela — 8 amigos, +11 años.** Santi, Tizi, Juan Cruz, Augusto, Joaco, Nico, Juani (el otro), yo. Escuela Casa de María. Fútbol como vínculo. Infraestructura emocional estable.
`keywords: pastela, amigos, infancia, grupo, santi, tizi, juan cruz, augusto, joaco, nico, casa de maria, futbol`

### Personas/Santi.md
**Amigo de toda la vida (+11 años).** Trío inseparable con Tizi y Juan Cruz. Jugaron fútbol juntos de pibes.
`keywords: santi, amigo, infancia, futbol, pastela`

### Personas/Tizi.md — Amigo infancia, Pastela.
### Personas/Juan-Cruz.md — Amigo infancia, Pastela.
### Personas/Augusto.md — Amigo infancia, Pastela.
### Personas/Joaco.md — Amigo infancia, Pastela.
### Personas/Nico.md — Amigo infancia, Pastela.
### Personas/Juani-Pastela.md — "El otro Juani" del grupo Pastela.

### Personal/Finanzas.md
**SENSIBLE. Ingresos: $1.5M ARS/mes (mayo 2026).** Proyección agosto >$2M con Prog 4. Target $2.5M a 12 meses. Gap $500K. Fuentes: TUPAD (monetario), Alberto (promoción académica), BlendPOS (revenue lateral). Palanca: plataforma escale fuera UTN.
`keywords: finanzas, ingresos, plata, sueldo, 1.5 millones, 2.5 millones, tupad, revenue, ahorro, sensible`

### Personal/Dia-Tipo.md
**Rutina entre semana.** 08:00 despertar + café. Mañana: plataforma con Alberto. Media mañana: desayuno (2 tostadas + yogur). Siesta: TUPAD. Post-jornada: gym. Desayuno post-arranque (perfil que arranca rápido). Gym al final = premio, no instrumento.
`keywords: rutina, dia tipo, horario, mañana, siesta, gym, cafe, desayuno, alberto, tupad, bloques`

### Personal/Lifestyle-e-Intereses.md
**Vida no-técnica.** Gym hipertrofia + Amazfit + creatina. River + análisis táctico + FM24. Ropa oversized (wide-leg, hoodies). Fragancias (Azzaro, Acqua di Giò, Polo 67, Givenchy). Café especialidad + fast food. Auto Corsa Classic 2011. Viajes Argentina↔Chile. Tokio.
`keywords: lifestyle, intereses, gym, hipertrofia, amazfit, creatina, ropa, fragancias, cafe, fast food, mcdonalds, auto, corsa, chile, japon, tokio, oversized`

### Personal/Hincha-de-River.md
**Identidad: hincha fanático enfermo de River Plate.** Análisis táctico profundo. Football Manager 24. Motor de Area-Analisis-Datos-Futbol. Mismo músculo mental que Project Orchestrator.
`keywords: river, plate, futbol, hincha, fanatico, fm24, football manager, tactica`

### Personal/2026-05-27-reunion-KINESIOLOGA.md
**Reunión kinesióloga 27/05/2026.** Lumbar y cervical muy contracturadas. Iniciar tratamiento. Próximo turno semana que viene.
`keywords: kinesiologa, contractura, lumbar, cervical, espalda, salud, turno, tratamiento`

### Personal/2026-05-18-cronotipo-gym-siesta-tarde-noche.md
**Ventana natural de gym: siesta o tarde-noche.** NO entrena a la mañana. Cronotipo confirmado. Decisión firme de volver al gym. Bloque a proteger = tarde.
`keywords: cronotipo, gym, horario, siesta, tarde, noche, volver, bloque`

### Personal/2026-05-18-gym-pausa-y-vuelta-comida-viaje-chile.md
**Gym: empezó marzo 2024, dejó abril 2026 (~2 años).** Motivo: cansancio + frío. Plan: volver. Comida mejorando. Quiere viajar a Chile a comprar ropa. Adherencia existe, el problema es energía/contexto.
`keywords: gym, pausa, vuelta, comida, alimentacion, chile, ropa, cansancio, frio, viaje`

### Personal/2026-05-20-preferencia-no-usar-tocar-base.md
**NO usar "tocar base" en mensajes.** Reemplazos: "te escribo para...", "quería saber cómo andás", "pasaba a ver si...".
`keywords: redaccion, estilo, tocar base, preferencia, mensajes, alumnos, comunicacion`

---

## 03-Recursos/

### Stock-Alimentos.md
**Stock actual de alimentos en casa.** Compra semanal: lunes. Yogur griego (6u, 8.8g prot c/u), leche proteica La Serenísima (4L), granola (290g), pan lactal Bimbo (350g), manteca de maní. Info nutricional completa (calorías, proteínas, azúcares). Historial de compras.
`keywords: alimentos, stock, comida, compras, yogur, leche, granola, pan, manteca mani, proteina, calorias, nutricion, desayuno, cocina, casa`

### Reglas-Colaboracion-IA.md
**Reglas innegociables para IA.** No Co-Authored-By, conventional commits, no buildear auto, usar bat/rg/fd/sd/eza, respuestas cortas, una pregunta a la vez, verificar antes de afirmar, voseo rioplatense.
`keywords: reglas, ia, colaboracion, commits, respuestas, verificacion, idioma, tono`

### Stack-Herramientas.md
**Herramientas de desarrollo.** LazyVim, Tmux, Zellij. CLI: bat, rg, fd, sd, eza. Pop!_OS, kernel 6.18, bash. Teclado Aula F75 Pro (switches Reaper). Amazfit. Suscripciones: Google AI Pro + Anthropic Max.
`keywords: herramientas, editor, lazyvim, tmux, zellij, bat, ripgrep, fd, cli, teclado, aula, amazfit, suscripciones, anthropic, google ai`

### Sistema-Memoria-Engram.md
**Sistema de memoria persistente.** Razonar antes de guardar. Generoso con búsqueda, mezquino con guardado. 0-2 mem_save por sesión. Buenos candidatos: gotchas, decisiones arquitectónicas (por qué), preferencias, continuidad multi-día.
`keywords: engram, memoria, persistente, guardar, buscar, sesion, compactacion`

### Skills-Claude-Code.md
**Skills auto-cargadas por contexto.** Go testing, skill creator, Impeccable (UI/UX con gates PRODUCT.md + DESIGN.md). Carga automática antes de escribir código.
`keywords: skills, claude code, auto carga, impeccable, go testing, ui, ux`

### Workflows-SDD-OPSX.md
**Dos flujos spec-driven.** SDD (grande, sub-agentes, multi-batch): proposal→specs→design→tasks→apply→verify→archive. OPSX (liviano, inline): explore→propose→apply→archive. Default: Interactive mode.
`keywords: sdd, opsx, workflow, spec, proposal, design, tasks, apply, verify, archive`

### 2026-05-24-prueba-capacidades-taskflow-mcps.md
**Prueba de capacidades del VPS.** TaskFlow demo (FastAPI + React). MCPs: Google Drive y Notion ok, Canva disponible, Playwright pendiente. VPS: 1 core, 3.8GB RAM, 48GB disco. Explicación para Luna de qué puede hacer el asistente.
`keywords: taskflow, demo, mcps, google drive, notion, canva, playwright, vps, capacidades, luna`

---

## 04-Plantillas/

### Nota-Inbox.md
**Template para notas nuevas en Inbox.** Frontmatter: fecha, hora, fuente, tags.
`keywords: plantilla, template, inbox, frontmatter`

---

## 03-Recursos/Logs-WhatsApp/
**~30 logs de conversaciones WhatsApp (mayo 2026).** Sesiones de chat capturadas automáticamente. No indexados individualmente — buscar por fecha si se necesita contexto de una conversación específica.
`keywords: whatsapp, logs, chat, conversaciones, historial`
