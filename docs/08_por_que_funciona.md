# 08. Por qué funciona esta arquitectura

Hasta aquí el "cómo". Ahora el "por qué". Una arquitectura que no entiendes se copia mal, así que vale la pena explicar la teoría de fondo.

## La idea en una frase

Lo que se carga siempre tiene que ser pequeño y estable. El detalle, que se necesita de vez en cuando, se guarda aparte y se trae bajo demanda.

¿Por qué funciona? Porque el contexto es un recurso con dos costes. Uno, ocupa sitio: lo que gastas en instrucciones no lo tienes para el trabajo. Dos, diluye: cuanto más texto compite por la atención del modelo, menos caso le hace a cada parte. Un núcleo pequeño ataca los dos costes a la vez. Ocupa poco y se sigue bien.

## Una sola fuente de verdad

Encima de eso va la regla que sostiene todo lo demás: cada cosa vive en un solo sitio. Dos ficheros que dicen lo mismo son dos sitios donde corregir. Corriges uno, te olvidas del otro, y acabas con dos versiones que se contradicen. El asistente no sabe cuál vale, y a veces obedece la vieja sin darse cuenta. Por eso, al descargar peso, se mueve entero y se deja un puntero en su lugar; no se copia.

## La evidencia, en las dos direcciones

Aquí toca ser honesto, porque hay evidencia a favor y en contra, y las dos importan.

Un estudio académico sobre cientos de tareas (con varias herramientas, Claude Code entre ellas) midió el efecto de tener un fichero de contexto. El resultado incomoda: un fichero escrito por un humano daba una mejora marginal, uno generado por un modelo **empeoraba** el resultado, y ambos subían el coste en torno a un 20%. Es decir, un fichero de contexto no es gratis ni es bueno por defecto.

El mismo estudio dice cuándo sí ayuda y cuándo no. Los **resúmenes e inventarios** (la estructura de carpetas, la lista de dependencias, un mapa del proyecto) **no ayudan**: el asistente descubre eso solo leyendo el código, así que ponérselo por escrito es coste puro. Lo único que aporta de verdad es el **conocimiento de dominio que no se puede deducir del código**: un comando propio, una convención rara, un aviso de "esta cuenta va aquí y no allá".

## Qué sacamos de esto

La arquitectura funciona porque **carga menos y mejor**, no por multiplicar ficheros. Concretando:

Se queda en el núcleo solo lo que el asistente **no puede descubrir solo**. Los inventarios que ya están autogenerados en otro sitio se van, porque leerlos es coste sin ganancia.

El fallo más citado de estos ficheros es que **se quedan desfasados y se obedecen a ciegas**: el modelo no nota que una instrucción ya no vale. Contra eso, la defensa buena no es la disciplina ("me acordaré de actualizarlo"), sino un **mecanismo** que lo compruebe solo. Un fichero al día es un fichero que un guardián vigila, no uno que prometes revisar.

Esa es la razón última de que funcione: mantiene el núcleo pequeño y guarda solo lo que aporta. Y para no pudrirse, no se fía de la buena voluntad: pone un guardián.


*Estudio académico citado: evaluación de ficheros de contexto para agentes sobre cientos de tareas (2026), con el matiz honesto de que ningún estudio mide todavía dominios que no son de código. Verificado contra la documentación oficial de Anthropic el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar.*
