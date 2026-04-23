# Backend - Clean Architecture

Este es el servidor backend del sistema de ubicación, desarrollado con **Clean Architecture**.

## 📁 Estructura del Backend

```
backend/
├── src/
│   ├── domain/                          # Capa de dominio (reglas puras)
│   │   ├── entities/                    # Entidades del negocio
│   │   └── interfaces/                  # Contratos
│   │
│   ├── application/                     # Capa de aplicación (casos de uso)
│   │   ├── use_cases/                   # Casos de uso
│   │   └── dtos/                        # Objetos de transferencia
│   │
│   ├── infrastructure/                  # Capa de infraestructura
│   │   ├── repositories/                # Implementaciones de repositorios
│   │   ├── database/                    # BD, migraciones
│   │   └── external_services/           # Servicios externos
│   │
│   ├── presentation/                    # Capa de presentación (HTTP)
│   │   ├── controllers/                 # Controllers REST
│   │   ├── middleware/                  # Middleware
│   │   └── routes/                      # Rutas
│   │
│   └── shared/                          # Código compartido
│       ├── utils/
│       ├── constants/
│       └── errors/
│
├── tests/                               # Tests unitarios e integración
│   ├── unit/
│   └── integration/
│
└── README.md
```

## 🚀 Primeros Pasos

### Instalación

```bash
npm install
```

### Configuración

1. Crear archivo `.env` en la raíz del backend:

```env
NODE_ENV=development
PORT=3000
DATABASE_URL=your_database_url
JWT_SECRET=your_secret_key
```

### Ejecutar en Desarrollo

```bash
npm run dev
```

### Tests

```bash
# Tests unitarios
npm run test:unit

# Tests de integración
npm run test:integration

# Todos los tests
npm test
```

## 📚 Capas Explicadas

### 🔵 Domain (Núcleo)
- Contiene entidades y reglas de negocio puras
- NO tiene dependencias externas
- Define interfaces que otros usan

### 🟢 Application (Orquestación)
- Implementa casos de uso
- Orquesta Domain e Infrastructure
- Transforma DTOs

### 🟡 Infrastructure (Acceso Externo)
- Implementa interfaces del Domain
- Acceso a BD, APIs externas
- Adaptadores tecnológicos

### 🔴 Presentation (HTTP)
- Controllers y rutas
- Middleware
- NO contiene lógica de negocio

### 🟣 Shared (Común)
- Código reutilizable
- Utilidades, constantes, errores

## 📝 Estructura de un Use Case

```typescript
// application/use_cases/CrearUsuarioUseCase.ts
export class CrearUsuarioUseCase {
  constructor(private usuarioRepository: IUsuarioRepository) {}
  
  async execute(dto: CrearUsuarioDTO): Promise<UsuarioResponseDTO> {
    // Validaciones de negocio
    // Crear entidad Usuario
    // Guardar usando repository
    // Retornar DTO
  }
}
```

## 🔌 Ejemplo: Crear un Endpoint

```typescript
// domain/entities/Usuario.ts
export class Usuario {
  constructor(nombre: string, email: string) {
    // Validaciones de negocio
  }
}

// application/dtos/CrearUsuarioDTO.ts
export interface CrearUsuarioDTO {
  nombre: string;
  email: string;
}

// infrastructure/repositories/UsuarioRepository.ts
export class UsuarioRepository implements IUsuarioRepository {
  async guardar(usuario: Usuario): Promise<Usuario> {
    // Lógica de BD
  }
}

// presentation/controllers/UsuarioController.ts
export class UsuarioController {
  constructor(private crearUsuarioUseCase: CrearUsuarioUseCase) {}
  
  async crear(req: Request, res: Response) {
    const resultado = await this.crearUsuarioUseCase.execute(req.body);
    res.json(resultado);
  }
}
```

## 🧪 Testing

Clean Architecture facilita el testing:

```typescript
// tests/unit/CrearUsuarioUseCase.test.ts
describe('CrearUsuarioUseCase', () => {
  it('debe crear un usuario válido', async () => {
    const mockRepository = {
      guardar: jest.fn()
    };
    
    const useCase = new CrearUsuarioUseCase(mockRepository);
    const resultado = await useCase.execute({
      nombre: 'Juan',
      email: 'juan@example.com'
    });
    
    expect(mockRepository.guardar).toHaveBeenCalled();
  });
});
```

## 📖 Documentación

Ver [CLEAN_ARCHITECTURE.md](../CLEAN_ARCHITECTURE.md) para más detalles.
