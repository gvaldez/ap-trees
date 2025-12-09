# 17. API REST de AgroGrid SaaS

> **Documentación completa de la API REST** para integración con el sistema AgroGrid

## Introducción

La API REST de AgroGrid SaaS proporciona acceso programático completo a todas las funcionalidades del sistema de gestión agrícola. Esta API permite:

- Gestión de fincas, sectores, lotes y árboles
- Registro y seguimiento de inspecciones fitosanitarias
- Programación de aplicaciones y cálculo de dosis
- Gestión de inventario y compras
- Registro de cosechas y análisis de producción
- Generación de reportes y exportación de datos
- Administración de usuarios, roles y permisos

## URL Base y Versionamiento

```
Base URL: https://api.agrogrid.io/api/v1
Tenant URL: https://{tenant}.agrogrid.io/api/v1
```

**Versionamiento:**
- La versión actual es `v1`
- La versión se incluye en la URL base
- Cambios compatibles no requieren cambio de versión
- Cambios incompatibles se publicarán en una nueva versión (v2, v3, etc.)
- Las versiones anteriores se mantendrán por al menos 12 meses después de una nueva versión

## Autenticación

La API utiliza **JSON Web Tokens (JWT)** para autenticación. El flujo es el siguiente:

### Flujo de Autenticación

1. **Login:** Enviar credenciales a `POST /api/auth/login`
2. **Recibir tokens:** Obtener `accessToken` (15 min) y `refreshToken` (7 días)
3. **Usar accessToken:** Incluir en cada request: `Authorization: Bearer {accessToken}`
4. **Renovar token:** Cuando expire, usar `POST /api/auth/refresh` con el refreshToken

