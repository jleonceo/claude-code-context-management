# 04. `@import`: por qué no lo usamos para aligerar

Hay una tentación lógica cuando el `CLAUDE.md` crece: partirlo en trozos y traerlos con `@import`. Parece que así aligeras. No es así, y conviene entender por qué.

## Qué es `@import`

Es una sintaxis oficial. Dentro del `CLAUDE.md` escribes `@ruta/al/fichero` y Claude Code trae ese fichero. Admite rutas relativas y absolutas, y los ficheros importados pueden importar otros, hasta un máximo de cuatro saltos. Para mencionar una ruta sin importarla, se escribe entre acentos graves (`` ` ``).

## Por qué NO ahorra contexto

Lo dice Anthropic con todas las letras: *"Imported files are expanded and loaded into context at launch alongside the CLAUDE.md that references them."* Es decir, el fichero importado **se expande y se carga en el contexto al arrancar**, igual que si lo hubieras escrito dentro del `CLAUDE.md`.

Y por si queda alguna duda, la propia doc lo repite en el apartado de ficheros grandes: *"Splitting into @path imports helps organization but doesn't reduce context, since imported files load at launch."* Partir con `@import` ayuda a organizar, pero **no reduce el contexto**, porque los importados cargan al arranque.

O sea, `@import` mueve texto de un fichero a otro, pero el total que se carga sigue siendo el mismo.

## Cuándo sí sirve

Para **organizar**. Si un `CLAUDE.md` largo se lee mejor partido en piezas temáticas, `@import` te deja tenerlo modular sin perder que todo se cargue. También sirve para un caso concreto de la doc: si tu repo ya usa `AGENTS.md` (el estándar de otros agentes), un `CLAUDE.md` con `@AGENTS.md` hace que las dos herramientas lean lo mismo sin duplicar.

## La regla que sacamos

Nunca `@import` para aligerar peso. Si el objetivo es que se cargue menos contexto, `@import` no lo consigue: hay que ir a los mecanismos **bajo demanda** (reglas con alcance, skills, ficheros de tema, o un puntero blando). Esto no es opinión nuestra: coincide con el dato oficial. Nuestro protocolo lo dice tal cual desde antes de este repo, y aquí queda con su cita.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar. A día de hoy, así funciona `@import`.*
