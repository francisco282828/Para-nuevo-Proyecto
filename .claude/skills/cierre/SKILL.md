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
3. **Confirma la fecha de hoy con `date -u`.** Una sesión larga puede cruzar
   la medianoche, y la fecha del inicio del chat queda obsoleta. Fechar mal la
   bitácora rompe el orden de la carpeta, que es su única forma de navegarse.
4. Crea el documento con `create_file`, `parentId` = la carpeta de arriba,
   `contentMimeType: text/markdown` y **sin** `disableConversionToGoogleType`
   (Drive lo convierte a Google Doc con encabezados reales, legible en celular).
   El `fileSize: 1` de la respuesta es ruido, no significa que salió vacío.
5. Título: `AAAA-MM-DD — <tema en pocas palabras>`.
6. Dale al usuario el `viewUrl` que devuelve la llamada.

**El conector de Drive solo tiene `create_file`: no puede editar ni borrar.**
Así que no existe "actualizar una bitácora". Si ya hay una del mismo día sobre
lo mismo, crea la nueva completa, marca al inicio cuál reemplaza, y dile al
usuario que borre la vieja a mano. Nunca dejes dos vigentes sin decir cuál manda.

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
