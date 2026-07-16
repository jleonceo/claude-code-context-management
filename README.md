# claude-code-context-management

**Cómo evitar que los ficheros de contexto de Claude Code (`CLAUDE.md` y `MEMORY.md`) crezcan hasta saturar el contexto. Una arquitectura de descarga fundamentada en la documentación oficial de Anthropic: teoría, mecanismos y un protocolo probado.**

> **Vigencia.** Verificado contra la documentación oficial de Anthropic el **16 de julio de 2026**. Esto puede cambiar: Claude Code evoluciona rápido. A día de hoy, esta es *una* forma de gestionar estos ficheros, con su porqué. No es una verdad fija ni la única manera.

## El problema

Claude Code empieza cada sesión sin memoria. La ventana de contexto está en blanco. Dos ficheros le devuelven lo que necesita saber de una sesión a la siguiente:

- **`CLAUDE.md`**: las instrucciones que escribes tú. Cómo se trabaja en el proyecto, convenciones, avisos.
- **`MEMORY.md`**: las notas que Claude se guarda solo, a partir de tus correcciones y preferencias.

Los dos se cargan al arranque de cada sesión. Y ahí está el problema: esa carga ocupa parte del contexto, que es el espacio que Claude tiene para pensar y trabajar. Si los ficheros engordan sin freno, pasan dos cosas, y las dos las dice Anthropic.

La primera: el fichero largo se sigue peor. La documentación oficial lo dice con estas palabras, *"Longer files consume more context and reduce adherence"*. Cuanto más largo el `CLAUDE.md`, menos caso le hace Claude.

La segunda: parte deja de cargarse. Del `MEMORY.md` solo entran las primeras 200 líneas o 25 KB, lo que llegue antes. Lo que pase de ahí no se carga al arrancar. Y no hay aviso.

Un fichero de contexto obeso no es que ocupe más: es que rinde peor. Este repo cuenta cómo mantener esos ficheros útiles sin que se coman el contexto. Con los mecanismos oficiales de Anthropic, y con una disciplina de ingeniería encima que hemos probado en un proyecto real.

## Un ejemplo

Nuestro `CLAUDE.md` llegó a 257 líneas. Lo dejamos por debajo de 150 sin perder nada.

¿Cómo? El detalle que solo hace falta de vez en cuando (tablas, inventarios, procedimientos largos) se movió a un segundo fichero que no se carga al arrancar. En su sitio quedó una línea que dice, en la práctica, "para esto, mira allí". Es un puntero. El núcleo, más corto, se sigue mejor. Y el detalle sigue estando, a un paso de distancia, para cuando de verdad haga falta.

Las cifras son reales. El ejemplo es genérico a propósito: aquí explicamos el método, no publicamos el contenido interno del proyecto.

## Lo que dice Anthropic y lo que ponemos nosotros

La regla honesta de todo el repo: separar el dato oficial de la convención propia. Anthropic marca los límites y da las vías. Nosotros añadimos disciplina encima.

| Tema | Oficial de Anthropic (verificado 16/07/2026) | Convención nuestra |
|-|-|-|
| Tamaño de `CLAUDE.md` | Objetivo *"under 200 lines"*. Se carga entero, mida lo que mida. | El objetivo de 200 lo medimos en vivo con dos métodos, no a ojo. |
| Tamaño de `MEMORY.md` | Se cargan las *"first 200 lines or 25KB, whichever comes first"*. Lo que exceda, no. | Un colchón por debajo del límite, para no rozarlo. |
| `@import` | Los ficheros importados *"still load and enter the context window at launch"*. No ahorra contexto. | Regla: nunca `@import` para aligerar peso. Solo organiza. |
| Ficheros secundarios | Los ficheros de tema (tipo `debugging.md`) *"are not loaded at startup. Claude reads them on demand"*. Vías oficiales para descargar el `CLAUDE.md`: `.claude/rules/` con `paths`, y skills. | Un puntero blando a un fichero de detalle, como alternativa. |
| Un tercer fichero a medida | Solo se autocargan `CLAUDE.md`, `CLAUDE.local.md`, `.claude/rules/` y `MEMORY.md`. Un `context.md` inventado no se cargaría. | Lo estudiamos y lo descartamos. Está contado en `docs/`. |

