- Tiene 3 tipos de variables:
	- *const* es constante 
	- *var* es global y ya casi no se usa
	- *let* puede cambiar y es por bloque
- Es dinamico ??
- Completar


Funciones flecha

```java
//Las funciones pasan de verse asi
function saludar(nombre) {
	console.log("hola");
}

//A asi

 saludar(nombre) => {
	console.log("hola");
}
```

### DOM

Document Object Model, cuando el navegador carga un HTML, transforma ese documento en una estructura de objetos que JS puede manipular. 
Permite que JS interactue con HTML.

Se puede usar con el objeto *document* 

```java
//HTML
<h1 id-"titulo> Hola </h1>

//JS
const titulo = document.getElementById("titulo");
```

Tiene metodos importantes como

- querySelector()
	- puede buscar por selector CSS *document.querySelector(#titulo);*
	- por id *document.querySelector(.boton)*;
	- por clase *document.querySelector("h1")*;'
- querySelectorAll()
	- lo mismo seleccionando una clase que tenga muchos elementos

Tambien tiene

```java
element.innerHTML // saca el html <h1>hola</h1>

titulo.textContent // saca el texto "Hola"
```

#### Eventos 

Existen 
- click
- submit
- input
- change
- keydown
- mouseover
- etc


Se utiliza
```java
const boton = document.getElementById("#boton");

boton.addEventListener("click", () => {
	console.log("hiciste click");
});
```

# JSON

Javascript Object Notation es un formato de texto para representar y transportar datos
JSON no permite undefined o funciones

#### JSON + Express

Si tenemos:

```java
app.use(express.json());
```

Estamos diciéndole a Express:

"Si me llega un body con JSON, interpretalo y ponelo disponible en `req.body`."

###### JSON.stringify() -> convierte objeto JS a texto JSON

Por ejemplo:

```java
const usuario = {

    nombre: "Giuliana",

    edad: 23

};
```

Hacemos:

```java
const json = JSON.stringify(usuario);
```

Ahora `json` contiene un **string**:

```java
{"nombre":"Giuliana","edad":23}
```

###### JSON.parse() -> convierte JSON a objeto JS

Pasa de String JSON -> JSON.parse() -> Objeto JS

Por ejemplo tenemos un string:

```java
const datos = '{"nombre":"Giuliana","edad":23}';
```

Hacemos:

```java
const usuario = JSON.parse(datos);
```

Ahora tenemos un objeto JavaScript:

```
usuario.nombre
```