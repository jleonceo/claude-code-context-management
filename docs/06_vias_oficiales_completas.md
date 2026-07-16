# 06. Todas las vías oficiales

Hasta aquí hemos usado unas pocas. Pero Anthropic ofrece más mecanismos para organizar y aligerar el contexto, y conviene tenerlos todos a la vista, aunque no uses todos. Esta es la lista, con cuándo sirve cada uno.

## Reglas con alcance (`.claude/rules/`)

Un directorio donde pones ficheros markdown, uno por tema (`testing.md`, `security.md`). Se descubren solos. Si un fichero de regla lleva un campo `paths` en su frontmatter, solo se carga cuando Claude toca ficheros que casan con ese patrón. Sin `paths`, se carga siempre, con la misma prioridad que el `CLAUDE.md` del proyecto. Es la vía oficial para trocear instrucciones por zona del código y que solo aparezcan donde importan.

## Skills

Paquetes de instrucciones que cargan **solo cuando se invocan** o cuando el asistente ve que hacen falta. El sitio natural de un procedimiento largo de varios pasos que no tiene que estar en contexto todo el rato. La doc lo dice: para instrucciones que no necesitan estar siempre, usa skills en vez de reglas.

## `CLAUDE.md` de organización (política gestionada)

Una empresa puede desplegar un `CLAUDE.md` central que se aplica a todas las máquinas y que un usuario no puede excluir. Va en una ruta de sistema o dentro de la configuración gestionada. Sirve para estándares de código, seguridad y cumplimiento a nivel de toda la organización.

## Excluir ficheros ajenos (`claudeMdExcludes`)

En un monorepo, los `CLAUDE.md` de otros equipos por encima de tu carpeta se cargan también, y puede que no te interesen. Con `claudeMdExcludes` en la configuración saltas ficheros por ruta o patrón. Menos ruido, menos contexto gastado en instrucciones que no son tuyas.

## Comentarios HTML que no gastan contexto

Los comentarios HTML de bloque en el `CLAUDE.md` se quitan antes de inyectar el contenido. Notas para el mantenedor humano que no cuestan tokens. Un detalle pequeño y muy práctico.

## El recorte de `/doctor`

Desde la versión 2.1.206, el chequeo de `/doctor` **propone recortes** de un `CLAUDE.md` versionado. Quita lo que el asistente puede deducir del propio código (estructura de carpetas, listas de dependencias, resúmenes de arquitectura) y conserva los avisos, el porqué y las convenciones que se apartan de lo que la herramienta hace por defecto. Es una segunda opinión automática sobre qué sobra.

## `/memory` y `--append-system-prompt`

`/memory` lista los ficheros de contexto cargados en la sesión y te deja abrirlos y editarlos. Y si necesitas una instrucción a nivel de prompt de sistema (no de contexto), está `--append-system-prompt`, que hay que pasar en cada arranque, más pensado para scripts que para uso interactivo.

## `AGENTS.md`

El estándar abierto que leen otras herramientas de agentes. Claude Code no lo lee de forma nativa: lee `CLAUDE.md`. Si tu repo ya tiene un `AGENTS.md`, la vía es un `CLAUDE.md` con `@AGENTS.md`, y así las dos herramientas leen lo mismo sin duplicar.

## Directorios adicionales

Con `--add-dir` das acceso a carpetas fuera de tu directorio de trabajo. Por defecto sus `CLAUDE.md` no se cargan; para que sí, hay una variable de entorno que lo activa.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Los números de versión y las opciones pueden cambiar. A día de hoy, estas son las vías.*
