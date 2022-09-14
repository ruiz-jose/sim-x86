# Arquitectura registro-memoria X86
El simulador posee una sintaxis ensamblador NASM y simula el comportadmiento de una CPU x86 de 8 bits.

Press Help inside the simulator to see an overview about the supported instructions.

####<a href="http://ruiz-jose.github.io/arq-registro-x86/index.html" target="_blank">Probar on-line</a>

### Caracteristicas
- CPU de 8 bits
- 4 registros de propositos general (A, B, C y D)
- 256 posiciones de memoria de 1 byte cada una
- Una pantalla de salida

### How to build
Make sure you have <a href="http://www.gruntjs.com/" target="_blank">Grunt</a> installed to compile the `asmsimulator.js` script.
Run `grunt` to build the project.

### Background
A technical introduction is available on my blog: [www.mschweighauser.com](https://www.mschweighauser.com/make-your-own-assembler-simulator-in-javascript-part1/).

### License
**The MIT License**