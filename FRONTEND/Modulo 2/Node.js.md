
Javascript -> lenguaje de programacion
Node.js -> entorno para ejecutar javascript en vez de ejecutarlo en el navegador

- Usa el mismo motor que chrome, V8
- Seria como Spring Boot
- Sirve para crear servidores, crear APIS y apps backend
- Para los servidores se usa HTTP

**Crear un servidor con Node.js**

```Javascript
const http = require("http");
const server = http.createServer((req, res) => {
	res.end("Hola mundo");
}
server.listen(3000);
```

- *http.createServer()* seria como *Servidor servidor = crearServidor();
- Devuelve un servidor y lo guarda en la constante server
- *req y res* son los parametros, dos objetos que Node.js da automaticamente, significan request y response
- *res.end* significa termina la respuesta y mandasela al cliente

Antes de eso hay que traer el modulo HTTP

```javascript
const http = require("http");
```

Se ejecuta por consola poniendo 
```javascript
node servidor.js
```

El package.json contiene informacion del proyecto y dependencias. Funciona como el pom.xml
## Npm

- Significa Node Package Manager
- Es el administrador de paquetes de Node.js
- Funciona como maven para instalar librerias y herramientas
## Express

Es una framework para Node.js que facillita la creacion de servidores y APIS. Para no tener que escribir todo con HTTP.

En lugar de hacer

```javascript
if (req.url === "/usuarios"){
	req.end ("lista de usuarios");
}
```

Con Express se hace

```javascript
app.get("/usuarios", (req, res) => {
	res.send("lista de usuarios");
});
```

#### Para crear un servidor con Express

```javascript
const express = require("express"); //Llama a express y lo guarda en la variable, es como si fuera un import en Java

const app = express(); //Crea la aplicacion express, la inicializa y la guarda en app

app.get("/", (req, res) => { //Peticion GET
	res.send ("Hola desde Express");
});

app.listen(4000, () => { //Encendemmos el servidor, ejecuta la funcion cuando arranco correctamente
	console.log("Servidor funcionando en http://localhost:4000");
});
```

**Diferencia params, query y body en metodo POST**
- Params se usa en la ruta, se usa para ids, con *req.params* y */usuarios/25*
- Query es para muchas consultas, se usa despues de ?, con *req.query*, es para fitros, se usa */usuarios?nombre=giuliana*
- Body es el cuerpo en formato JSON
##### Métodos POST, GET, PUT, DELETE

**GET**

```java
app.get("/url", (req, res) => {
	res.send("respuesta");
});
```

**POST**

Se tienen que probar con Postman
```JAVA
app.use(express.json()); //Le dice a Express que interprete JSON que tengan las peticiones

app.post("/usuarios", (req, res) => {
	const nombre = req.query.nombre; 
	const edad = req.query.edad;
	
	res.send("Usuario recibido: ${nombre}, edad: ${edad}");
});

app.listen(4000, () => {
	console.log("Estoy funcionando");
});
```

**PUT**

```java
app.put("/usuarios/:id",(req, res) =>{
	const id = req.params.id;
	const nombre = req.body.nombre;
	const apellido = req.body.apellido;
	
	res.send(`Actualizando usuario ${id}: ${nombre} ${apellido}`);
})
```

**DELETE**

```java
 app.delete("/usuarios/:id", (req, res) => {
	 const id = req.params.id;
	 res.send(`Usuario ${id} eliminado`);
 });
```

falta:

dom
react
cuestionario
vite y typescript