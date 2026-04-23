# Clean Architecture - Ruta 12

## 📋 Descripción

La Clean Architecture es una arquitectura propuesta por Robert C. Martin que enfatiza la independencia de frameworks, UI, base de datos y cualquier agencia externa. El objetivo es crear sistemas testables, mantenibles y escalables.

## 🎯 Principios Fundamentales

1. **Independencia de Frameworks**: La arquitectura no depende de un framework específico
2. **Testeable**: La lógica de negocio puede testearse sin UI, BD, web, etc.
3. **Independencia de UI**: El cambio de UI no afecta el resto del sistema
4. **Independencia de BD**: La lógica de negocio no conoce qué BD se usa
5. **Independencia de Agencias Externas**: La lógica central no depende de APIs externas

## 🏗️ Estructura de Capas

Las capas están organizadas concéntricamente de adentro hacia afuera:

```
┌─────────────────────────────────────────┐
│  Frameworks & Drivers (Web, BD, UI)     │
├─────────────────────────────────────────┤
│  Interface Adapters (Controllers, etc)  │
├─────────────────────────────────────────┤
│  Application (Use Cases, DTOs)          │
├─────────────────────────────────────────┤
│  Domain (Entities, Business Rules)      │
└─────────────────────────────────────────┘
```

### 📌 Regla de Dependencia
**Las dependencias SIEMPRE apuntan hacia adentro (hacia la capa Domain)**

Domain → Application → Interface Adapters → Frameworks & Drivers

Las capas externas nunca deben ser conocidas por las capas internas.

## 🔍 Descripción por Capa

### 1. **DOMAIN** (Núcleo - Reglas de Negocio)

El corazón de la aplicación. Contiene la lógica pura del negocio.

#### `/domain/entities`
- Entidades que representan conceptos principales del dominio
- Contienen **SOLO** lógica de negocio (reglas que nunca cambian)
- Independientes de DB, frameworks, etc.
- No tienen dependencias externas

```
Ejemplo:
- Usuario.ts: Reglas sobre usuarios (validaciones básicas)
- Ubicacion.ts: Reglas sobre ubicaciones
```

#### `/domain/interfaces`
- Contratos/interfaces que definen cómo interactuar con servicios
- El Domain define qué necesita, no importa cómo se implemente
- **NO contienen lógica de implementación**

```
Ejemplo:
- IUsuarioRepository.ts: Define métodos para acceder a usuarios
- IEmailService.ts: Define cómo enviar emails
```

---

### 2. **APPLICATION** (Casos de Uso - Orquestación)

Lógica de aplicación que orquesta entidades y servicios externos.

#### `/application/use_cases`
- Casos de uso (acciones que puede realizar el usuario)
- Orquestan entidades y servicios
- Transforman inputs en outputs
- UNO por caso de uso (Single Responsibility)

```
Ejemplo:
- CrearUsuarioUseCase.ts: Crea un nuevo usuario
- BuscarUbicacionUseCase.ts: Busca ubicaciones
```

#### `/application/dtos`
- Data Transfer Objects (objetos de transferencia)
- Definen la forma de entrada/salida de los Use Cases
- Aislación: no exponen la estructura interna del Domain

```
Ejemplo:
- CrearUsuarioDTO.ts
- UsuarioResponseDTO.ts
```

---

### 3. **INFRASTRUCTURE** (Implementaciones - Acceso Externo)

Implementaciones concretas de interfaces definidas en Domain.

#### `/infrastructure/repositories`
- Implementan `IRepository` del Domain
- Acceso a base de datos
- Convierten datos de BD a Entidades

```
Ejemplo:
- UsuarioRepository.ts: Implementa IUsuarioRepository
```

#### `/infrastructure/database`
- Configuración y conexión a BD
- Migraciones
- Seeders

```
Estructura:
- connection.ts
- migrations/
- seeders/
```

#### `/infrastructure/external_services`
- Implementan servicios externos (APIs, email, etc)
- No son core del negocio

```
Ejemplo:
- EmailService.ts: Envía emails reales
- MapboxService.ts: Integración con Mapbox
```

---

### 4. **PRESENTATION** (Controladores - HTTP/UI)

Capa que expone la aplicación al mundo exterior.

#### `/presentation/controllers`
- Controllers HTTP/REST
- Reciben requests y llaman Use Cases
- **NO contienen lógica de negocio**
- Retornan responses HTTP

```
Ejemplo:
- UsuarioController.ts
  - crear()
  - obtener()
- UbicacionController.ts
```

