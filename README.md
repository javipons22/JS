## 1. Syntax Parsers , Execution Context and lexical environments


`Syntax Parsers:` A program that reads your code and determines what it does and if its grammar is valid
a traves de compiladores, leen el codigo caracter por caracter y transforman en un lenguaje maquina

`Lexical environment:` Where something sits physically in the code you write
donde escribis algo es importante
define donde se pone en la memoria

`Execution context:` A wrapper to help manage the code that is running
It can contain things beyond what youve written inyour code
Contiene el codigo y mas


## 2. Name/Value Pairs and Objects


`Name / Value Pair:`A name which maps to a unique value
The name may be defined more than oncontextmenu, but can only have one value in any given context
that value may be more name/value pairs

`Object:` A collection of name value pairs


## 3. The Global Environment and The Global Object

Cuando el código se ejecuta en JavaScript, se ejecuta dentro de un CONTEXTO DE EJECUCION (EXECUTION CONTEXT).
Hay mas de uno. El principal es el global.

Este contexto crea dos cosas. El global Object , una variable especial this , un link al outerEnvironment (que es null en el global) y el codigo.

windows object es el global object en el browser.

>Cuando decimos global en javascript significa: **Not inside a function.**


En JavaScript, cuando creas variables y funciones, y no estás dentro de una función, esas variables y funciones se adjuntan al objeto global.


## 4. The Execution Context - Creation and Hoisting

El execution context es creado en dos partes. CREATION PHASE (crea el global object ,this y la referencia al outer environment.) Tambienhace un setup memory space for variables and functions (HOISTING).

En el hoisting a las variables no les asigna el valor , sino que los deja como (UNDEFINED). 
```javascript
console.log(a); //devuelve undefined (valor por defecto que el hoisting le da a las variables en la creation phase)

var a = 'hello world';
```

A las funciones las guarda en memoria por eso se pueden ejecutar antes de ser declaradas:
```javascript
a(); //se llama la funcion 

function a() { // se declara la funcion
    console.log('hola'); 
}

//esto devuelve el console log : hola sin problemas
```

## 5. Conceptual Aside: Javascript and 'undefined'

undefined es un VALOR especial de javascript que significa que a la variable no se le asigno nada.

  ```javascript
var a; //su valor es undefined.
  ```



## 6. The Execution Context - Code Execution

EXECUTION PHASE. tiene todos los elementos del CREATION PHASE y ejecuta el codigo linea por linea.
  ```javascript
function b() {
    console.log('Called b!');
}

b(); // devuelve en consola called b!

console.log(a); // devuelve undefined

var a = 'Hello World!';

console.log(a); // devuelve Hello World!
  ```


## 13. Conceptual Aside: Single Threaded, Synchronous Execution

### Single Threaded
Un comando a la vez.

### Synchronous
una linea a la vez y en orden


## 14. Function Invocation and the Execution Stack

### Invocation 
running a function. ejecutar una funcion. (con parentesis () ).

Primero se crea el global execution context. Cuando se invoca la funcion se agrega un execution context y se lo pone en el execution execution stack . ejemplo:

 ```javascript
//primero se ejecuta el global execution context
function b() {

}

function a() {
    b(); // se agrega b al execution stack con las dos etapas (CREATION PHASE Y EXECUTION PHASE)
}

a(); // cuando el global llega a a() se agrega a al execution stack con las dos etapas (CREATION PHASE Y EXECUTION PHASE)
 ```
El stack se ve como algo asi 

 ```
        b() cuando se ejecuta se hace un pop off y sigue con a()
    Execution context
   create and execute
 ```
  ```
        a() cuando se ejecuta se hace un pop off y sigue con global
    Execution context 
   create and execute
 ```
  ```
 Global Execution Context
created and code is executed
 ```

En este cuadro se ejecutan las funciones de arriba hacia abajo linea por linea sincronamente y sigue con el global

 ```javascript
function a() {
    b(); // 2.ejecuta b()
    var c; // 4. se pone c como undefined
}

function b() {
    var d; // 3. se pone d como variable undefined
}

a(); // 1. ejecuta a()
var d; // 5. se pone d como undefined
 ```



