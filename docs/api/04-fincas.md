# API - Fincas

> Endpoints para gestión de fincas (propiedades agrícolas)

## Resumen

Las fincas son el nivel más alto de la jerarquía del campo en AgroGrid. Representan propiedades o haciendas completas pertenecientes a un tenant.

---

## Endpoints

### `GET /api/v1/fincas`

Lista todas las fincas del tenant actual.

**Autenticación:** Requerida

**Permisos:** `fincas:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| page | integer | No | Número de página |
| size | integer | No | Elementos por página |
| activo | boolean | No | Filtrar por estado activo |
| buscar | string | No | Búsqueda por nombre o código |

**Response 200:**
```json
{
  "data": [
    {
      "id": 5,
      "nombre": "Finca La Esperanza",
      "codigo": "FLE-001",
      "ubicacion": {
        "direccion": "Km 45 Carretera Federal",
        "ciudad": "Fresno",
        "estado": "Tolima",
        "pais": "Colombia",
        "latitud": 4.9175,
        "longitud": -75.0778
      },
      "area_total_ha": 50.0,
      "area_cultivada_ha": 42.0,
      "sectores_count": 4,
      "lotes_count": 10,
      "arboles_count": 3450,
      "activo": true,
      "created_at": "2023-12-01T00:00:00Z"
    }
  ],
  "meta": {
    "page": 1,
    "size": 20,
    "total_elements": 3,
    "total_pages": 1
  }
}
```

---

### `POST /api/v1/fincas`

Crea una nueva finca.

**Autenticación:** Requerida

**Permisos:** `fincas:crear`

**Request Body:**
```json
{
  "nombre": "Finca Los Naranjos",
  "codigo": "FLN-001",
  "ubicacion": {
    "direccion": "Carretera Principal Km 12",
    "ciudad": "Medellín",
    "estado": "Antioquia",
    "pais": "Colombia",
    "latitud": 6.2442,
    "longitud": -75.5812,
    "altitud_msnm": 1450
  },
  "area_total_ha": 30.0,
  "area_cultivada_ha": 25.0,
  "descripcion": "Finca especializada en cultivo de cítricos",
  "contacto_encargado": {
    "nombre": "Pedro Ramírez",
    "telefono": "+57 310 456 7890",
    "email": "pedro@finca.com"
  }
}
```

**Response 201:**
```json
{
  "id": 6,
  "nombre": "Finca Los Naranjos",
  "codigo": "FLN-001",
  "tenant_id": "uuid-tenant",
  "created_at": "2024-01-15T14:00:00Z"
}
```

---

### `GET /api/v1/fincas/{id}`

Obtiene detalles completos de una finca.

**Autenticación:** Requerida

**Permisos:** `fincas:leer`

**Response 200:**
```json
{
  "id": 5,
  "nombre": "Finca La Esperanza",
  "codigo": "FLE-001",
  "ubicacion": {
    "direccion": "Km 45 Carretera Federal",
    "ciudad": "Fresno",
    "estado": "Tolima",
    "pais": "Colombia",
    "latitud": 4.9175,
    "longitud": -75.0778,
    "altitud_msnm": 1200,
    "zona_horaria": "America/Bogota"
  },
  "area_total_ha": 50.0,
  "area_cultivada_ha": 42.0,
  "area_sin_cultivar_ha": 8.0,
  "perimetro": {
    "type": "Polygon",
    "coordinates": [[[...]]],
    "area_calculada_ha": 50.2
  },
  "contacto_encargado": {
    "nombre": "Juan Pérez",
    "telefono": "+57 310 123 4567",
    "email": "juan@finca.com"
  },
  "estadisticas": {
    "sectores_count": 4,
    "lotes_count": 10,
    "arboles_count": 3450,
    "arboles_saludables": 3200,
    "arboles_en_produccion": 3100,
    "ultima_inspeccion": "2024-01-10T09:00:00Z",
    "ultima_aplicacion": "2024-01-12T07:00:00Z"
  },
  "activo": true,
  "created_at": "2023-12-01T00:00:00Z",
  "updated_at": "2024-01-15T10:00:00Z"
}
```

---

### `PUT /api/v1/fincas/{id}`

Actualiza una finca.

**Autenticación:** Requerida

**Permisos:** `fincas:actualizar`

**Request Body:** (parcial permitido)
```json
{
  "nombre": "Finca La Esperanza (Actualizado)",
  "area_cultivada_ha": 45.0,
  "contacto_encargado": {
    "telefono": "+57 310 999 8888"
  }
}
```

**Response 200:**
```json
{
  "id": 5,
  "nombre": "Finca La Esperanza (Actualizado)",
  "updated_at": "2024-01-15T14:30:00Z"
}
```

---

### `DELETE /api/v1/fincas/{id}`

Desactiva (soft delete) una finca.

**Autenticación:** Requerida

**Permisos:** `fincas:eliminar`

**Response 204:** Sin contenido

**Notas:** No se elimina físicamente, solo se marca como inactiva

---

### `GET /api/v1/fincas/{id}/estadisticas`

Obtiene estadísticas detalladas de la finca.

**Autenticación:** Requerida

**Permisos:** `fincas:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| periodo | string | No | `dia`, `semana`, `mes`, `año` (default: mes) |

**Response 200:**
```json
{
  "finca_id": 5,
  "periodo": "mes",
  "fecha_inicio": "2024-01-01T00:00:00Z",
  "fecha_fin": "2024-01-31T23:59:59Z",
  "estructura": {
    "sectores": 4,
    "lotes": 10,
    "arboles_totales": 3450,
    "arboles_productivos": 3100,
    "area_cultivada_ha": 42.0
  },
  "salud": {
    "saludables": 3200,
    "en_observacion": 150,
    "en_riesgo": 80,
    "criticos": 20,
    "tasa_salud": 92.75
  },
  "actividades": {
    "inspecciones": 45,
    "aplicaciones": 12,
    "cosechas": 8,
    "tareas_completadas": 156
  },
  "produccion": {
    "cosecha_total_kg": 4500,
    "rendimiento_kg_por_arbol": 1.45,
    "valor_estimado": 22500.00,
    "moneda": "COP"
  }
}
```

---
