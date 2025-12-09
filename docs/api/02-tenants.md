# API - Tenants (Gestión Multi-Tenant)

> Endpoints para administración de tenants/clientes (solo admin de plataforma)

## Resumen

Los endpoints de tenants están restringidos a administradores de la plataforma AgroGrid. Permiten crear, configurar y gestionar organizaciones clientes (tenants) en el sistema SaaS.

**Acceso:** Solo usuarios con roles `ROLE_SUPER_ADMIN` o `ROLE_ADMIN_SOPORTE`

---

## Endpoints

### `GET /api/v1/admin/tenants`

Lista todos los tenants de la plataforma.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`, `ROLE_ADMIN_SOPORTE`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| page | integer | No | Número de página (default: 1) |
| size | integer | No | Elementos por página (default: 20, max: 100) |
| sort | string | No | Campo de ordenamiento: `nombre,asc`, `fecha_registro,desc` |
| activo | boolean | No | Filtrar por estado activo |
| plan | string | No | Filtrar por plan: `starter`, `professional`, `enterprise` |
| buscar | string | No | Búsqueda por nombre, slug o email |

**Response 200:**
```json
{
  "data": [
    {
      "id": "uuid-tenant-1",
      "nombre": "Finca La Esperanza",
      "slug": "finca-esperanza",
      "plan": {
        "id": 2,
        "nombre": "Professional",
        "limite_arboles": 10000,
        "limite_usuarios": 20
      },
      "activo": true,
      "email_contacto": "admin@finca-esperanza.com",
      "fecha_registro": "2024-01-01T00:00:00Z",
      "fecha_activacion": "2024-01-01T10:30:00Z",
      "usuarios_activos": 8,
      "arboles_registrados": 3450,
      "storage_usado_gb": 2.5
    }
  ],
  "meta": {
    "page": 1,
    "size": 20,
    "total_elements": 45,
    "total_pages": 3
  }
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos de admin de plataforma

---

### `POST /api/v1/admin/tenants`

Crea un nuevo tenant/cliente.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Request Body:**
```json
{
  "nombre": "Finca Los Naranjos",
  "slug": "finca-naranjos",
  "plan_id": 2,
  "razon_social": "Finca Los Naranjos S.A.",
  "rfc": "FLN123456ABC",
  "pais": "México",
  "ciudad": "Morelos",
  "email_contacto": "contacto@naranjos.com",
  "telefono": "+52 777 123 4567",
  "configuracion": {
    "timezone": "America/Mexico_City",
    "idioma": "es",
    "moneda": "MXN"
  },
  "admin_usuario": {
    "nombre": "Carlos",
    "apellido": "Rodríguez",
    "email": "carlos@naranjos.com",
    "password": "SecurePassword123!"
  }
}
```

**Response 201:**
```json
{
  "id": "uuid-nuevo-tenant",
  "nombre": "Finca Los Naranjos",
  "slug": "finca-naranjos",
  "activo": true,
  "fecha_registro": "2024-01-15T10:30:00Z",
  "admin_usuario": {
    "id": 123,
    "email": "carlos@naranjos.com",
    "nombre": "Carlos",
    "apellido": "Rodríguez"
  },
  "subdomain_url": "https://finca-naranjos.agrogrid.io"
}
```

**Códigos de error:**
- `400` - Datos inválidos
- `401` - No autenticado
- `403` - Sin permisos
- `409` - Slug ya existe

---

### `GET /api/v1/admin/tenants/{id}`

Obtiene detalles completos de un tenant.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`, `ROLE_ADMIN_SOPORTE`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Response 200:**
```json
{
  "id": "uuid-tenant",
  "nombre": "Finca La Esperanza",
  "slug": "finca-esperanza",
  "razon_social": "Finca La Esperanza S.A.",
  "rfc": "FLE123456ABC",
  "direccion": "Km 45 Carretera Federal",
  "ciudad": "Fresno, Tolima",
  "pais": "Colombia",
  "telefono": "+57 310 123 4567",
  "email_contacto": "admin@finca-esperanza.com",
  "plan": {
    "id": 2,
    "nombre": "Professional",
    "precio_mensual": 149.99,
    "limite_arboles": 10000,
    "limite_usuarios": 20,
    "limite_storage_gb": 50
  },
  "suscripcion": {
    "estado": "activa",
    "fecha_inicio": "2024-01-01T00:00:00Z",
    "fecha_vencimiento": "2024-12-31T23:59:59Z",
    "ciclo_facturacion": "anual",
    "metodo_pago": "tarjeta"
  },
  "uso_actual": {
    "usuarios_activos": 8,
    "usuarios_limite": 20,
    "arboles_registrados": 3450,
    "arboles_limite": 10000,
    "storage_usado_gb": 2.5,
    "storage_limite_gb": 50,
    "api_calls_mes": 12500
  },
  "configuracion": {
    "timezone": "America/Bogota",
    "idioma": "es",
    "moneda": "COP",
    "features_habilitados": ["ml_deteccion", "reportes_avanzados"]
  },
  "activo": true,
  "fecha_registro": "2024-01-01T00:00:00Z",
  "fecha_activacion": "2024-01-01T10:30:00Z",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant no encontrado

---

### `PUT /api/v1/admin/tenants/{id}`

Actualiza información de un tenant.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Request Body:**
```json
{
  "nombre": "Finca La Esperanza (actualizado)",
  "email_contacto": "nuevo-contacto@finca.com",
  "telefono": "+57 310 999 8888",
  "configuracion": {
    "features_habilitados": ["ml_deteccion", "reportes_avanzados", "integracion_erp"]
  }
}
```

**Response 200:**
```json
{
  "id": "uuid-tenant",
  "nombre": "Finca La Esperanza (actualizado)",
  "updated_at": "2024-01-15T11:00:00Z"
}
```

**Códigos de error:**
- `400` - Datos inválidos
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant no encontrado

---

### `POST /api/v1/admin/tenants/{id}/suspend`

Suspende un tenant (desactiva acceso).

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Request Body:**
```json
{
  "motivo": "Falta de pago - factura vencida desde hace 30 días"
}
```

**Response 200:**
```json
{
  "id": "uuid-tenant",
  "activo": false,
  "fecha_suspension": "2024-01-15T11:00:00Z",
  "motivo_suspension": "Falta de pago - factura vencida desde hace 30 días"
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant no encontrado
- `409` - Tenant ya suspendido

---

### `POST /api/v1/admin/tenants/{id}/activate`

Reactiva un tenant suspendido.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Response 200:**
```json
{
  "id": "uuid-tenant",
  "activo": true,
  "fecha_activacion": "2024-01-15T12:00:00Z"
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant no encontrado
- `409` - Tenant ya activo

---

### `PUT /api/v1/admin/tenants/{id}/plan`

Cambia el plan de suscripción de un tenant.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Request Body:**
```json
{
  "plan_id": 3,
  "aplicar_inmediato": true,
  "ajuste_prorrateado": true
}
```

**Response 200:**
```json
{
  "id": "uuid-tenant",
  "plan_anterior": {
    "id": 2,
    "nombre": "Professional"
  },
  "plan_nuevo": {
    "id": 3,
    "nombre": "Enterprise"
  },
  "fecha_cambio": "2024-01-15T12:00:00Z",
  "ajuste_facturacion": {
    "credito_prorrateo": 45.50,
    "cargo_nuevo_plan": 299.99,
    "total_ajuste": 254.49
  }
}
```

**Códigos de error:**
- `400` - Plan inválido
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant o plan no encontrado

---

### `GET /api/v1/admin/tenants/{id}/usage`

Obtiene estadísticas de uso del tenant.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`, `ROLE_ADMIN_SOPORTE`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| periodo | string | No | Periodo: `dia`, `semana`, `mes`, `año` (default: mes) |

**Response 200:**
```json
{
  "tenant_id": "uuid-tenant",
  "periodo": "mes",
  "fecha_inicio": "2024-01-01T00:00:00Z",
  "fecha_fin": "2024-01-31T23:59:59Z",
  "uso": {
    "usuarios": {
      "activos": 8,
      "limite": 20,
      "porcentaje_uso": 40
    },
    "arboles": {
      "registrados": 3450,
      "limite": 10000,
      "porcentaje_uso": 34.5
    },
    "storage": {
      "usado_gb": 2.5,
      "limite_gb": 50,
      "porcentaje_uso": 5
    },
    "api_calls": {
      "total": 45000,
      "por_dia": 1500
    },
    "modulos_usados": {
      "inspecciones": 125,
      "aplicaciones": 45,
      "cosechas": 12,
      "reportes_generados": 78
    }
  },
  "alertas": [
    {
      "tipo": "warning",
      "mensaje": "Uso de árboles al 80% del límite",
      "fecha": "2024-01-25T10:00:00Z"
    }
  ]
}
```

**Códigos de error:**
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant no encontrado

---

### `DELETE /api/v1/admin/tenants/{id}`

Elimina permanentemente un tenant y todos sus datos.

**⚠️ ADVERTENCIA:** Esta acción es irreversible y elimina todos los datos del tenant.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| id | uuid | ID del tenant |

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| confirmacion | string | Sí | Debe ser el slug del tenant para confirmar |

**Response 204:** Sin contenido

**Códigos de error:**
- `400` - Confirmación incorrecta
- `401` - No autenticado
- `403` - Sin permisos
- `404` - Tenant no encontrado
- `409` - Tenant con suscripción activa (debe suspenderse primero)

---

## Notas de Implementación

### Multi-Tenancy

- Cada tenant tiene datos completamente aislados usando PostgreSQL Row Level Security (RLS)
- El `tenant_id` se establece automáticamente en el contexto de sesión
- Los índices están optimizados para consultas multi-tenant

### Seguridad

- Solo administradores de plataforma pueden acceder a estos endpoints
- Todas las acciones se registran en audit log
- Cambios críticos (suspensión, eliminación) requieren confirmación adicional

### Performance

- Lista de tenants con paginación optimizada
- Estadísticas de uso se calculan de forma asíncrona
- Cache de información de tenant para reducir consultas
