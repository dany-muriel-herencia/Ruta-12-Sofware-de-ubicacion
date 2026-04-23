# Frontend - Arquitectura de Capas

## Descripción
Frontend del sistema de ubicación. Interfaz de usuario para interactuar con el sistema.

## Estructura de Capas

### 📁 `/src/components`
- Componentes reutilizables
- Botones, cards, modales, etc.
- Componentes sin lógica de negocio

### 📁 `/src/pages`
- Páginas completas de la aplicación
- Combinan múltiples componentes
- Conectadas a rutas

### 📁 `/src/services`
- Comunicación con el backend
- Llamadas HTTP/API
- Manejo de datos externos

### 📁 `/src/hooks`
- Custom hooks de React
- Lógica reutilizable
- Estado compartido

### 📁 `/src/utils`
- Funciones auxiliares
- Helpers generales
- Formatos y transformaciones

### 📁 `/src/styles`
- Estilos globales
- Temas
- Variables CSS

### 📁 `/src/assets`
- Imágenes
- Iconos
- Fuentes
- Recursos estáticos

### 📁 `/public`
- Archivos públicos
- HTML principal
- Favicon

## Instalación
```bash
npm install
```

## Ejecutar (Desarrollo)
```bash
npm start
```

## Build (Producción)
```bash
npm run build
```
