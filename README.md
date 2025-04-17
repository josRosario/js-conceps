# JavaScript: Conceptos

En JavaScript, los conceptos de **funciones de primer orden** y **funciones de orden superior** están relacionados con cómo se manejan las funciones en el lenguaje. JavaScript trata a las funciones como **ciudadanos de primera clase** (*first-class citizens*), lo que significa que las funciones pueden ser asignadas a variables, pasadas como argumentos y devueltas desde otras funciones. Esto permite trabajar con funciones de manera flexible y poderosa.

---
**Debounce** es una técnica utilizada en programación para limitar la frecuencia con la que se ejecuta una función. Es especialmente útil en situaciones donde una función se llama repetidamente en un corto período de tiempo, como en eventos de entrada del usuario (por ejemplo, `keyup`, `scroll`, `resize`). El debounce asegura que la función solo se ejecute después de que haya pasado un período de tiempo específico desde la última vez que se activó el evento.

### Ejemplo de uso de debounce en JavaScript

Aquí tienes un ejemplo básico de cómo implementar y usar debounce:

```javascript
// Implementación de debounce
function debounce(func, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

// Ejemplo de uso
const handleResize = () => {
  console.log('Window resized:', new Date().toISOString());
};

// Aplicar debounce con un retraso de 500ms
const debouncedResize = debounce(handleResize, 500);

// Escuchar el evento de redimensionamiento de la ventana
window.addEventListener('resize', debouncedResize);
```

### Explicación del código

1. **Función `debounce`**:
   - Recibe dos parámetros: la función que deseas ejecutar (`func`) y el tiempo de espera en milisegundos (`delay`).
   - Usa un temporizador (`timeoutId`) para retrasar la ejecución de la función.
   - Si el evento se dispara nuevamente antes de que el temporizador expire, se cancela el temporizador anterior con `clearTimeout`.

2. **Uso práctico**:
   - En este ejemplo, la función `handleResize` se ejecutará solo después de que hayan pasado 500ms desde la última vez que se activó el evento `resize`.

### Ejemplo práctico con un campo de búsqueda

```javascript
const searchInput = document.getElementById('search');

const fetchResults = (query) => {
  console.log(`Fetching results for: ${query}`);
};

const debouncedFetchResults = debounce(fetchResults, 300);

searchInput.addEventListener('input', (event) => {
  debouncedFetchResults(event.target.value);
});
```

En este caso, la función `fetchResults` se ejecutará solo después de que el usuario deje de escribir durante 300ms, evitando llamadas innecesarias a la API mientras el usuario escribe.

---
### **1. Funciones de Primer Orden**
- **Definición**: Una función de primer orden es simplemente una función que no toma otras funciones como argumentos ni devuelve funciones como resultado. Es una función "normal" que realiza una tarea específica.
- **Ejemplo**:
  ```javascript
  function saludar(nombre) {
      return `Hola, ${nombre}`;
  }

  console.log(saludar('Juan')); // "Hola, Juan"
  ```

En este caso, `saludar` es una función de primer orden porque no interactúa con otras funciones.

---

**Throttling** es una técnica utilizada en programación para controlar la frecuencia con la que se ejecuta una función. A diferencia de **debounce**, que retrasa la ejecución de la función hasta que los eventos se detienen, **throttling** asegura que la función se ejecute a intervalos regulares, incluso si los eventos ocurren con mayor frecuencia.

Esto es útil para manejar eventos que se disparan continuamente, como `scroll`, `mousemove`, o `resize`, y evitar que una función se ejecute demasiadas veces en un corto período de tiempo, lo que podría afectar el rendimiento.

### Ejemplo de Throttling en JavaScript

Aquí tienes un ejemplo básico de cómo implementar y usar throttling:

```javascript
// Implementación de throttling
function throttle(func, limit) {
  let lastFunc;
  let lastRan;
  return function (...args) {
    const context = this;
    if (!lastRan) {
      func.apply(context, args);
      lastRan = Date.now();
    } else {
      clearTimeout(lastFunc);
      lastFunc = setTimeout(() => {
        if (Date.now() - lastRan >= limit) {
          func.apply(context, args);
          lastRan = Date.now();
        }
      }, limit - (Date.now() - lastRan));
    }
  };
}

// Ejemplo de uso
const handleScroll = () => {
  console.log('Scroll event triggered:', new Date().toISOString());
};

// Aplicar throttling con un límite de 1000ms
const throttledScroll = throttle(handleScroll, 1000);

// Escuchar el evento de scroll
window.addEventListener('scroll', throttledScroll);
```

### Explicación del código

1. **Función `throttle`**:
   - Recibe dos parámetros: la función que deseas ejecutar (`func`) y el intervalo de tiempo en milisegundos (`limit`).
   - Usa dos variables:
     - `lastRan`: Almacena el momento en que la función se ejecutó por última vez.
     - `lastFunc`: Almacena un temporizador para ejecutar la función después del intervalo si es necesario.

2. **Uso práctico**:
   - En este ejemplo, la función `handleScroll` se ejecutará como máximo una vez por segundo (1000ms), incluso si el evento `scroll` se dispara continuamente.

### Diferencia entre Debounce y Throttle

| **Característica**       | **Debounce**                                      | **Throttle**                                      |
|--------------------------|--------------------------------------------------|--------------------------------------------------|
| **Ejecución**            | Solo después de que los eventos se detienen.     | A intervalos regulares mientras ocurren eventos. |
| **Uso común**            | Campos de búsqueda, validación de formularios.   | Scroll, resize, mousemove.                       |
| **Frecuencia de ejecución** | Baja frecuencia si los eventos son continuos.   | Frecuencia constante según el límite definido.   |

Ambas técnicas son útiles para optimizar el rendimiento en aplicaciones web, dependiendo del caso de uso.

Similar code found with 1 license type


---

### **2. Funciones de Orden Superior**
- **Definición**: Una función de orden superior es una función que:
  1. **Recibe una función como argumento**, o
  2. **Devuelve otra función como resultado**, o ambas.
- **Uso**: Estas funciones son fundamentales en la programación funcional y son comunes en JavaScript, especialmente al trabajar con métodos como `map`, `filter`, `reduce`, etc.

#### **Ejemplo 1: Recibir una función como argumento**
```javascript
function procesarTexto(texto, callback) {
    return callback(texto);
}

function convertirMayusculas(texto) {
    return texto.toUpperCase();
}

console.log(procesarTexto('hola', convertirMayusculas)); // "HOLA"
```
En este ejemplo:
- `procesarTexto` es una función de orden superior porque recibe otra función (`callback`) como argumento.
- `convertirMayusculas` es una función de primer orden.

#### **Ejemplo 2: Devolver una función**
```javascript
function crearMultiplicador(multiplicador) {
    return function (numero) {
        return numero * multiplicador;
    };
}

const multiplicarPorDos = crearMultiplicador(2);
console.log(multiplicarPorDos(5)); // 10
```
En este ejemplo:
- `crearMultiplicador` es una función de orden superior porque devuelve otra función.
- La función devuelta (anónima) es una función de primer orden.

---

### **3. Funciones como Ciudadanos de Primera Clase**
En JavaScript, las funciones son **ciudadanos de primera clase**, lo que significa que pueden:
1. **Asignarse a variables**:
   ```javascript
   const sumar = function (a, b) {
       return a + b;
   };
   console.log(sumar(2, 3)); // 5
   ```

2. **Pasarse como argumentos**:
   ```javascript
   function ejecutarOperacion(a, b, operacion) {
       return operacion(a, b);
   }

   function restar(a, b) {
       return a - b;
   }

   console.log(ejecutarOperacion(5, 3, restar)); // 2
   ```

3. **Devolverse desde otras funciones**:
   ```javascript
   function crearSaludo(saludo) {
       return function (nombre) {
           return `${saludo}, ${nombre}`;
       };
   }

   const saludarEnIngles = crearSaludo('Hello');
   console.log(saludarEnIngles('John')); // "Hello, John"
   ```

---

### **4. Métodos de Orden Superior en JavaScript**
JavaScript incluye varios métodos de orden superior en los objetos como `Array`. Estos métodos son funciones de orden superior porque reciben otras funciones como argumentos.

#### **Ejemplo: `map`**
```javascript
const numeros = [1, 2, 3, 4];
const cuadrados = numeros.map(num => num * num);
console.log(cuadrados); // [1, 4, 9, 16]
```

#### **Ejemplo: `filter`**
```javascript
const numeros = [1, 2, 3, 4];
const pares = numeros.filter(num => num % 2 === 0);
console.log(pares); // [2, 4]
```

#### **Ejemplo: `reduce`**
```javascript
const numeros = [1, 2, 3, 4];
const suma = numeros.reduce((acumulador, num) => acumulador + num, 0);
console.log(suma); // 10
```

---

### **5. Ventajas de las Funciones de Orden Superior**
1. **Reutilización de código**:
   - Puedes escribir funciones genéricas que acepten diferentes comportamientos a través de callbacks.
2. **Legibilidad**:
   - Facilitan la escritura de código más declarativo y fácil de entender.
3. **Composición**:
   - Permiten combinar funciones pequeñas para crear funcionalidades más complejas.

---

### **6. Diferencias entre Funciones de Primer Orden y de Orden Superior**

| **Característica**               | **Funciones de Primer Orden**                     | **Funciones de Orden Superior**                     |
|----------------------------------|--------------------------------------------------|----------------------------------------------------|
| **Definición**                   | No interactúan con otras funciones.              | Reciben o devuelven otras funciones.               |
| **Interacción con otras funciones** | No reciben ni devuelven funciones.               | Pueden recibir o devolver funciones.               |
| **Ejemplo**                      | `function sumar(a, b) { return a + b; }`         | `array.map(num => num * 2)`                        |