## 15. Functions, Context, and Variable Environments

Variable environment: Donde vive la variable y como se relaciona con otras en memoria. cada execution context tiene su propio variable environment
 ```javascript
function b() {
    var myVar;
    console.log(myVar);  // variable environment es b()
}

function b() {
    var myVar = 2;
    console.log(myVar); // variable environment es a()
    b();
}

var myVar = 1; // variable environment es el global execution context
console.log(myVar);
a();

// devuelve en consola:

// 1
// 2
// undefined
 ```
 
 ```javascript
function b() {
    var myVar;
    console.log(myVar);  // variable environment es b()
}

function b() {
    var myVar = 2;
    console.log(myVar); // variable environment es a()
    b();
}

var myVar = 1; // variable environment es el global execution context
console.log(myVar);
a();
console.log(myVar); // que va a devolver?

// devuelve en consola:

// 1
// 2
// undefined
// 1
 ```

todo esto sucede porque cada global execution context tiene su propia memoria para variables . o variable environment y cada variable vive en su contexto.



## 16. The Scope Chain

 ```javascript
function b() {
    console.log(myVar); // que va a devolver?
}

function a() {
    var myVar = 2;
    b();
}

var myVar = 1;
a(); 

// devuelve en consola:
// 1
 ```
 
cuando se ejecuta una variable se busca en el execution context, si no lo encuentra lo busca en el OUTER ENVIRONMENT, este outer environment no es el siguiente en el stack sino el lexical environment o donde esta ESCRITA la funcion. Entonces b() lexicamente esta en el global environment . por eso myVar es 1. El outer reference (elemento en el creation phase), apunta al lexical environment. 

Esto se llama Scope Chain.

 ```javascript

function a() {

    function b() {
        console.log(myVar); // que va a devolver?
    }

    var myVar = 2;
    b();
}

var myVar = 1;
a();
b(); // va a dar error porque no esta en el global.

// devuelve en consola:
// 2

 ```
 ```javascript

function a() {

    function b() {
        console.log(myVar); // que va a devolver?
    }
    // si no encuentra myVar en a() lo busca en el siguiente en el scope chain. es decir el global
    b();
}

var myVar = 1;
a();


// devuelve en consola:
// 1

 ```

## 17. Scope, ES6, and let

### Scope
where a variable is available in your code.

### let 
block scoping . No se puede usar hasta que el execution phase la determine. En vez de dar undefined da Error. y esta disponible SOLO dentro del block.
en un for loop da una variable distinta cada vez que el for loop se ejecuta.

### block
lo que esta adentro de llaves {}.



## 18. What About Asynchronous Callbacks?

### Asynchronous
More than one at a time 

### pregunta
Como hace javascript al ser sincrono ,ejecutar asincronamente?
Existe el javascript engine, con  el rendering engine, http request, etc , que se ejecutan asincronamente.

### Respuesta
Existe el EVENT QUEUE. cuando el stack esta limpio (se ejecutan todas las funciones) , javascript mira en el EVENT QUEUE. y cuando se ejecuta el event lo agrega al stack.

 ```javascript
// long running function
function waitThreeSeconds() {
    var ms = 3000 + new Date().getTime();
    while (new Date() < ms){}
    console.log('finished function'); // la funcion espera 3 segundos a ser ejecutada
}

function clickHandler() {
    console.log('click event!'); // da un console log al hacer click en el document   
}

# listen for the click event
document.addEventListener('click', clickHandler);


waitThreeSeconds();
console.log('finished execution'); // cuando termina devuelve el console log
 ```
Pero cuando se da el evento? (console.log('click event!'))

### Respuesta

si se da click dentro de los 3 segundos el console log dara:

finished function
finished execution
click event! 

todo porque se mira el event queue con eventos solo cuando todos los execution context de funciones termina.



## 19. Conceptual Aside: Types and Javascript

### Dynamic Typing
You dont tell the engine what type of data a variable holds. it figures it out while your code is running.  variables can hold different types of values because its all figured out during execution.

