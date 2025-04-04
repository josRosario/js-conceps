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

