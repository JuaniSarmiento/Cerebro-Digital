# Histórico del grupo

Pegá acá los exports de WhatsApp del grupo (formato `.md` o `.txt`).

## Cómo exportar
1. WhatsApp → chat del grupo → menú → Más → Exportar chat → Sin medios
2. Renombrar el archivo a algo como `2026-05-export-completo.md` y pegarlo en este directorio
3. Restart el bot (`pm2 restart wa-claude`) para que vea los nuevos archivos

## Cómo lo usa el bot
Cuando alguien en el grupo pregunta sobre algo del pasado ("te acordás cuando..."), el bot hace `Grep` recursivo sobre este directorio buscando palabras clave del mensaje.

Si encuentra contexto, lo cita. Si no encuentra, dice "ni idea, no me acuerdo".
