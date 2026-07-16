# 07. El hook, y su utilidad para este problema

Un hook no es exactamente parte de la gestión de los ficheros de contexto, pero está tan cerca que dejarlo fuera sería un hueco. Lo metemos aquí, sopesando si sirve para este problema o no.

## La distinción que hay que entender

Ya lo dijimos en [01](01_los_dos_ficheros.md): el `CLAUDE.md` y la memoria son **guía**, no imposición. El asistente los lee y trata de seguirlos, pero no hay garantía. Anthropic es explícito: *"To block an action regardless of what Claude decides, use a PreToolUse hook instead."* Para bloquear una acción pase lo que pase el asistente, se usa un hook, no una instrucción en el `CLAUDE.md`.

Un hook es código que se ejecuta en un momento fijo del ciclo (antes de una herramienta, al terminar, etc.), y se cumple lo decida lo que lo decida el modelo. La diferencia es esa: la instrucción es intención, el hook es mecanismo.

## Un hook oficial que ayuda aquí

La doc menciona uno útil para este tema: el hook `InstructionsLoaded`, que registra exactamente **qué ficheros de instrucciones se cargan, cuándo y por qué**. Sirve para depurar cuando algo no se está siguiendo y sospechas que ni siquiera se ha cargado. Antes de dar por hecho que el asistente "ignora" tu `CLAUDE.md`, este hook te dice si lo ha leído.

## ¿Sirve un hook para el problema del tamaño?

Aquí va el juicio honesto. El problema de este repo es que los ficheros no engorden. ¿Encaja un hook? Sí, como **guardarraíl**: un hook que mida el `CLAUDE.md` y el `MEMORY.md` y avise (o falle) si pasan del límite convierte una buena intención ("hay que mantenerlos cortos") en un mecanismo que se cumple solo. Eso es exactamente pasar de intención a mecanismo, que es lo que buscamos.

Pero con un matiz que hemos aprendido a base de tropezar: **un hook da falsa confianza**. Cubre una puerta, no todo el recurso. Un guardián que vigila el tamaño no garantiza que el contenido sea bueno, ni que no haya duplicados, ni que un fichero esté al día. Hace una cosa concreta, y solo esa. El trabajo de fondo (qué se queda, qué se mueve, qué se borra) sigue siendo criterio, no automatismo.

## El veredicto

Un hook vale como **segunda capa**: un vigilante barato que caza el desbordamiento de tamaño y te avisa. No vale como **primera línea**: no sustituye la disciplina de decidir qué va en cada fichero. Úsalo para lo que sabe hacer, que es cumplir una regla mecánica, y no le pidas el juicio, que no lo tiene.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory y Hooks) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar. A día de hoy, así encaja el hook.*