---

### **Conclusión**
- **Funciones de primer orden**: Son funciones "normales" que no interactúan con otras funciones.
- **Funciones de orden superior**: Son funciones que reciben o devuelven otras funciones, y son fundamentales en la programación funcional.
- JavaScript, al tratar las funciones como ciudadanos de primera clase, permite trabajar con ellas de manera flexible, lo que facilita la creación de código modular, reutilizable y declarativo.


---

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


En JavaScript, los tipos de datos se dividen en **primitivos** y **complejos**. Aquí tienes una lista con ejemplos:

---

### **Tipos de datos primitivos**
1. **`String`**: Representa texto.
   ```javascript
   let name = "Alice";
   console.log(typeof name); // "string"
   ```

2. **`Number`**: Representa números (enteros o decimales).
   ```javascript
   let age = 25;
   let pi = 3.14;
   console.log(typeof age); // "number"
   ```

3. **`BigInt`**: Representa números enteros muy grandes.
   ```javascript
   let bigNumber = 123456789012345678901234567890n;
   console.log(typeof bigNumber); // "bigint"
   ```

4. **`Boolean`**: Representa valores lógicos (`true` o `false`).
   ```javascript
   let isActive = true;
   console.log(typeof isActive); // "boolean"
   ```

5. **`Undefined`**: Representa una variable declarada pero no inicializada.
   ```javascript
   let x;
   console.log(typeof x); // "undefined"
   ```

6. **`Null`**: Representa la ausencia intencional de un valor.
   ```javascript
   let y = null;
   console.log(typeof y); // "object" (es un error histórico en JS)
   ```

7. **`Symbol`**: Representa un valor único e inmutable.
   ```javascript
   let uniqueId = Symbol("id");
   console.log(typeof uniqueId); // "symbol"
   ```

---

### **Tipos de datos complejos**
1. **`Object`**: Representa una colección de pares clave-valor.
   ```javascript
   let person = { name: "Alice", age: 25 };
   console.log(typeof person); // "object"
   ```

2. **`Array`**: Es un tipo especial de objeto para almacenar listas ordenadas.
   ```javascript
   let numbers = [1, 2, 3];
   console.log(typeof numbers); // "object"
   ```

3. **`Function`**: Es un objeto invocable.
   ```javascript
   function greet() {
       console.log("Hello!");
   }
   console.log(typeof greet); // "function"
   ```

---

### **Tabla Resumen**

| Tipo de dato   | Ejemplo                          | Resultado de `typeof` |
|----------------|----------------------------------|------------------------|
| `String`       | `"Hello"`                       | `"string"`            |
| `Number`       | `42`, `3.14`                    | `"number"`            |
| `BigInt`       | `12345678901234567890n`         | `"bigint"`            |
| `Boolean`      | `true`, `false`                 | `"boolean"`           |
| `Undefined`    | `let x;`                        | `"undefined"`         |
| `Null`         | `let y = null;`                 | `"object"` (error histórico) |
| `Symbol`       | `Symbol("id")`                  | `"symbol"`            |
| `Object`       | `{ name: "Alice", age: 25 }`    | `"object"`            |
| `Array`        | `[1, 2, 3]`                     | `"object"`            |
| `Function`     | `function() {}`                 | `"function"`          |

Estos tipos de datos son fundamentales para trabajar con JavaScript.


---
## closure
🧠 **`¿Qué es un closure en JavaScript?`**
Un closure (o clausura) es cuando una función "recuerda" el lugar donde fue creada, y puede seguir accediendo a las variables de ese lugar, incluso si esa función se ejecuta fuera de ese contexto.


```js
function saludar(nombre) {
  return function() {
    console.log(`Hola, ${nombre}`);
  }
}

const saludoAna = saludar("Ana");

saludoAna(); // Hola, Ana

```

**`¿Qué pasó aquí?`**
- saludar("Ana") crea una función interna que usa la variable nombre.

- Aunque saludar ya terminó de ejecutarse, la función interna recuerda el valor de nombre gracias al closure.

- Así que cuando llamas saludoAna(), sigue teniendo acceso a nombre.

  
### **Ventajas**
- Permiten mantener datos privados.
- Facilitan la reutilización de código.
- Son esenciales para patrones como el módulo o la programación funcional.

### **Desventajas**
- Pueden consumir más memoria si no se manejan correctamente, ya que las variables referenciadas no se liberan hasta que el closure deja de usarse.

En resumen, los closures son una herramienta poderosa para manejar el alcance y la persistencia de datos en JavaScript. Se deben usar cuando se necesite encapsulación, modularidad o persistencia de datos en funciones.

---
## hoisting

El **hoisting** en JavaScript es un comportamiento por el cual las declaraciones de variables, funciones y clases se "mueven" (o parecen moverse) al inicio de su contexto de ejecución (scope) durante la fase de compilación. Esto significa que puedes usar variables o funciones antes de declararlas en el código, aunque el comportamiento depende del tipo de declaración.

---

### **Cómo funciona el hoisting**

1. **Declaraciones de funciones**: Las funciones declaradas se "elevan" completamente, lo que significa que puedes llamarlas antes de declararlas.

   ```javascript
   sayHello(); // Output: "Hello!"

   function sayHello() {
       console.log("Hello!");
   }
   ```

2. **Declaraciones de variables (`var`)**: Solo la declaración se eleva, pero no su inicialización. Si intentas acceder a la variable antes de inicializarla, obtendrás `undefined`.

   ```javascript
   console.log(x); // Output: undefined
   var x = 5;
   console.log(x); // Output: 5
   ```

3. **Declaraciones con `let` y `const`**: Estas también se elevan, pero quedan en un "Temporal Dead Zone" (TDZ) hasta que se inicializan. Si intentas acceder a ellas antes de la declaración, obtendrás un error.

   ```javascript
   console.log(y); // ReferenceError: Cannot access 'y' before initialization
   let y = 10;

   console.log(z); // ReferenceError: Cannot access 'z' before initialization
   const z = 20;
   ```

4. **Clases**: Las clases también se elevan, pero no puedes usarlas antes de declararlas debido al TDZ.

   ```javascript
   const obj = new MyClass(); // ReferenceError: Cannot access 'MyClass' before initialization
   class MyClass {
       constructor() {
           this.name = "Example";
       }
   }
   ```

---

### **Ejemplo práctico del hoisting**

```javascript
function example() {
    console.log(a); // Output: undefined (declaración elevada, pero no inicialización)
    var a = 10;

    console.log(b); // ReferenceError: Cannot access 'b' before initialization
    let b = 20;

    console.log(c); // ReferenceError: Cannot access 'c' before initialization
    const c = 30;
}

example();
```

---

### **Reglas importantes sobre el hoisting**

1. **Funciones declaradas**: Se elevan completamente, incluyendo su cuerpo.
2. **`var`**: Solo la declaración se eleva, no la inicialización.
3. **`let` y `const`**: Se elevan, pero no se pueden usar antes de su declaración debido al TDZ.
4. **Clases**: Se elevan, pero no se pueden usar antes de declararlas.

---

### **Resumen**

| Tipo            | ¿Se eleva? | ¿Se puede usar antes de declararla? | Comportamiento antes de inicialización |
|------------------|------------|-------------------------------------|-----------------------------------------|
| `function`       | Sí         | Sí                                  | Funciona normalmente.                   |
| `var`            | Sí         | Sí                                  | Devuelve `undefined`.                   |
| `let` / `const`  | Sí         | No                                  | Lanza un `ReferenceError`.              |
| `class`          | Sí         | No                                  | Lanza un `ReferenceError`.              |

El hoisting es un concepto clave para entender cómo JavaScript ejecuta el código y cómo manejar correctamente el alcance de variables y funciones.

---

### **Scope y diferencias entre `var`, `let` y `const`**

El **scope** (alcance) en JavaScript define dónde una variable es accesible en el código. Hay tres tipos principales de scope:

1. **Global Scope**: Variables accesibles desde cualquier parte del código.
2. **Function Scope**: Variables accesibles solo dentro de la función donde se declaran.
3. **Block Scope**: Variables accesibles solo dentro del bloque `{}` donde se declaran.

---

### **Diferencias entre `var`, `let` y `const`**

| Característica            | `var`                              | `let`                              | `const`                            |
|---------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| **Scope**                 | Function scope.                    | Block scope.                       | Block scope.                       |
| **Re-declaración**        | Permitida en el mismo scope.       | No permitida en el mismo scope.    | No permitida en el mismo scope.    |
| **Re-asignación**         | Permitida.                         | Permitida.                         | No permitida (es constante).       |
| **Inicialización**        | Opcional (por defecto es `undefined`). | Opcional (por defecto es `undefined`). | Obligatoria (debe asignarse un valor). |
| **Hoisting**              | Se eleva, pero inicializa como `undefined`. | Se eleva, pero no se puede usar antes de declararla (TDZ). | Se eleva, pero no se puede usar antes de declararla (TDZ). |
| **Uso recomendado**       | Evitar su uso (obsoleto).          | Usar para variables que cambian.   | Usar para valores constantes.      |

---

### **Ejemplos**

1. **Scope de `var`**:
   ```javascript
   function exampleVar() {
       if (true) {
           var x = 10; // Declarada con var
       }
       console.log(x); // Output: 10 (function scope)
   }
   exampleVar();
   ```

2. **Scope de `let`**:
   ```javascript
   function exampleLet() {
       if (true) {
           let y = 20; // Declarada con let
       }
       console.log(y); // ReferenceError: y is not defined (block scope)
   }
   exampleLet();
   ```

