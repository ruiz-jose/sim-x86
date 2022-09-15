# Arquitectura registro-memoria X86
El simulador posee una sintaxis ensamblador NASM y simula el comportadmiento de una CPU x86 de 8 bits.

Press Help inside the simulator to see an overview about the supported instructions.

####<a href="http://ruiz-jose.github.io/arq-registro-x86/index.html" target="_blank">Probar on-line</a>

## Caracteristicas
La computadora virtual consta de los siguientes componentes:
- Memoria RAM (256 bytes). La memoria contiene las instrucciones y datos del programa. Tiene 256 posiciones de memoria de 1 byte cada una. todos los valores deben estar entre 0 y 255.
- CPU de 8 bits. La CPU realiza el ciclo de instruccion: captar de memoria y ejecutar.
- Salida de consola. La salida de la consola utiliza la asignación de memoria y asigna una parte específica de la memoria a la consola. Por lo tanto, escribir en la salida de la consola es tan simple como escribir en una ubicación de memoria específica.

### la cpu
El corazón del simulador es la CPU. La CPU consta de 4 registros de propósito general ( A, B, C y D ) y su trabajo es mantener los valores necesarios para ejecutar una instrucción ¿Cómo sabe la CPU qué ejecutar? Para ello, utilizamos un puntero de instrucción ( IP ). Técnicamente, la IP es solo otro registro con alguna funcionalidad adicional. La IP mantiene la ubicación de la siguiente instrucción en la memoria y en cada ciclo de la CPU, la CPU toma esta instrucción y la ejecuta.

Si bien esta pequeña funcionalidad es suficiente para ejecutar algunos programas, no es suficiente para ejecutar ningún tipo de programa. Por ejemplo, para proporcionar la funcionalidad IF-then-else, la CPU necesita tomar una decisión basada en el resultado de la instrucción ejecutada previamente. Esos resultados se almacenan en banderas de 1 bit. Nuestra CPU contendrá tres banderas diferentes:

- Cero ( Z ). El más importante. Si el resultado de una instrucción es 0, esta bandera contiene 1 de lo contrario 0.
- Llevar ( C ). Si una instrucción generó un acarreo, esta bandera se establece en 1.
- Falta En caso de que una instrucción conduzca a un estado defectuoso de la CPU (p. ej., división por 0), este indicador se establece en 1. En caso de falla, la CPU se detiene y no se ejecutan más instrucciones.

Por último, pero no menos importante, actualizamos nuestra CPU con un registro de puntero de pila. El puntero de pila ( SP ) como el nombre ya da puntos a la posición actual de la pila en la memoria. Puede ser incrementado y decrementado por el programa para almacenar datos e implementar funciones.

### Formato de instruccion
Por lo general una instrucción contiene el código de operación y sus operandos (parámetros). El primer operando suele ser el destino y el segundo es la fuente. Si una instrucción tiene solo un operando, este operando es destino y fuente a la vez.

Por ejemplo: 
[Opcode] [Operand1] [Operand2]
ADD     reg,       reg
ADD     reg,       [address]
ADD     reg,       constant

### Ciclo de instruccion
El código es realmente sencillo. Lee la siguiente instrucción de la memoria con la ayuda de la IP. Luego, se captan los operandos de la instrucción de la memoria y finalmente se ejecuta la instrucción.
Una instruccion con dos operandos requiere 3 bytes de memoria, 1 para el codigo de operacion de instrucción y dos para cada operando. Por lo tanto, el puntero de instrucción (IP) se incrementa en 3 en lugar de 1 para este tipo de instrucción.


## Para desplegar
Instalar <a href="http://www.gruntjs.com/" target="_blank">Grunt</a>.
Run `grunt` para construir el proyecto.

## Background
A technical introduction is available on my blog: [www.mschweighauser.com](https://www.mschweighauser.com/make-your-own-assembler-simulator-in-javascript-part1/).

## License
**The MIT License**