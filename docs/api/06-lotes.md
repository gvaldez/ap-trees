# API - Lotes

> Endpoints para gestión de lotes de cultivo

## Resumen

Los lotes son unidades de producción específicas dentro de sectores, generalmente dedicadas a un cultivo particular.

---

## Endpoints

### `GET /api/v1/sectores/{sector_id}/lotes`

Lista lotes de un sector.

**Autenticación:** Requerida

**Permisos:** `lotes:leer`

**Response 200:**
```json
{
  "data": [
    {
      "id": 25,
      "sector_id": 10,
      "nombre": "Lote Norte A",
      "codigo": "LNA-01",
      "cultivo": {
        "codigo": "AGU",
        "nombre": "Aguacate",
        "variedad": "Hass"
      },
      "area_ha": 4.0,
      "arboles_count": 280,
      "fecha_plantacion": "2020-03-15",
      "edad_anos": 3.8,
      "en_produccion": true
    }
  ]
}
```

---

### `POST /api/v1/sectores/{sector_id}/lotes`

Crea un nuevo lote.

**Autenticación:** Requerida

**Permisos:** `lotes:crear`

**Request Body:**
```json
{
  "nombre": "Lote Sur B",
  "codigo": "LSB-01",
  "cultivo_codigo": "LIM",
  "variedad": "Tahití",
  "area_ha": 3.5,
  "fecha_plantacion": "2022-06-01",
  "densidad_siembra": {
    "arboles_por_ha": 70,
    "distancia_entre_filas_m": 4.0,
    "distancia_entre_plantas_m": 3.5
  },
  "sistema_riego": "goteo",
  "ubicacion_geografica": {
    "type": "Polygon",
    "coordinates": [[[...]]]
  }
}
```

**Response 201:**
```json
{
  "id": 26,
  "sector_id": 10,
  "nombre": "Lote Sur B",
  "created_at": "2024-01-15T16:00:00Z"
}
```

---

### `GET /api/v1/lotes/{id}`

Obtiene detalles completos de un lote.

**Autenticación:** Requerida

**Permisos:** `lotes:leer`

**Response 200:**
```json
{
  "id": 25,
  "sector_id": 10,
  "finca_id": 5,
  "nombre": "Lote Norte A",
  "codigo": "LNA-01",
  "cultivo": {
    "codigo": "AGU",
    "nombre": "Aguacate",
    "nombre_cientifico": "Persea americana",
    "variedad": "Hass"
  },
  "area_ha": 4.0,
  "fecha_plantacion": "2020-03-15",
  "edad_anos": 3.8,
  "densidad_siembra": {
    "arboles_por_ha": 70,
    "distancia_entre_filas_m": 4.0,
    "distancia_entre_plantas_m": 3.5
  },
  "sistema_riego": "goteo",
  "ubicacion_geografica": {
    "type": "Polygon",
    "coordinates": [[[...]]],
    "area_calculada_ha": 4.02
  },
  "estadisticas": {
    "arboles_totales": 280,
    "arboles_productivos": 270,
    "arboles_saludables": 260,
    "tasa_salud": 92.86,
    "produccion_ultima_cosecha_kg": 1200,
    "rendimiento_kg_por_arbol": 4.44
  },
  "en_produccion": true,
  "activo": true,
  "created_at": "2020-04-01T00:00:00Z",
  "updated_at": "2024-01-15T10:00:00Z"
}
```

---

### `PUT /api/v1/lotes/{id}`

Actualiza un lote.

**Autenticación:** Requerida

**Permisos:** `lotes:actualizar`

**Request Body:** (parcial)
```json
{
  "nombre": "Lote Norte A Premium",
  "en_produccion": true,
  "sistema_riego": "goteo_automatizado"
}
```

**Response 200:**
```json
{
  "id": 25,
  "nombre": "Lote Norte A Premium",
  "updated_at": "2024-01-15T16:30:00Z"
}
```

---

### `DELETE /api/v1/lotes/{id}`

Desactiva un lote.

**Autenticación:** Requerida

**Permisos:** `lotes:eliminar`

**Response 204:** Sin contenido

---

### `GET /api/v1/lotes/{id}/estadisticas`

Obtiene estadísticas detalladas del lote.

**Autenticación:** Requerida

**Permisos:** `lotes:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| periodo | string | No | `mes`, `trimestre`, `año` (default: mes) |

**Response 200:**
```json
{
  "lote_id": 25,
  "periodo": "mes",
  "fecha_inicio": "2024-01-01T00:00:00Z",
  "fecha_fin": "2024-01-31T23:59:59Z",
  "arboles": {
    "totales": 280,
    "saludables": 260,
    "en_tratamiento": 15,
    "criticos": 5
  },
  "actividades": {
    "inspecciones": 8,
    "aplicaciones": 3,
    "riegos": 24,
    "podas": 5
  },
  "produccion": {
    "cosecha_kg": 1200,
    "rendimiento_kg_arbol": 4.44,
    "proyeccion_anual_kg": 14400
  }
}
```

---
