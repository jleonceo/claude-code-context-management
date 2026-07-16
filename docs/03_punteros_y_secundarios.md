# 03. Punteros y ficheros secundarios

Ya sabemos el problema: lo que se carga siempre ocupa contexto, y si crece, rinde peor. La solución se dice pronto, aunque aplicarla no sea tan simple: sacar el detalle del fichero que se carga siempre y llevarlo a otro que solo se lea cuando haga falta.

## Lo que se carga bajo demanda (oficial)

Anthropic da tres vías para esto, y las tres cargan solo cuando toca:

Los **ficheros de tema de la memoria** (tipo `debugging.md`). La doc lo dice claro: *"Topic files are not loaded at startup. Claude reads them on demand using its standard file tools when it needs the information."* No entran al arrancar; el asistente los abre cuando los necesita.

Las **reglas con alcance** (`.claude/rules/` con un campo `paths` en el frontmatter). Son instrucciones que solo se cargan si Claude toca ficheros que casan con un patrón. Por ejemplo, una regla para `src/api/**/*.ts` que solo aparece cuando trabajas en la API.

Las **skills**. Se cargan solo cuando las invocas o cuando el asistente ve que hacen falta para tu petición. Son el sitio natural de un procedimiento largo de varios pasos que no tiene que estar en contexto todo el rato.

## Nuestro puntero blando

Encima de esas vías, nosotros usamos un patrón propio para el `CLAUDE.md`: el **puntero blando**. El detalle que solo hace falta de vez en cuando se mueve a un fichero aparte (llamémoslo el fichero de detalle), que no se carga al arrancar. En el `CLAUDE.md` queda una línea que dice, en la práctica, "para esto, mira allí". El asistente la lee y, si necesita el detalle, abre el otro fichero.

No es la única forma. Las reglas con alcance y las skills son las vías oficiales para lo mismo. El puntero blando es una alternativa cuando quieres un único sitio de detalle general, no troceado por patrón de fichero.

## La disciplina: mover entero, nunca copiar

Aquí está el truco, y es una regla dura: al descargar peso, se **mueve el contenido entero** y se deja un puntero. **Nunca se copia.**

¿Por qué? Porque dos ficheros que dicen lo mismo son dos sitios donde corregir. Y tarde o temprano corriges uno y te olvidas del otro. Entonces tienes dos versiones que se contradicen, y el asistente no sabe cuál vale. Esto es **una sola fuente de verdad**, y es la regla que evita el fallo más común de todo esto.

## El núcleo y el fichero de detalle

La forma que sale de todo esto tiene dos capas. Un **núcleo** corto y estable, que se carga siempre. Y un **fichero de detalle**, aparte, que se trae solo cuando hace falta. No es un invento nuestro: repos de referencia que trabajan este problema llegan a la misma estructura de dos capas, con un fichero raíz de entrada por debajo de 50 o 60 líneas. Que varias personas lleguen por separado a la misma conclusión no lo demuestra, pero da confianza en el patrón.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar. A día de hoy, estas son las vías.*
