---
fecha: 2026-05-15
hora: 12:00
fuente: claude-code (sesion-conversacional)
tags: [inbox, sin-revisar, proyecto, jarvis, ia-personal, vision]
---

# Proyecto Jarvis — Visión inicial

Juani compartió la visión completa de un proyecto personal: **[[Jarvis]]**, una IA personal corriendo 24/7 en su máquina, con voz, oídos, ojos y manos sobre el sistema. Documento extenso de scope, presentado como una sola idea integrada.

## Resumen de las capas

1. **Sentidos**: wake word "Jarvis", voz clonada natural, OCR de pantalla, voice ID (solo le obedece a él).
2. **Cerebro y memoria**: routing entre Claude / Copilot / modelo local; memoria viviendo en el [[CerebroDigital|vault]] de Obsidian, indexado vectorialmente, re-indexación automática, conexiones [[]] auto-generadas.
3. **Manos**: control total de apps, archivos, comandos del sistema, monitor de salud.
4. **Segundo cerebro activo**: captura por voz al vault, reorganización inteligente, revisión espaciada, modo investigación, daily note automática.
5. **Comunicación con el mundo**: mails, WhatsApp (con voz clonada opcional), calendario, recordatorios, resúmenes de YouTube / artículos, traducción en vivo.
6. **Programación delegada a Claude Code**: Jarvis no programa, es el intermediario. Recupera contexto del vault, arma prompts, lanza Claude Code, monitorea múltiples instancias en paralelo, hace triaje de costos.
7. **Multimedia**: edición de imágenes por voz, generación local (SD/Flux), transcripción, lectura de PDFs.
8. **Modo desarrollo personal**: tutor, sparring intelectual, coach de proyectos, journaling.
9. **Personalidad consistente**: modos contextuales (mañana / tarde / noche / "estoy hecho mierda"), modo silencioso, aprendizaje de rutinas.
10. **Proactividad**: briefings, detector de patrones, sugerencias contextuales, iniciativa propia acotada.
11. **Interfaz visual — esfera Ultron**: orbe flotante always-on-top con estados visuales (idle / escuchando / pensando / ejecutando / hablando), grafo 3D de Obsidian flotando alrededor, stream de pensamiento. Paleta: negro profundo, violeta eléctrico, cian, blancos cálidos.
12. **Seguridad**: confirmación para acciones destructivas, logs auditables, voice ID + lockdown, kill switch, sandboxing, sin grabación permanente.
13. **Extensibilidad**: plugin system, MCP servers, API local, futuro modo móvil.

## El día ideal que describió

Wake up → briefing → "modo programación proyecto X" (todo se acomoda solo) → conversación natural mientras Jarvis recupera notas del vault → dicta el prompt a Claude Code → mientras tanto Juani manda audios con voz clonada a su vieja → vuelve, supervisa Claude Code por voz → piensa en voz alta y Jarvis va escribiendo en Obsidian linkeado → a la noche organiza descargas mientras duerme → daily note automática. **Casi sin tocar mouse ni teclado**.

## Mi devolución técnica (resumida)

- La visión cierra conceptualmente y nada es técnicamente imposible — cada pieza existe (Picovoice, XTTS, Piper, Resemblyzer, LlamaIndex, MCP, etc.).
- Pero **no es UN proyecto, son veinte disfrazados**. Es scope de 6 personas / 2 años. Riesgo alto de morir por ambición.
- Tres puntos donde la visión no cierra como la planteó:
  - "Fusión interna Claude + Copilot transparente" → no existe, hay que diseñar un router con reglas.
  - "Múltiples Claude Codes coordinados" → Claude Code no está pensado para ser orquestado por otro proceso; se puede pero es áspero.
  - "Memoria = Obsidian" → necesita dos memorias mínimo (vault para largo plazo + SQLite/Redis para estado vivo). Obsidian es grafo de notas, no DB operativa.

## Próximo paso

Le pregunté cuál es **la UNA capacidad** que, sola, si la tuviera andando en 3 semanas, ya valdría la pena. Esa va a ser la cuña por donde empezar.

## Pendiente de capturar cuando responda

- Qué capacidad eligió como cuña inicial
- Si esto se convierte en proyecto activo → mover esta nota a `01-Proyectos/Jarvis/` con sub-notas por capa
