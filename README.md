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

  ## localStorage

**`localStorage`** es una API del navegador que permite almacenar datos de manera persistente en el cliente (es decir, en el navegador del usuario). Los datos almacenados en `localStorage` no tienen fecha de expiración y permanecen disponibles incluso después de cerrar y volver a abrir el navegador.

---

### Características principales:
1. **Almacenamiento clave-valor**: Los datos se almacenan como pares clave-valor en formato de texto.
2. **Persistencia**: Los datos permanecen almacenados hasta que se eliminen manualmente o mediante código.
3. **Capacidad**: Generalmente, tiene un límite de 5-10 MB por dominio.
4. **Acceso sincrónico**: Las operaciones de lectura y escritura son sincrónicas.
5. **Solo en el navegador**: No está disponible en Node.js, ya que es una API del navegador.

---

### Métodos principales de `localStorage`:

1. **`setItem(key, value)`**:
   - Almacena un valor asociado a una clave.
   ```js
   localStorage.setItem('nombre', 'Juan');
   ```

2. **`getItem(key)`**:
   - Recupera el valor asociado a una clave.
   ```js
   const nombre = localStorage.getItem('nombre');
   console.log(nombre); // 'Juan'
   ```

3. **`removeItem(key)`**:
   - Elimina un elemento almacenado por su clave.
   ```js
   localStorage.removeItem('nombre');
   ```

4. **`clear()`**:
   - Elimina todos los datos almacenados en `localStorage`.
   ```js
   localStorage.clear();
   ```

5. **`length`**:
   - Devuelve el número de elementos almacenados.
   ```js
   console.log(localStorage.length);
   ```

6. **`key(index)`**:
   - Devuelve la clave en una posición específica.
   ```js
   const primeraClave = localStorage.key(0);
   console.log(primeraClave);
   ```

---

### Ejemplo básico:
```js
// Almacenar datos
localStorage.setItem('usuario', 'Juan');
localStorage.setItem('edad', '30');

// Recuperar datos
console.log(localStorage.getItem('usuario')); // 'Juan'
console.log(localStorage.getItem('edad')); // '30'

// Eliminar un dato
localStorage.removeItem('edad');

// Limpiar todo
localStorage.clear();
```

---

### Almacenamiento de objetos:
Como `localStorage` solo almacena cadenas de texto, debes convertir objetos a JSON antes de almacenarlos.

```js
const usuario = { nombre: 'Juan', edad: 30 };

// Almacenar un objeto
localStorage.setItem('usuario', JSON.stringify(usuario));

// Recuperar el objeto
const usuarioRecuperado = JSON.parse(localStorage.getItem('usuario'));
console.log(usuarioRecuperado.nombre); // 'Juan'
```

---

### Diferencias entre `localStorage` y `sessionStorage`:
| Característica       | `localStorage`                     | `sessionStorage`                  |
|----------------------|-------------------------------------|------------------------------------|
| **Persistencia**     | Los datos permanecen indefinidamente. | Los datos se eliminan al cerrar la pestaña. |
| **Capacidad**        | Generalmente 5-10 MB.              | Generalmente 5-10 MB.             |
| **Alcance**          | Compartido entre pestañas del mismo dominio. | Solo disponible en la pestaña actual. |

---

### Limitaciones:
1. **Tamaño limitado**: No es adecuado para almacenar grandes cantidades de datos.
2. **Solo texto**: No puede almacenar directamente datos binarios o complejos (aunque puedes usar JSON).
3. **No seguro**: Los datos no están cifrados, por lo que no es recomendable almacenar información sensible.

---

### Resumen:
- **`localStorage`** es una API del navegador para almacenar datos clave-valor de manera persistente.
- Es útil para guardar configuraciones, preferencias del usuario o datos que no cambian con frecuencia.
- Los datos permanecen disponibles incluso después de cerrar el navegador, a menos que se eliminen manualmente o mediante código.

---
## this
En **JavaScript**, **`this`** es una palabra clave especial que hace referencia al **contexto de ejecución** en el que se encuentra el código. El valor de `this` depende de **cómo** y **dónde** se llama una función o método.

---

### Conceptos básicos de `this`:

1. **`this` en el contexto global**:
   - En el navegador, `this` en el contexto global hace referencia al objeto global `window`.
   - En Node.js, hace referencia al objeto global `global`.

   **Ejemplo:**
   ```javascript
   console.log(this); // En el navegador: window, en Node.js: global
   ```

---

2. **`this` dentro de un objeto**:
   - Cuando `this` se usa dentro de un método de un objeto, hace referencia al **objeto que contiene el método**.

   **Ejemplo:**
   ```javascript
   const persona = {
     nombre: "Juan",
     saludar: function () {
       console.log(`Hola, soy ${this.nombre}`);
     },
   };

   persona.saludar(); // Hola, soy Juan
   ```

   Aquí, `this.nombre` hace referencia a la propiedad `nombre` del objeto `persona`.

---

