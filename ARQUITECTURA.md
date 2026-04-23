# Arquitectura Clean Architecture - Ruta 12

## 📋 Resumen General

**⚠️ IMPORTANTE**: Este proyecto utiliza **Clean Architecture** propuesta por Robert C. Martin.

Esta arquitectura es superior a una arquitectura de capas tradicional porque:

- ✓ La lógica de negocio es **completamente independiente** de frameworks
- ✓ Es **altamente testeable** sin necesidad de BD, UI, o APIs
- ✓ **Flexible para cambios**: Cambiar BD, framework o UI es trivial
- ✓ **Escalable**: Perfecto para proyectos que crecen

## 🎯 Principios Clave

1. **Independencia de Frameworks**: El código no depende de Express, React, etc.
2. **Testeable**: La lógica de negocio se prueba sin UI, BD o acceso externo
3. **Independencia de UI**: Cambiar de web a mobile no afecta la lógica
4. **Independencia de BD**: La lógica central no conoce qué BD se usa
5. **Independencia de Agencias Externas**: No depende de APIs externas

## 🏗️ Estructura

```
Ruta-12-Sofware-de-ubicacion/
│
├── 📁 backend/ (Node.js/Express/TypeScript)
│   ├── src/
│   │   ├── 🔵 domain/                   # NÚCLEO: Reglas de negocio puras
│   │   │   ├── entities/                # Entidades del dominio
│   │   │   └── interfaces/              # Contratos
│   │   │
│   │   ├── 🟢 application/              # Casos de uso
│   │   │   ├── use_cases/               # Un archivo por caso de uso
│   │   │   └── dtos/                    # Objetos de transferencia
│   │   │
│   │   ├── 🟡 infrastructure/           # Implementaciones externas
│   │   │   ├── repositories/            # Acceso a datos
│   │   │   ├── database/                # Conexión, migraciones
│   │   │   └── external_services/       # APIs, email, etc.
│   │   │
│   │   ├── 🔴 presentation/             # HTTP/Controllers
│   │   │   ├── controllers/
│   │   │   ├── middleware/
│   │   │   └── routes/
│   │   │
│   │   └── 🟣 shared/                   # Utilidades compartidas
│   │
│   └── tests/
│
├── 📁 frontend/ (React/TypeScript)
│   ├── src/
│   │   ├── 🔵 domain/                   # Entidades de negocio
│   │   ├── 🟢 application/              # Casos de uso
│   │   ├── 🟡 infrastructure/           # API, almacenamiento
│   │   ├── 🔴 presentation/             # React components
│   │   └── 🟣 shared/                   # Utilities
│   │
│   └── public/
│
├── 📁 documentacion/
│
├── CLEAN_ARCHITECTURE.md                # Guía completa
├── ARQUITECTURA.md                      # Este archivo
└── README.md
```

## 🔄 Regla de Dependencia (CRÍTICA)

```
Las dependencias SIEMPRE apuntan HACIA ADENTRO

Domain ← Application ← Infrastructure ← Presentation

Domain NUNCA depende de ninguna otra capa
```

## 📊 Flujo de Datos en Clean Architecture

```
HTTP Request
    ↓
Presentation (Controller)
    ↓
Application (Use Case) ← Orquesta todo
    ↓
Domain (Entities) ← Lógica pura de negocio
    ↓
Infrastructure (Repository) ← Accede a BD
    ↓
Base de Datos
    │
    ↓ (Retorna datos)
    ↓
Presentation (DTO)
    ↓
HTTP Response (JSON)
```

## 🎓 Ejemplo Práctico

### Crear un Usuario

**Frontend:**
1. Usuario completa formulario
2. Component llama `ObtenerUsuariosUseCase`
3. Use Case llama `UsuarioAPI.crear()`
4. API hace POST a backend

**Backend:**
1. Controller recibe POST /usuarios
2. Llama `CrearUsuarioUseCase`
3. Use Case:
   - Valida entidad Usuario (reglas de negocio)
   - Llama `IUsuarioRepository.guardar()`
4. Repository:
   - Implementa `IUsuarioRepository`
   - Guarda en Base de Datos
5. Retorna UsuarioResponseDTO
6. Controller retorna JSON

## 🎯 Capas Resumidas

| Capa | Responsabilidad | Ejemplo |
|------|-----------------|---------|
| 🔵 Domain | Reglas de negocio puras | Validar email único |
| 🟢 Application | Orquestar domain + infra | CrearUsuarioUseCase |
| 🟡 Infrastructure | Implementaciones externas | UsuarioRepository, EmailService |
| 🔴 Presentation | HTTP/UI | UsuarioController, UsuarioPage |
| 🟣 Shared | Código compartido | Validadores, formatos |

## ✅ Ventajas de Clean Architecture

✓ **Código testeable**: Tests unitarios sin BD, sin framework
✓ **Lógica protegida**: Las reglas de negocio nunca cambian
✓ **Flexible**: Cambiar BD/framework es fácil
✓ **Escalable**: Agregar features es adicionar Use Cases
✓ **Profesional**: Usado en proyectos grandes y exitosos
✓ **Independencia**: Desarrollo paralelo de frontend/backend

## 📖 Documentación Completa

👉 Ver [CLEAN_ARCHITECTURE.md](CLEAN_ARCHITECTURE.md) para documentación detallada

👉 Ver [backend/README.md](backend/README.md) para guía del backend

👉 Ver [frontend/README.md](frontend/README.md) para guía del frontend

## 🚀 Próximos Pasos

1. ✅ Arquitectura creada
2. ⏭️ Configurar `package.json` en backend y frontend
3. ⏭️ Crear primeras entidades en Domain
4. ⏭️ Crear primeros Use Cases
5. ⏭️ Implementar Repositories
6. ⏭️ Crear Controllers
7. ⏭️ Escribir tests unitarios
