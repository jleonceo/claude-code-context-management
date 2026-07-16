# 01. Los dos ficheros

Claude Code arranca cada sesión sin memoria. La ventana de contexto está vacía. Para que no empiece de cero cada vez, Anthropic tiene dos mecanismos, y los dos se cargan al principio de cada conversación.

## `CLAUDE.md`: lo que escribes tú

Es un fichero de texto (markdown) con las instrucciones que le das al asistente sobre el proyecto. Lo escribes tú, a mano. Anthropic dice que ahí van los estándares de código, la arquitectura, las convenciones, los comandos de build y de test, y los flujos de trabajo habituales. Es lo que le explicarías a alguien que entra nuevo al proyecto y tendrías que repetir cada vez.

Se carga **entero** al arranque, mida lo que mida. Puede vivir en varios sitios, con distinto alcance: en el proyecto (`./CLAUDE.md`, compartido con el equipo por control de versiones), en tu usuario (`~/.claude/CLAUDE.md`, tus preferencias para todos los proyectos), en local sin compartir (`CLAUDE.local.md`), o gestionado por la organización (política de empresa, que no se puede anular).

## `MEMORY.md`: lo que el asistente se guarda solo

Es la auto-memoria. Aquí no escribes tú: escribe Claude. Va anotando lo que aprende de tus correcciones y preferencias, como comandos de build, hallazgos de depuración o cómo te gusta que trabaje. Él decide qué merece la pena recordar. No guarda algo en cada sesión.

`MEMORY.md` es el índice de esa memoria. A su lado viven ficheros por tema (por ejemplo `debugging.md`), que Claude lee bajo demanda cuando los necesita. El índice se carga cada sesión, pero solo en parte, y eso tiene su regla propia (ver [02](02_tamano_y_limites.md)).

## La diferencia que importa: guía, no orden

Los dos son **contexto**, no configuración que se imponga. En palabras de Anthropic: *"Both are loaded at the start of every conversation. Claude treats them as context, not enforced configuration."* O sea, el asistente los lee y trata de seguirlos, pero no hay garantía de cumplimiento estricto, sobre todo si una instrucción es vaga o se contradice con otra.

Esto tiene una consecuencia práctica que mucha gente pasa por alto: si necesitas que algo se cumpla pase lo que pase (bloquear un comando, impedir tocar un fichero), un fichero de contexto no es la herramienta. Para eso está el hook, que se ejecuta como código. Lo vemos en [07](07_hooks.md).

## Cuándo usar cada uno

Usa `CLAUDE.md` cuando quieras **guiar** el comportamiento: convenciones, arquitectura, "haz siempre X". Deja que la auto-memoria capture lo que Claude **descubre** trabajando contigo, sin que tengas que escribirlo. Uno es intención tuya; el otro, aprendizaje suyo.


*Verificado contra la documentación oficial de Anthropic (Claude Code, Memory) el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar. A día de hoy, así funciona.*
