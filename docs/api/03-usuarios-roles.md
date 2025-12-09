# API - Usuarios y Roles

> Endpoints para gestión de usuarios, roles y permisos dentro de un tenant

## Resumen

Estos endpoints permiten administrar usuarios del tenant, asignar roles y gestionar permisos granulares. Los usuarios solo pueden gestionar usuarios dentro de su propio tenant.

---

## Endpoints de Usuarios

### `GET /api/v1/usuarios`

Lista usuarios del tenant actual.

**Autenticación:** Requerida

**Permisos:** `admin:usuarios`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| page | integer | No | Número de página (default: 1) |
| size | integer | No | Elementos por página (default: 20, max: 100) |
| sort | string | No | Campo de ordenamiento: `nombre,asc`, `email,asc` |
| activo | boolean | No | Filtrar por estado activo |
| rol_id | integer | No | Filtrar por rol |
| finca_id | integer | No | Filtrar por finca asignada |
| buscar | string | No | Búsqueda por nombre o email |

**Response 200:**
```json
{
  "data": [
    {
      "id": 42,
      "email": "juan.perez@finca.com",
      "nombre": "Juan",
      "apellido": "Pérez",
      "telefono": "+57 310 123 4567",
      "avatar_url": "https://storage.agrogrid.io/avatars/42.jpg",
      "roles": [
        {
          "id": 5,
          "nombre": "ROLE_TENANT_MANAGER",
          "descripcion": "Gerente con acceso completo",
          "finca_id": null
        }
      ],
      "finca_principal_id": 5,
      "activo": true,
      "email_verificado": true,
      "ultimo_login": "2024-01-15T10:30:00Z",
      "created_at": "2023-12-01T00:00:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "size": 20,
    "total_elements": 8,
    "total_pages": 1
  }
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos

---

### `POST /api/v1/usuarios`

Crea un nuevo usuario en el tenant.

**Autenticación:** Requerida

**Permisos:** `admin:usuarios`

**Request Body:**
```json
{
  "email": "maria.lopez@finca.com",
  "nombre": "María",
  "apellido": "López",
  "telefono": "+57 310 987 6543",
  "password": "TempPassword123!",
  "roles": [6],
  "finca_id": 5,
  "enviar_email_bienvenida": true,
  "requiere_cambio_password": true
}
```

**Response 201:**
```json
{
  "id": 43,
  "email": "maria.lopez@finca.com",
  "nombre": "María",
  "apellido": "López",
  "activo": true,
  "email_verificado": false,
  "requiere_cambio_password": true,
  "created_at": "2024-01-15T12:00:00Z"
}
```

**Códigos de error:**
- `400` - Datos inválidos
- `401` - No autenticado
- `403` - Sin permisos o límite de usuarios alcanzado
- `409` - Email ya existe

---

### `GET /api/v1/usuarios/{id}`

Obtiene detalles de un usuario.

**Autenticación:** Requerida

**Permisos:** `admin:usuarios` o mismo usuario

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | integer | ID del usuario |

**Response 200:**
```json
{
  "id": 42,
  "email": "juan.perez@finca.com",
  "nombre": "Juan",
  "apellido": "Pérez",
  "telefono": "+57 310 123 4567",
  "avatar_url": "https://storage.agrogrid.io/avatars/42.jpg",
  "roles": [
    {
      "id": 5,
      "nombre": "ROLE_TENANT_MANAGER",
      "descripcion": "Gerente con acceso completo",
      "finca_id": null,
      "fecha_asignacion": "2023-12-01T00:00:00Z"
    }
  ],
  "permisos": [
    "fincas:leer",
    "fincas:actualizar",
    "arboles:crear",
    "arboles:actualizar",
    "inspecciones:crear",
    "aplicaciones:aprobar"
  ],
  "finca_principal_id": 5,
  "activo": true,
  "email_verificado": true,
  "ultimo_login": "2024-01-15T10:30:00Z",
  "ultimo_cambio_password": "2024-01-01T00:00:00Z",
  "requiere_cambio_password": false,
  "created_at": "2023-12-01T00:00:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Usuario no encontrado

---

### `PUT /api/v1/usuarios/{id}`

Actualiza información de un usuario.

**Autenticación:** Requerida

**Permisos:** `admin:usuarios` o mismo usuario (campos limitados)

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | integer | ID del usuario |

**Request Body:**
```json
{
  "nombre": "Juan Carlos",
  "apellido": "Pérez García",
  "telefono": "+57 310 111 2222",
  "finca_id": 5
}
```

**Response 200:**
```json
{
  "id": 42,
  "nombre": "Juan Carlos",
  "apellido": "Pérez García",
  "telefono": "+57 310 111 2222",
  "updated_at": "2024-01-15T12:30:00Z"
}
```

**Códigos de error:**
- `400` - Datos inválidos
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Usuario no encontrado

**Notas:**
- Los usuarios solo pueden actualizar sus propios datos personales (nombre, teléfono, avatar)
- Solo admins pueden cambiar roles, estado activo, finca asignada

---

### `DELETE /api/v1/usuarios/{id}`

Desactiva (soft delete) un usuario.

**Autenticación:** Requerida

**Permisos:** `admin:usuarios`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | integer | ID del usuario |

**Response 204:** Sin contenido

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Usuario no encontrado
- `409` - No se puede eliminar el último owner del tenant

**Notas:**
- No se elimina físicamente, solo se marca como inactivo
- Las sesiones activas se invalidan
- Los registros históricos se mantienen

---

### `PUT /api/v1/usuarios/{id}/avatar`

Actualiza la foto de perfil del usuario.

**Autenticación:** Requerida

**Permisos:** Mismo usuario o `admin:usuarios`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | integer | ID del usuario |

**Request Body:** `multipart/form-data`
```
avatar: [archivo de imagen]
```

**Response 200:**
```json
{
  "avatar_url": "https://storage.agrogrid.io/avatars/42.jpg",
  "updated_at": "2024-01-15T12:45:00Z"
}
```

**Códigos de error:**
- `400` - Archivo inválido (formato, tamaño)
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Usuario no encontrado

**Requisitos:**
- Formatos: JPG, PNG, WebP
- Tamaño máximo: 2 MB
- Dimensiones recomendadas: 256x256 px

---

## Endpoints de Roles

### `GET /api/v1/roles`

Lista roles disponibles en el tenant.

**Autenticación:** Requerida

**Permisos:** `admin:roles`

**Response 200:**
```json
{
  "data": [
    {
      "id": 5,
      "nombre": "ROLE_TENANT_MANAGER",
      "descripcion": "Gerente con acceso completo",
      "nivel": "tenant",
      "permisos": [
        {
          "id": 1,
          "modulo": "fincas",
          "accion": "leer",
          "nombre": "fincas:leer"
        },
        {
          "id": 2,
          "modulo": "fincas",
          "accion": "actualizar",
          "nombre": "fincas:actualizar"
        }
      ],
      "usuarios_count": 2,
      "activo": true
    }
  ]
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos

---

### `POST /api/v1/usuarios/{id}/roles`

Asigna roles a un usuario.

**Autenticación:** Requerida

**Permisos:** `admin:roles`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | integer | ID del usuario |

**Request Body:**
```json
{
  "roles": [
    {
      "rol_id": 6,
      "finca_id": null
    },
    {
      "rol_id": 7,
      "finca_id": 5
    }
  ]
}
```

**Response 200:**
```json
{
  "usuario_id": 42,
  "roles_asignados": [
    {
      "id": 6,
      "nombre": "ROLE_TENANT_AGRONOMO",
      "finca_id": null
    },
    {
      "id": 7,
      "nombre": "ROLE_TENANT_SUPERVISOR",
      "finca_id": 5
    }
  ],
  "fecha_asignacion": "2024-01-15T13:00:00Z"
}
```

**Códigos de error:**
- `400` - Datos inválidos
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Usuario o rol no encontrado

**Notas:**
- `finca_id` null = rol aplica a todas las fincas del tenant
- `finca_id` específico = rol limitado a esa finca
- Los roles anteriores se reemplazan completamente

---

### `DELETE /api/v1/usuarios/{id}/roles/{rol_id}`

Remueve un rol de un usuario.

**Autenticación:** Requerida

**Permisos:** `admin:roles`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | integer | ID del usuario |
| rol_id | integer | ID del rol a remover |

**Response 204:** Sin contenido

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Usuario o asignación no encontrada
- `409` - No se puede remover el último rol owner

---

## Endpoints de Permisos

### `GET /api/v1/permisos`

Lista todos los permisos disponibles del sistema.

**Autenticación:** Requerida

**Permisos:** `admin:roles`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| modulo | string | No | Filtrar por módulo |

**Response 200:**
```json
{
  "data": [
    {
      "id": 1,
      "modulo": "fincas",
      "accion": "leer",
      "nombre": "fincas:leer",
      "descripcion": "Ver información de fincas",
      "activo": true
    },
    {
      "id": 2,
      "modulo": "fincas",
      "accion": "actualizar",
      "nombre": "fincas:actualizar",
      "descripcion": "Editar fincas",
      "activo": true
    }
  ],
  "modulos": [
    "fincas",
    "sectores",
    "lotes",
    "arboles",
    "inspecciones",
    "aplicaciones",
    "tareas",
    "inventario",
    "compras",
    "cosecha",
    "reportes",
    "config",
    "admin"
  ]
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos

---

## Roles Predefinidos del Sistema

### ROLE_TENANT_OWNER
- **Descripción:** Dueño/propietario de la organización
- **Permisos:** Todos los permisos del tenant
- **Limitaciones:** Solo puede haber uno por tenant, no puede ser removido

### ROLE_TENANT_MANAGER
- **Descripción:** Gerente con acceso completo
- **Permisos:** Todos excepto gestión de plan y facturación

### ROLE_TENANT_AGRONOMO
- **Descripción:** Agrónomo/ingeniero con acceso técnico
- **Permisos:** Gestión de campo, inspecciones, aplicaciones, reportes técnicos

### ROLE_TENANT_SUPERVISOR
- **Descripción:** Supervisor de campo
- **Permisos:** Asignación de tareas, ejecución de aplicaciones, registro de cosechas

### ROLE_TENANT_OPERARIO
- **Descripción:** Trabajador de campo
- **Permisos:** Ejecución de tareas, registro básico de actividades

### ROLE_TENANT_VIEWER
- **Descripción:** Solo lectura/consulta
- **Permisos:** Solo lectura en módulos principales

---

## Notas de Seguridad

### Control de Acceso

- Los permisos se verifican a nivel de endpoint usando Spring Security
- El `tenant_id` se extrae del JWT para aislamiento multi-tenant
- Los roles se verifican tanto a nivel de método como de endpoint

### Auditoría

- Todos los cambios de roles y permisos se registran
- Se mantiene historial de asignaciones
- Los accesos denegados se logean para análisis

### Buenas Prácticas

1. Asignar el rol mínimo necesario (principio de menor privilegio)
2. Revisar roles periódicamente
3. Usar roles con scope de finca cuando sea posible
4. Remover roles cuando un usuario cambia de función
