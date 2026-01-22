# Introduccion

Un compilador es una herramienta de programacion que permite pasar codigo de alto nivel, a un codigo de bajo nivel objetivo, como puede ser binario o algun lenguaje especifico.
La tarea de construir uno no es nada facil y tampoco suele facil de entender o explicar, pero estas son algunas notas de un estudiante que puede que sean mas simples de entender 
y mejor aun, estan libres para el publico


## Una caja negra 

Normalmente es el nombre que se le da una funcion o software del que no se esta seguro como funciona o no se tiene 
conocimiento sobre como esta escrito el codigo, esto no es necesariamente malo ya que el software privativo suele tener esta carecteristica y muchos temas de programacion igual, no es que el codigo no este disponible, simplemente son temas muy complejos que no siempre son necesarios saber a fondo, Motores de busqueda, motores de bases de datos o librerias complejas.

Un compilador en realidad se podria decir que es una maquina de estados que sigue una serie de relgas o instrucciones, para asegurarse de que el codigo que estamos ingresando es correcto y se puede traducir a un lenguaje maquina objetivo. 

Parte importante de un compilador es la gramatica y es que basicamente es imposible lograr un buen compilador si la gramatica del lenguaje no esta bien definida o no existe, ya que a partir de esa gramatica se construye un compilador

### Gramatica

En el ambito de los automatas (un compilador es un automata en escencia) se trata de un conjunto finito de reglas, las cuales describe secuencias validas de simbolos que pertenecen a un lenguaje. El tipo de gramatica que se suele usar es una gramatica libre de contexto, formalmente una gramatica de este tipo se define asi 

```js
G = (V,T,P,S)
```

De aqui podemos obtenemos que: 

- V(No terminlaes) es un conjunto finito de simbolos abstractos o variables
- T(Terminales) es un conjunto finito de simbolos que forman palabras que corresponden al lenguaje 
- P(Producciones) es un conjunto de reglas que definen como se puede reemplazar los simbolos no terminales
- S(No terminal especial) es un simbolo no terminal que es un poco difentes pues el que marca el inicio de la generacion de cadenas

En general se suelen usar gramaticas que son conocidas como gramaticas libres de contexto, esto es porque cualquier relga se puede aplicar siempre y cuando aparesca el simbolo, no importa que simbolos esten al rededor, (de ahi que no sean sensibles al contexto).

Aunque esto pueda parecer demasiado formalismo realmente es simple de comprender como se vera a continuación con estos ejemplos

