# 02. Tamaño y límites

Aquí están las cifras. Unas son de Anthropic; otras, convención nuestra. Se dice cuál es cuál.

## `CLAUDE.md`: objetivo por debajo de 200 líneas

La documentación oficial marca el objetivo con estas palabras: *"target under 200 lines per CLAUDE.md file. Longer files consume more context and reduce adherence."* Traducido: apunta a menos de 200 líneas por fichero, porque los ficheros más largos consumen más contexto y hacen que el asistente se adhiera menos a lo que dicen.

Dos matices importantes, y honestos:

Primero, el `CLAUDE.md` se carga **entero**, sea cual sea su longitud. No se trunca. Un `CLAUDE.md` de 500 líneas se carga completo; lo que pasa es que se sigue peor y ocupa más sitio.

Segundo, 200 es un **objetivo, no un acantilado**. No hay una cifra a partir de la cual el fichero "deje de valer". Pero la degradación por longitud sí es oficial: cuanto más largo, menos caso. Por eso el número es útil como referencia, no como frontera mágica.

## `MEMORY.md`: primeras 200 líneas o 25 KB

Con la auto-memoria la regla es distinta, y aquí sí hay corte. La doc oficial: se cargan las *"first 200 lines or 25KB, whichever comes first"*. Lo que pase de ese umbral **no se carga** al arrancar la sesión. Y no hay ningún aviso de que se ha quedado fuera.

Por eso `MEMORY.md` es un índice, no un almacén. El detalle vive en ficheros por tema que se leen bajo demanda, y el índice se mantiene corto para que entre entero.

## Por qué importa el tamaño

Dos razones, ya vistas. Una, la adherencia: un fichero hinchado se sigue peor. Dos, el truncado: en `MEMORY.md`, lo que pasa del umbral es como si no existiera. Un fichero de contexto obeso no es que ocupe más. Es que rinde peor y, en el caso de la memoria, esconde parte de sí mismo.

## Un truco oficial: comentarios que no gastan contexto

Un detalle útil de la doc: los comentarios HTML de bloque (una nota oculta para el mantenedor humano) se **quitan antes** de inyectar el `CLAUDE.md` en el contexto. Sirven para dejar notas a humanos sin gastar tokens. Los comentarios dentro de bloques de código sí se conservan.

## Nuestra convención

Anthropic da los números; nosotros añadimos dos hábitos. Uno, medir el tamaño **en vivo con dos métodos** que coincidan, no a ojo, porque una cifra escrita a mano caduca en cuanto editas el fichero. Dos, dejar un **colchón por debajo del límite** en lugar de rozarlo, para no descubrir el truncado el día menos pensado.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar. A día de hoy, estos son los límites.*
