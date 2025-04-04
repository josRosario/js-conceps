# JavaScript: Conceptos

# 📦 Promesas en JavaScript

Una **promesa** en JavaScript es un objeto que representa la **eventual finalización (o falla)** de una operación **asíncrona** y su **valor resultante**.

## 🧠 ¿Para qué sirven?

Las promesas se utilizan para manejar tareas asíncronas como:
- Llamadas a APIs
- Esperas con `setTimeout`
- Lectura de archivos
- Otras operaciones que toman tiempo y no deben bloquear el flujo del programa



---

## 🛠️ Sintaxis básica

```js
const promesa = new Promise((resolve, reject) => {
  // lógica asíncrona
  if (todoSalióBien) {
    resolve('Éxito');
  } else {
    reject('Algo salió mal');
  }
});

```

##  Promise.all 
- Espera a que todas las promesas se resuelvan o una falle.
- Si una promesa falla, el conjunto entero rechaza con ese error.
- Si todas se resuelven, devuelve un array con sus resultado


```js

  const p1 = Promise.resolve(10);
  const p2 = new Promise((resolve) => setTimeout(() => resolve(20), 1000));
  const p3 = Promise.reject('Error');
  Promise.all([p1, p2, p3])
    .then(console.log)
    .catch(console.error); // 'Error
```

##  Promise.allSettled
- Espera a que todas las promesas terminen, sin importar si se resuelven o rechazan.
- Devuelve un array con objetos { status, value } para resueltas o { status, reason } para
 rechazadas.

```js
const p1 = Promise.resolve(10);
  const p2 = Promise.reject('Error');
  const p3 = new Promise((resolve) => setTimeout(() => resolve(30), 1000));
  Promise.allSettled([p1, p2, p3]).then(console.log);
```


## promise.any
- Devuelve el primer resultado exitoso (resuelto).
-  Si todas las promesas fallan, devuelve un AggregateError.

```js

const p1 = Promise.reject('Error 1');
  const p2 = new Promise((resolve) => setTimeout(() => resolve(20), 500));
  const p3 = Promise.reject('Error 2');
  Promise.any([p1, p2, p3])
    .then(console.log) // 20
    .catch(console.error);


```


## promise.race
- Devuelve la promesa que termine primero, ya sea resuelta o rechazada.

```js
 const p1 = new Promise((resolve) => setTimeout(() => resolve('Ganó P1'), 1000));
  const p2 = new Promise((_, reject) => setTimeout(() => reject('Falló P2'), 500));
  Promise.race([p1, p2])
    .then(console.log) // 'Falló P2'
    .catch(console.error)
```

---

# ⚙️ Tipos de Funciones en JavaScript

JavaScript permite declarar funciones de múltiples maneras, cada una con sus particularidades en cuanto a sintaxis, comportamiento y uso. Esta guía resume todos los tipos de funciones con ejemplos claros y precisos.

---

## 🧱 1. Función Declarada (Function Declaration)

```js
function saludar(nombre) {
  return `Hola, ${nombre}`;
}

```
- ✅ Se puede llamar antes de ser definida (hoisting).

- ✅ Tiene su propio this.

- ✅ Ideal para funciones reutilizables.


## 🧾 2. Función Expresada (Function Expression)

```js
const despedir = function(nombre) {
  return `Adiós, ${nombre}`;
};

```

- ❌ No tiene hoisting.

- 📦 Asignada a una variable.

- 🔍 Puede ser anónima o con nombre (útil para depuración).

 ## 🎯 3. Arrow Function (Función de Flecha) 

 ```js
const sumar = (a, b) => a + b;

const saludar = (nombre) => {
  const mensaje = `Hola, ${nombre}`;
  return mensaje;
};

```

- ⚡ Sintaxis más corta.

- ❌ No tiene su propio this, arguments, ni super.

- ✅ Ideal para callbacks y funciones de una sola línea.


## 🌀 4. Función Generadora (Generator Function)

```js
function* generador() {
  yield 1;
  yield 2;
  yield 3;
}
```

- 🔁 Se puede pausar y reanudar con yield.

- ✅ Devuelve un iterador que se maneja con next().

- Útil para secuencias controladas y procesamiento paso a paso.

## ⏳ 5. Función Asíncrona (Async Function)

```js
async function obtenerDatos() {
  const respuesta = await fetch('https://api.com');
  return await respuesta.json();
}
```

- ⏱️ Siempre retorna una promesa.

- ✅ Puede usar await para operaciones asíncronas.

- ✅ Uso moderno y legible para trabajar con Promesas.

## ⚡ 6. IIFE (Immediately Invoked Function Expression)
```js
(function () {
  console.log('Soy una IIFE');
})();
```

- 🚀 Se ejecuta inmediatamente tras definirse.

- 🔒 Crea un ámbito privado.

- ✅ Común en patrones de módulos y librerías.

También puede usarse como función de flecha:
```js
(() => {
  console.log('IIFE con arrow');
})();
```

## 🧠 7. Método en Objeto
```js
const persona = {
  nombre: 'Ana',
  saludar() {
    return `Hola, soy ${this.nombre}`;
  }
};
```

- 📦 Declarado como propiedad de un objeto.

- ✅ Tiene acceso a this del objeto contenedor.

- Común en programación orientada a objetos.


## 🧩 8. Función como Propiedad (Function Expression en objeto)

```js
const calculadora = {
  sumar: function(a, b) {
    return a + b;
  }
};
```

- 🔧 Función asignada como propiedad.

- ✅ Similar a la función expresada pero dentro de un objeto.


## 🏗️ 9. Función Constructora

```js
function Persona(nombre) {
  this.nombre = nombre;
}

const juan = new Persona('Juan');
```

- 🏗️ Usada con new para crear instancias.

- ✅ Define propiedades y métodos en objetos.

- 📌 Reemplazada en muchos casos por class, pero sigue siendo válida.
