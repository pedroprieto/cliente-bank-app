# cliente-bank-app

> Cliente Vue para utilizar con una API REST de banca online

## Build Setup

``` bash
# install dependencies
$ npm run install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).

## Creación de la aplicación

1. Crear proyecto de Nuxt
2. Establecer baseURL para Axios (URL de la API)
3. Crear página en directorio `pages`
4. Modificar fichero nuxt.config.js para que cargue la variable de entorno de acceso a la API para que la utilice AXIOS por defecto
```js
  axios: {
    baseURL: process.env.baseURL
  },
```
5. Modificar fichero `package.json` para que cree las variables de entorno necesarias para la conexión a la API en modo local y en modo producción
```json
  "scripts": {
    "dev": "baseURL='http://localhost:5000' nuxt",
    "build": "baseURL='http://nueva-env.qyt5jae7qv.eu-west-1.elasticbeanstalk.com' nuxt build",
    "start": "nuxt start",
    "generate": "nuxt generate"
  },
```

## Despliegue en S3
1. Ejecutar `npm run build`
2. Crear bucket en S3
3. Configurar bucket como público
4. Configurar bucket como contenedor de sitio web estático
5. Subir contenido de carpeta `dist` al bucket
6. Modificar todos los archivos y carpetas del bucket para que sean públicos (opción Hacer público)
7. Acceder al punto de enlace del bucket para cargar el cliente web
