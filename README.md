# cliente-bank-app

> Cliente VueJS para utilizar con una API REST de banca online

## Tutorial en Youtube
Puedes ver el procedimiento de despliegue de la aplicación en este tutorial en Youtube:
[![Tutorial en Youtube](https://img.youtube.com/vi/gPqthQb_I6o/0.jpg)](https://www.youtube.com/watch?v=gPqthQb_I6o)

## API
La aplicación con la API de banca online está disponible en https://github.com/pedroprieto/api-bank-app. Ambas aplicaciones deben estar funcionando para poder interactuar.

## Instalación
La aplicación está realizada en [NodeJS](https://nodejs.org/es/).

``` bash
# Clonar repositorio
git clone https://github.com/pedroprieto/cliente-bank-app.git

# Acceder a la carpeta
cd cliente-bank-app

# Instalar dependencias
$ npm install

# Testear en local. Acceso en localhost:3000. La API debe estar en marcha en https://localhost:5001
$ npm run dev

# Compilar aplicación para producción (el resultado estará en la carpeta dist)
$ npm run build

# (Opcional) Lanzar aplicación en modo producción
$ npm run start
```

## Instrucciones de despliegue en AWS S3
Dado que se trata de recursos estáticos (no precisan de lógica de servidor), el despliegue puede realizarse en un servicio como **S3**. Para mejorar su distribución se puede utilizar también un servicio de CDN como **CloudFront**.

1. Ejecutar `npm install` para instalar las dependencias de la aplicación
2. Modificar fichero `package.json` y cambiar `DIRECCION_DESPLIEGUE_API` con la URL de conexión a la API desplegada en AWS:
    ```json
    "scripts": {
        "dev": "baseURL='https://localhost:5001' nuxt",
        "build": "baseURL='DIRECCION_DESPLIEGUE_API' nuxt build",
        "start": "nuxt start",
        "generate": "nuxt generate"
    },
    ```
3. Ejecutar `npm run build`
4. Crear un bucket en S3
5. Configurar bucket como público
6. Configurar bucket como contenedor de sitio web estático
7. Subir contenido de carpeta `dist` al bucket
8. Modificar todos los archivos y carpetas del bucket para que sean públicos
9. Acceder al punto de enlace del bucket para cargar el cliente web

## Pasos seguidos para la creación de la aplicación
1. Crear proyecto de [Nuxt](https://nuxtjs.org/)
2. Establecer `baseURL` para Axios (URL de la API)
3. Crear página en directorio `pages`
4. Modificar fichero `nuxt.config.js` para que cargue la variable de entorno de acceso a la API para que la utilice AXIOS por defecto
    ```js
    axios: {
    baseURL: process.env.baseURL
    },
    ```
5. Modificar fichero `package.json` para que cree las variables de entorno necesarias para la conexión a la API en modo local y en modo producción
    ```json
    "scripts": {
        "dev": "baseURL='https://localhost:5001' nuxt",
        "build": "baseURL='DIRECCION_DESPLIEGUE_API' nuxt build",
        "start": "nuxt start",
        "generate": "nuxt generate"
    },
    ```
