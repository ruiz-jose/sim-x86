# Programación de ensamblador

¡Hola! Este es un tutorial para el [Simulador ensamblador simple de 8 bits en Javascript] (https://schweigi.github.io/assembler-simulator/). 

## CPU 

La CPU tiene algunas pequeñas memorias en su interior llamadas *registros*. En este caso, estos registros pueden almacenar un solo byte (8 bits). Entonces, en un momento dado, cada uno de estos registros de 8 bits tiene un solo valor de `0` a `255`, o `$ 00` a `$ FF` en hexadecimal. 

![Registros](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/registers.png)

Esta CPU también tiene un registro en su interior llamado *flags*, donde cada bit representa valores booleanos. Entonces, en un momento dado, cada una de estas banderas de 1 bit tiene un valor de 'VERDADERO' o 'FALSO'. 

![Flags](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/flags.png) 

Estos registros y banderas juntos constituyen el *estado interno* de la CPU en un momento dado, y tienen varios propósitos que verás!

Esta CPU tiene cuatro *registros de propósito general* llamados *A*, *B*, *C* y *D*. Se denominan de propósito general porque se deja en manos del programador decidir cómo usarlos. A menudo es conveniente e incluso necesario tener un espacio temporal para mantener los valores que se manipulan y ahí es donde estos registros resultan útiles. 

![Registros de propósito general](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/gpr.png) 

Esta CPU también tiene dos *registros de propósito especial* llamados *puntero de instrucción* (*IP*) y el *puntero de pila* (*SP*). Se llaman *punteros* porque tienen un valor que representa una ubicación en la RAM. Es decir que *señalan* un lugar en la memoria.

![Registros de propósito especial](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/spr.png) 

El puntero de instrucción apunta a la siguiente instrucción de programa en memoria que se ejecutará y el puntero de pila apunta a la parte superior actual de la pila (más sobre ambos más adelante).

Esta CPU tiene tres banderas llamadas *bandera cero* (**Z**), *bandera de acarreo*, (**C**) y *bandera de falta* **F** que indica una instrucción invalida para el CPU. Estas banderas se utilizan para almacenar los resultados de la ALU. El programador puede leer estos resultados y usarlos para decidir qué hacer a continuación. Por ejemplo, al restar dos números, el indicador cero se establece en "VERDADERO" si el resultado es 0.  

## Memoria

Esta CPU simulada tiene una RAM de 256 bytes. Cada uno de estos bytes está ordenado de arriba a la izquierda a abajo a la derecha y tiene un número asignado que es la *dirección de memoria* de ese byte. Por ejemplo, en la captura de pantalla, el valor en la ubicación de memoria `$00` es `$1F`, y el valor en la ubicación de memoria `$12` es `$06`. El programador puede indicarle a la CPU que lea y escriba valores de bytes hacia y desde cada dirección de memoria. 

![RAM](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/ram.png) 

## Entrada/salida 

En realidad, esta CPU simulada tiene mapeada en memoria una pantalla de caracteres ASCII de 24 celdas.

![Salida](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/output.png) 

Esta pantalla de caracteres simplemente muestra los caracteres correspondientes a los valores ASCII codificados presentes en una porción específica de la memoria, las ubicaciones de memoria se corresponden desde `$E8` hasta `$FF`. Este es un ejemplo de *E/S mapeada en memoria*, lo que significa que se puede acceder a alguna forma de entrada y/o salida del sistema leyendo o escribiendo en ubicaciones específicas de la memoria. 

![E/S mapeada en memoria](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/mmio.png) 

## Programas

Un *programa* es una secuencia de *instrucciones* que indican a la CPU qué hacer. La mayoría de las instrucciones consisten en una *operación* y uno o más *operandos* dependiendo de la operación. 

Una *operación* es como una función integrada en la CPU y proporcionada para que el programador la use de inmediato. Cada operación tiene un nombre breve y fácil de recordar llamado *nemonico*. En lenguaje ensamblador este nemonico hace referencia a las operaciones. 

Un *operando* es como un argumento para una operación. Un operando puede referirse a un registro de la CPU, una ubicación en la memoria o un valor literal. 

![Código de ensamblaje](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/assembly.png)

En última instancia, la CPU solo entiende los números. Cuando presiona el botón "Ensamblar", el código ensamblador se convierte en una representación numérica del programa llamada *código de máquina* y luego se coloca en la memoria. 

Cada nemonico tiene una operación que tiene su representación numérica asociada llamada *opcode*. Existe una correlación de uno a uno entre cada nemónico y cada código de operación. Cuando una instrucción se ensambla en un código de máquina, los nemonicos se reemplazan sistemáticamente con sus códigos de operación correspondientes. 

![Opcodes](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/opcodes.png) 

## Modos de direccionamiento 

Un *modo de direccionamiento* es una forma de referirse al valor real utilizado como operando .

*Direccionamiento inmediato* es cuando el valor se da directamente después de especificar la operación. Se llama direccionamiento inmediato, porque el valor codificado se coloca *inmediatamente después* del código de operación en el código de máquina. 

*Direccionamiento directo* es cuando el valor que se va a utilizar se encuentra en algún lugar de la memoria. En lugar de especificar directamente el valor, se especifica la *dirección* del valor en la memoria. Se llama direccionamiento directo en contraste con *direccionamiento indirecto*.

*Direccionamiento indirecto* es cuando la dirección de memoria se encuentra en un registro de proposito general (por lo general B). 

## Pila

Una pila es una estructura de datos que parece una pila física de elementos. Es una estructura LIFO (último en entrar, primero en salir), lo que significa que lo primero que saca de la pila es lo último que pone. Imagine una pila de platos. El único plato que puede sacar de la parte superior de la pila es el último plato que se colocó encima. 

Una pila en la memoria se implementa como una secuencia de valores que constituyen los elementos de la pila más un *puntero de pila*. El puntero de pila es un puntero que siempre apunta a lo que se considera *la parte superior de la pila*.

Cada vez que desee *poner* un elemento en la pila (en otras palabras, colocar un nuevo elemento en la parte superior de la pila), simplemente copie el valor en el lugar de la memoria señalado por el puntero de la pila, y luego incremente (o disminuya, según la dirección en que *sacar* la pila) el puntero de la pila para señalar el siguiente espacio libre, el lugar *justo arriba* del elemento que acaba de agregar a la pila. 

Cada vez que desee *sacar* un elemento de la pila (en otras palabras, eliminar el último elemento que se colocó en la parte superior de la pila). Si desea el valor que acaba de extraer, simplemente haga referencia al valor en la memoria al que ahora apunta el puntero de la pila.

¡Esta CPU tiene una sola pila integrada! El puntero de pila de esta pila integrada está contenido en el propio registro de puntero de pila de la CPU. En este caso, la pila *crece hacia abajo* y cuando se reinicia la CPU, el puntero de la pila se inicializa en `$E7`, lo que significa que *la parte inferior de la pila* se encuentra en la ubicación de memoria `$E7`. Pero estos detalles no son importantes para usar la pila. 

La CPU proporciona las operaciones `PUSH` y `POP` que le permiten insertar y sacar valores de la pila. Depende del programador decidir cómo usar la pila, pero los usos comunes son preservar valores temporalmente, pasar argumentos entre funciones o realizar un seguimiento de las direcciones de retorno (más sobre estas cosas más adelante). 

## Ejecución del programa

Cuando la CPU se está ejecutando, funciona ejecutando instrucciones en la memoria una por una. Primero busca la instrucción en la memoria a la que apunta el puntero de instrucción. Esto incluye el código de operación, así como cualquier valor de byte de operando que pueda seguir dependiendo de la operación. Luego lleva a cabo esta instrucción, posiblemente afectando el estado interno de la CPU y/o el contenido de la memoria. Finalmente, el puntero de instrucción se establece en la ubicación que sigue inmediatamente a la instrucción que se acaba de ejecutar y el proceso continúa.

Cuando se reinicia la CPU, el puntero de instrucción se establece en 0, lo que significa que la primera instrucción que se ejecuta es la instrucción ubicada al comienzo de la memoria. Cuando se ejecuta la CPU, continuará ejecutándose hasta que encuentre una instrucción *halt* (`HLT`), momento en el cual se congelará. Alternativamente, puede ejecutar una sola instrucción a la vez (llamada *stepping*). 

![Controles](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/controls.png) 

## Lenguaje ensamblador 

Una instrucción está escrita en lenguaje ensamblador comenzando con un nemonico seguido de cualquier operando separado por comas.

Los valores literales se pueden usar como operandos simplemente incluyendo un valor numérico o un carácter ASCII encerrándolo entre comillas simples. Esto es direccionamiento inmediato. 

Los contenidos de los registros de la CPU se pueden usar como operandos simplemente escribiendo el nombre del registro de la CPU. Por ejemplo, `A` se refiere al registro A. 

Los valores presentes en la memoria se pueden usar como operandos encerrando otro valor entre `[` y `]`. Por ejemplo, el valor en la dirección de memoria `$20` se puede usar como operando escribiendo `[$20]`. Esto es direccionamiento directo. 

También puede colocar un nombre de registro entre `[` y `]` para usarlo como un operando del valor en la memoria ubicado en la dirección contenida en ese registro. Por ejemplo, si el registro A contiene `$5` entonces `[A]` se refiere al valor ubicado en la memoria en la dirección `$5`.

En lugar de especificar direcciones de memoria explícitamente, es mucho más común y conveniente usar *etiquetas* para marcar ubicaciones de memoria en el código del programa. Puede colocar una etiqueta en el código ensamblador para marcar la dirección ensamblada de lo que sigue inmediatamente escribiendo un nombre seguido de dos puntos. Por ejemplo `start:` crea una etiqueta llamada "start". Luego puede usar el nombre `start` en cualquier otro lugar del programa en lugar de una dirección de memoria para referirse a ese lugar en el código. 

Puede incluir datos arbitrarios en su programa usando la directiva `DB`. Esto significa *Define Byte* y no es un nemonico para una operación de CPU, sino que indica al ensamblador que incluya algunos datos binarios en ese punto en lugar de ensamblar código allí. Esto es útil para incluir valores constantes predefinidos en su programa.

Por último, los comentarios en lenguaje ensamblador generalmente se marcan con un punto y coma. 

![Instrucciones](https://gist.github.com/MegaLoler/5ffe47668b5271faed0a3626ed5949b1/raw/956d3ea70d9f4bc159fa67276eacea89d9d65b78/instructions.png) 

## ¡Hola mundo! 

¡Caminemos por el Hola Mundo! ¡ejemplo! 

Como referencia, aquí está el código de ejemplo del simulador: 
```asm 
; Ejemplo sencillo 
; Escribe Hello World en la salida 

	JMP start 
hello: DB "¡Hola mundo!" ; variable 
       DB 0 ; Inicio del terminador de cadena 

: 
	MOV C, hola; Apunte a var 
	MOV D, 232 ; Apunte a la salida 
	CALL print 
        HLT ; Detener ejecución 

imprimir: ; imprimir (C:*desde, D:*hasta)
	PUSH A 
	PUSH B 
	MOV B, 0 
.bucle: 
	MOV A, [C] ; Obtener char de var 
	MOV [D], A ; Escribir en la salida 
	INC C 
	INC D   
	CMP B, [C] ; Compruebe si finaliza 
	JNZ .loop; saltar si no 

	POP B 
	POP A 
	RET 
``` 

Como se mencionó anteriormente, cuando se reinicia la CPU, el puntero de instrucción se establece en 0, lo que significa que el inicio del programa es la parte superior del archivo. Así que la primera instrucción en el programa es esta: 
```asm 
JMP start 
```

La operación `JMP` simplemente establece el puntero de instrucción en su operando. Es decir, *salta* a un lugar determinado de la memoria y desde allí continúa ejecutando el programa. En este caso, el operando es `start`, que es una etiqueta que marca esta parte del código: 
```asm 
start: 
    MOV C, hello ; Apunte a var 
    MOV D, 232 ; Apunte a la salida 
    CALL print 
    HLT ; Detener la ejecución 
``` 

Después de ejecutar el salto, la siguiente instrucción es esta: 
```asm 
    MOV C, hola; Apunta a var 
```
La instrucción `MOV` copia su segundo operando en el lugar descrito por su primer operando. Es decir, *mueve* los datos. En este caso, el segundo operando es `hola` y el primer operando es `C`. Esto significa que la dirección de memoria marcada por la etiqueta `hola` se copia en el registro C. `hola` marca esto en el código: 
```asm 
hola: DB "¡Hola mundo!" ; Variable 
``` 

Esta es la cadena sin procesar "¡Hola mundo!" que se imprimirá. Ahora, el registro C contiene la ubicación de la cadena que vamos a imprimir. En otras palabras, el registro C *apunta* a la cadena. 

La siguiente instrucción es otro `MOV`: 
```asm 
    MOV D, 232 ; Apunta a la salida 
```
Esto coloca el valor `232` en el registro D. `232` (`$E8` en hexadecimal) es la dirección de memoria de nuestra pantalla de salida de caracteres mapeados en memoria. Entonces, si escribimos la cadena en la memoria comenzando en esta ubicación, aparecerá en nuestra pantalla. 

En resumen, en este punto de la ejecución, el registro C apunta a la cadena que queremos mostrar y el registro D apunta a la memoria de visualización. Todo lo que tenemos que hacer ahora es copiar la cadena a la que apunta C en la ubicación a la que apunta D. 

La siguiente instrucción es una llamada a nuestra función de impresión: 
``` asm 
    CALL print 
```
La operación `CALL` es muy similar a la operación `JMP` en que también salta a otra ubicación en la memoria para continuar la ejecución. La única diferencia es que antes de saltar, empuja el valor actual del puntero de instrucción a la pila. La ubicación de memoria a la que se salta está destinada a ser el comienzo de una función (también llamada *subrutina* en el lenguaje ensamblador). 

Generalmente, la última instrucción de una función es una instrucción `RET`. La operación `RET`, lo adivinaste, *regresa* de la función. Hace esto configurando el puntero de instrucción a la dirección de retorno recuperada al sacarla de la pila. Esto es posible porque la operación `CALL` empuja la dirección de retorno antes de saltar a la función.

Hay múltiples formas de pasar argumentos a funciones en lenguaje ensamblador. La forma en que se hace aquí es precargando los registros de la CPU con valores para pasar a la función. En este caso, nuestra función de impresión toma dos argumentos: un puntero a la cadena para imprimir y un puntero a la ubicación en la memoria para imprimirlo. Espera que estos argumentos se proporcionen a través de los registros C y D respectivamente. ¡Es por eso que previamente cargamos esos registros con esos valores! 

Entonces, en este caso, está llamando a la función marcada con la etiqueta `imprimir`, por lo que la dirección de retorno se coloca en la pila, y luego el puntero de instrucción se establece al inicio de la función de impresión, y continuamos desde allí. 

El primer par de instrucciones en la función de impresión se ven así: 
```asm 
	PUSH A 
	PUSH B
	MOV B, 0 
``` 
Esto es típico de un *prólogo de función*. Un *prólogo de función* es un código inicial en una función que prepara la pila y los registros de la CPU para su uso. 

En este caso, estamos colocando los registros A y B en la pila para preservar su valor. Hacemos esto porque el cuerpo de esta función corromperá el contenido de estos registros. Al preservar sus valores primero, podemos restaurarlos antes de regresar del programa. De esta forma, cualquier parte del código que llame a esta función no tiene que preocuparse por la modificación del contenido de los registros después de llamar a la función. 

Después de las pulsaciones, también estamos inicializando el registro B para que contenga el valor `0`. Verás por qué pronto.

Después del prólogo de la función, lo primero en el cuerpo de nuestra función es un *bucle*: 
```asm 
.loop: 
	MOV A, [C] ; Obtener char de var 
	MOV [D], A ; Escribir en la salida 
	INC C 
	INC D   
	CMP B, [C] ; Compruebe si finaliza 
	JNZ .loop; saltar si no 
``` 
La etiqueta `.loop` marca el comienzo del bucle. (No se preocupe por el punto al comienzo del nombre. Es simplemente una forma convencional de indicar *etiquetas locales*, etiquetas cuya relevancia se limita a alguna parte localizada del código. Por ejemplo, puede tener muchos bucles en su programa , ninguno de los cuales necesita referirse entre sí, y es posible que no desee dar a cada ciclo un nombre único).

Te diré de antemano lo que hace este bucle: copia cada carácter de la cadena de origen al destino. 

Dado que C actualmente apunta a la cadena de origen, lo primero que debe hacer es tomar el primer carácter, el carácter en la dirección del puntero: 
```asm 
	MOV A, [C] ; Get char from var 
``` 
Esto copia el valor en la memoria al que apunta el contenido del registro C en el registro A. 

El siguiente paso es copiar este carácter recuperado a la pantalla de salida, a la que actualmente apunta el registro D: 
```asm 
	MOV [D], A ; Escribir en la salida 
``` 
Esto copia el carácter (en A) a la memoria a la que apunta D.

Ahora que hemos copiado con éxito el primer carácter en la pantalla de salida, ¡es hora de hacer el siguiente! Para hacer esto, simplemente necesitamos incrementar los punteros de origen y destino en uno para que podamos recuperar el siguiente carácter de la cadena de origen y escribirlo en la siguiente celda en la pantalla de caracteres. La operación `INC` hace justamente eso, incrementa el contenido de un registro en uno: 
```asm 
	INC C 
	INC D   
``` 

En este punto, simplemente podríamos usar una operación `JMP` para volver al inicio de la bucle en `.loop` para copiar el resto de los caracteres. El único problema con esto es que esto crearía un *bucle infinito* ya que no habría manera de saber parar cuando se alcance el final de la cadena.

En su lugar, necesitamos una forma de volver al comienzo del bucle solo si no hemos terminado de copiar la cadena. Además, necesitamos una forma de saber si hemos terminado de copiar la cadena o no. La forma en que lo sabemos es marcando el final de la cadena con un *terminador nulo* que es un valor de byte de '0' colocado directamente después de la cadena en la memoria. Esta es la razón por la que el valor de la cadena en el programa va seguido de `DB 0`: 
```asm 
hello: DB "¡Hola mundo!" ; variable 
       DB 0 ; Terminador de cadena 
``` 

Ahora todo lo que tenemos que hacer es verificar y ver si el siguiente carácter es un `0` antes de continuar con el ciclo. Si no es un `0`, vuelve al principio del bucle. Si es un `0`, entonces hemos terminado, continuamos ejecutando el programa más allá del bucle.

Para comprobar si el siguiente carácter es un `0`, podemos utilizar la operación `CMP`: 
```asm 
	CMP B, [C] ; Compruebe si finaliza 
``` 
La operación `CMP` *compara* sus dos operandos y establece las banderas de la CPU en consecuencia. Para nuestro caso, todo lo que necesitamos saber es que si los dos operandos son numéricamente iguales, el indicador cero se establece en "VERDADERO". (La lógica detrás de esto es que para comparar los dos números, la operación 'CMP' resta internamente el segundo del primero, y si el resultado es '0', establece el indicador cero. Por supuesto, una diferencia de `0` significa que los dos operandos son equivalentes).

En este caso, el indicador cero se establece en "VERDADERO" si el siguiente carácter en la cadena es "0". (Recuerde, simplemente incrementamos el contenido de C para apuntar al siguiente carácter en la cadena, y en el prólogo de la función inicializamos B a `0`. ¡Esta es la razón por la que hicimos eso!) 

Finalmente, usamos `JNZ` operación para volver al comienzo del ciclo *solo si* el indicador cero se ha establecido en `VERDADERO`: 
```asm 
	JNZ .loop ; saltar si no 
``` 
La operación `JNZ` es igual que la operación `JMP` excepto que solo realiza el salto si el indicador cero es actualmente `FALSO`. En otras palabras, *(j)umps si (n)ot (z)ero*. Si el indicador cero es actualmente `FALSO`, entonces no sucede nada y el programa continúa con la siguiente instrucción.

Que en este caso, es el *epílogo de funciones*! El epílogo de función es la contrapartida del prólogo de función. Aquí los registros de la pila y la CPU están preparados para regresar de la función. En nuestro caso, simplemente restauramos los valores conservados previamente de los registros A y B para que la parte del código que originalmente llamó a esta función no sepa que los registros A y B se usaron en absoluto. Esto es importante en caso de que el código de llamada utilice A y B para fines propios: 
```asm 
	POP B 
	POP A 
```

Finalmente, *regresamos* de la función con la operación `RET`, que, como se mencionó anteriormente, extrae la dirección de retorno de la pila que originalmente fue insertada por la instrucción `CALL` correspondiente, luego vuelve a colocar esta dirección extraída en la instrucción registro de puntero. 

La última instrucción de nuestro programa es entonces la instrucción `HLT`: 
```asm 
    HLT ; Detener la ejecución 
``` 
La instrucción `HLT` *detiene* la CPU, marcando el final de la ejecución del programa.