## Qué hay en este repo

- **[01. Los dos ficheros](docs/01_los_dos_ficheros.md)**: qué es cada uno, quién lo escribe, cuándo se carga.
- **[02. Tamaño y límites](docs/02_tamano_y_limites.md)**: las cifras oficiales y por qué el tamaño importa.
- **[03. Punteros y secundarios](docs/03_punteros_y_secundarios.md)**: cómo descargar peso a ficheros que solo se leen cuando hacen falta.
- **[04. `@import` descartado](docs/04_import_descartado.md)**: qué es `@import`, cuándo sirve y por qué no lo usamos para aligerar.
- **[05. Por qué no un tercer fichero](docs/05_por_que_no_un_tercer_fichero.md)**: por qué descartamos crear un `context.md`.
- **[06. Todas las vías oficiales](docs/06_vias_oficiales_completas.md)**: todas las vías oficiales, no solo las que usamos.
- **[07. El hook](docs/07_hooks.md)**: el hook como mecanismo relacionado, y su utilidad para este problema, sopesada.
- **[08. Por qué funciona](docs/08_por_que_funciona.md)**: la teoría, por qué esta arquitectura funciona.
- **[09. Cómo lo gestionamos](docs/09_como_lo_gestionamos.md)**: el protocolo práctico, paso a paso.

## Por qué funciona

La idea de fondo es sencilla. Lo que se carga siempre tiene que ser pequeño y estable. El detalle, que se necesita de vez en cuando, se guarda aparte y se trae bajo demanda. Un núcleo caliente y corto, más un archivo frío al que se apunta.

Encima va una regla que evita el fallo más común: una sola fuente de verdad. Dos ficheros que dicen lo mismo son dos sitios donde corregir, y tarde o temprano uno queda desfasado. Por eso, al descargar peso, se mueve el contenido entero y se deja un puntero. Nunca se copia.

## Repos relacionados

Este repo no es una isla. Cuenta una pieza de un sistema más grande:

- **[agent-memory-governance](https://github.com/jleonceo/agent-memory-governance)**: la pieza hermana. Aquel gobierna la calidad de lo que se recuerda, que el corpus de memoria no se pudra, con linter, logs y citas re-ejecutables. Este gobierna el presupuesto de carga de los ficheros de contexto. Uno cuida *qué* se recuerda. El otro, *cuánto de eso entra* cada sesión.
- **[verificacion-determinista-ia](https://github.com/jleonceo/verificacion-determinista-ia)**: código que recomprueba la coherencia del estado sin IA.
- **[gobernanza-skills-analiticas](https://github.com/jleonceo/gobernanza-skills-analiticas)**: golden sets, puertas de no-regresión, autonomía por riesgo.

## Fuentes

- Documentación oficial de Anthropic, Claude Code, *Memory*: <https://code.claude.com/docs/en/memory> (consultada el 16/07/2026).

Toda afirmación marcada como oficial sale de ahí, con su cita literal. Lo demás es convención nuestra, y se dice.

# English

**Keeping Claude Code's context files (`CLAUDE.md` and `MEMORY.md`) from bloating the context window. An offload architecture grounded in Anthropic's official docs: theory, mechanisms, and a tested protocol.**

> **Validity.** Verified against Anthropic's official documentation on **16 July 2026**. This can change: Claude Code moves fast. As of today this is *one* way to manage these files, with its reasoning. It is not a fixed truth, nor the only way.

## The problem

Claude Code starts every session with no memory. The context window is empty. Two files bring back what it needs to know from one session to the next:

- **`CLAUDE.md`**: the instructions you write. How the project works, conventions, warnings.
- **`MEMORY.md`**: the notes Claude keeps for itself, from your corrections and preferences.

Both load at the start of every session. That is the problem: this load takes up part of the context, which is the room Claude has to think and work. If the files grow unchecked, two things happen, and Anthropic states both.

First, a long file is followed worse. The official docs say it plainly: *"Longer files consume more context and reduce adherence."*

Second, part stops loading. Of `MEMORY.md`, only the first 200 lines or 25KB load, whichever comes first. Anything past that does not load at startup. And there is no warning.

An overweight context file does not just take up more room: it performs worse. This repo explains how to keep these files useful without them eating the context. With Anthropic's official mechanisms, plus an engineering discipline on top that we have tested on a real project.

## An example

Our `CLAUDE.md` reached 257 lines. We cut it below 150 without losing anything.

How? The detail that is only needed now and then (tables, inventories, long procedures) moved to a second file that does not load at startup. In its place we left a line that says, in effect, "for this, look there". That is a pointer. The core, now shorter, is followed better. And the detail is still there, one step away, for when it is actually needed.

The figures are real. The example is generic on purpose: here we explain the method, we do not publish the project's internal content.

## What Anthropic says vs what we add

The honest rule of the whole repo: separate the official fact from our own convention. Anthropic sets the limits and gives the paths. We add discipline on top.

| Topic | Official Anthropic (verified 16/07/2026) | Our convention |
|-|-|-|
| `CLAUDE.md` size | Target *"under 200 lines"*. Loads in full, whatever its length. | We measure the 200 target live, with two methods, not by eye. |
| `MEMORY.md` size | The *"first 200 lines or 25KB, whichever comes first"* load. The rest does not. | A buffer below the limit, so we never graze it. |
| `@import` | Imported files *"still load and enter the context window at launch"*. No context saved. | Rule: never `@import` to save weight. It only organizes. |
| Secondary files | Topic files (like `debugging.md`) *"are not loaded at startup. Claude reads them on demand"*. Official offload paths: `.claude/rules/` with `paths`, and skills. | A soft pointer to a detail file, as an alternative. |
| A custom third file | Only `CLAUDE.md`, `CLAUDE.local.md`, `.claude/rules/` and `MEMORY.md` autoload. An invented `context.md` would not load. | We studied it and dropped it. Explained in `docs/`. |

## What's in this repo

- **[01. The two files](docs/01_los_dos_ficheros.md)**: what each is, who writes it, when it loads.
- **[02. Size and limits](docs/02_tamano_y_limites.md)**: the official figures and why size matters.
- **[03. Pointers and secondary files](docs/03_punteros_y_secundarios.md)**: how to offload weight to files read only when needed.
- **[04. `@import` dropped](docs/04_import_descartado.md)**: what `@import` is, when it helps, why we do not use it to lighten load.
- **[05. Why not a third file](docs/05_por_que_no_un_tercer_fichero.md)**: why we dropped creating a `context.md`.
- **[06. All the official paths](docs/06_vias_oficiales_completas.md)**: every official mechanism, not just the ones we use.
- **[07. The hook](docs/07_hooks.md)**: the hook as a related mechanism, its usefulness here weighed.
- **[08. Why it works](docs/08_por_que_funciona.md)**: the theory behind the architecture.
- **[09. How we manage it](docs/09_como_lo_gestionamos.md)**: the practical protocol, step by step.

*(The docs are written in Spanish.)*

## Why it works

The core idea is simple. What always loads must be small and stable. The detail, needed now and then, is kept aside and pulled on demand. A short hot core, plus a cold archive it points to.

On top goes the rule that avoids the most common failure: a single source of truth. Two files saying the same thing are two places to fix, and sooner or later one falls out of date. So when offloading, the content moves whole and a pointer stays. It is never copied.

## Related repos

This repo is not an island. It tells one piece of a larger system:

- **[agent-memory-governance](https://github.com/jleonceo/agent-memory-governance)**: the sibling. That one governs the quality of what is remembered, so the memory corpus does not rot, with a linter, logs and re-runnable citations. This one governs the loading budget of the context files. One looks after *what* is remembered. The other, *how much of it loads* each session.
- **[verificacion-determinista-ia](https://github.com/jleonceo/verificacion-determinista-ia)**: code that re-checks the coherence of the state without AI.
- **[gobernanza-skills-analiticas](https://github.com/jleonceo/gobernanza-skills-analiticas)**: golden sets, regression gates, autonomy by risk.

## Sources

- Anthropic official documentation, Claude Code, *Memory*: <https://code.claude.com/docs/en/memory> (accessed 16/07/2026).

Everything marked official comes from there, with its literal quote. The rest is our convention, and it is labeled as such.

*Bilingual README. Licencia MIT / MIT License.*