3. **Scope de `const`**:
   ```javascript
   function exampleConst() {
       if (true) {
           const z = 30; // Declarada con const
       }
       console.log(z); // ReferenceError: z is not defined (block scope)
   }
   exampleConst();
   ```

4. **Re-asignación y re-declaración**:
   ```javascript
   var a = 1;
   var a = 2; // Permitido con var

   let b = 1;
   // let b = 2; // Error: no se puede re-declarar con let

   const c = 1;
   // c = 2; // Error: no se puede re-asignar con const
   ```

---

### **Resumen**

- Usa `let` para variables que cambian de valor.
- Usa `const` para valores constantes o que no cambian.
- Evita `var` debido a su comportamiento impredecible con el scope y el hoisting.

  ### **Prototype y Prototype Chain en JavaScript**

En JavaScript, **`prototype`** y la **prototype chain** son conceptos fundamentales del modelo de herencia basado en prototipos. Aquí te explico ambos conceptos en detalle:

---

### **1. ¿Qué es el `prototype`?**

El **`prototype`** es un objeto especial que existe en todas las funciones constructoras y objetos en JavaScript. Permite compartir propiedades y métodos entre instancias de un objeto.

- Cada función en JavaScript tiene una propiedad llamada `prototype`.
- Los objetos creados a partir de una función constructora heredan las propiedades y métodos definidos en el `prototype` de esa función.

#### **Ejemplo básico del `prototype`**
```javascript
function Person(name) {
    this.name = name;
}

// Agregamos un método al prototype
Person.prototype.greet = function () {
    console.log(`Hello, my name is ${this.name}`);
};

// Creamos instancias
const alice = new Person("Alice");
const bob = new Person("Bob");

alice.greet(); // Output: Hello, my name is Alice
bob.greet();   // Output: Hello, my name is Bob
```

En este ejemplo:
- `greet` está definido en `Person.prototype`.
- Tanto `alice` como `bob` tienen acceso al método `greet` porque heredan del `prototype` de `Person`.

---

### **2. ¿Qué es la Prototype Chain?**

La **prototype chain** (cadena de prototipos) es el mecanismo mediante el cual JavaScript resuelve las propiedades y métodos de un objeto. Si una propiedad o método no se encuentra en el objeto, JavaScript busca en su prototipo, y así sucesivamente, hasta llegar al final de la cadena (el prototipo base, que es `null`).

#### **Ejemplo de la Prototype Chain**
```javascript
function Animal(type) {
    this.type = type;
}

Animal.prototype.eat = function () {
    console.log(`${this.type} is eating`);
};

const dog = new Animal("Dog");

// Acceso directo a la propiedad
console.log(dog.type); // Output: Dog

// Acceso a un método en el prototype
dog.eat(); // Output: Dog is eating

// Ver la cadena de prototipos
console.log(dog.__proto__ === Animal.prototype); // true
console.log(Animal.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null
```

En este ejemplo:
1. `dog` no tiene el método `eat` directamente, pero lo encuentra en `Animal.prototype`.
2. Si no se encuentra en `Animal.prototype`, JavaScript buscaría en `Object.prototype`.
3. La cadena termina en `null`.

---

### **3. Propiedades importantes del Prototype**

1. **`__proto__`**:
   - Es una referencia al prototipo del objeto.
   - Se usa para acceder a la cadena de prototipos.
   ```javascript
   console.log(dog.__proto__ === Animal.prototype); // true
   ```

2. **`constructor`**:
   - Es una propiedad del `prototype` que apunta a la función constructora.
   ```javascript
   console.log(dog.constructor === Animal); // true
   ```

3. **Herencia con prototipos**:
   - Puedes extender prototipos para crear jerarquías de objetos.
   ```javascript
   function Mammal(type) {
       Animal.call(this, type); // Llamamos al constructor de Animal
   }

   Mammal.prototype = Object.create(Animal.prototype); // Heredamos de Animal
   Mammal.prototype.constructor = Mammal;

   const cat = new Mammal("Cat");
   cat.eat(); // Output: Cat is eating
   ```

---

### **4. Ventajas del Prototype y Prototype Chain**

- **Reutilización de código**: Los métodos definidos en el `prototype` se comparten entre todas las instancias, lo que ahorra memoria.
- **Herencia**: Permite crear jerarquías de objetos y extender funcionalidades.

---

### **5. Ejemplo avanzado: Prototype Chain en acción**

```javascript
function Vehicle(type) {
    this.type = type;
}

Vehicle.prototype.start = function () {
    console.log(`${this.type} is starting`);
};

function Car(type, brand) {
    Vehicle.call(this, type); // Llamamos al constructor de Vehicle
    this.brand = brand;
}

// Heredamos de Vehicle
Car.prototype = Object.create(Vehicle.prototype);
Car.prototype.constructor = Car;

// Agregamos un método específico para Car
Car.prototype.drive = function () {
    console.log(`${this.brand} is driving`);
};

// Instancia de Car
const tesla = new Car("Electric Car", "Tesla");

tesla.start(); // Output: Electric Car is starting (heredado de Vehicle)
tesla.drive(); // Output: Tesla is driving (definido en Car)
```

En este ejemplo:
1. `Car` hereda de `Vehicle` mediante `Object.create`.
2. `tesla` tiene acceso tanto a los métodos de `Vehicle` como a los de `Car`.

---

### **Resumen**

| Concepto               | Descripción                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| **`prototype`**         | Objeto asociado a funciones constructoras para compartir métodos/propiedades. |
| **`__proto__`**         | Referencia al prototipo del objeto.                                         |
| **Prototype Chain**     | Mecanismo de búsqueda de propiedades/métodos en la cadena de prototipos.    |
| **Herencia**            | Se logra extendiendo prototipos mediante `Object.create` o clases modernas. |

El **prototype** y la **prototype chain** son esenciales para entender cómo funciona la herencia y la reutilización de código en JavaScript.

---
La **manipulación del DOM (Document Object Model)** en JavaScript permite interactuar con los elementos HTML de una página web. Esto incluye tareas como seleccionar elementos, modificar su contenido, estilos, atributos, o incluso agregar y eliminar elementos.

---

### **1. Seleccionar elementos del DOM**

Para manipular el DOM, primero necesitas seleccionar los elementos. JavaScript ofrece varios métodos para hacerlo:

#### **Ejemplos:**
```javascript
// Seleccionar por ID
const elementById = document.getElementById("myId");

// Seleccionar por clase
const elementsByClass = document.getElementsByClassName("myClass");

// Seleccionar por etiqueta
const elementsByTag = document.getElementsByTagName("div");

// Seleccionar usando selectores CSS
const elementQuery = document.querySelector(".myClass"); // Selecciona el primer elemento
const elementsQueryAll = document.querySelectorAll(".myClass"); // Selecciona todos los elementos
```

---

### **2. Modificar contenido y texto**

Puedes cambiar el contenido o texto de un elemento usando propiedades como `innerHTML` o `textContent`.

#### **Ejemplos:**
```javascript
// Cambiar el contenido HTML
const myDiv = document.getElementById("myDiv");
myDiv.innerHTML = "<p>Nuevo contenido HTML</p>";

// Cambiar solo el texto
myDiv.textContent = "Nuevo texto sin HTML";
```

---

### **3. Modificar atributos**

Puedes agregar, cambiar o eliminar atributos de un elemento.

#### **Ejemplos:**
```javascript
const myImage = document.querySelector("img");

// Cambiar el atributo src
myImage.setAttribute("src", "new-image.jpg");

// Obtener un atributo
console.log(myImage.getAttribute("src"));

// Eliminar un atributo
myImage.removeAttribute("alt");
```

---

### **4. Modificar estilos**

Puedes cambiar los estilos de un elemento directamente a través de la propiedad `style` o agregando clases.

#### **Ejemplos:**
```javascript
const myButton = document.querySelector("button");

// Cambiar estilos directamente
myButton.style.backgroundColor = "blue";
myButton.style.color = "white";

// Agregar o quitar clases
myButton.classList.add("active");
myButton.classList.remove("inactive");

// Alternar clases
myButton.classList.toggle("highlight");
```

---

### **5. Crear y agregar elementos**

Puedes crear nuevos elementos y agregarlos al DOM.

#### **Ejemplos:**
```javascript
// Crear un nuevo elemento
const newElement = document.createElement("p");
newElement.textContent = "Soy un nuevo párrafo";

// Agregarlo al DOM
const container = document.getElementById("container");
container.appendChild(newElement);

// Insertar antes de otro elemento
const referenceElement = document.getElementById("reference");
container.insertBefore(newElement, referenceElement);
```

---

### **6. Eliminar elementos**

Puedes eliminar elementos del DOM usando `removeChild` o `remove`.

#### **Ejemplos:**
```javascript
const elementToRemove = document.getElementById("removeMe");

// Usando removeChild
elementToRemove.parentNode.removeChild(elementToRemove);

// Usando remove (moderno)
elementToRemove.remove();
```

---

### **7. Manejar eventos**

Puedes agregar eventos a los elementos para que respondan a interacciones del usuario.

#### **Ejemplos:**
```javascript
const myButton = document.getElementById("myButton");

// Agregar un evento de clic
myButton.addEventListener("click", function () {
    alert("¡Botón clickeado!");
});

// Usar una función externa
function handleClick() {
    console.log("Botón clickeado");
}
myButton.addEventListener("click", handleClick);
```

---

