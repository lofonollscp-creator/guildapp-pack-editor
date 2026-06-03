# GuildApp Pack Editor

Editor web de paquetes de contenido para GuildApp.

URL esperada de publicación:

- `https://lofonollscp-creator.github.io/guildapp-pack-editor/`

Qué permite esta versión web:

- crear y editar paquetes;
- importar y exportar JSON;
- cargar el catálogo remoto de `guildapp-content`;
- preparar paquetes remotos y marcarlos como `Próximamente`;
- publicar contenido en GitHub usando un token con permiso `Contents: Read and write`.

Limitaciones reales del modo web:

- no existe proxy local para Ollama Cloud;
- no existe endpoint local `/__validate_pack`;
- la autoconfiguración de claves locales solo funciona cuando el editor se abre en `localhost`.

Para trabajo local completo, usa el editor servido desde:

- `python3 editor/guildapp_editor_server.py`
