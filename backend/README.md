# Backend - Arquitectura de Capas

## Descripción
Backend del sistema de ubicación. Esta capa gestiona toda la lógica de negocio, comunicación con la base de datos y expone APIs REST.

## Estructura de Capas

### 📁 `/controllers`
- Responsables de recibir las peticiones HTTP
- Validan los datos entrada
- Llaman a los servicios
- Retornan las respuestas al cliente

### 📁 `/services`
- Contienen la lógica de negocio principal
- Interactúan con los modelos y la base de datos
- Independientes de las rutas HTTP
- Reutilizables desde diferentes puntos

### 📁 `/models`
- Definen la estructura de datos
- Modelos ORM/esquemas
- Validaciones a nivel de datos

### 📁 `/routes`
- Definen los endpoints de la API
- Mapean URLs a controladores
- Asocian middleware

### 📁 `/middleware`
- Funciones que interceptan peticiones
- Autenticación y autorización
- Logging, validación, etc.

### 📁 `/config`
- Configuración de la aplicación
- Variables de entorno
- Conexión a base de datos

### 📁 `/database`
- Migraciones
- Seeds (datos iniciales)
- Conexion a DB

### 📁 `/utils`
- Funciones auxiliares reutilizables
- Helpers
- Formatters, parsers, validadores

## Instalación
```bash
npm install
```

## Ejecutar
```bash
npm start
```
