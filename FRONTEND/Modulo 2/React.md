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

