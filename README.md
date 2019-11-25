# cliente-bank-app

> Cliente Vue para utilizar con una API REST de banca online

## API
La aplicación con la API de banca online está disponible en https://github.com/pedroprieto/api-bank-app. Ambas aplicaciones deben estar funcionando para poder interactuar.

## Uso

``` bash
# Clonar repositorio
git clone https://github.com/pedroprieto/cliente-bank-app.git

# Acceder a la carpeta
cd cliente-bank-app

# Instalar dependencias
$ npm run install

# Testear en local. Acceso en localhost:3000. La API debe estar en marcha en https://localhost:5001
$ npm run dev

# Compilar aplicación para producción (el resultado estará en la carpeta dist)
$ npm run build

# (Opcional) Lanzar aplicación en modo producción
$ npm run start
```

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
    "dev": "baseURL='https://localhost:5001' nuxt",
    "build": "baseURL='DIRECCION_DESPLIEGUE_API' nuxt build",
    "start": "nuxt start",
    "generate": "nuxt generate"
  },
```

## Despliegue en S3
1. Ejecutar `npm run install` para instalar las dependencias de la aplicación
2. Ejecutar `npm run build`
3. Crear bucket en S3
4. Configurar bucket como público
5. Configurar bucket como contenedor de sitio web estático
6. Subir contenido de carpeta `dist` al bucket
7. Modificar todos los archivos y carpetas del bucket para que sean públicos (opción Hacer público)
8. Acceder al punto de enlace del bucket para cargar el cliente web