### **Ejemplo completo: Manipulación del DOM**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manipulación del DOM</title>
</head>
<body>
    <div id="container">
        <h1 id="title">Título original</h1>
        <button id="changeTitle">Cambiar Título</button>
    </div>

    <script>
        // Seleccionar elementos
        const title = document.getElementById("title");
        const button = document.getElementById("changeTitle");

        // Agregar un evento al botón
        button.addEventListener("click", function () {
            // Cambiar el texto del título
            title.textContent = "Título cambiado con JavaScript";

            // Cambiar el estilo del título
            title.style.color = "red";
        });
    </script>
</body>
</html>
```

---

### **Resumen**

| Tarea                          | Método/Propiedad                          |
|--------------------------------|-------------------------------------------|
| Seleccionar elementos          | `getElementById`, `querySelector`, etc.   |
| Cambiar contenido              | `innerHTML`, `textContent`                |
| Modificar atributos            | `setAttribute`, `getAttribute`, `removeAttribute` |
| Cambiar estilos                | `style`, `classList`                      |
| Crear y agregar elementos      | `createElement`, `appendChild`, `insertBefore` |
| Eliminar elementos             | `removeChild`, `remove`                   |
| Manejar eventos                | `addEventListener`                        |

La manipulación del DOM es esencial para crear aplicaciones web dinámicas e interactivas.

---

### **Evento Bubbling y Event Delegation en JavaScript**

---

### **1. ¿Qué es el Evento Bubbling?**

El **evento bubbling** (propagación de eventos) es un comportamiento en el que un evento que ocurre en un elemento hijo se propaga hacia sus elementos padres, subiendo por la jerarquía del DOM hasta el elemento raíz (`document`).

#### **Ejemplo de Evento Bubbling**
```html
<div id="parent" style="padding: 20px; background-color: lightblue;">
    Parent
    <button id="child">Child Button</button>
</div>

<script>
    const parent = document.getElementById("parent");
    const child = document.getElementById("child");

    // Evento en el elemento padre
    parent.addEventListener("click", () => {
        console.log("Parent clicked");
    });

    // Evento en el elemento hijo
    child.addEventListener("click", () => {
        console.log("Child clicked");
    });
</script>
```

#### **Salida al hacer clic en el botón `Child`**:
1. "Child clicked" (se ejecuta el evento del botón).
2. "Parent clicked" (el evento se propaga al padre).

Esto ocurre porque el evento se "burbujea" desde el elemento hijo (`button`) hacia el padre (`div`).

---

### **2. ¿Qué es Event Delegation?**

La **delegación de eventos** es una técnica que aprovecha el bubbling para manejar eventos de múltiples elementos hijos desde un único evento en el elemento padre. En lugar de agregar un evento a cada hijo, se agrega un único evento al padre y se utiliza la propiedad `event.target` para identificar el elemento que disparó el evento.

#### **Ventajas de Event Delegation**:
- Mejora el rendimiento al reducir el número de listeners.
- Útil para manejar elementos dinámicos (que se crean después de cargar la página).

---

#### **Ejemplo de Event Delegation**
```html
<div id="parent" style="padding: 20px; background-color: lightgreen;">
    <button class="child">Button 1</button>
    <button class="child">Button 2</button>
    <button class="child">Button 3</button>
</div>

<script>
    const parent = document.getElementById("parent");

    // Delegación de eventos en el padre
    parent.addEventListener("click", (event) => {
        if (event.target.classList.contains("child")) {
            console.log(`Clicked on: ${event.target.textContent}`);
        }
    });
</script>
```

#### **Salida al hacer clic en un botón**:
Si haces clic en "Button 2", el resultado será:
```
Clicked on: Button 2
```

En este caso:
1. El evento se escucha en el `div` padre.
2. Usamos `event.target` para identificar cuál de los botones fue clickeado.

---

### **Diferencias entre Bubbling y Delegation**

| **Concepto**            | **Evento Bubbling**                                      | **Event Delegation**                                  |
|--------------------------|---------------------------------------------------------|------------------------------------------------------|
| **Definición**           | Propagación de eventos desde el hijo hacia el padre.    | Técnica para manejar eventos en múltiples hijos desde el padre. |
| **Uso**                 | Ocurre automáticamente en eventos como `click`.         | Se implementa manualmente usando bubbling.           |
| **Listeners**            | Se agregan a cada elemento individualmente.             | Se agrega un único listener al padre.                |
| **Escalabilidad**        | Menos eficiente si hay muchos elementos.                | Más eficiente, especialmente con elementos dinámicos. |

---

### **3. Detener el Evento Bubbling**

Si no deseas que un evento se propague hacia los elementos padres, puedes usar el método `event.stopPropagation()`.

#### **Ejemplo: Detener el bubbling**
```html
<div id="parent" style="padding: 20px; background-color: lightcoral;">
    Parent
    <button id="child">Child Button</button>
</div>

<script>
    const parent = document.getElementById("parent");
    const child = document.getElementById("child");

    parent.addEventListener("click", () => {
        console.log("Parent clicked");
    });

    child.addEventListener("click", (event) => {
        console.log("Child clicked");
        event.stopPropagation(); // Detiene el bubbling
    });
</script>
```

#### **Salida al hacer clic en el botón `Child`**:
1. "Child clicked" (el evento del botón se ejecuta).
2. El evento del padre no se ejecuta porque el bubbling fue detenido.

---

### **Resumen**

- **Evento Bubbling**: Es el comportamiento predeterminado donde los eventos se propagan desde el hijo hacia los padres.
- **Event Delegation**: Técnica que aprovecha el bubbling para manejar eventos de múltiples elementos desde un único listener.
- **`event.stopPropagation()`**: Detiene la propagación del evento hacia los elementos padres.

Ambos conceptos son esenciales para manejar eventos de manera eficiente en aplicaciones web dinámicas.


---
En JavaScript, los **spread** y **rest operators** utilizan la misma sintaxis (`...`), pero tienen propósitos diferentes dependiendo del contexto en el que se usen.

---

### **1. Spread Operator (`...`)**

El **spread operator** se utiliza para "expandir" elementos de un array, objeto o iterable en lugares donde se esperan múltiples valores (como argumentos de una función o elementos de un array).

#### **Ejemplos de Spread Operator**

1. **Copiar arrays**:
   ```javascript
   const arr1 = [1, 2, 3];
   const arr2 = [...arr1]; // Copia de arr1
   console.log(arr2); // Output: [1, 2, 3]
   ```

2. **Combinar arrays**:
   ```javascript
   const arr1 = [1, 2];
   const arr2 = [3, 4];
   const combined = [...arr1, ...arr2];
   console.log(combined); // Output: [1, 2, 3, 4]
   ```

3. **Copiar objetos**:
   ```javascript
   const obj1 = { a: 1, b: 2 };
   const obj2 = { ...obj1 }; // Copia de obj1
   console.log(obj2); // Output: { a: 1, b: 2 }
   ```

4. **Combinar objetos**:
   ```javascript
   const obj1 = { a: 1 };
   const obj2 = { b: 2 };
   const combined = { ...obj1, ...obj2 };
   console.log(combined); // Output: { a: 1, b: 2 }
   ```

5. **Pasar argumentos a funciones**:
   ```javascript
   const numbers = [1, 2, 3];
   console.log(Math.max(...numbers)); // Output: 3
   ```

---

### **2. Rest Operator (`...`)**

El **rest operator** se utiliza para "agrupar" múltiples elementos en un solo array o un objeto. Se usa principalmente en parámetros de funciones o en desestructuración.

#### **Ejemplos de Rest Operator**

1. **En funciones (parámetros variables)**:
   ```javascript
   function sum(...numbers) {
       return numbers.reduce((acc, num) => acc + num, 0);
   }
   console.log(sum(1, 2, 3, 4)); // Output: 10
   ```

2. **Desestructuración de arrays**:
   ```javascript
   const [first, ...rest] = [1, 2, 3, 4];
   console.log(first); // Output: 1
   console.log(rest);  // Output: [2, 3, 4]
   ```

3. **Desestructuración de objetos**:
   ```javascript
   const { a, ...rest } = { a: 1, b: 2, c: 3 };
   console.log(a);    // Output: 1
   console.log(rest); // Output: { b: 2, c: 3 }
   ```

---

### **Diferencias entre Spread y Rest**

| **Característica**       | **Spread Operator**                          | **Rest Operator**                          |
|--------------------------|----------------------------------------------|-------------------------------------------|
| **Propósito**            | Expande elementos de un array/objeto.        | Agrupa múltiples elementos en un array/objeto. |
| **Uso principal**        | Copiar, combinar o pasar valores.            | Agrupar parámetros o desestructurar.       |
| **Contexto**             | Arrays, objetos, argumentos de funciones.    | Parámetros de funciones, desestructuración. |

---

### **Ejemplo combinado de Spread y Rest**

```javascript
function greet(first, ...others) {
    console.log(`Hello, ${first}!`);
    console.log(`Others: ${others.join(", ")}`);
}

const names = ["Alice", "Bob", "Charlie"];
greet(...names); // Spread para expandir el array
// Output:
// Hello, Alice!
// Others: Bob, Charlie
```

En este ejemplo:
- El **spread operator** expande el array `names` en argumentos individuales.
- El **rest operator** agrupa los argumentos restantes en el array `others`.

---

### **Resumen**

- **Spread (`...`)**: Expande elementos de arrays/objetos.
- **Rest (`...`)**: Agrupa múltiples elementos en arrays/objetos.

Ambos operadores son esenciales para trabajar con datos de manera flexible y eficiente en JavaScript.

---

### **¿Qué es JSON?**

**JSON** (JavaScript Object Notation) es un formato ligero de intercambio de datos. Es fácil de leer y escribir para los humanos, y fácil de interpretar y generar para las máquinas. JSON se utiliza comúnmente para enviar y recibir datos entre un cliente y un servidor.

#### **Ejemplo de JSON**
```json
{
    "name": "Alice",
    "age": 25,
    "isStudent": false,
    "skills": ["JavaScript", "HTML", "CSS"]
}
```

---

### **`JSON.stringify`**

El método **`JSON.stringify`** convierte un objeto o valor de JavaScript en una cadena JSON. Es útil para enviar datos a un servidor o almacenarlos en un formato legible.

#### **Ejemplo de `JSON.stringify`**
```javascript
const user = {
    name: "Alice",
    age: 25,
    isStudent: false,
    skills: ["JavaScript", "HTML", "CSS"]
};

