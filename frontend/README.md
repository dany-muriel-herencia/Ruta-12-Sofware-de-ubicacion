# Frontend - Clean Architecture

Este es el cliente frontend del sistema de ubicación, desarrollado con **Clean Architecture** adaptada a React.

## 📁 Estructura del Frontend

```
frontend/
├── src/
│   ├── domain/                          # Entidades del negocio
│   │   └── entities/                    # Modelos de datos
│   │
│   ├── application/                     # Casos de uso
│   │   └── use_cases/                   # Orquestación de lógica
│   │
│   ├── infrastructure/                  # Acceso a externos
│   │   ├── api/                         # Llamadas HTTP
│   │   └── storage/                     # LocalStorage, SessionStorage
│   │
│   ├── presentation/                    # Interfaz de usuario
│   │   ├── components/                  # Componentes React
│   │   ├── pages/                       # Páginas
│   │   ├── hooks/                       # Custom hooks
│   │   └── styles/                      # Estilos globales
│   │
│   └── shared/                          # Código compartido
│       ├── utils/
│       └── constants/
│
├── public/                              # Assets estáticos
│
└── README.md
```

## 🚀 Primeros Pasos

### Instalación

```bash
npm install
```

### Configuración

1. Crear archivo `.env` en la raíz del frontend:

```env
REACT_APP_API_URL=http://localhost:3000/api
```

### Ejecutar en Desarrollo

```bash
npm start
```

### Build para Producción

```bash
npm run build
```

## 📚 Capas Explicadas

### 🔵 Domain (Modelos)
- Entidades que representan conceptos del negocio
- Modelos de datos principales

```typescript
// domain/entities/Usuario.ts
export interface Usuario {
  id: string;
  nombre: string;
  email: string;
}
```

### 🟢 Application (Casos de Uso)
- Lógica de aplicación reutilizable
- Orquesta Domain e Infrastructure

```typescript
// application/use_cases/ObtenerUsuarios.ts
export class ObtenerUsuariosUseCase {
  constructor(private usuarioService: IUsuarioService) {}
  
  async ejecutar(): Promise<Usuario[]> {
    return await this.usuarioService.obtenerTodos();
  }
}
```

### 🟡 Infrastructure (API/Storage)
- Comunicación con backend
- Almacenamiento local

```typescript
// infrastructure/api/UsuarioAPI.ts
export class UsuarioAPI {
  async obtenerTodos(): Promise<Usuario[]> {
    const response = await fetch(`${API_URL}/usuarios`);
    return response.json();
  }
}
```

### 🔴 Presentation (React)
- Componentes visuales
- Integración con Use Cases

```typescript
// presentation/pages/UsuariosPage.tsx
export function UsuariosPage() {
  const [usuarios, setUsuarios] = useState<Usuario[]>([]);
  
  useEffect(() => {
    const useCase = new ObtenerUsuariosUseCase(new UsuarioAPI());
    useCase.ejecutar().then(setUsuarios);
  }, []);
  
  return (
    <div>
      {usuarios.map(u => <UsuarioCard key={u.id} usuario={u} />)}
    </div>
  );
}
```

### 🟣 Shared (Utilidades)
- Código reutilizable
- Constantes globales
- Funciones helpers

## 🪝 Custom Hooks

Los custom hooks en Clean Architecture encapsulan lógica de casos de uso:

```typescript
// presentation/hooks/useObtenerUsuarios.ts
export function useObtenerUsuarios() {
  const [usuarios, setUsuarios] = useState<Usuario[]>([]);
  const [loading, setLoading] = useState(false);
  
  useEffect(() => {
    setLoading(true);
    const useCase = new ObtenerUsuariosUseCase(new UsuarioAPI());
    useCase.ejecutar()
      .then(setUsuarios)
      .finally(() => setLoading(false));
  }, []);
  
  return { usuarios, loading };
}

// Uso en componentes
export function UsuariosPage() {
  const { usuarios, loading } = useObtenerUsuarios();
  
  if (loading) return <div>Cargando...</div>;
  return <div>...</div>;
}
```

## 📝 Estructura de un Componente

```typescript
// presentation/components/UsuarioCard.tsx
interface UsuarioCardProps {
  usuario: Usuario;
}

export function UsuarioCard({ usuario }: UsuarioCardProps) {
  return (
    <div className="usuario-card">
      <h3>{usuario.nombre}</h3>
      <p>{usuario.email}</p>
    </div>
  );
}
```

## 🎯 Beneficios en Frontend

- **Testeable**: Lógica separada de componentes
- **Reutilizable**: Custom hooks para lógica compartida
- **Mantenible**: Componentes enfocados en presentación
- **Escalable**: Fácil agregar nuevas funcionalidades
- **Independiente**: No depende de React internamente

## 📖 Documentación

Ver [CLEAN_ARCHITECTURE.md](../CLEAN_ARCHITECTURE.md) para más detalles.