3. **`this` en una función normal**:
   - En una función normal, el valor de `this` depende de **cómo se llama la función**:
     - Si se llama en el contexto global, `this` será el objeto global (`window` en el navegador o `global` en Node.js).
     - Si se llama como un método de un objeto, `this` será el objeto que contiene la función.

   **Ejemplo:**
   ```javascript
   function mostrarThis() {
     console.log(this);
   }

   mostrarThis(); // En el navegador: window, en Node.js: global

   const obj = {
     metodo: mostrarThis,
   };

   obj.metodo(); // { metodo: [Function: mostrarThis] }
   ```

---

4. **`this` en una función flecha**:
   - Las funciones flecha **no tienen su propio `this`**. En su lugar, heredan el valor de `this` del contexto en el que fueron definidas.

   **Ejemplo:**
   ```javascript
   const persona = {
     nombre: "Ana",
     saludar: () => {
       console.log(`Hola, soy ${this.nombre}`);
     },
   };

   persona.saludar(); // Hola, soy undefined
   ```

   Aquí, `this` no hace referencia al objeto `persona`, sino al contexto global, porque las funciones flecha no tienen su propio `this`.

---

5. **`this` en una clase**:
   - En una clase, `this` hace referencia a la instancia de la clase.

   **Ejemplo:**
   ```javascript
   class Persona {
     constructor(nombre) {
       this.nombre = nombre;
     }

     saludar() {
       console.log(`Hola, soy ${this.nombre}`);
     }
   }

   const juan = new Persona("Juan");
   juan.saludar(); // Hola, soy Juan
   ```

---

6. **`this` con `call`, `apply` y `bind`**:
   - Puedes cambiar el valor de `this` manualmente usando los métodos `call`, `apply` o `bind`.

   **Ejemplo con `call`:**
   ```javascript
   function saludar() {
     console.log(`Hola, soy ${this.nombre}`);
   }

   const persona = { nombre: "Carlos" };

   saludar.call(persona); // Hola, soy Carlos
   ```

   **Ejemplo con `bind`:**
   ```javascript
   const saludarCarlos = saludar.bind(persona);
   saludarCarlos(); // Hola, soy Carlos
   ```

---

### Resumen con ejemplos simples:

1. **Global:**
   ```javascript
   console.log(this); // window (en el navegador)
   ```

2. **En un objeto:**
   ```javascript
   const obj = {
     nombre: "Pedro",
     mostrar: function () {
       console.log(this.nombre);
     },
   };

   obj.mostrar(); // Pedro
   ```

3. **En una función normal:**
   ```javascript
   function prueba() {
     console.log(this);
   }

   prueba(); // window (en el navegador)
   ```

4. **En una función flecha:**
   ```javascript
   const flecha = () => {
     console.log(this);
   };

   flecha(); // window (en el navegador)
   ```

5. **En una clase:**
   ```javascript
   class Animal {
     constructor(nombre) {
       this.nombre = nombre;
     }

     hablar() {
       console.log(this.nombre);
     }
   }

   const perro = new Animal("Firulais");
   perro.hablar(); // Firulais
   ```

---

### Nota clave:
El valor de `this` **depende del contexto de ejecución**, y puede cambiar dependiendo de cómo se llame la función o método. Si tienes dudas, puedes usar `console.log(this)` para inspeccionar su valor en diferentes contextos.

En JavaScript, `call`, `apply` y `bind` son métodos que permiten manipular el valor de `this` en funciones. Aquí te explico cada uno con ejemplos y una tabla comparativa:

---

### **1. `call`**
El método `call` invoca una función con un valor específico de `this` y permite pasar argumentos de forma individual.

```javascript
function greet(greeting) {
    console.log(`${greeting}, I am ${this.name}`);
}

const person = { name: "Alice" };

greet.call(person, "Hello"); // Output: "Hello, I am Alice"
```

En este ejemplo, usamos call para que this dentro de la función greet apunte al objeto person.

---

### **2. `apply`**
El método `apply` es similar a `call`, pero los argumentos se pasan como un array o un objeto similar a un array.

```javascript
function greet(greeting, punctuation) {
    console.log(`${greeting}, I am ${this.name}${punctuation}`);
}

const person = { name: "Bob" };

greet.apply(person, ["Hi", "!"]); // Output: "Hi, I am Bob!"
```

---

### **3. `bind`**
El método `bind` no ejecuta la función inmediatamente. En su lugar, devuelve una nueva función con un valor fijo de `this`.

```javascript
function greet(greeting) {
    console.log(`${greeting}, I am ${this.name}`);
}

const person = { name: "Charlie" };

const boundGreet = greet.bind(person);
boundGreet("Hey"); // Output: "Hey, I am Charlie"
```

---

### **Tabla Comparativa**

| Método  | ¿Qué hace?                                                                 | ¿Cuándo se ejecuta?         | ¿Cómo se pasan los argumentos? |
|---------|----------------------------------------------------------------------------|-----------------------------|---------------------------------|
| `call`  | Invoca la función con un valor específico de `this`.                       | Inmediatamente.             | Como argumentos individuales.  |
| `apply` | Invoca la función con un valor específico de `this`.                       | Inmediatamente.             | Como un array o similar.       |
| `bind`  | Crea una nueva función con un valor fijo de `this` (no la ejecuta).        | Cuando se llama la nueva función. | Como argumentos individuales.  |

---

Estos métodos son útiles para controlar el contexto de `this`, especialmente en funciones reutilizables o en programación orientada a objetos.


