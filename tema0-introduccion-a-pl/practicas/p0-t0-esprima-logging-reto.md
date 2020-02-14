---
layout: default
title: p0-t0-esprima-logging-reto
formulario: https://campusvirtual.ull.es/1920/mod/assign/view.php?id=187733
permalink: /tema0-introduccion-a-pl/practicas/p0-t0-esprima-logging-reto/
next:
  url: /tema1-introduccion-a-javascript/practicas/p1-t1-iaas/
previous: 
  url: /tema0-introduccion-a-pl/practicas/p0-t0-esprima-logging/
---

# Práctica {{ page.title }}

## Reto 1: Funciones Flecha Gorda

Añada la capacidad de procesar funciones con sintáxis ECMA6 *flecha gorda* como:

```js
let z = (e => e +1)(4);
```

Ejemplo de ejecución:

```js
[~/.../eval/p0-t0-esprima-logging(master)]$ ./logging-espree.js input.js -o output.js 
input:
function foo(a, b, c) {
  let x = 'tutu';
  let y = (function (x) { return x*x })(2);
  let z = (e => e +1)(4);
  console.log(x,y,z);
}
foo(1, 'wut', 3);
---
Output in file 'output.js'
[~/.../eval/p0-t0-esprima-logging-CristoNavarro(master)]$ node output.js 
Entering foo(1, wut, 3) at line 1
Entering <anonymous function>(2) at line 3
tutu 4 5
```

Vea aquí [El AST Espree de este ejemplo](https://astexplorer.net/#/gist/b5826862c47dfb7dbb54cec15079b430/latest). Use el parser de `espree` pasándole la opción `ecmaVersion`:

```js
const ast = espree.parse(code, {ecmaVersion:6});
```

## Reto 2: Número de Línea

Añada el número de línea a la información de log de la función en la que se entra:

```
[~/javascript-learning/esprima-pegjs-jsconfeu-talk(develop)]$ ./p0-t0-esprima-logging-sol.js input.js -o salida.js
input:
```
```js
function foo(a, b) {
  var x = 'blah';
  var y = (function (z) {
    return z+3;
  })(2);
}
foo(1, 'wut', 3);
```
```
---
Output in file 'salida.js'
[esprima-pegjs-jsconfeu-talk(develop)]$ cat salida.js
```
```js
function foo(a, b) {
    console.log(`Entering foo(${ a },${ b }) at line 1`);
    var x = 'blah';
    var y = function (z) {
        console.log(`Entering <anonymous function>(${ z }) at line 3`);
        return z + 3;
    }(2);
}
```
```
foo(1, 'wut', 3);[vi esprima-pegjs-jsconfeu-talk(develop)]$ node salida.js 
Entering foo(1,wut) at line 1
Entering <anonymous function>(2) at line 3
```