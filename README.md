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