otros lenguajes de programacion tienen el static typing. le decis antes que tipo de variable es
ejemplo:
 ```
bool isNew = 'hello'; //error
 ```

en javascript es dinamico , no hace falta marcar que tipo es la variable.



### 20. Primitive Types

hay 6 primitive types( tipo de dato que representa un solo valor , no es un objeto):

* undefined (lack of existance);
* null (lack of existance.);
* boolean (true or false);
* number(floating point number no tiene integer ni otro , es solo number);
* string('caracteres' o "caracteres");
* symbol(nuevo en ES6);


## 21. Conceptual Aside: Operators

### Operators
a special function that is written differently. Toman dos parametros y DEVUELVEN UN RESULTADO.


### infix notation
la funcion esta en medio de los dos parametros

### prefix notation
la funcion esta antes de los dos parametros.

 ```
infix: 3 + 4;
prefix: +(3,4);  // no anda en javascript la funcion con el signo + es solo para ejemplo
 ```


## 22. Operator Precedence and Associativity

### Operator precedence 
which operator function gets called first. Higher precedence wins. when there are more than one in a line.


### Associativity
what order operator functions get called in: left-to-right or right-to-left. when functions have the same precedence.
 ```
https:#developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence
 ```
 
```javascript
var a = 3 + 4 * 5;
console.log(a);

# devuelve 23 por la presedencia
 ```
 
```javascript
var a = 2, b = 3 , c = 4;

a = b = c;

console.log(a);
console.log(b);
console.log(c);

// devuelve todo 4 (para las tres variables) porque el assignment tiene associativity right to left. por eso es b = c y despues a es igual a ese resultado
 ```

## 24. Conceptual Aside: Coercion

### Coercion
converting a value from one type to another.

 ```javascript

var a = 'hello' + ' world';
console.log(a); #devuelve hello world

var a = 1 + '2';
console.log(a); # devuelve '12' porque hizo coersion. 
 ```


### 25. Comparison Operators

 ```javascript
console.log(1 < 2 < 3);
#devuelve true

console.log(3 < 2 < 1);
// devuelve true 
 ```
### ¿porque?

### Respuesta
primero hay que ver la associativity (right-to left) entones se ejecuta 3 < 2 lo que devuelve false , entonces queda ( false < 1 ) entonces false coersiona a 0 y queda ( 0 < 1 ) lo que devuelve true


### formas sin cohersion
el triple igual (===) "strict inequality"

 ```javascript
console.log(3 == 3);
// devuelve true

console.log("3" == 3);
// true

console.log("3" === 3)
// false (no cohersiona)
 ```

### USAR SIEMPRE ===


## 26. Equality Comparisons Table

 ```
ver https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness
 ```




## 27. Existence and Booleans

La funcion Boolean(); cohersiona un valor a true o false.
entonces:
 ```
Boolean(undefined);  //devuelve false
Boolean(null);  //devuelve false
Boolean("");  //devuelve false
Boolean(0);  //devuelve false
 ```
If cohersiona a true o false
 ```javascript
var a;


if (a) { //  determina si a existe, como el valor es undefined, cohersiona a false y el if no es ejecutado 
  console.log('Something is there.');  
}

// CUIDADO con el cero que cohersiona a false (si se quiere verificar que a exista o tenga un valor , cuando sea 0 no va a ejecutar el if) , salvo que se realice lo siguiente

var a = 0;

if (a || a === 0) {
  console.log('Something is there.');  
}

//  ejecuta el if (en el caso anterior no)

 ```


## 27. Default Values
 ```javascript
function greet(name) {
    name = name || '<Your name here>'; // en este caso si no pasamos variable a name es undefined por lo tanto cohesiona a false , lo que lleva a que en el or se setee un 'default value'(en este caso <Your name here>)
    console.log('Hello ' + name);    
}

greet('Tony'); // devuelve Hello Tony
greet(); // devuelve Hello <Your name here>
 ```


## 28. Framework Aside: Default Values

