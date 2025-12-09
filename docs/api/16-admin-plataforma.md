# API - Administración de Plataforma

> Endpoints para administración de plataforma (solo admin)

## Resumen

Endpoints exclusivos para administradores de la plataforma AgroGrid para gestión de planes, suscripciones, facturación y soporte.

**Acceso:** Solo usuarios con roles `ROLE_SUPER_ADMIN` o `ROLE_ADMIN_SOPORTE`

---

## Gestión de Planes

### `GET /api/v1/admin/planes`

Lista planes de suscripción disponibles.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Response 200:**
```json
{
  "data": [
    {
      "id": 1,
      "nombre": "Starter",
      "precio_mensual": 49.99,
      "precio_anual": 499.99,
      "limite_arboles": 2000,
      "limite_usuarios": 5,
      "limite_storage_gb": 10,
      "features": [
        "gestion_basica",
        "reportes_basicos",
        "soporte_email"
      ],
      "activo": true
    },
    {
      "id": 2,
      "nombre": "Professional",
      "precio_mensual": 149.99,
      "precio_anual": 1499.99,
      "limite_arboles": 10000,
      "limite_usuarios": 20,
      "limite_storage_gb": 50,
      "features": [
        "gestion_completa",
        "reportes_avanzados",
        "ml_deteccion",
        "soporte_prioritario"
      ],
      "activo": true
    }
  ]
}
```

---

### `POST /api/v1/admin/planes`

Crea un nuevo plan de suscripción.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Request Body:**
```json
{
  "nombre": "Enterprise",
  "descripcion": "Plan empresarial con límites extendidos",
  "precio_mensual": 299.99,
  "precio_anual": 2999.99,
  "limite_arboles": 50000,
  "limite_usuarios": 100,
  "limite_storage_gb": 500,
  "limite_api_calls_mes": 50000,
  "features": [
    "gestion_completa",
    "reportes_avanzados",
    "ml_deteccion",
    "integracion_erp",
    "soporte_24_7",
    "account_manager"
  ]
}
```

**Response 201:**
```json
{
  "id": 3,
  "nombre": "Enterprise",
  "created_at": "2024-01-16T00:00:00Z"
}
```

---

## Gestión de Suscripciones

### `GET /api/v1/admin/suscripciones`

