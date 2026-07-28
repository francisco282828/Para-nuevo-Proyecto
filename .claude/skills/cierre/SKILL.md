---
name: cierre
description: Cierra la sesión guardando una bitácora en Google Drive. Úsalo cuando el usuario quiera terminar, vaciar el chat, "dejar esto por ahora", o pida guardar el estado antes de empezar de cero. También al retomar: leer la última bitácora para recuperar contexto.
---

# Cierre de sesión

El usuario prefiere chats cortos. En vez de arrastrar conversaciones largas,
cierra la sesión con una bitácora en Drive y empieza limpio.

## Dónde

Carpeta de Drive **"Bitácoras de sesión — Claude"**:

```
1QNZ1Lpo7RblBKd9G8SjYjOCglVlssscI
```

Si el ID falla (carpeta movida o borrada), búscala con `search_files`:
`mimeType = 'application/vnd.google-apps.folder' and title contains 'Bitácoras'`.
Si de verdad no existe, créala en `root` y sigue.

## Al cerrar

1. Repasa la conversación completa y extrae lo que un yo futuro, sin memoria,
   necesitaría para retomar.
2. Antes de escribir, **verifica el estado real** — no lo cites de memoria:
   `git status`, `git log --oneline -5`, rama actual, si hay push o PR pendiente.
   Lo que creías al inicio de la sesión pudo cambiar.
3. Crea el documento con `create_file`, `parentId` = la carpeta de arriba,
   `contentMimeType: text/markdown` y **sin** `disableConversionToGoogleType`
   (Drive lo convierte a Google Doc con encabezados reales, legible en celular).
4. Título: `AAAA-MM-DD — <tema en pocas palabras>`. Si ya hay una bitácora
   de ese día sobre lo mismo, actualízala en vez de duplicar.
5. Dale al usuario el `viewUrl` que devuelve la llamada.

### Estructura

```markdown
# Bitácora — <fecha larga>

**Repo:** <owner/repo> · rama <rama>     ← omitir si no hubo código

## Contexto
Qué es esto y en qué punto está. Breve.

## Pendiente 1 — <título>
Qué falta, por qué, y los pasos concretos para hacerlo.

## Decisiones
Lo que se acordó y no debe re-discutirse la próxima vez.
```

Solo secciones con contenido real. Una sesión sin pendientes no necesita
bitácora — dilo y no crees el documento.

Reglas del contenido:

- **Pendientes accionables**: ruta de archivo, comando, nombre de rama. No
  "revisar el README" sino "revertir las dos URLs del README si no se renombra".
- **Decisiones ya tomadas** van explícitas, para no volver a proponerlas.
- **Nada de relleno**: sin narrar el proceso ni listar lo que ya quedó cerrado.
- Sin secretos, tokens ni rutas privadas.

## Al retomar

Cuando el usuario diga "retomemos", "seguimos con lo de antes" o similar sin
más contexto: busca en la carpeta con `search_files` (`parentId = '<id>'`),
lee la más reciente con `read_file_content`, y confirma el estado real contra
git antes de actuar — la bitácora puede haber quedado desactualizada.

## Límite conocido

Esta skill vive en `.claude/skills/` de este repo, así que solo carga en
sesiones abiertas sobre él. Para usarla en otros proyectos hay que copiar la
carpeta `cierre/` al repo correspondiente.
