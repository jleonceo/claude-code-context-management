# 09. Cómo lo gestionamos, paso a paso

La teoría está en los otros documentos. Este es el protocolo práctico, el que seguimos cuando hay que tocar un fichero de contexto. Es lo que hemos probado en un proyecto real.

## 1. Copia de seguridad antes de tocar

Antes de editar el `CLAUDE.md` o el `MEMORY.md`, una copia con fecha. Estos ficheros condicionan el comportamiento del asistente en todas las sesiones. Un cambio malo o un borrado no se pueden deshacer si no hay copia. Guardar la huella de la copia permite comprobar luego que se puede volver atrás de verdad.

## 2. Medir en vivo, con dos métodos

El tamaño se mide en el momento, no se cita de memoria. Una cifra escrita a mano ("el fichero tiene 150 líneas") caduca en cuanto editas. Y se mide con dos métodos que coincidan, no con uno solo, porque un método determinista puede estar mal y darte una cifra falsa con cara de exacta. Si los dos convergen, la cifra es de fiar.

## 3. Mover entero, dejar puntero, verificar cero pérdidas

Cuando un bloque sale del núcleo, sale **entero** a su fichero de detalle, y en su sitio queda un puntero. Nunca se parte por la mitad ni se copia. Después se comprueba que no se ha perdido nada: se compara el original con el destino (por recuento de líneas o de marcas) y tiene que dar cero faltantes. Esta comprobación se hace en cada movimiento, no al final.

## 4. Qué se queda en el núcleo y qué puede salir

En el núcleo se queda lo que el asistente **no puede descubrir solo** y necesita en cada sesión: la forma de trabajar, las reglas permanentes, los avisos que evitan errores caros (un nombre de campo que si te equivocas rompe algo). Fuera de él van los inventarios, las tablas largas, los procedimientos de varios pasos y todo lo que ya está autogenerado en otro sitio. La decisión se toma bloque a bloque, no de golpe.

## 5. El guardián que vigila por ti

La disciplina no basta, porque los ficheros se pudren en silencio: uno se queda desfasado y se sigue obedeciendo. Contra eso, un mecanismo que lo compruebe solo. Un guardián que mida el tamaño y avise si se pasa del límite. Un chequeo que cace un inventario que se ha quedado atrás respecto a su fuente autogenerada. El objetivo es que el sistema se ponga en rojo por su cuenta, no que alguien se acuerde de mirar.

## El resumen

Copia antes de tocar. Mide en vivo con dos métodos. Mueve entero y verifica que no se pierde nada. Deja en el núcleo solo lo que no se puede deducir. Y pon un guardián que vigile por ti. Con eso, los ficheros de contexto se mantienen útiles sin engordar, y el día que alguien intente hincharlos otra vez, algo salta.


*Protocolo probado en un proyecto real (2026). Verificado contra la documentación oficial de Anthropic el 16/07/2026: <https://code.claude.com/docs/en/memory>. Esto puede cambiar. A día de hoy, así lo gestionamos.*