En el HTML los script ponen el codigo en una lista de acuerdo al orden en que son llamados

ej .
 ```HTML
<script src="1.js"></script>

<!-- js tiene la siguiente variable (var libraryName = "uno";) -->
<script src="2.js"></script>

<!-- 2.js tiene la siguiente variable (var libraryName = "dos"; y console.log(libraryName)) -->
 ```
pone el contenido de 1.js arriba de 2.js entonces el resultado del console log es "dos", porque se reemplaza la variable

para que no se reemplazen las variables en los frameworks generalmente hacen esto:
 ```javascript
window.libraryName = window.libraryName || "dos";
 ```

para que no se reemplaze la variable libraryName en caso de existir



## 30. Objects and the Dot

* Un objeto es una coleccion de valores, puede tener properties y methods.
* El objeto tiene un address y references a las properties y methods en memoria.
* Se accede a estos properties y methods con el punto (.) y con brackets ( [] )

 ```javascript
var person = new Object();

person["firstname"] = "Tony";

var firstNameProperty = "firstname";

console.log(person[firstNameProperty]); // dinamicamente podemos agregar un key a logear en el syntax de brackets. Dinamicamente significa poner el key en una variable y asignar esa variable al bracket. esto no funciona con el dot o punto , no se puede hacer:

person.firstNameProperty; // no anda
person.firstname ; // si anda
 
 ```
 ```javascript

person.address = new Object(); // se crea un objeto dentro de otro objeto
person.address.street = '111 main st.';

person["address"]["street"] ; // tambien funciona
 ```
**SE RECOMIENDA USAR EL DOT**




## 31. Objects and Object Literals
 ```javascript
var person = {}; // esto es un object literal y es lo mismo a new Object(); usado en el ejemplo anterior


var Tony = { 
    firstname: 'Tony', 
    lastname: 'Alicea',
    address: {
        street: '111 Main St.',
        city: 'New York',
        state: 'NY'
    }
}; // esto es un object literal

function greet(person) {
    console.log('Hi ' + person.firstname);
} // llamo a la funcion y paso un objeto en la linea siguiente

greet(Tony); // se pasa un objeto 

greet({ // o se pasa un objeto "on the fly"
    firstname: 'Mary', 
    lastname: 'Doe' 
});

Tony.address2 = { 
    street: '333 Second St.'
}
 ```


## 32. Framework Aside: Faking Namespaces

## Namespace
a container for variables and functions. Typically to keep variables and functions with the same name separate

javascript no tiene namespaces. otros lenguajes si. por eso en los frameworks hacen fake namespaces , es decir se crea un objeto para cuando la variable es la misma , y se le aplican valores distintos a la misma variable.

 ```javascript
var spanish = {};
var english = {};

spanish.greet = 'Hola!';
english.greet = 'Hi!';

console.log(english);
 ```


## 33. JSON and Object Literals


### JSON : JavaScript Object Notation

Antes para mandar datos a traves de internet se usaba XML. pero eran muchos caracteres. 
en JSON los nombres de las propiedades estan entre "".
 ```JSON
{
    "firstname": "Mary",
    "isAProgrammer": true
}
```
Todo JSON es valido en object literal , pero no todo object literal es valido JSON.

para cualquier objeto se puede hacer
 ```javascript
JSON.stringify(object); // Esto le pone quotes ("") a los names y los pasa a string
 ```
y para un string se puede hacer 
 ```javascript
JSON.parse('{ "name": "javier" }'); // esto convierte el json a un objeto
 ```


## 34. Functions are Objects

### First class functions 
Everything you can do with other types (ex. primitive types) you can do with functions. Assign them to variables, pass them around , create them on the fly.

Function es un tipo especial de objeto. se pueden poner primitives, objetos y funciones dentro de esta . 

Tiene un **name** que puede ser anonimo (opcional) y tiene un codigo que va a ser ejecutado (propiedad **CODE**) , este code es **invocable**.

