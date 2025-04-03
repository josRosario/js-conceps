# JavaScript: 50 Preguntas y Respuestas Frecuentes

Este archivo contiene 50 preguntas y respuestas comunes sobre JavaScript con ejemplos de código.

## 1. ¿Qué es JavaScript?
JavaScript es un lenguaje de programación de alto nivel utilizado para crear interactividad en páginas web.

```js
console.log("Hola, JavaScript!");
```

## 2. ¿Cómo declarar una variable en JavaScript?
Se pueden usar `var`, `let` o `const`.

```js
let nombre = "Juan";
const edad = 25;
var ciudad = "Santo Domingo";
```

## 3. ¿Cuál es la diferencia entre `let`, `var` y `const`?
- `var`: Tiene alcance de función.
- `let`: Tiene alcance de bloque.
- `const`: No se puede reasignar.

```js
var x = 10;
let y = 20;
const z = 30;
```

## 4. ¿Qué es el `typeof` en JavaScript?
Muestra el tipo de dato de una variable.

```js
console.log(typeof 42); // "number"
console.log(typeof "Hola"); // "string"
```

## 5. ¿Cómo convertir una cadena en número en JavaScript?
Se usa `Number()`, `parseInt()` o `parseFloat()`.

```js
let num = Number("42");
let entero = parseInt("42");
let decimal = parseFloat("42.5");
```

## 6. ¿Cómo convertir un número en cadena?
Se usa `String()` o `.toString()`.

```js
let num = 42;
console.log(String(num)); // "42"
console.log(num.toString()); // "42"
```

## 7. ¿Qué es una función en JavaScript?
Es un bloque de código reutilizable.

```js
function saludar(nombre) {
  return "Hola, " + nombre;
}
console.log(saludar("Carlos"));
```

## 8. ¿Qué es una función flecha?
Una forma más corta de escribir funciones.

```js
const sumar = (a, b) => a + b;
console.log(sumar(2, 3)); // 5
```

## 9. ¿Qué es `null` y `undefined`?
- `null`: Valor intencionalmente vacío.
- `undefined`: Valor no asignado.

```js
let x = null;
let y;
console.log(y); // undefined
```

## 10. ¿Qué es un array en JavaScript?
Es una estructura de datos que almacena múltiples valores.

```js
let frutas = ["Manzana", "Banana", "Uva"];
console.log(frutas[1]); // "Banana"
```

## 11-50. Otras Preguntas y Respuestas

### 11. ¿Cómo agregar elementos a un array?
```js
let numeros = [1, 2, 3];
numeros.push(4);
console.log(numeros);
```

### 12. ¿Cómo eliminar elementos de un array?
```js
let frutas = ["Manzana", "Banana", "Uva"];
frutas.pop();
console.log(frutas);
```

### 13. ¿Cómo iterar un array?
```js
let numeros = [1, 2, 3];
numeros.forEach(num => console.log(num));
```

### 14. ¿Qué es un objeto?
```js
let persona = { nombre: "Carlos", edad: 30 };
console.log(persona.nombre);
```

### 15. ¿Cómo agregar propiedades a un objeto?
```js
let usuario = { nombre: "Ana" };
usuario.edad = 25;
console.log(usuario);
```

### 16. ¿Cómo eliminar una propiedad de un objeto?
```js
delete persona.edad;
console.log(persona);
```

### 17. ¿Qué es la desestructuración?
```js
let { nombre, edad } = persona;
console.log(nombre);
```

### 18. ¿Cómo usar el operador ternario?
```js
let acceso = edad >= 18 ? "Permitido" : "Denegado";
```

### 19. ¿Qué es una promesa?
```js
let promesa = new Promise((resolve) => {
  setTimeout(() => resolve("Completado"), 2000);
});
promesa.then(console.log);
```

### 20. ¿Cómo manejar errores en promesas?
```js
promesa.catch(console.log);
```

### 21. ¿Qué es async/await?
```js
async function obtenerDatos() {
  let data = await fetch("https://api.example.com");
  return data.json();
}
```

### 22. ¿Cómo usar `fetch`?
```js
fetch("https://api.example.com")
  .then(res => res.json())
  .then(data => console.log(data));
```

### 23. ¿Cómo almacenar datos en `localStorage`?
```js
localStorage.setItem("usuario", "Juan");
console.log(localStorage.getItem("usuario"));
```

### 24. ¿Cómo remover datos de `localStorage`?
```js
localStorage.removeItem("usuario");
```

### 25. ¿Cómo funciona `setTimeout`?
```js
setTimeout(() => console.log("Hola"), 2000);
```

### 26. ¿Cómo funciona `setInterval`?
```js
let intervalo = setInterval(() => console.log("Repetir"), 1000);
clearInterval(intervalo);
```

### 50. ¿Qué es una expresión regular?
```js
let regex = /hola/i;
console.log(regex.test("Hola Mundo"));
```

---
