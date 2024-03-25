Ejemplo de búsqueda primero en profundidad

Tomemos el ejemplo de las tinas, donde se parte de dos tinas (recipientes para contener agua) y se quiere lograr que la de 4 galones tenga 2 galones. Las tinas no tienes marcas que indiquen la cantidad de galones.

Vamos a definir algunas reglas de forma general:
Restricciones
1) “Si una tina se lleno hasta el tope, no llenar la otra”.
2) “Si una tina se acaba de llenar hasta el tope, no se puede botar”.
Orden de las reglas:
1ro. Aplicar reglas para el criterio de solución.
2do. Aplicar reglas para llenar las tinas.
3ro. Aplicar reglas para vaciar de una para otra.
4to. Aplicar reglas para botar el contenido de una de ella.
Criterio de solución
R1: Si (la tina de 4) (tiene) (dos galones) entonces SOLUCION.
R2: Si (la tina de 3) (tiene) (dos galones) y (la tina de 4) (esta vacia) entonces (vaciar el contenido) (para) (la tina de 4)
R3: Si (la tina de 3) (tiene) (dos galones) y (la tina de 4) (no) (esta vacia) entonces (botar el contenido de) (la tina de 4)

Veamos cómo se va efectuando la búsqueda paso a paso

Estado inicial: (0,0) ambas tinas vacías
Estado 1: (0,0) -> (0,4) Se llena una de las tinas (la de 4 galones).
Estado 2: (0,4) -> (3,1,) Se vacía el contenido de la de 4 en la de 3 galones.
Estado 3: (3,1) -> (0,1) Se bota el contenido de la tina de 3 galones
Estado 4: (0,1) -> (1,0) Se vacía el contenido de la de 4 en la de 3 galones.
Estado 5: (1,0) -> (1,4) Se llena la tina de 4 galones.
Estado 6: (1,4) -> (3,2) Se vacía la tina de 4 en la de 3 galones
Estado final (3,2) Se alcanza la solución al quedar 2 galones en la tina de 4