// Convertir el objeto a una cadena JSON
const jsonString = JSON.stringify(user);
console.log(jsonString);
// Output: {"name":"Alice","age":25,"isStudent":false,"skills":["JavaScript","HTML","CSS"]}
```

---

### **`JSON.parse`**

El método **`JSON.parse`** convierte una cadena JSON en un objeto de JavaScript. Es útil para recibir datos de un servidor y convertirlos en un formato que puedas manipular.

#### **Ejemplo de `JSON.parse`**
```javascript
const jsonString = '{"name":"Alice","age":25,"isStudent":false,"skills":["JavaScript","HTML","CSS"]}';

// Convertir la cadena JSON a un objeto
const user = JSON.parse(jsonString);
console.log(user.name); // Output: Alice
console.log(user.skills); // Output: ["JavaScript", "HTML", "CSS"]
```

---

### **Otros métodos relacionados con JSON**

1. **Espaciado en `JSON.stringify`**:
   Puedes agregar un espaciado para que la salida sea más legible.
   ```javascript
   const user = { name: "Alice", age: 25 };
   const prettyJson = JSON.stringify(user, null, 2);
   console.log(prettyJson);
   // Output:
   // {
   //   "name": "Alice",
   //   "age": 25
   // }
   ```

2. **Reemplazo con `JSON.stringify`**:
   Puedes usar un segundo argumento para filtrar o transformar las propiedades.
   ```javascript
   const user = { name: "Alice", age: 25, password: "12345" };

   // Excluir la propiedad "password"
   const filteredJson = JSON.stringify(user, ["name", "age"]);
   console.log(filteredJson); // Output: {"name":"Alice","age":25}
   ```

3. **Manejo de errores con `JSON.parse`**:
   Si intentas analizar una cadena JSON malformada, se lanzará un error.
   ```javascript
   try {
       const invalidJson = '{"name": "Alice", "age": 25'; // Falta un cierre
       const user = JSON.parse(invalidJson);
   } catch (error) {
       console.error("Error al analizar JSON:", error.message);
   }
   ```

---

### **Diferencias clave entre JSON y objetos de JavaScript**

| **Característica**       | **JSON**                                      | **Objeto de JavaScript**                     |
|--------------------------|-----------------------------------------------|---------------------------------------------|
| **Formato**              | Cadena de texto (string).                    | Estructura de datos en memoria.             |
| **Claves**               | Siempre deben estar entre comillas dobles.   | No necesitan comillas.                      |
| **Tipos de datos**       | Solo admite tipos básicos (`string`, `number`, `boolean`, `array`, `object`, `null`). | Admite funciones, `undefined`, y más.       |

---

### **Resumen**

| Método           | Descripción                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `JSON.stringify` | Convierte un objeto de JavaScript en una cadena JSON.                      |
| `JSON.parse`     | Convierte una cadena JSON en un objeto de JavaScript.                      |

JSON es una herramienta esencial para trabajar con datos en aplicaciones web, especialmente en la comunicación entre cliente y servidor.


---
### **Memory Management en JavaScript**

El **memory management** (gestión de memoria) se refiere al proceso de asignar, usar y liberar memoria en un programa. En JavaScript, este proceso es automático gracias al **Garbage Collector**, pero entender cómo funciona es importante para evitar problemas como **memory leaks** (fugas de memoria).

---

### **1. ¿Qué es el Garbage Collection?**

El **Garbage Collector (GC)** es un mecanismo que automáticamente libera memoria que ya no se está utilizando. En JavaScript, el GC identifica los objetos que ya no tienen referencias activas y los elimina para liberar espacio en la memoria.

#### **¿Cómo funciona?**
- **Alcance de las referencias**: Si un objeto no puede ser alcanzado desde el código (es decir, no tiene referencias activas), se considera "inaccesible" y el GC lo elimina.
- **Algoritmo de Mark-and-Sweep**: Este es el algoritmo más común en JavaScript:
  1. Marca todos los objetos accesibles desde el **root** (como `window` o `global`).
  2. Los objetos no marcados se consideran inaccesibles y se eliminan.

---

### **2. ¿Qué es una Fuga de Memoria (Memory Leak)?**

Una **fuga de memoria** ocurre cuando la memoria que ya no se necesita no es liberada, lo que puede causar que el programa consuma más memoria de la necesaria y eventualmente se ralentice o falle.

#### **Causas comunes de Memory Leaks**
1. **Referencias no eliminadas**:
   - Si un objeto sigue teniendo referencias, no será eliminado por el GC.
   ```javascript
   let obj = {};
   const ref = obj; // Referencia activa
   obj = null; // Pero `ref` sigue apuntando al objeto
   ```

2. **Closures mal manejados**:
   - Los closures pueden mantener referencias a variables que ya no se necesitan.
   ```javascript
   function createClosure() {
       let largeArray = new Array(1000000); // Gran cantidad de memoria
       return function () {
           console.log(largeArray.length);
       };
   }
   const closure = createClosure(); // `largeArray` nunca se libera
   ```

3. **Listeners no eliminados**:
   - Los eventos que no se eliminan pueden mantener referencias innecesarias.
   ```javascript
   const button = document.getElementById("myButton");
   function handleClick() {
       console.log("Clicked!");
   }
   button.addEventListener("click", handleClick);
   // Si el botón se elimina del DOM, el listener sigue ocupando memoria
   ```

4. **Objetos globales**:
   - Las variables globales permanecen en memoria mientras la aplicación esté activa.
   ```javascript
   window.largeData = new Array(1000000); // Nunca se libera automáticamente
   ```

---

### **3. Memoria Viva y Muerta**

- **Memoria Viva**: Es la memoria que está siendo utilizada activamente por el programa. Por ejemplo, objetos que tienen referencias activas.
- **Memoria Muerta**: Es la memoria que ya no se necesita, pero que aún no ha sido liberada por el Garbage Collector.

---

### **4. Cómo evitar Memory Leaks**

1. **Eliminar referencias innecesarias**:
   - Asegúrate de establecer las referencias a `null` cuando ya no sean necesarias.
   ```javascript
   let obj = { key: "value" };
   obj = null; // Permite que el GC lo elimine
   ```

2. **Eliminar event listeners**:
   - Usa `removeEventListener` para eliminar listeners cuando ya no sean necesarios.
   ```javascript
   const button = document.getElementById("myButton");
   function handleClick() {
       console.log("Clicked!");
   }
   button.addEventListener("click", handleClick);

   // Eliminar el listener
   button.removeEventListener("click", handleClick);
   ```

3. **Evitar variables globales**:
   - Usa `let` o `const` en lugar de declarar variables en el objeto global (`window`).

4. **Usar herramientas de depuración**:
   - Usa herramientas como el **Memory Profiler** en Chrome DevTools para identificar y solucionar fugas de memoria.

---

### **5. Ejemplo práctico de Memory Leak**

```javascript
function createLeak() {
    const largeArray = new Array(1000000).fill("data");
    document.getElementById("leakButton").addEventListener("click", () => {
        console.log(largeArray.length);
    });
}