### Headers Requeridos

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
Accept: application/json
```

### Extracción Automática del Tenant

El `tenant_id` se extrae automáticamente del JWT y no debe incluirse en el body de las peticiones. El backend aplica aislamiento multi-tenant a nivel de base de datos usando Row Level Security (RLS).

## Códigos de Respuesta Estándar

### Códigos de Éxito (2xx)

| Código | Descripción | Uso |
|--------|-------------|-----|
| `200 OK` | Petición exitosa | GET, PUT, PATCH exitosos |
| `201 Created` | Recurso creado exitosamente | POST exitoso |
| `204 No Content` | Petición exitosa sin contenido | DELETE exitoso |

### Códigos de Error del Cliente (4xx)

| Código | Descripción | Uso |
|--------|-------------|-----|
| `400 Bad Request` | Datos inválidos o incompletos | Validación fallida |
| `401 Unauthorized` | No autenticado | Token ausente o inválido |
| `403 Forbidden` | Sin permisos suficientes | Usuario sin permiso para la acción |
| `404 Not Found` | Recurso no encontrado | ID inexistente o fuera del tenant |
| `409 Conflict` | Conflicto con estado actual | Duplicado, restricción de negocio |
| `422 Unprocessable Entity` | Datos válidos pero lógica de negocio falla | Reglas de negocio no cumplidas |
| `429 Too Many Requests` | Límite de tasa excedido | Demasiadas peticiones |

### Códigos de Error del Servidor (5xx)

| Código | Descripción | Uso |
|--------|-------------|-----|
| `500 Internal Server Error` | Error interno del servidor | Error no controlado |
| `503 Service Unavailable` | Servicio temporalmente no disponible | Mantenimiento, sobrecarga |

## Formato de Errores

La API sigue el estándar **RFC 7807 Problem Details** para respuestas de error:

```json
{
  "type": "https://api.agrogrid.io/errors/validation-error",
  "title": "Validation Error",
  "status": 400,
  "detail": "Los datos proporcionados no son válidos",
  "instance": "/api/v1/fincas",
  "timestamp": "2024-01-15T10:30:00Z",
  "errors": [
    {
      "field": "nombre",
      "message": "El nombre de la finca es requerido",
      "code": "required"
    },
    {
      "field": "area_total",
      "message": "El área debe ser mayor a 0",
      "code": "min_value"
    }
  ]
}
```

### Campos del Error

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `type` | string | URI que identifica el tipo de error |
| `title` | string | Resumen corto del problema |
| `status` | integer | Código de estado HTTP |
| `detail` | string | Explicación detallada del error |
| `instance` | string | URI de la petición que causó el error |
| `timestamp` | string | Fecha y hora del error (ISO 8601) |
| `errors` | array | Lista de errores específicos (validación) |

## Paginación Estándar

Todos los endpoints que retornan listas soportan paginación consistente:

### Query Parameters

| Parámetro | Tipo | Default | Descripción |
|-----------|------|---------|-------------|
| `page` | integer | 1 | Número de página (inicia en 1) |
| `size` | integer | 20 | Elementos por página (max: 100) |
| `sort` | string | - | Campo y dirección: `campo,asc` o `campo,desc` |

### Ejemplo de Request

```http
GET /api/v1/arboles?page=2&size=50&sort=codigo,asc
Authorization: Bearer {token}
```

### Formato de Respuesta Paginada

```json
{
  "data": [
    {
      "id": 123,
      "codigo": "A-001",
      ...
    },
    ...
  ],
  "meta": {
    "page": 2,
    "size": 50,
    "total_elements": 1250,
    "total_pages": 25,
    "has_next": true,
    "has_previous": true
  },
  "links": {
    "self": "/api/v1/arboles?page=2&size=50&sort=codigo,asc",
    "first": "/api/v1/arboles?page=1&size=50&sort=codigo,asc",
    "prev": "/api/v1/arboles?page=1&size=50&sort=codigo,asc",
    "next": "/api/v1/arboles?page=3&size=50&sort=codigo,asc",
    "last": "/api/v1/arboles?page=25&size=50&sort=codigo,asc"
  }
}
```

## Filtrado y Búsqueda

Los endpoints de listado soportan filtros mediante query parameters:

```http
GET /api/v1/arboles?finca_id=5&estado=SAL&cultivo=AGU
GET /api/v1/aplicaciones?fecha_desde=2024-01-01&fecha_hasta=2024-01-31&estado=completada
GET /api/v1/usuarios?buscar=juan&activo=true
```

## Rate Limiting

La API implementa límites de tasa para proteger el servicio:

| Plan | Requests por minuto | Requests por hora |
|------|---------------------|-------------------|
| Starter | 60 | 1,000 |
| Professional | 120 | 5,000 |
| Enterprise | 300 | 20,000 |

Los headers de respuesta incluyen información del límite:

```http
X-RateLimit-Limit: 120
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1705320600
```

Cuando se excede el límite, se retorna `429 Too Many Requests`:

```json
{
  "type": "https://api.agrogrid.io/errors/rate-limit-exceeded",
  "title": "Rate Limit Exceeded",
  "status": 429,
  "detail": "Ha excedido el límite de 120 requests por minuto",
  "retry_after": 45
}
```

## Webhooks

AgroGrid puede enviar notificaciones webhook para eventos importantes:

- Cambios de estado en árboles
- Aplicaciones completadas
- Alertas de plagas/enfermedades
- Cosechas registradas
- Límites de inventario alcanzados

Ver documentación detallada en: [Webhooks](./api/17-webhooks.md) _(próximamente)_

## Índice de Documentación por Dominio

### Autenticación y Seguridad
- [**01. Autenticación**](./api/01-autenticacion.md) - Login, logout, refresh token, recuperación de contraseña

### Gestión Multi-Tenant y Usuarios
- [**02. Tenants**](./api/02-tenants.md) - Gestión de tenants (admin de plataforma)
- [**03. Usuarios y Roles**](./api/03-usuarios-roles.md) - CRUD usuarios, roles y permisos

### Estructura del Campo
- [**04. Fincas**](./api/04-fincas.md) - Gestión de fincas
- [**05. Sectores**](./api/05-sectores.md) - Gestión de sectores
- [**06. Lotes**](./api/06-lotes.md) - Gestión de lotes
- [**07. Árboles**](./api/07-arboles.md) - Gestión de árboles individuales

### Operaciones de Campo
- [**08. Inspecciones**](./api/08-inspecciones.md) - Inspecciones fitosanitarias y fenológicas
- [**09. Aplicaciones**](./api/09-aplicaciones.md) - Aplicaciones fitosanitarias y cálculo de dosis
- [**10. Riego**](./api/10-riego.md) - Infraestructura y programación de riego
- [**11. Inventario y Compras**](./api/11-inventario-compras.md) - Gestión de insumos y órdenes de compra
- [**12. Cosecha**](./api/12-cosecha.md) - Registro de cosechas y producción

### Planificación y Análisis
- [**13. Tareas y Planificación**](./api/13-tareas-planificacion.md) - Gestión de tareas y calendario
- [**14. Reportes**](./api/14-reportes.md) - Generación de reportes y exportación

### Catálogos y Configuración
- [**15. Catálogos**](./api/15-catalogos.md) - Catálogos de cultivos, plagas, productos, etc.
- [**16. Administración de Plataforma**](./api/16-admin-plataforma.md) - Gestión de planes, suscripciones y configuración

## SDKs y Librerías

Próximamente disponibles:
- JavaScript/TypeScript SDK
- Python SDK
- Java SDK
- CLI Tool

## Soporte y Recursos

- **Documentación Interactiva:** https://api.agrogrid.io/swagger-ui
- **Postman Collection:** [Descargar](https://api.agrogrid.io/postman)
- **Portal de Desarrolladores:** https://developers.agrogrid.io
- **Soporte Técnico:** api-support@agrogrid.io
- **Status de API:** https://status.agrogrid.io

## Changelog

### v1.0.0 (2024-01-15)
- Lanzamiento inicial de la API REST
- Endpoints completos para todos los módulos
- Documentación completa
- Autenticación JWT
- Rate limiting por plan

---

> **Nota:** Esta documentación está en constante actualización. La versión más reciente siempre está disponible en https://docs.agrogrid.io
