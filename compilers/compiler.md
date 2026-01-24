# Introducción

Un compilador es una herramienta de programación que permite traducir código de alto nivel a un código de bajo nivel u objetivo, como puede ser binario o algún lenguaje intermedio o específico.

La tarea de construir un compilador no es nada fácil y tampoco suele ser sencilla de entender o explicar. Estas son algunas notas de un estudiante que buscan ser más simples de comprender y, mejor aún, están disponibles de forma libre para el público.

---

## Una caja negra

Normalmente, el término *caja negra* se utiliza para referirse a una función o software del que no se conoce con certeza su funcionamiento interno o cómo está escrito su código. Esto no es necesariamente algo negativo, ya que el software privativo suele tener esta característica, al igual que muchos temas de programación. No es que el código no esté disponible, sino que son temas muy complejos que no siempre es necesario conocer a fondo, como los motores de búsqueda, los motores de bases de datos o ciertas librerías complejas.

Un compilador, en realidad, podría describirse como una máquina de estados que sigue una serie de reglas o instrucciones para asegurarse de que el código que estamos ingresando es correcto y puede traducirse a un lenguaje máquina objetivo.

Una parte fundamental de un compilador es la gramática. Es prácticamente imposible construir un buen compilador si la gramática del lenguaje no está bien definida o simplemente no existe, ya que a partir de esta gramática se construyen la mayoría de sus componentes.

---

### Gramática

En el ámbito de los autómatas (un compilador es, en esencia, un sistema basado en autómatas), una gramática se define como un conjunto finito de reglas que describen secuencias válidas de símbolos que pertenecen a un lenguaje.

El tipo de gramática que se suele utilizar en la construcción de compiladores es la **gramática libre de contexto**. Formalmente, una gramática de este tipo se define como:

```text
G = (V, T, P, S)
```

De aquí obtenemos que:

* **V (No terminales)**: conjunto finito de símbolos abstractos o variables.
* **T (Terminales)**: conjunto finito de símbolos que forman las palabras del lenguaje.
* **P (Producciones)**: conjunto de reglas que definen cómo se pueden reemplazar los símbolos no terminales.
* **S (Símbolo inicial)**: símbolo no terminal especial que marca el inicio de la generación de cadenas del lenguaje.

En general se suelen usar gramaticas que son conocidas como gramaticas libres de contexto, esto es porque cualquier relga se puede aplicar siempre y cuando aparesca el simbolo, no importa que simbolos esten al rededor, (de ahi que no sean sensibles al contexto).

Aunque esto pueda parecer demasiado formalismo realmente es simple de comprender como se vera a continuación con estos ejemplos

Tomemos esta gramatica por ejemplo

```text
program -> table_decl+
table_decl -> "table" IDENT "{" table_item* "}" ";"
table_item -> column_decl | relation_decl
column_decl -> IDENT type column_size? column_modifier* ";"
column_size -> "(" NUM ")"
column_modifier -> "increment" | "null" | "not_null" | "primary"
type -> "int" | "string"
relation_decl -> "relation" IDENT "(" IDENT ")" "to" IDENT "(" IDENT ")" ";"
```

Esta es una grmatica muy simple para poder escribir las declaraciones de tablas en motores de bases de datos como mysql o mariadb