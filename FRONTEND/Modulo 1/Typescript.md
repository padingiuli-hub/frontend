Es un lenguaje desarrollado por Microsoft que agrega tipado estatico a Javascript.
Te permite agregar informacion de tipos: 

```java
const nombre: string = "Giuliana";
```

Por ejemplo un array:

```java
const usuario: {
    nombre: string;
    edad: number;
} = {
    nombre: "Giuliana",
    edad: 23
};
```

En caso de que no queremos que typescript controle el tipo se usa:

```java
let dato: any = "hola";
```

El navegador no ejectura Typescript directamente, se usa *tsconfig.json* para configurar como se debe compilar el proyecto.

Por ejemplo:

```java
{
    "compilerOptions": {
        "target": "ES2020",
        "strict": true
    }
}
```

