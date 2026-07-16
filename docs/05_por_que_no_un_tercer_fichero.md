# 05. Por qué descartamos un tercer fichero (`context.md`)

Llega un momento en que el `CLAUDE.md` mezcla dos cosas distintas: **cómo comportarse** (convenciones, comandos, reglas) y **qué es el proyecto** (arquitectura, glosario, decisiones). La idea que surge sola es separar eso en un tercer fichero, un `context.md`. La estudiamos en serio, y la descartamos. Aquí está el porqué, para que nadie tenga que volver a derivarlo.

## Un `context.md` no se carga solo

Lo primero, un dato oficial: Claude Code solo autocarga `CLAUDE.md`, `CLAUDE.local.md`, las reglas de `.claude/rules/` y la auto-memoria `MEMORY.md`. Un fichero inventado llamado `context.md` **no se carga**. Solo entraría de dos formas, y ninguna resuelve nada:

Importado con `@context.md`. Pero un import carga al arranque y no ahorra contexto (ver [04](04_import_descartado.md)). Solo reordena.

Apuntado con un puntero blando, para leerlo bajo demanda. Pero entonces esos hechos no están al arrancar, y además choca con el punto siguiente.

## La trampa de la segunda fuente de verdad

Si ya tienes un fichero de detalle bajo demanda (nuestro puntero blando del [03](03_punteros_y_secundarios.md)), crear un `context.md` **al lado** te deja dos ficheros con el mismo papel: los dos son "el detalle que se lee cuando hace falta". Eso es una segunda fuente de verdad. Dos sitios que hablan de lo mismo, que hay que mantener a la vez, y que un día se contradicen. Es justo el fallo que la regla de una sola fuente de verdad existe para evitar.

## Lo que dice el sector

La idea de separar contexto-de-proyecto de instrucciones-al-agente es buena, y no somos los únicos que la vemos. La comunidad coincide en el **concepto**. Lo que no hay es un nombre estándar ni herramientas que lo carguen. El estándar de facto entre herramientas es otro fichero, `AGENTS.md`, que Claude Code no lee de forma nativa (se trae con `@AGENTS.md`). Un `context.md` a medida es convención suelta, sin nadie detrás que lo autocargue.

Y hay evidencia de que un fichero de contexto mal planteado puede hacer daño. Un estudio académico sobre cientos de tareas midió que un fichero de contexto escrito por un humano daba una mejora marginal, uno generado por un modelo empeoraba el resultado, y ambos subían el coste en torno a un 20%. La causa: **duplican** lo que ya está en el código, y un fichero desactualizado se obedece a ciegas. Añadir ficheros, por sí solo, no ayuda.

## La decisión

No creamos `context.md`. No tiene soporte oficial, un `@import` no descarga, y un tercer fichero bajo demanda duplica el papel del que ya tenemos. Si algún día el problema cambia (muchos proyectos, un incidente real de adherencia), se reabre la decisión con una fecha de revisión, no se deja en el aire.

Lo que sí hicimos fue lo único que la evidencia respalda de verdad: quitar de los ficheros los **inventarios que ya están autogenerados** en otro sitio. Eso es puro beneficio, porque un listado duplicado es coste y una fuente de desactualización, y no aporta nada que el asistente no descubra solo.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Estudio académico citado: evaluación de ficheros de contexto sobre cientos de tareas (2026). Esto puede cambiar. A día de hoy, esta es la decisión.*
