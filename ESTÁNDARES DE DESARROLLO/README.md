# Estándares de Desarrollo API-Criminalistica

**Versión:** 1.0  
**Última actualización:** 2026-05-04  
**Autor:** API-Criminalistica Team

---

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Estándares de Código](#estándares-de-código)
3. [Arquitectura](#arquitectura)
4. [API REST](#api-rest)
5. [Manejo de Errores](#manejo-de-errores)
6. [Control de Versiones](#control-de-versiones)
7. [Pruebas](#pruebas)
8. [Documentación](#documentación)

---

## Introducción

Este documento establece los estándares de desarrollo que todo miembro del equipo de **API-Criminalistica** debe seguir. El objetivo es garantizar:

- **Consistencia** en el código
- **Mantenibilidad** a largo plazo
- **Escalabilidad** del proyecto
- **Calidad** en la entrega
- **Colaboración** eficiente entre equipos

---

## Estándares de Código

### Convenciones de Nombres

#### Backend (C#)

| Elemento | Convención | Ejemplo |
|----------|-----------|---------|
| **Clases** | PascalCase | `CriminalCase`, `UserService` |
| **Interfaces** | PascalCase con prefijo `I` | `ICriminalRepository`, `IAuthService` |
| **Métodos** | PascalCase con verbo | `GetCriminalById()`, `CreateCase()`, `UpdateStatus()` |
| **Variables locales** | camelCase | `criminalId`, `caseNumber` |
| **Propiedades** | PascalCase | `CriminalId { get; set; }` |
| **Constantes** | UPPER_SNAKE_CASE | `MAX_RECORDS = 1000`, `DB_TIMEOUT = 30` |
| **Privadas** | camelCase con prefijo `_` | `_logger`, `_repository` |

#### Frontend (TypeScript)

| Elemento | Convención | Ejemplo |
|----------|-----------|---------|
| **Componentes** | PascalCase | `CriminalList`, `CaseForm` |
| **Interfaces/Types** | PascalCase | `ICriminal`, `CaseDetails` |
| **Funciones** | camelCase | `getCriminalData()`, `formatDate()` |
| **Variables** | camelCase | `itemCount`, `isLoading` |
| **Constantes** | UPPER_SNAKE_CASE | `API_BASE_URL`, `MAX_ITEMS` |

### Métodos - Verbos Recomendados

```
GET     → GetXXX()           (Obtener datos)
POST    → CreateXXX()        (Crear nuevo)
PUT     → UpdateXXX()        (Actualizar completo)
PATCH   → UpdateXXX()        (Actualización parcial)
DELETE  → DeleteXXX()        (Eliminar)
SEARCH  → SearchXXX()        (Búsqueda)
LIST    → GetAllXXX()        (Listar)
```

### Reglas Generales de Código

- **Longitud de línea:** Máximo 120 caracteres
- **Indentación:** 4 espacios (nunca tabs)
- **Comentarios:** Solo para lógica compleja, no para código obvio
- **No usar:** Números mágicos, variables ambiguas
- **DRY:** Don't Repeat Yourself - reutiliza código

---

## Arquitectura

### Patrón de Capas

```
┌─────────────────────────────────────┐
│     API REST / Controladores        │
│     (Controllers)                   │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│     Servicios / Lógica de Negocio   │
│     (Services)                      │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│     Acceso a Datos                  │
│     (Repositories)                  │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│     Base de Datos / Recursos        │
│     (Database / APIs Externas)      │
└─────────────────────────────────────┘
```

### Responsabilidades por Capa

#### 1. **Controllers (Entrada/Salida)**
- Recibir solicitudes HTTP
- Validar parámetros básicos
- Llamar a servicios
- Devolver respuestas formatadas
- NO implementar lógica de negocio

**Ejemplo:**
```csharp
[ApiController]
[Route("api/[controller]")]
public class CriminalController : ControllerBase
{
    private readonly ICriminalService _service;
    
    public CriminalController(ICriminalService service)
    {
        _service = service;
    }
    
    [HttpGet("{id}")]
    public async Task<IActionResult> GetCriminal(int id)
    {
        var criminal = await _service.GetCriminalByIdAsync(id);
        return Ok(criminal);
    }
}
```

#### 2. **Services (Lógica de Negocio)**
- Implementar reglas de negocio
- Coordinar operaciones
- Manejo de transacciones
- Validaciones complejas
- NO acceder directo a BD

**Ejemplo:**
```csharp
public class CriminalService : ICriminalService
{
    private readonly ICriminalRepository _repository;
    private readonly ILogger<CriminalService> _logger;
    
    public async Task<CriminalDTO> GetCriminalByIdAsync(int id)
    {
        if (id <= 0)
            throw new ValidationException("ID debe ser mayor a 0");
        
        var criminal = await _repository.GetByIdAsync(id);
        if (criminal == null)
            throw new NotFoundException("Criminal no encontrado");
        
        return MapToDTOAsync(criminal);
    }
}
```

#### 3. **Repositories (Acceso a Datos)**
- Operaciones CRUD
- Consultas a BD
- Mapeo de entidades
- NO contener lógica de negocio

**Ejemplo:**
```csharp
public class CriminalRepository : ICriminalRepository
{
    private readonly ApiDbContext _context;
    
    public async Task<Criminal> GetByIdAsync(int id)
    {
        return await _context.Criminals.FindAsync(id);
    }
    
    public async Task<Criminal> CreateAsync(Criminal criminal)
    {
        _context.Criminals.Add(criminal);
        await _context.SaveChangesAsync();
        return criminal;
    }
}
```

---

## API REST

### Convención de Endpoints

```
GET     /api/recursos              (Listar todos)
GET     /api/recursos/{id}         (Obtener uno)
POST    /api/recursos              (Crear nuevo)
PUT     /api/recursos/{id}         (Actualizar completo)
PATCH   /api/recursos/{id}         (Actualización parcial)
DELETE  /api/recursos/{id}         (Eliminar)
GET     /api/recursos/search       (Búsqueda avanzada)
```

### Ejemplo Completo: Delitos

```
GET     /api/delitos               → Listar todos
GET     /api/delitos/{id}          → Obtener un delito
GET     /api/delitos/search        → Buscar delitos
POST    /api/delitos               → Crear delito
PUT     /api/delitos/{id}          → Actualizar delito
PATCH   /api/delitos/{id}          → Actualizar parcialmente
DELETE  /api/delitos/{id}          → Eliminar delito
```

### Códigos de Estado HTTP

| Código | Significado | Casos de Uso |
|--------|-------------|--------------|
| **200** | OK | Solicitud exitosa |
| **201** | Created | Recurso creado exitosamente |
| **204** | No Content | Eliminación exitosa (sin cuerpo) |
| **400** | Bad Request | Validación fallida, parámetros inválidos |
| **401** | Unauthorized | No autenticado |
| **403** | Forbidden | No autorizado |
| **404** | Not Found | Recurso no encontrado |
| **409** | Conflict | Conflicto (ej: duplicado) |
| **500** | Internal Server Error | Error del servidor |
| **503** | Service Unavailable | Servicio no disponible |

### Estructura de Respuestas

#### Respuesta Exitosa (200/201)
```json
{
  "success": true,
  "data": {
    "id": 1,
    "nombre": "Homicidio",
    "descripcion": "Delito grave",
    "fecha": "2026-05-04T10:30:00Z"
  },
  "message": "Operación completada exitosamente"
}
```

#### Respuesta Paginada (200)
```json
{
  "success": true,
  "data": [
    { "id": 1, "nombre": "Delito 1" },
    { "id": 2, "nombre": "Delito 2" }
  ],
  "pagination": {
    "page": 1,
    "pageSize": 10,
    "total": 25,
    "pages": 3
  }
}
```

---

## Manejo de Errores

### Formato Estándar de Errores

```json
{
  "success": false,
  "status": 400,
  "message": "Error de validación",
  "details": "El campo fecha es obligatorio",
  "timestamp": "2026-05-04T10:30:00Z",
  "traceId": "0HN2F8DVGFMQG:00000001"
}
```

### Ejemplos por Tipo de Error

#### Validación (400)
```json
{
  "success": false,
  "status": 400,
  "message": "Error de validación",
  "details": "El campo 'email' debe ser un email válido",
  "errors": [
    {
      "field": "email",
      "message": "Formato inválido"
    },
    {
      "field": "fechaNacimiento",
      "message": "No puede ser futura"
    }
  ]
}
```

#### No Encontrado (404)
```json
{
  "success": false,
  "status": 404,
  "message": "Recurso no encontrado",
  "details": "El criminal con ID 999 no existe"
}
```

#### No Autorizado (401)
```json
{
  "success": false,
  "status": 401,
  "message": "No autenticado",
  "details": "Token inválido o expirado"
}
```

#### Error del Servidor (500)
```json
{
  "success": false,
  "status": 500,
  "message": "Error interno del servidor",
  "details": "Contacte al administrador",
  "traceId": "0HN2F8DVGFMQG:00000001"
}
```

### Política de Excepciones (C#)

```csharp
// Crear excepciones personalizadas
public class ValidationException : Exception
{
    public ValidationException(string message) : base(message) { }
}

public class NotFoundException : Exception
{
    public NotFoundException(string message) : base(message) { }
}

// En controlador
try
{
    var result = await _service.GetCriminalAsync(id);
    return Ok(new ApiResponse { Success = true, Data = result });
}
catch (ValidationException ex)
{
    return BadRequest(new ApiError { Status = 400, Message = ex.Message });
}
catch (NotFoundException ex)
{
    return NotFound(new ApiError { Status = 404, Message = ex.Message });
}
catch (Exception ex)
{
    _logger.LogError(ex, "Error no manejado");
    return StatusCode(500, new ApiError { Status = 500, Message = "Error interno" });
}
```

---

## Control de Versiones

### Estructura de Ramas

```
main/master
  ├── production (versión en producción)
  ├── develop (desarrollo integrado)
  └── release (preparación de releases)

features
  ├── feature/criminal-module
  ├── feature/auth-system
  └── feature/search-filter

fixes
  ├── bugfix/fix-login-issue
  ├── bugfix/null-reference-error
  └── hotfix/security-patch
```

### Nomenclatura de Ramas

```
feature/nombre-corto          (nuevas funcionalidades)
bugfix/problema-corto         (corrección de bugs)
hotfix/problema-critico       (corrección urgente)
refactor/mejora-codigo        (mejoras de código)
docs/actualizacion            (cambios de documentación)
```

### Commits

#### Formato de Mensaje
```
<tipo>(<alcance>): <descripción corta>

<descripción detallada>

<referencias>
```

#### Tipos de Commits
```
feat     - Nueva funcionalidad
fix      - Corrección de bug
refactor - Cambios sin alterar funcionalidad
docs     - Cambios de documentación
test     - Agregar/modificar tests
chore    - Cambios en herramientas, deps
```

#### Ejemplos
```
feat(criminal): agregar búsqueda por nombre

Implementa búsqueda case-insensitive de criminales
- Validar entrada
- Paginar resultados
- Ordenar por relevancia

Fixes #123
```

---

## Pruebas

### Cobertura Mínima

- **Servicios:** 80% cobertura
- **Repositories:** 70% cobertura
- **Controllers:** 60% cobertura

### Estructura de Tests

```
Tests/
├── UnitTests/
│   ├── Services/
│   │   └── CriminalServiceTests.cs
│   ├── Repositories/
│   │   └── CriminalRepositoryTests.cs
│   └── Controllers/
│       └── CriminalControllerTests.cs
└── IntegrationTests/
    └── CriminalApiTests.cs
```

### Nomenclatura de Tests

```csharp
[Fact]
public void GetCriminal_WithValidId_ReturnsOk()
{
    // Arrange
    int id = 1;
    
    // Act
    var result = _service.GetCriminalById(id);
    
    // Assert
    Assert.NotNull(result);
}
```

---

##  Documentación

### XML Comments (C#)

```csharp
/// <summary>
/// Obtiene un criminal por su ID
/// </summary>
/// <param name="id">ID del criminal a buscar</param>
/// <returns>Datos del criminal encontrado</returns>
/// <exception cref="NotFoundException">Si el criminal no existe</exception>
public async Task<CriminalDTO> GetCriminalByIdAsync(int id)
{
    // implementación
}
```

### README en Repositorios

Todo repositorio debe incluir:

- Descripción del proyecto
- Requisitos previos
- Instalación
- Configuración
- Uso/Ejemplos
- Estructura del proyecto

---

## Checklist de Calidad

Antes de hacer commit/push:

- [ ] Código sigue convenciones de nombres
- [ ] Funciones/métodos tienen responsabilidad única
- [ ] Manejo de errores implementado
- [ ] Tests unitarios escritos
- [ ] Documentación actualizada
- [ ] Sin código comentado o eliminado
- [ ] Sin valores hardcodeados
- [ ] Logs apropiados
- [ ] Código formateado correctamente
- [ ] Revisión de pares completada

---

**Última revisión:** 2026-05-04  
**Próxima revisión:** 2026-09-04