// Llamar a la función crea un listener que nunca se elimina
createLeak();
```

#### **Solución**:
Eliminar el listener cuando ya no sea necesario:
```javascript
function createLeak() {
    const largeArray = new Array(1000000).fill("data");
    const button = document.getElementById("leakButton");

    function handleClick() {
        console.log(largeArray.length);
    }

    button.addEventListener("click", handleClick);

    // Eliminar el listener después de usarlo
    button.removeEventListener("click", handleClick);
}
createLeak();
```

---

### **Resumen**

| Concepto               | Descripción                                                                 |
|-------------------------|-----------------------------------------------------------------------------|
| **Memory Management**   | Proceso de asignar, usar y liberar memoria en un programa.                 |
| **Garbage Collection**  | Mecanismo automático que elimina objetos inaccesibles para liberar memoria.|
| **Memory Leak**         | Memoria que no se libera porque sigue teniendo referencias activas.        |
| **Memoria Viva**        | Memoria que está siendo utilizada activamente por el programa.             |
| **Memoria Muerta**      | Memoria que ya no se necesita pero no ha sido liberada.                    |

Entender cómo funciona la gestión de memoria y el Garbage Collector es clave para escribir código eficiente y evitar problemas de rendimiento en aplicaciones JavaScript.

---
En JavaScript, los bucles son estructuras que permiten ejecutar un bloque de código varias veces, dependiendo de una condición. Son útiles cuando necesitas realizar tareas repetitivas, como iterar sobre elementos de un array o realizar cálculos repetidos.

---

### **Tipos de bucles en JavaScript**

#### **1. `for`**
El bucle `for` se usa cuando conoces de antemano cuántas veces necesitas iterar.

**Sintaxis**:
```javascript
for (inicialización; condición; incremento) {
    // Código a ejecutar en cada iteración
}
```

**Ejemplo**:
```javascript
// Imprimir números del 1 al 5
for (let i = 1; i <= 5; i++) {
    console.log(i);
}
```

**Cuándo usarlo**:
- Cuando sabes cuántas iteraciones necesitas.
- Para recorrer arrays o realizar tareas repetitivas con un contador.

---

#### **2. `while`**
El bucle `while` se usa cuando no sabes cuántas iteraciones necesitas, pero tienes una condición que debe cumplirse para continuar.

**Sintaxis**:
```javascript
while (condición) {
    // Código a ejecutar mientras la condición sea verdadera
}
```

**Ejemplo**:
```javascript
// Imprimir números del 1 al 5
let i = 1;
while (i <= 5) {
    console.log(i);
    i++;
}
```

**Cuándo usarlo**:
- Cuando no sabes cuántas iteraciones necesitas.
- Cuando la condición depende de algo externo que puede cambiar durante la ejecución.

---

#### **3. `do...while`**
El bucle `do...while` es similar a `while`, pero garantiza que el bloque de código se ejecute al menos una vez, incluso si la condición es falsa desde el principio.

**Sintaxis**:
```javascript
do {
    // Código a ejecutar
} while (condición);
```

**Ejemplo**:
```javascript
// Imprimir números del 1 al 5
let i = 1;
do {
    console.log(i);
    i++;
} while (i <= 5);
```

**Cuándo usarlo**:
- Cuando necesitas ejecutar el bloque de código al menos una vez antes de verificar la condición.

---

#### **4. `for...of`**
El bucle `for...of` se usa para iterar sobre elementos de objetos iterables, como arrays, cadenas de texto, mapas, y conjuntos.

**Sintaxis**:
```javascript
for (const elemento of iterable) {
    // Código a ejecutar para cada elemento
}
```

**Ejemplo**:
```javascript
// Recorrer un array
const frutas = ['manzana', 'banana', 'naranja'];
for (const fruta of frutas) {
    console.log(fruta);
}
```

**Cuándo usarlo**:
- Para recorrer elementos de un array o cualquier objeto iterable.
- Cuando no necesitas el índice del elemento.

---

#### **5. `for...in`**
El bucle `for...in` se usa para iterar sobre las propiedades enumerables de un objeto.

**Sintaxis**:
```javascript
for (const propiedad in objeto) {
    // Código a ejecutar para cada propiedad
}
```

**Ejemplo**:
```javascript
// Recorrer las propiedades de un objeto
const persona = { nombre: 'Juan', edad: 30, ciudad: 'Madrid' };
for (const clave in persona) {
    console.log(`${clave}: ${persona[clave]}`);
}
```

**Cuándo usarlo**:
- Para recorrer las propiedades de un objeto.
- No se recomienda para arrays, ya que también puede incluir propiedades heredadas.

---

### **Comparación de bucles**

| Bucle       | Uso principal                                                                 |
|-------------|-------------------------------------------------------------------------------|
| `for`       | Iteraciones con un contador conocido.                                        |
| `while`     | Iteraciones basadas en una condición que puede cambiar dinámicamente.        |
| `do...while`| Ejecutar el bloque al menos una vez antes de verificar la condición.         |
| `for...of`  | Iterar sobre elementos de objetos iterables (arrays, cadenas, etc.).         |
| `for...in`  | Iterar sobre las propiedades de un objeto.                                   |

---

### **Ejemplo práctico**
Supongamos que tienes un array de números y necesitas imprimir solo los números pares.

**Código**:
```javascript
const numeros = [1, 2, 3, 4, 5, 6];

// Usando un bucle for
for (let i = 0; i < numeros.length; i++) {
    if (numeros[i] % 2 === 0) {
        console.log(`Número par: ${numeros[i]}`);
    }
}

// Usando for...of
for (const numero of numeros) {
    if (numero % 2 === 0) {
        console.log(`Número par: ${numero}`);
    }
}
```

**Salida**:
```plaintext
Número par: 2
Número par: 4
Número par: 6
```

---

### **Conclusión**
- Usa `for` cuando necesites un contador o índice.
- Usa `while` o `do...while` cuando la condición de iteración no sea fija.
- Usa `for...of` para recorrer elementos de arrays o iterables.
- Usa `for...in` para recorrer propiedades de objetos.

Elige el bucle adecuado según el caso de uso para escribir código más claro y eficiente.

---
El **Event Loop** en JavaScript es el mecanismo que permite que el lenguaje maneje tareas asíncronas, como temporizadores, eventos de usuario o solicitudes a servidores, mientras sigue ejecutando código sin bloquear el flujo principal.

---

### **Etapas del Event Loop**

1. **Fase de Ejecución Inicial (Call Stack)**  
   - El **Call Stack** (pila de llamadas) es donde se ejecuta el código sincrónico.  
   - Cuando llamas a una función, esta se agrega al Call Stack y se ejecuta.  
   - Si la función termina, se elimina del Call Stack.

   **Ejemplo**:
   ```javascript
   console.log("Inicio"); // Se ejecuta directamente en el Call Stack
   ```

---

2. **Cola de Tareas (Callback Queue)**  
   - Las tareas asíncronas (como `setTimeout`) se colocan en la **Callback Queue** cuando están listas para ejecutarse.  
   - El Event Loop mueve estas tareas al Call Stack **solo cuando el Call Stack está vacío**.

   **Ejemplo**:
   ```javascript
   console.log("Inicio");

   setTimeout(() => {
       console.log("Tarea asíncrona");
   }, 1000);

   console.log("Fin");
   ```

   **Salida**:
   ```
   Inicio
   Fin
   Tarea asíncrona
   ```

   **Explicación**:
   - `console.log("Inicio")` y `console.log("Fin")` se ejecutan directamente en el Call Stack.
   - `setTimeout` coloca su callback en la Callback Queue después de 1 segundo.
   - Cuando el Call Stack está vacío, el Event Loop mueve el callback al Call Stack para ejecutarlo.

---

3. **Cola de Microtareas (Microtask Queue)**  
   - Las **Microtareas** (como `Promise.then`) tienen mayor prioridad que las tareas normales en la Callback Queue.  
   - Antes de procesar la Callback Queue, el Event Loop procesa todas las Microtareas pendientes.

   **Ejemplo**:
   ```javascript
   console.log("Inicio");

   setTimeout(() => {
       console.log("Tarea asíncrona");
   }, 0);

   Promise.resolve().then(() => {
       console.log("Microtarea");
   });

   console.log("Fin");
   ```

   **Salida**:
   ```
   Inicio
   Fin
   Microtarea
   Tarea asíncrona
   ```

   **Explicación**:
   - `console.log("Inicio")` y `console.log("Fin")` se ejecutan directamente.
   - La Microtarea (`Promise.then`) se procesa antes que la tarea normal (`setTimeout`), incluso si ambas están listas.

---

4. **Repetición del Event Loop**  
   - El Event Loop sigue este ciclo:
     1. Ejecuta todo el código sincrónico en el Call Stack.
     2. Procesa todas las Microtareas en la Microtask Queue.
     3. Mueve una tarea de la Callback Queue al Call Stack (si está vacío).
     4. Repite.

---

### **Ejemplo Completo del Event Loop**

```javascript
console.log("Inicio");

setTimeout(() => {
    console.log("Tarea asíncrona 1");
}, 0);

Promise.resolve().then(() => {
    console.log("Microtarea 1");
}).then(() => {
    console.log("Microtarea 2");
});

setTimeout(() => {
    console.log("Tarea asíncrona 2");
}, 0);

console.log("Fin");
```

**Salida**:
```
Inicio
Fin
Microtarea 1
Microtarea 2
Tarea asíncrona 1
Tarea asíncrona 2
```

**Explicación**:
1. `console.log("Inicio")` y `console.log("Fin")` se ejecutan directamente en el Call Stack.
2. `setTimeout` coloca sus callbacks en la Callback Queue.
3. `Promise.then` coloca sus callbacks en la Microtask Queue.
4. El Event Loop procesa las Microtareas (`Microtarea 1`, `Microtarea 2`) antes de las tareas normales (`Tarea asíncrona 1`, `Tarea asíncrona 2`).

---

### **Resumen del flujo del Event Loop**
1. Ejecuta el código sincrónico en el Call Stack.
2. Procesa todas las Microtareas en la Microtask Queue.
3. Procesa una tarea de la Callback Queue (si el Call Stack está vacío).
4. Repite el ciclo.

---

### **Concepto clave**
El Event Loop asegura que JavaScript pueda manejar tareas asíncronas sin bloquear el flujo principal, permitiendo que el lenguaje sea eficiente y no se detenga mientras espera respuestas o eventos.

---
Las **Microtasks** en JavaScript son tareas que tienen una prioridad más alta que las tareas normales (macrotasks) en el Event Loop. Estas tareas se ejecutan inmediatamente después de que el código sincrónico actual haya terminado y antes de que se procesen las tareas en la cola de macrotasks.

---

### **Características principales de las Microtasks:**
1. **Alta prioridad**:
   - Las Microtasks siempre se ejecutan antes que las tareas normales (macrotasks) en el Event Loop.
   - Ejemplo de macrotasks: `setTimeout`, `setInterval`.

2. **Ejemplos de Microtasks**:
   - Promesas resueltas (`Promise.then`, `Promise.catch`, `Promise.finally`).
   - `MutationObserver` (API para observar cambios en el DOM).

3. **Cola de Microtasks**:
   - Las Microtasks se colocan en una cola especial llamada **Microtask Queue**.
   - Antes de que el Event Loop procese la cola de macrotasks, se asegura de vaciar completamente la cola de Microtasks.

---

### **Cómo funcionan las Microtasks en el Event Loop**
1. Ejecuta el código sincrónico en el **Call Stack**.
2. Procesa todas las Microtasks en la **Microtask Queue**.
3. Procesa una tarea de la **Callback Queue** (macrotask).
4. Repite el ciclo.

---

### **Ejemplo básico de Microtasks con Promesas**
```javascript
console.log("Inicio");