#### `/presentation/middleware`
- Autenticación
- Validación de requests
- Logging
- Manejo de errores

#### `/presentation/routes`
- Definición de rutas
- Mapeo de rutas a controllers

---

### 5. **SHARED** (Utilitarios Compartidos)

Código reutilizable en toda la aplicación.

#### `/shared/utils`
- Funciones auxiliares (formateos, conversiones, etc)

#### `/shared/constants`
- Constantes globales

#### `/shared/errors` (Backend)
- Clases de errores customizadas

---

### 6. **TESTS** (Backend)

#### `/tests/unit`
- Tests de entidades y Use Cases
- Sin dependencias externas

#### `/tests/integration`
- Tests de controllers
- Tests end-to-end

---

## 📁 Estructura Completa

```
Ruta-12-Sofware-de-ubicacion/
│
├── 📁 backend/
│   ├── src/
│   │   ├── 🔵 domain/                          # Reglas de negocio puras
│   │   │   ├── entities/                       # Entidades del dominio
│   │   │   └── interfaces/                     # Contratos
│   │   │
│   │   ├── 🟢 application/                     # Casos de uso
│   │   │   ├── use_cases/                      # Un archivo por caso de uso
│   │   │   └── dtos/                           # Objetos de transferencia
│   │   │
│   │   ├── 🟡 infrastructure/                  # Implementaciones
│   │   │   ├── repositories/                   # Implementan IRepository
│   │   │   ├── database/                       # Conexión, migraciones
│   │   │   └── external_services/              # Google, email, etc
│   │   │
│   │   ├── 🔴 presentation/                    # HTTP/Controllers
│   │   │   ├── controllers/                    # Endpoints
│   │   │   ├── middleware/                     # Auth, logging
│   │   │   └── routes/                         # Mapeo de rutas
│   │   │
│   │   └── 🟣 shared/                          # Código compartido
│   │       ├── utils/
│   │       ├── constants/
│   │       └── errors/
│   │
│   └── tests/
│       ├── unit/                               # Tests unitarios
│       └── integration/                        # Tests de integración
│
├── 📁 frontend/
│   ├── src/
│   │   ├── 🔵 domain/                          # Entidades del negocio
│   │   │   └── entities/
│   │   │
│   │   ├── 🟢 application/                     # Casos de uso
│   │   │   └── use_cases/
│   │   │
│   │   ├── 🟡 infrastructure/                  # API, Storage
│   │   │   ├── api/                            # Llamadas HTTP
│   │   │   └── storage/                        # LocalStorage, etc
│   │   │
│   │   ├── 🔴 presentation/                    # UI
│   │   │   ├── components/                     # Componentes React
│   │   │   ├── pages/                          # Páginas
│   │   │   ├── hooks/                          # Custom hooks
│   │   │   └── styles/
│   │   │
│   │   └── 🟣 shared/                          # Código compartido
│   │
│   └── public/
│
└── 📁 documentacion/
```

## 🔄 Flujo de Datos en Clean Architecture

```
HTTP Request → Controller → Use Case → Repositories → Database
                                   ↓
                            Entities (Business Rules)
                                   ↓
                         Response DTO
                                   ↓
                           HTTP Response
```

## ✅ Ventajas de Clean Architecture

- ✓ **Testeable**: Cada capa se puede testear independientemente
- ✓ **Escalable**: Fácil agregar nuevas funcionalidades
- ✓ **Mantenible**: Código organizado y claro
- ✓ **Flexible**: Cambiar frameworks sin afectar lógica
- ✓ **Independencia**: No depende de decisiones técnicas tempranas
- ✓ **Reutilizable**: Use Cases pueden usarse desde Web, APIs, CLI, etc

## 🎓 Ejemplo Práctico: Crear un Usuario

```
1. HTTP POST /usuarios
2. Controller recibe request
3. Controller valida formato y llama CrearUsuarioUseCase
4. Use Case: 
   - Valida datos de negocio
   - Crea instancia de Usuario (Entidad Domain)
   - Llama UsuarioRepository (interfaz del Domain)
5. Repository (Infrastructure):
   - Implementa la interfaz
   - Guarda en BD
   - Retorna Usuario
6. Use Case retorna UsuarioResponseDTO
7. Controller retorna JSON al cliente
```

## 📚 Referencias

- Clean Architecture por Robert C. Martin
- The Clean Code Blog
- Clean Architecture in Node.js/TypeScript tutorials
