Es una biblioteca/framework de JS para construir interfaces de usuario. Sirve para el *FRONTEND*. Desarrollada por Facebook en 2011. 

##### Características

- Declarativo: le dice como debe verse la interfaz y React se encarga de actualizarla.
- Componentes reutilizables:  se pueden crear componentes que son partes de la interfaz para reutilizarlos
- Rendimiento y Virtual DOM: React mantiene una representacion en memoria llamada virtual DOM, detecta que partes necesitan cambiar y actualiza el DOM real de forma eficiente.
- Ecosistema fuerte: Hay muchas herramientas y librerias alrededor de React


#### Estructura React - Vite

- *index.html*: Pagina principal donde se monta la aplicación. 
- *src/main.jsx*: Punto de entrada de la aplicación React.
- *src/App.jsx*: Componente raiz de la aplicacion.
- *src/components/*: Carpeta para componentes personalizados
- *public*: Carpeta para activos estaticos como imagenes y fuentes
- *package.json*: Archivo de configuracion del proyecto que incluye informacion sobre el proyecto y sus dependencias.
- *vite.config.js*: Archivo de configuracion de Vite. Permite personalizar el comportamiento de Vite, incluyendo configuraciones de plugins.

##### JSX

JSX significa JavaScript XML, es una extension de la sintaxis de JS utilizada en React para describir la estructura de la interfaz de usuario. Permite escribir elementos UI dentro de JS de manera declarativa.

Elemento JSX:
```java
const element = <h1> Hola {nombre} </h1>;
const nombre = 'Giuliana';
```

#### Componentes React

Un componente es una parte de la pantalla que convertis en una pieza independiente y reutilizable. 

Un componente es basicamente una funcion:
```java
function Saludo() {
	return <h1>Hola Giuliana</h1>;
}

// y despues llamarlo
<Saludo/>
```

#### Props

Son datos que un componente recibe desde afuera. (Parametros)

Por ejemplo:
```java
function Producto(props) {
	return (
		<div>
			<h2>{props.nombre}</h2>
			<p>{props.precio}</p>
		</div>
	);
}

//y se puede usar asi

<Producto nombre= "Remera" precio="500 />
```

#### State

Son datos que pertenecen al componente y pueden cambiar. Por ejemplo, un contador. Es un estado del componente. Puede cambiar. 

En React moderno se usa State con useState (un hook):

```java
import {useState} from "react";

function Contador() {
	const [contador, setContador] = useState(0); //crea un estado que inicialmente valga 0, crea un contador con valor actual y un setContador para cambiarlo
	
	return (
		<div> 
			<p>{contador}</p>
			
			<button onClick={() => setContador(contador+1)}> </button>
		</div>
	);
}

```

## Eventos en React

Los eventos son acciones que pasan al hacer click. Se escriben en camel case y no vez de escribir la funcion entre comillas y con parentesis `"handleClick()"` se pasa entre {}.

```java
const handleClick = () => {
	alert('Hiciste click!');
}

function App() {
	return <button onClick={handleClick}>Click me</button>
}
```

#### preventDefault

Algunos eventos tienen un comportamiento por defecto del navegador. Por ejemplo cuando envias un formulario `(submit)`, el navegador recarga la pagina automaticamente. 
`event.preventDefault()` evita que se recargue la pagina. 

```
function handleSubmit(event) {
	event.preventDefault();
	alert('Formulario enviado!');
}

function App() {
	return (
		<form onSubmit={handleSubmit}>
			<button type="submit">Submit</button>
		</form>
	);
}
```

### Renderizado condicional

##### 1. Operador ternario (condicion ? A : B)
Significa, si se cumple esto, mostrame A, si no, mostrame B

```java
const Greeting = ({isLoggedIn}) => {
	return (
		<div>
			{isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign up.</h1>}
		</div>
	);
}
```

##### 2. Operador && (mostrar algo SOLO si se cumple)
##### 3. Operador if-else
##### 4. Guard clauses (primero no se cumple, despues caso principal)
##### 5. Ternario anidado (se juntan dos ternarios uno dentro del otro)

## Hooks

Son funciones que te dejan usar los estados, ciclos de vida y contexto en componentes funcionales. Siempre empiezan con la palabra `use: useState, useEffect, useContext`, etc

- Regla 1: solo arriba del todo, nunca dentro de un if o un loop.
- Regla 2: solo se usan adentro de componentes de React.  

##### `useState` - guardar un dato que cambia

```java
import React, {useState} from 'react';

function Counter() {
	const [count, setCount] = useState(0);
	
	return (
		<div>
			<p> Clickeaste {count} veces </p>
			<button onClick={() => setCount(count + 1)}>Click me</button>
		</div>
	);
}
```

- `useState(0)` crea un dato que arranca en 0.
- te devuelve 2 cosas dentro de un array: `count` (el valor actual) y `setCount` la funcion para cambiarlo.
- cuando llamas `setCount(algo)`, React actualiza el valor y vuelve a renderizar el componente mostrando el numero nuevo. 

##### `useContext` - compartir datos sin pasar props a mano

```java
const ThemeContext = React.createContext('light'); //creas el contexto, valor por defecto "light"

function ThemeButton() {
	const theme = useContext(ThemeContext); //lo lees directo, sin que nadie te lo pase por prop
	return <button>Estoy usando el tema: {theme}</button>
}
```

##### `useEffect` - hacer algo cuando el componente se monta / se actualiza / se desmonta

Sirve para decir: despues de que se dibuje esto en pantalla, hace algo extra.
Ese algo extra suele ser cosas que no son parte del dibujo en si, pedir datos a una API, prender un timer, escuchar eventos del teclado, etc

```java
useEffect(() => {
	document.title = `Clickeaste ${count} veces`;
}, [count]);
```

Cada vez que `count` cambie, ejecuta esta funcion (que cambia el titulo de la pestania)

Tiene dos partes:
- La funcion, el que hacer (cambiar el titulo).
- El array `[count]` (segundo argumento), el cuando hacerlo. Le dice, solo ejecutate si el `count` cambio. 

Variantes del array:
- `useEffect(() => {...}, []` -> Array vacio = "ejecutate 1 sola vez, apenas aparece el componente en la pantalla"
- `useEffect(() => {...}, [count])` -> "ejecutate cada vez que `count` cambie"
- `useEffect(() => {...})` -> sin array = "ejecutate despues de CADA render" (casi no se usa)
#### Hooks Personalizados 

Si tienes una logica que se repite en varios componentes, la puedes empaquetar en tu propia funcion que empiece con `use`


```java
const useFetch = (url) => {
	const [data, setData] = useState(null);
	const [loading, setLoading] = useState(true);
	
	useEffect(() => {
	fetch(url)
		.then (res => res.json())
		.then (result => {
			setData(result);
			setLoading(false);
		});
	}, [url]);
	
	return { data, loading }; //devuelves el componente que necesita
}
```