setTimeout(() => {
    console.log("Macrotask: setTimeout");
}, 0);

Promise.resolve().then(() => {
    console.log("Microtask: Promise.then");
});

console.log("Fin");
```

**Salida**:
```
Inicio
Fin
Microtask: Promise.then
Macrotask: setTimeout
```

**Explicación**:
1. `console.log("Inicio")` y `console.log("Fin")` se ejecutan directamente en el Call Stack.
2. La Microtask (`Promise.then`) se coloca en la Microtask Queue.
3. La Macrotask (`setTimeout`) se coloca en la Callback Queue.
4. El Event Loop procesa primero la Microtask antes de la Macrotask.

---

### **Ejemplo con múltiples Microtasks**
```javascript
console.log("Inicio");

Promise.resolve().then(() => {
    console.log("Microtask 1");
    return Promise.resolve();
}).then(() => {
    console.log("Microtask 2");
});

console.log("Fin");
```

**Salida**:
```
Inicio
Fin
Microtask 1
Microtask 2
```

**Explicación**:
1. `console.log("Inicio")` y `console.log("Fin")` se ejecutan directamente.
2. La primera Microtask (`Microtask 1`) se coloca en la Microtask Queue.
3. Cuando se resuelve la primera Microtask, se genera otra Microtask (`Microtask 2`), que también se coloca en la Microtask Queue.
4. El Event Loop procesa todas las Microtasks antes de continuar con otras tareas.

---

### **Diferencia entre Microtasks y Macrotasks**
| **Microtasks**                     | **Macrotasks**                     |
|------------------------------------|------------------------------------|
| Tienen mayor prioridad.            | Se ejecutan después de las Microtasks. |
| Ejemplos: `Promise.then`, `MutationObserver`. | Ejemplos: `setTimeout`, `setInterval`, eventos del DOM. |
| Se procesan antes de las Macrotasks. | Se procesan después de vaciar la Microtask Queue. |

---

### **Cuándo usar Microtasks**
- **Promesas**: Para manejar tareas asíncronas que deben ejecutarse inmediatamente después del código sincrónico.
- **Optimización del DOM**: Usar `MutationObserver` para detectar y reaccionar a cambios en el DOM.

---

### **Conclusión**
Las Microtasks son una parte esencial del Event Loop en JavaScript, ya que permiten manejar tareas asíncronas con alta prioridad. Entender cómo funcionan te ayudará a escribir código más eficiente y predecible.

El **contexto de ejecución de JavaScript** es el entorno en el que se ejecuta un fragmento de código. Define qué variables, objetos y funciones están disponibles y cómo se comportan. Cada vez que se ejecuta un código en JavaScript, se crea un nuevo contexto de ejecución.

---

### **Tipos de Contextos de Ejecución**

1. **Contexto Global**:
   - Es el contexto predeterminado cuando se ejecuta un script por primera vez.
   - En un navegador, el objeto global es `window`, y en Node.js es `global`.
   - Variables y funciones definidas fuera de cualquier función pertenecen al contexto global.

   ```javascript
   var globalVar = "Soy global";

   function test() {
       console.log(globalVar); // Accede al contexto global
   }
   ```

2. **Contexto de Función**:
   - Se crea cada vez que se llama a una función.
   - Cada función tiene su propio contexto de ejecución.
   - Las variables definidas dentro de una función no son accesibles fuera de ella (alcance local).

   ```javascript
   function test() {
       var localVar = "Soy local";
       console.log(localVar); // Accesible dentro de la función
   }
   console.log(localVar); // Error: localVar no está definido
   ```

3. **Contexto de Evaluación (`eval`)**:
   - Se crea cuando se ejecuta código dentro de una función `eval`.
   - No se recomienda usar `eval` debido a problemas de seguridad y rendimiento.

---

### **Componentes del Contexto de Ejecución**

1. **Objeto de Variables (Variable Environment)**:
   - Contiene las variables, funciones y parámetros definidos en el contexto actual.

2. **Objeto `this`**:
   - Representa el objeto desde el cual se invoca el código.
   - Su valor depende de cómo se llama a la función:
     - En el contexto global, `this` es el objeto global (`window` en navegadores).
     - En un método de un objeto, `this` es el objeto que contiene el método.
     - En funciones flecha, `this` se hereda del contexto donde se definió.

   ```javascript
   console.log(this); // En un navegador, imprime el objeto `window`

   const obj = {
       method: function() {
           console.log(this); // Imprime `obj`
       }
   };
   obj.method();
   ```

3. **Scope Chain (Cadena de Alcance)**:
   - Define el acceso a variables y funciones desde el contexto actual hacia los contextos superiores.
   - Si una variable no está en el contexto actual, JavaScript busca en los contextos superiores hasta llegar al global.

   ```javascript
   var globalVar = "Soy global";

   function outer() {
       var outerVar = "Soy de outer";

       function inner() {
           console.log(globalVar); // Accede al contexto global
           console.log(outerVar); // Accede al contexto de `outer`
       }
       inner();
   }
   outer();
   ```

---

### **Ciclo de Vida del Contexto de Ejecución**

1. **Creación**:
   - Se crea el contexto y se inicializan las variables, funciones y el objeto `this`.

2. **Ejecución**:
   - El código se ejecuta línea por línea dentro del contexto.

3. **Destrucción**:
   - Una vez que el código ha terminado de ejecutarse, el contexto se destruye (excepto el contexto global, que persiste durante toda la ejecución del programa).

---

### **Relación con el Event Loop**

El contexto de ejecución trabaja junto con el **Event Loop** para manejar tareas asincrónicas. Cuando se ejecuta un código asincrónico (como `setTimeout` o `fetch`), el contexto de ejecución principal no se bloquea. En su lugar, las tareas asincrónicas se manejan en la cola de tareas y se ejecutan cuando el contexto principal está libre.

---

### **Conclusión**

El contexto de ejecución es fundamental para entender cómo JavaScript maneja variables, funciones y el objeto `this`. Comprenderlo es clave para escribir código eficiente y evitar errores relacionados con el alcance y el comportamiento de `this`.


### **Entorno Léxico en JavaScript**

El **entorno léxico** es una estructura que JavaScript utiliza para organizar y gestionar las variables y funciones en un programa. Cada vez que se ejecuta una función o bloque de código, se crea un nuevo entorno léxico que contiene referencias a las variables y funciones definidas dentro de ese contexto.

---

### **Ejemplo Visual del Entorno Léxico**

```javascript
const globalVar = "Soy global";

function outer() {
    const outerVar = "Soy de outer";

    function inner() {
        const innerVar = "Soy de inner";
        console.log(globalVar); // Accede al entorno global
        console.log(outerVar);  // Accede al entorno de `outer`
        console.log(innerVar);  // Accede al entorno de `inner`
    }

    inner();
}

outer();
```

---

### **Representación del Entorno Léxico como JSON**

Podemos imaginar cómo JavaScript estructura los entornos léxicos en memoria utilizando un formato similar a JSON. Cada entorno léxico tiene un conjunto de variables locales y una referencia al entorno padre.

```json
{
  "GlobalEnvironment": {
    "variables": {
      "globalVar": "Soy global",
      "outer": "function"
    },
    "outer": null
  },
  "OuterEnvironment": {
    "variables": {
      "outerVar": "Soy de outer",
      "inner": "function"
    },
    "outer": "GlobalEnvironment"
  },
  "InnerEnvironment": {
    "variables": {
      "innerVar": "Soy de inner"
    },
    "outer": "OuterEnvironment"
  }
}
```

---

### **Explicación del JSON**

1. **GlobalEnvironment**:
   - Contiene las variables y funciones definidas en el ámbito global (`globalVar` y `outer`).
   - Su referencia `outer` es `null` porque no tiene un entorno padre.

2. **OuterEnvironment**:
   - Se crea cuando se ejecuta la función `outer`.
   - Contiene las variables locales de `outer` (`outerVar` y `inner`).
   - Su referencia `outer` apunta al `GlobalEnvironment`.

3. **InnerEnvironment**:
   - Se crea cuando se ejecuta la función `inner`.
   - Contiene las variables locales de `inner` (`innerVar`).
   - Su referencia `outer` apunta al `OuterEnvironment`.

---

### **Cómo JavaScript Busca Variables**

Cuando JavaScript necesita acceder a una variable:
1. Busca en el entorno léxico actual.
2. Si no la encuentra, sube al entorno padre (referencia `outer`).
3. Continúa subiendo hasta llegar al entorno global.
4. Si no encuentra la variable, lanza un error (`ReferenceError`).

---

### **Ejemplo con Scope Chain**

```javascript
const globalVar = "Soy global";

function outer() {
    const outerVar = "Soy de outer";

    function inner() {
        const innerVar = "Soy de inner";
        console.log(globalVar); // Busca en el entorno global
        console.log(outerVar);  // Busca en el entorno de `outer`
        console.log(innerVar);  // Busca en el entorno de `inner`
    }

    inner();
}

