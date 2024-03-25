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

# Ahora vamos a explicar el problema de las tinas de manera sencilla, como si estuviera hablando con estudiantes de primaria, y cómo se puede resolver usando un sistema experto en Python.

## Explicación del Problema:
Imagina que tenemos dos tinas, una puede contener 3 galones de agua y la otra puede contener 4 galones de agua. Queremos encontrar la forma de tener exactamente 2 galones de agua en la tina de 4 galones, utilizando solo estas dos tinas y siguiendo algunas reglas simples.
Reglas:

Si llenamos una tina hasta arriba, no debemos llenar la otra.
Si acabamos de llenar una tina hasta arriba, no la vaciamos.
Si una tina se llena con agua, no la botamos.
Si una tina se vacía en la otra, no la botamos.

## Cómo lo resolvemos con un sistema experto en Python:
Ahora, imaginemos que tenemos un amigo muy inteligente llamado Sistema Experto, que sabe cómo ayudarnos a resolver este problema. Le decimos que tenemos una tina de 3 galones y una de 4 galones, y le pedimos que nos ayude a encontrar la forma de tener 2 galones en la tina de 4 galones.
El Sistema Experto usa un algoritmo especial llamado "búsqueda primero en profundidad" para probar diferentes combinaciones de llenar, vaciar y verter agua entre las dos tinas, siguiendo las reglas que le hemos dado.

## Resultado al que debes de llegar:
Después de probar diferentes combinaciones, el Sistema Experto encuentra una forma de tener 2 galones en la tina de 4 galones, y nos muestra los pasos que debemos seguir. ¡Así, con la ayuda del Sistema Experto, logramos resolver nuestro problema de las tinas!
Usando Python, el Sistema Experto puede escribir un programa que simule este proceso y nos muestre los pasos para resolver el problema de las tinas, todo gracias a su inteligencia y habilidades especiales.


