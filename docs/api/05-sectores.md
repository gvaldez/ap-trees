# API - Sectores

> Endpoints para gestión de sectores dentro de fincas

## Resumen

Los sectores son divisiones lógicas de una finca, generalmente basadas en ubicación, topografía o criterios de manejo.

---

## Endpoints

### `GET /api/v1/fincas/{finca_id}/sectores`

Lista sectores de una finca.

**Autenticación:** Requerida

**Permisos:** `sectores:leer`

**Parámetros de ruta:**
| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| finca_id | integer | ID de la finca |

**Response 200:**
```json
{
  "data": [
    {
      "id": 10,
      "finca_id": 5,
      "nombre": "Sector Norte",
      "codigo": "SN-01",
      "area_ha": 12.5,
      "lotes_count": 3,
      "arboles_count": 850,
      "ubicacion_geografica": {
        "type": "Polygon",
        "coordinates": [[[...]]]
      },
      "activo": true
    }
  ]
}
```

---

### `POST /api/v1/fincas/{finca_id}/sectores`

Crea un nuevo sector en una finca.

**Autenticación:** Requerida

**Permisos:** `sectores:crear`

**Request Body:**
```json
{
  "nombre": "Sector Este",
  "codigo": "SE-01",
  "area_ha": 10.0,
  "descripcion": "Sector con buena exposición solar",
  "ubicacion_geografica": {
    "type": "Polygon",
    "coordinates": [[[...]]]
  }
}
```

**Response 201:**
```json
{
  "id": 11,
  "finca_id": 5,
  "nombre": "Sector Este",
  "created_at": "2024-01-15T15:00:00Z"
}
```

---

### `GET /api/v1/sectores/{id}`

Obtiene detalles de un sector.

**Autenticación:** Requerida

**Permisos:** `sectores:leer`

**Response 200:**
```json
{
  "id": 10,
  "finca_id": 5,
  "nombre": "Sector Norte",
  "codigo": "SN-01",
  "area_ha": 12.5,
  "descripcion": "Zona norte con pendiente moderada",
  "ubicacion_geografica": {
    "type": "Polygon",
    "coordinates": [[[...]]]
  },
  "estadisticas": {
    "lotes_count": 3,
    "arboles_count": 850,
    "arboles_saludables": 780,
    "densidad_arboles_ha": 68,
    "ultima_inspeccion": "2024-01-12T10:00:00Z"
  },
  "activo": true,
  "created_at": "2023-12-05T00:00:00Z",
  "updated_at": "2024-01-15T10:00:00Z"
}
```

---

### `PUT /api/v1/sectores/{id}`

Actualiza un sector.

**Autenticación:** Requerida

**Permisos:** `sectores:actualizar`

**Request Body:** (parcial)
```json
{
  "nombre": "Sector Norte Premium",
  "area_ha": 13.0
}
```

**Response 200:**
```json
{
  "id": 10,
  "nombre": "Sector Norte Premium",
  "updated_at": "2024-01-15T15:30:00Z"
}
```

---

### `DELETE /api/v1/sectores/{id}`

Desactiva un sector.

**Autenticación:** Requerida

**Permisos:** `sectores:eliminar`

**Response 204:** Sin contenido

---