outer();
```

#### **Scope Chain Visualizado**
- Cuando `inner` intenta acceder a `globalVar`, `outerVar` e `innerVar`, JavaScript sigue esta cadena:
  - **`innerVar`**: Encontrado en el entorno de `inner`.
  - **`outerVar`**: No encontrado en `inner`, sube al entorno de `outer`.
  - **`globalVar`**: No encontrado en `inner` ni en `outer`, sube al entorno global.

---

### **Conclusión**

El entorno léxico es una estructura jerárquica que organiza las variables y funciones en JavaScript. Cada entorno tiene su propio conjunto de variables y una referencia al entorno padre, lo que permite a JavaScript buscar variables en la cadena de alcance (**scope chain**). Este concepto es fundamental para entender cómo funcionan el alcance, los closures y la ejecución del código en JavaScript.

En JavaScript, `eval` es una función que evalúa o ejecuta un código JavaScript representado como una cadena de texto. Es una función global que puede interpretar y ejecutar código dinámico en tiempo de ejecución.

---

### **Sintaxis**

```javascript
eval(string);
```

- **`string`**: Una cadena de texto que contiene código JavaScript válido.

---

### **Ejemplo Básico**

```javascript
const x = 10;
const y = 20;
const result = eval("x + y"); // Ejecuta el código "x + y"
console.log(result); // 30
```

En este ejemplo, `eval` interpreta la cadena `"x + y"` como código JavaScript y lo ejecuta en el contexto actual.

---

### **Usos de `eval`**

1. **Ejecutar código dinámico**:
   - `eval` puede ser útil si necesitas ejecutar código generado dinámicamente en tiempo de ejecución.

   ```javascript
   const code = "const a = 5; const b = 10; a * b;";
   console.log(eval(code)); // 50
   ```

2. **Acceso dinámico a variables**:
   - Puedes usar `eval` para acceder a variables cuyo nombre se genera dinámicamente.

   ```javascript
   const varName = "dynamicVar";
   const dynamicVar = 42;
   console.log(eval(varName)); // 42
   ```

---

### **Problemas y Riesgos de `eval`**

1. **Problemas de Seguridad**:
   - `eval` puede ejecutar cualquier código, lo que lo hace vulnerable a ataques de inyección de código si se usa con entradas no confiables.
   - **Ejemplo de riesgo**:
     ```javascript
     const userInput = "alert('Hacked!')";
     eval(userInput); // Ejecuta el código malicioso
     ```

2. **Impacto en el Rendimiento**:
   - `eval` es más lento que el código normal porque el motor de JavaScript debe interpretar y compilar el código en tiempo de ejecución.

3. **Dificultad para Depurar**:
   - El uso de `eval` puede dificultar la depuración, ya que el código generado dinámicamente no está presente en el archivo fuente.

---

### **Alternativas a `eval`**

1. **`JSON.parse`**:
   - Si estás procesando datos en formato JSON, usa `JSON.parse` en lugar de `eval`.

   ```javascript
   const jsonString = '{"name": "John", "age": 30}';
   const data = JSON.parse(jsonString);
   console.log(data.name); // "John"
   ```

2. **Funciones Constructoras**:
   - Usa el constructor de funciones para ejecutar código dinámico de manera más controlada.

   ```javascript
   const func = new Function("a", "b", "return a + b;");
   console.log(func(5, 10)); // 15
   ```

3. **Acceso Dinámico a Propiedades**:
   - Usa objetos o estructuras de datos para acceder dinámicamente a valores en lugar de `eval`.

   ```javascript
   const obj = { dynamicVar: 42 };
   const varName = "dynamicVar";
   console.log(obj[varName]); // 42
   ```

---

### **Cuándo Evitar `eval`**

- **Entradas no confiables**: Nunca uses `eval` con datos proporcionados por el usuario.
- **Código crítico**: Evita `eval` en aplicaciones donde el rendimiento y la seguridad son importantes.

---

### **Conclusión**

`eval` es una herramienta poderosa pero peligrosa. Aunque puede ser útil en casos específicos, su uso debe evitarse siempre que sea posible debido a problemas de seguridad, rendimiento y mantenibilidad. En su lugar, utiliza alternativas más seguras y controladas para lograr el mismo objetivo.

### **Concepto y Diferencias entre Session, Cookie, LocalStorage y SessionStorage**

En el desarrollo web, **Session**, **Cookie**, **LocalStorage** y **SessionStorage** son mecanismos para almacenar datos en el cliente o en el servidor. Cada uno tiene características específicas que los hacen adecuados para diferentes casos de uso.

---

### **1. Cookie**

#### **Concepto**:
- Las cookies son pequeños fragmentos de datos almacenados en el navegador y enviados al servidor con cada solicitud HTTP.
- Se utilizan principalmente para mantener sesiones de usuario, almacenar preferencias o realizar seguimiento entre solicitudes.

#### **Características**:
- **Almacenamiento**: Se almacenan en el navegador.
- **Tamaño máximo**: Aproximadamente 4 KB por cookie.
- **Duración**: Puede ser temporal (de sesión) o persistente (con fecha de expiración).
- **Acceso**: Disponible tanto en el cliente como en el servidor.
- **Seguridad**: Puede ser vulnerable si no se configura correctamente (`HttpOnly`, `Secure`, `SameSite`).

#### **Ejemplo**:
```javascript
// Crear una cookie
document.cookie = "username=JohnDoe; expires=Fri, 31 Dec 2025 23:59:59 GMT; path=/";

// Leer cookies
console.log(document.cookie); // "username=JohnDoe"
```

---

### **2. Session**

#### **Concepto**:
- Una sesión es un mecanismo del lado del servidor para almacenar datos específicos del usuario mientras navega por un sitio web.
- Se utiliza para mantener el estado del usuario (por ejemplo, autenticación) entre solicitudes HTTP.

#### **Características**:
- **Almacenamiento**: En el servidor.
- **Duración**: Dura mientras el navegador esté abierto o hasta que el servidor la invalide.
- **Acceso**: Solo accesible desde el servidor.
- **Seguridad**: Más seguro que las cookies porque los datos no se almacenan en el cliente.

#### **Ejemplo** (usando Node.js con Express):
```javascript
const session = require('express-session');

app.use(session({
  secret: 'mySecretKey',
  resave: false,
  saveUninitialized: true,
  cookie: { secure: true }
}));

app.get('/', (req, res) => {
  req.session.username = 'JohnDoe'; // Almacenar datos en la sesión
  res.send('Sesión iniciada');
});
```

---

### **3. LocalStorage**

#### **Concepto**:
- `LocalStorage` es una API del navegador que permite almacenar datos de manera persistente en el cliente.
- Los datos persisten incluso después de cerrar el navegador.

#### **Características**:
- **Almacenamiento**: En el navegador.
- **Tamaño máximo**: Aproximadamente 5-10 MB por dominio.
- **Duración**: Persistente hasta que se elimine manualmente.
- **Acceso**: Solo accesible desde el cliente.
- **Seguridad**: No es seguro para datos sensibles porque es accesible desde JavaScript.

#### **Ejemplo**:
```javascript
// Guardar datos
localStorage.setItem('username', 'JohnDoe');

// Leer datos
console.log(localStorage.getItem('username')); // "JohnDoe"

// Eliminar datos
localStorage.removeItem('username');
```

---

### **4. SessionStorage**

#### **Concepto**:
- `SessionStorage` es similar a `LocalStorage`, pero los datos solo persisten mientras la pestaña del navegador esté abierta.
- Los datos se eliminan al cerrar la pestaña o el navegador.

#### **Características**:
- **Almacenamiento**: En el navegador.
- **Tamaño máximo**: Aproximadamente 5 MB por dominio.
- **Duración**: Solo durante la sesión de la pestaña.
- **Acceso**: Solo accesible desde el cliente.
- **Seguridad**: No es seguro para datos sensibles porque es accesible desde JavaScript.

#### **Ejemplo**:
```javascript
// Guardar datos
sessionStorage.setItem('username', 'JohnDoe');

// Leer datos
console.log(sessionStorage.getItem('username')); // "JohnDoe"

// Eliminar datos
sessionStorage.removeItem('username');
```

---

### **Diferencias Principales**

| **Característica**       | **Cookie**                     | **Session**                  | **LocalStorage**             | **SessionStorage**           |
|--------------------------|--------------------------------|-----------------------------|------------------------------|------------------------------|
| **Almacenamiento**       | Cliente (navegador)           | Servidor                    | Cliente (navegador)          | Cliente (navegador)          |
| **Duración**             | Configurable (sesión o persistente) | Hasta que se cierre el navegador o se invalide | Persistente hasta que se elimine | Solo durante la sesión de la pestaña |
| **Tamaño máximo**        | ~4 KB                         | Depende del servidor        | ~5-10 MB                     | ~5 MB                        |
| **Acceso**               | Cliente y servidor            | Solo servidor               | Solo cliente                 | Solo cliente                 |
| **Seguridad**            | Vulnerable si no se configura correctamente | Más seguro                  | No seguro para datos sensibles | No seguro para datos sensibles |
| **Uso típico**           | Autenticación, preferencias   | Estado del usuario          | Almacenamiento persistente   | Almacenamiento temporal      |

---

### **Cuándo Usar Cada Uno**

1. **Cookies**:
   - Para almacenar datos que deben enviarse al servidor con cada solicitud (por ejemplo, tokens de sesión o preferencias de usuario).

2. **Session**:
   - Para manejar datos sensibles o mantener el estado del usuario en el servidor (por ejemplo, autenticación).

3. **LocalStorage**:
   - Para almacenar datos persistentes en el cliente que no son sensibles (por ejemplo, configuraciones de la aplicación).

4. **SessionStorage**:
   - Para almacenar datos temporales que solo son relevantes mientras la pestaña esté abierta (por ejemplo, datos de formularios).

---

### **Conclusión**

Cada mecanismo tiene su propósito y limitaciones. La elección entre cookies, sesiones, `LocalStorage` y `SessionStorage` depende del caso de uso, la seguridad requerida y la duración de los datos que necesitas almacenar.