**las functions son como los objetos pero con la diferencia de tener un code (propiedad) , que va a ser ejecutado o invocado**

 ```javascript
function greet() {
    console.log('hi');   
}

greet.language = 'english';
console.log(greet.language); // devuelve english. estamos asignando una propiedad a la funcion (ya que es un objeto)
 ```


## 35. Function Statements and Function Expressions


### Expression
Unit of code that results in a value . it doesnt have to save inside a variable.

 ```javascript
var a;
a = 3;
// devuelve 3 (es una expresion)
 ```
  ```javascript
var a;

if (a === 3) {
    // (if) es un statement , no devuelve valor
}
```
 ```javascript

greet();

function greet() { // es un function statement , no hace nada en la ejecucion salvo que sea invocada
    console.log('hi');   
} 

var anonymousGreet = function() {// es una funcion anonima no tiene nombre despues de la palabra function
    console.log('hi');   // es un function expression , en ejecucion se crea un objeto del tipo funcion y se aplica a la variable anonymuous greet. es expression porque termina en un valor , en este caso el objeto de la funcion anonima 
}

anonymousGreet(); // devuelve hi

function log(a) {
   a();    
}

log(function() {
    console.log('hi');  // se pueden pasar function statements a otras funciones  
});

 ```



## 36. Conceptual Aside: By Value vs By Reference


### mutate
change something. Immutable: cant change.

### Primitive types: by value
### Object type: by reference

### by value (primitives)

 ```javascript
var a = 3; // setea un nuevo espacio en la memoria para la variable
var b;

b = a; // asigna otro espacio en memoria para b con el valor mismo que a (es decir ahora hay dos variables en memoria iguales)
a = 2; // cambia 2 en el valor de la memoria de a , b queda igual a 3 porque estaba en un bloque de memoria separado

console.log(a);
console.log(b);

// el console.log devuelve 2 y 3
 ```

### by reference (all objects (including functions))

```javascript
var c = { greeting: 'hi' }; // setea un nuevo espacio en la memoria para la variable
var d;

d = c; // asigna una referencia al espacio de memoria antes creado por c 
c.greeting = 'hello'; // mutate

console.log(c);
console.log(d);

// el console.log devuelve 'hello' para los dos , c y d
 ```
### by reference (even as parameters)

```javascript
function changeGreeting(obj) {
    obj.greeting = 'Hola'; //  mutate   
}

changeGreeting(d);
console.log(c);
console.log(d);

// el console.log devuelve 'hola' para las dos variables

// equals operator sets up new memory space (new address)
c = { greeting: 'howdy' };
console.log(c);
console.log(d);

// en este caso el console.log devuelve howdy y Hola porque al no existir el objeto que se asigno a c , el signo = setea un nuevo espacio en memoria para c , al cual d no hace referencia
```



## 37. Objects, Functions, and 'this'

Cuando una funcion es invocada se hace un nuevo execution context , con variable environment , referencia al outer environment , y THIS
```javascript
// Global 
console.log(this);
// devuelve el window object

// function statement
function a() {
    console.log(this);
}
a();
// devuelve el window object , cuando invocas una funcion todavia apunta al global object

//function expression
var b = function(){
    console.log(this);
}
// devuelve el window object tambien
```
```javascript

function a(){
    this.newVariable = 'hello'; // se hace 'attach' de la variable al global
}
a();

console.log(newVariable);
// devuelve la variable en el global

// Objetos
var c = {
    name : 'The c object',
    log: function() {
        console.log(this);
    }
}

c.log(); // devuelve el objeto como this , en objetos this hace referencia a el objeto en si a diferencia de las funciones que hace referencia al global.
```

**Problema:**

```javascript

var c = {
    name: 'The c object',
    log: function() {
        
        this.name = 'Updated c object';
        console.log(this); // devuelve el objeto con el name updated c object
        
        var setname = function(newname) {
            this.name = newname;   // en este caso en una funcion dentro de un metodo de un objeto , this hace referencia al global
        }
        setname('Updated again! The c object'); // esto no devuelve nada
        console.log(this); // devuelve Updated c object
    }
}

c.log();
```
**Solucion:** 
```javascript
var c = {
    name: 'The c object',
    log: function() {
        var self = this; // asignamos una variable a this para que cuando busque en el contexto lexico self haga referencia al objeto
        
        self.name = 'Updated c object';
        console.log(self);
        
        var setname = function(newname) {
            self.name = newname;   
        }
        setname('Updated again! The c object');
        console.log(self);
    }
}

c.log();

```


