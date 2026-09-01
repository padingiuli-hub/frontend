Es un framework construido encima de React. Le agrega:
- Rutas automaticas (sin tener que instalar y configurar una libreria de routing aparte).
- Renderizado en el servidor (Server-Side Rendering, SSR): la pagina se arma en el servidor antes de mandarla al navegador, lo cual mejora la velocidad de carga.
- API routes: se puede escribir backend dentro del mismo proyecto de Next.js.

Se usa cuando quieres una app de React completa y lista para produccion, sin tener que armar manualmente el ruteo, el servidor, etc

```jsx

//pages/index.js 
export default function Home() {
	return <h1>Hola desde Next.js</h1>
}
```

### Caracteristicas de Next.js

#### 1. Routing (el sistema de rutas)

En Next.js no se configuran las rutas a mano, lo que hace es mirar la estructura de carpetas de tu proyecto, y cada carpeta se convierte automaticamente en una URL. 
```

app/
├── page.js              →  se convierte en la ruta:  /
├── dashboard/
│   └── page.js           →  se convierte en la ruta:  /dashboard
└── blog/
    └── [id].js            →  se convierte en la ruta:  /blog/lo-que-sea
```

Para que una carpeta sea una ruta visible, tiene que tener dentro un archivo que se llame exactametne `page.js`.  
Dentro de ese `page.js`, siempre va una funcion de esta forma:

```jsx
export default function Page() {
	return <h1> Hello </h1>
}
```

#### 2. Layouts

El problema que resuelve: cuando hay varias paginas`(/dashboard, /blog) ` y todas necesitan el mismo header y el mismo menu, sin layouts se tendria que copiar y pegar ese header en cada pagina. Layouts permite defeinirlo una sola vez y que se aplique a todas las paginas dentro de esa carpeta.

Como se usa: creas un archivo que se llame `layout.js` dentro de una carpeta

```java
export default function DashboardLayout ({ children }) {// parametro especial, representa lo que sea que vaya dentro, en este caso el contenido de la pagina (page.js de esa carpeta)
	return (
		<section>
			<nav> Aca va el menu de navegacion </nav>
			{children} //aca next.js inserta el contenido de cada pagina individual
		</section>
		)
}
```

##### Ejemplo de ubicacion

```jsx
app/
├── layout.js          ← Root Layout (el más externo, obligatorio)
└── dashboard/
    ├── layout.js       ← Layout de esta carpeta
    └── page.js         ← Página de esta carpeta
```


#### 3. Link component

El problema que resuelve: en HTML normal, para ir de una pagina a otra se usa la etiqueta`<a>` 

```html
<a href="/dashboard">Dashboard</a>
```

El problema de esto en una app moderna: cada vez que hacés click en un `<a>` normal, el navegador **recarga toda la página desde cero** (se ve un pantallazo blanco un instante, se vuelve a cargar todo). Eso es lento e innecesario si ya estás dentro de tu app.

El componente`Link` reemplaza al `<a>`
```javascript
import Link from 'next/link'

export default function Page() {
	return <Link href="/dashboard">Dashboard</Link>
}
```

Se usa **prácticamente igual** que un `<a>` normal, pero por dentro hace la navegación **sin recargar la página entera** — solo actualiza la parte que cambió.

#### 4. Instant loading state `(loading.js)`

Es un archivo que, con solo existir en la carpeta correcta, Next.js muestra automaicamente mientras esa pagina esta cargando datos.

```javascript
export default function Loading() {
	return <LoadingSkeleton /> //frase que quieras, puede ser un spinner
}
```

Es la misma logica de `page.js` y `layout.js`. No se escribe a mano `if (loading)`, next se encarga de mostrar el `loading.js`  mientras espera, segun la carpeta que este.

```
app/
└── dashboard/
    ├── page.js       ← el contenido real de /dashboard
    ├── layout.js      ← el molde que envuelve a page.js
    └── loading.js     ← lo que se muestra MIENTRAS page.js está cargando
```

#### 5. Dinamic Routes

Son rutas que cambian segun lo que ponga el usuario en la URL.
En vez de poner un nombre fijo a la carpeta, ponés el nombre **entre corchetes**. Eso le dice a Next.js: "esta parte de la URL puede ser **cualquier cosa**, no un texto fijo".
```

app/
 └── blog/
  └── [nombre]/
   └── page.js
```

Como acceder a ese valor dentro del componente

```jsx
export default function Page ({ params }) {
	return <div> My post: {params.nombre} </div>
}
```

Entonces, si el usuario visita `/blog/receta-de-milanesas`, adentro de esa función, `params.nombre` va a valer literalmente el texto `"receta-de-milanesas"`.

#### 6. API Routes

Es para crear endpoints dentro del mismo proyecto, sin necesidad de armar un servidor aparte (tipo Node + Express)

Como se hace:

```
app/
 └── api/
  └── users.js
```

Y adentro en vez de devolver JSX (como lo hacia en `page.js`), devolves datos:

```js
export default function handler (req, res) {
	res.status(200).json({ users: [] })
}
```

- `handler` → nombre de la función (podría llamarse distinto, es convención).
- `req` → la **request**, o sea, lo que llega (parecido a un `HttpServletRequest` en Java).
- `res` → la **response**, lo que vos devolvés (parecido a `HttpServletResponse`).
- `res.status(200)` → código de estado HTTP (200 = todo OK, como en cualquier API REST).
- `.json({ users: [] })` → le decís "devolvé esto como JSON".

API Routes te deja crear endpoints de backend (que devuelven JSON en vez de pantallas) dentro del mismo proyecto de Next.js, usando la misma lógica de carpetas que usás para las páginas.

#### 7. Fetching Data en Next.js

En un Server Component de Next.js, podés usar `async/await` directamente en el componente para traer datos **antes** de mostrar la página, sin necesitar `useState`, `useEffect` ni mostrar un "Loading..." — el servidor hace el trabajo pesado antes de que la página llegue al usuario.

El codigo

```jsx
async function getData() {
	 const res = await fetch ('http://api.example.com/...')
	 
	 if (!res.ok) {
		 throw new Error('fail')
	 }
	 return res.json()
}

export default async function Page () {
	const data = await getData()
	return <main>{data aca}</main>
}
```

**`async`** le dice a JavaScript: "esta función va a hacer algo que tarda tiempo (como pedirle datos a otro servidor), y quiero poder esperar ese resultado con `await`".

`fetch(...)` tarda un tiempo en responder (viaja por internet, el servidor procesa, etc.). `await` le dice a JavaScript: "esperá acá hasta que `fetch` termine, y después seguí". Sin `await`, seguirías de largo sin tener todavía la respuesta.

**Por qué esto es una ventaja:** el usuario nunca ve un "Loading..." en pantalla parpadeando — cuando la página llega a su navegador, **ya viene con los datos adentro**, porque el servidor los buscó primero. Esto también ayuda al SEO (los buscadores como Google ven la página ya con contenido, no una pantalla vacía esperando datos).