Lista todas las suscripciones activas.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`, `ROLE_ADMIN_SOPORTE`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| estado | string | No | activa, vencida, cancelada |
| plan_id | integer | No | Filtrar por plan |

**Response 200:**
```json
{
  "data": [
    {
      "id": 45,
      "tenant": {
        "id": "uuid-tenant",
        "nombre": "Finca La Esperanza"
      },
      "plan": {
        "id": 2,
        "nombre": "Professional"
      },
      "estado": "activa",
      "fecha_inicio": "2024-01-01T00:00:00Z",
      "fecha_vencimiento": "2024-12-31T23:59:59Z",
      "ciclo_facturacion": "anual",
      "precio_mensual": 149.99,
      "descuento_porcentaje": 0,
      "metodo_pago": "tarjeta",
      "auto_renovacion": true
    }
  ]
}
```

---

## Facturación

### `GET /api/v1/admin/facturas`

Lista facturas generadas.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| tenant_id | uuid | No | Filtrar por tenant |
| estado | string | No | pendiente, pagada, vencida, cancelada |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |

**Response 200:**
```json
{
  "data": [
    {
      "id": 234,
      "numero_factura": "FAC-2024-0234",
      "tenant": {
        "id": "uuid-tenant",
        "nombre": "Finca La Esperanza"
      },
      "fecha_emision": "2024-01-01",
      "fecha_vencimiento": "2024-01-15",
      "monto_subtotal": 149.99,
      "impuestos": 28.50,
      "monto_total": 178.49,
      "moneda": "USD",
      "estado": "pagada",
      "fecha_pago": "2024-01-05T10:30:00Z",
      "metodo_pago": "tarjeta"
    }
  ]
}
```

---

## Tickets de Soporte

### `GET /api/v1/admin/tickets`

Lista tickets de soporte.

**Autenticación:** Requerida

**Permisos:** `ROLE_ADMIN_SOPORTE`, `ROLE_SUPER_ADMIN`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| estado | string | No | abierto, en_proceso, resuelto, cerrado |
| prioridad | string | No | baja, media, alta, urgente |
| asignado_a | integer | No | ID del agente de soporte |

**Response 200:**
```json
{
  "data": [
    {
      "id": 567,
      "numero_ticket": "TICK-2024-0567",
      "tenant": {
        "id": "uuid-tenant",
        "nombre": "Finca La Esperanza"
      },
      "usuario": {
        "id": 42,
        "nombre": "Juan Pérez",
        "email": "juan@finca.com"
      },
      "asunto": "Error al exportar reporte",
      "categoria": "bug",
      "prioridad": "media",
      "estado": "abierto",
      "fecha_creacion": "2024-01-15T10:00:00Z",
      "asignado_a": {
        "id": 100,
        "nombre": "Ana Soporte"
      }
    }
  ]
}
```

---

### `GET /api/v1/admin/tickets/{id}`

Obtiene detalles de un ticket.

**Autenticación:** Requerida

**Permisos:** `ROLE_ADMIN_SOPORTE`, `ROLE_SUPER_ADMIN`

**Response 200:**
```json
{
  "id": 567,
  "numero_ticket": "TICK-2024-0567",
  "tenant": {
    "id": "uuid-tenant",
    "nombre": "Finca La Esperanza"
  },
  "usuario": {
    "id": 42,
    "nombre": "Juan Pérez",
    "email": "juan@finca.com",
    "telefono": "+57 310 123 4567"
  },
  "asunto": "Error al exportar reporte",
  "descripcion": "Al intentar exportar el reporte de producción a PDF, aparece un error...",
  "categoria": "bug",
  "prioridad": "media",
  "estado": "en_proceso",
  "fecha_creacion": "2024-01-15T10:00:00Z",
  "fecha_ultima_actualizacion": "2024-01-15T14:30:00Z",
  "asignado_a": {
    "id": 100,
    "nombre": "Ana Soporte"
  },
  "comentarios": [
    {
      "id": 1001,
      "autor": {
        "id": 100,
        "nombre": "Ana Soporte"
      },
      "mensaje": "Hola Juan, estamos investigando el problema...",
      "fecha": "2024-01-15T14:30:00Z",
      "interno": false
    }
  ],
  "adjuntos": [
    {
      "nombre": "screenshot-error.png",
      "url": "https://storage.agrogrid.io/tickets/567/screenshot.png"
    }
  ]
}
```

---

### `POST /api/v1/admin/tickets/{id}/comentarios`

Agrega un comentario a un ticket.

**Autenticación:** Requerida

**Permisos:** `ROLE_ADMIN_SOPORTE`, `ROLE_SUPER_ADMIN`

**Request Body:**
```json
{
  "mensaje": "Hemos identificado el problema y estamos trabajando en la solución...",
  "interno": false,
  "actualizar_estado": "en_proceso"
}
```

**Response 201:**
```json
{
  "id": 1002,
  "ticket_id": 567,
  "mensaje": "Hemos identificado el problema...",
  "fecha": "2024-01-15T15:00:00Z"
}
```

---

## Logs de Auditoría

### `GET /api/v1/admin/audit-logs`

Lista registros de auditoría del sistema.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| tenant_id | uuid | No | Filtrar por tenant |
| usuario_id | integer | No | Filtrar por usuario |
| accion | string | No | login, logout, create, update, delete, etc. |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |

**Response 200:**
```json
{
  "data": [
    {
      "id": 123456,
      "fecha": "2024-01-15T10:30:00Z",
      "tenant": {
        "id": "uuid-tenant",
        "nombre": "Finca La Esperanza"
      },
      "usuario": {
        "id": 42,
        "email": "juan@finca.com"
      },
      "accion": "update",
      "recurso": "arboles",
      "recurso_id": 1234,
      "detalles": {
        "campo_modificado": "estado",
        "valor_anterior": "SAL",
        "valor_nuevo": "OBS"
      },
      "ip_address": "192.168.1.10",
      "resultado": "exitoso"
    }
  ],
  "meta": {
    "page": 1,
    "size": 100,
    "total_elements": 50000
  }
}
```

---

## Configuración de Plataforma

### `GET /api/v1/admin/configuracion`

Obtiene configuración global de la plataforma.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Response 200:**
```json
{
  "email": {
    "smtp_host": "smtp.agrogrid.io",
    "smtp_port": 587,
    "from_address": "no-reply@agrogrid.io"
  },
  "storage": {
    "proveedor": "aws_s3",
    "bucket": "agrogrid-storage",
    "region": "us-east-1"
  },
  "rate_limiting": {
    "habilitado": true,
    "limite_global_rpm": 1000
  },
  "ml_services": {
    "deteccion_plagas": true,
    "url_servicio": "https://ml.agrogrid.io"
  },
  "mantenimiento": {
    "activo": false,
    "mensaje": null,
    "fecha_programada": null
  }
}
```

---

### `PUT /api/v1/admin/configuracion`

Actualiza configuración de la plataforma.

**Autenticación:** Requerida

**Permisos:** `ROLE_SUPER_ADMIN`

**Request Body:** (parcial)
```json
{
  "rate_limiting": {
    "limite_global_rpm": 1200
  }
}
```

**Response 200:**
```json
{
  "mensaje": "Configuración actualizada exitosamente",
  "updated_at": "2024-01-16T01:00:00Z"
}
```

---