## 38. Conceptual Aside: Arrays - Collections of Anything

```javascript
var arr = new Array();
var arr = [1,2,3];
```

En javascript los arrays pueden contener cualquier type
```javascript
var arr = [
    1, 
   
    false , 
    
    { name: "javi" },
    
    function(name){
        var greeting = "hola";
        console.log(greeting + name);
    },
    
    "hello"
];

console.log(arr);
arr[3](arr[2].name); // devuelve holajavi
```


## 39. 'arguments' and spread

En el execution context de la funcion tambien se crea un keyword especial llamado 'arguments', tiene todos los valores que pasas a la funcion.

`argument:` parametros que pasas a una funcion. Javascript gives you a keyword of the same name that contains them all

```javascript
funcion greet(first,last,lan){
    console.log(first);
    console.log(last);
    console.log(lan);
}

greet(); // al no pasar argumentos ,no da error , settea los argumentos como undefined

```
Se pueden pasar parte de parametros , uno o dos (en este caso) y dejar otros como undefined.
```javascript
funcion greet(first,last,lan){

    lan = lan || 'español'; // default parameter .

    if(arguments.length === 0){
        console.log('missing parameters');
        return; // deja de ejecutar la funcion , las lineas siguientes no se ejecutan
    }

    console.log(first);
    console.log(last);
    console.log(lan);
    console.log(arguments); // contiene una lista de todos los argumentos que se pasaron , es un arraylike (no es exactamente un array porque no tiene todas las funciones del array)
}
```
**Spread**
```javascript
funcion greet(first,last,lan, ...other){

}

// ...other es el spread y todas los argumentos extra se ponen en un array determinado

greet('john', 'doe', 'es', '111 mainst', 'NY');// los ultimos dos entrarian al array del spread

```


## 40. Framework Aside: Function Overloading

Lo siguiente es un ejemplo de function overloading:

```javascript
function greet(firstname, lastname, language) {
        
    language = language || 'en';
    
    if (language === 'en') {
        console.log('Hello ' + firstname + ' ' + lastname);   
    }
    
    if (language === 'es') {
        console.log('Hola ' + firstname + ' ' + lastname);   
    }
    
}

function greetEnglish(firstname, lastname) {
    greet(firstname, lastname, 'en');   
}

function greetSpanish(firstname, lastname) {
    greet(firstname, lastname, 'es');   
}

greetEnglish('John', 'Doe');
greetSpanish('John', 'Doe');
```

## 41. Conceptual Aside: Syntax Parsers

El syntax parser lee letra por letra el codigo y define que se va a realizar .


## 42. Dangerous Aside: Automatic Semicolon Insertion

Los semicolon `;` son opcionales , javascript los pone automaticamente . Pero como regla siempre hay que poner semicolon para evitar confusiones.

Ejemplo:
```javascript
return // en una sola linea se traduce a return; (lo que sigue en las siguientes lineas no lo ejecuta)
```

## 43. Framework Aside: Whitespace

`whitespace:` invisible caracters that create space in your code.
>En frameworks hay mucho espacio y comments.



## 44. Immediately Invoked Functions Expressions (IIFEs)

### Using an Immediately Invoked Function Expression (IIFE)
```javascript
var greeting = function(name) {
    
    return 'Hello ' + name;
    
}('John');

```

### IIFE
```javascript
var firstname = 'John';

(function(name) {
    
    var greeting = 'Inside IIFE: Hello';
    console.log(greeting + ' ' + name);
    
}(firstname)); // IIFE

```


## 45. Framework Aside: IIFEs and Safe Code

>Lo que permiten las IIFEs es evitar que al haber muchos archivos , el nombre de una funcion colisione con otra


## 46. Understanding Closures



