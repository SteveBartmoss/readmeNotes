# Tipos de datos

En typescript la forma de asignar un valor de la siguiente manera

```ts
let name: string = 'Steve'
```

De esta forna agregamos la regla de traspilacion que le asigna un tipo de dato a la variable que declaramos, esto es asi porque ts trabaja sobre js y al momento de ser 'compilado' se manejan los tipos de datos que se determinan para una variable

**Ventajas**

- Deteccion de errores: Como se esta agregando la bandera del tipo de dato, podemos evitar un 'casteo' involuntario de tipo de dato en javascript, como se puede ver en 
el siguiente ejemplo

```js
let name = 'Steve'

name = {
    age: 10,
    name: 'Steve'
}
```

Esto en js es valido, ya que el tipo de dato no esta defino y simplenente se asigna la memoria necesaria para el nuevo tipo de dato, esto no es necesariamente un problema pero algunas personas no pueden manejar esos caso y decidieron crear una solucion.

```ts
let name: string = 'Steve'

name = {
    age: 10,
    name: 'Steve'
}
```

Esto en ts no es valido, ya que al inicio definimos name como un string y por eso nos permite detectar un 'error' en el momento de asignar un tipo de dato diferente.

- Mejor uso del ide: Como typescript hace la asignacion 


# Interfaces

Una interface es una manera de declarar la estructura de un objeto y que sus campos sean consistentes.

```ts
export interface Pokemon {
    id: number;
    name: string;
    age?: number;
}
```

Con lo anterior nos aseguramos que los campos sean respetados con su tipo y ademas que no falten, pues la unica forma de que el campo pueda ser opcional es agregando el ? despues del nombre del campo ya que de esta manera el campo puede o no estar definido en una instancia de la interfaz. 

Las interfaces son muy parecidas a los objetos de la toda la vida, pero estan definidos de una forma mas simple y ademas no son por definición un objeto