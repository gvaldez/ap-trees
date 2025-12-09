# API - Inspecciones

> Endpoints para inspecciones fitosanitarias y fenológicas

## Resumen

Las inspecciones permiten registrar observaciones de salud, plagas, enfermedades y estado fenológico de los árboles.

---

## Endpoints

### `GET /api/v1/inspecciones`

Lista inspecciones del tenant.

**Autenticación:** Requerida

**Permisos:** `inspecciones:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| finca_id | integer | No | Filtrar por finca |
| lote_id | integer | No | Filtrar por lote |
| fecha_desde | date | No | Fecha inicial (ISO 8601) |
| fecha_hasta | date | No | Fecha final (ISO 8601) |
| inspector_id | integer | No | Filtrar por inspector |

**Response 200:**
```json
{
  "data": [
    {
      "id": 789,
      "fecha": "2024-01-15T09:00:00Z",
      "tipo": "fitosanitaria",
      "lote": {
        "id": 25,
        "nombre": "Lote Norte A"
      },
      "inspector": {
        "id": 42,
        "nombre": "Juan Pérez"
      },
      "arboles_inspeccionados": 50,
      "hallazgos_count": 8,
      "estado": "completada"
    }
  ]
}
```

---

### `POST /api/v1/inspecciones`

Crea una nueva inspección.

**Autenticación:** Requerida

**Permisos:** `inspecciones:crear`

**Request Body:**
```json
{
  "fecha": "2024-01-15T09:00:00Z",
  "tipo": "fitosanitaria",
  "lote_id": 25,
  "arboles_objetivo": [1234, 1235, 1236],
  "observaciones_generales": "Revisión rutinaria mensual"
}
```

**Response 201:**
```json
{
  "id": 789,
  "fecha": "2024-01-15T09:00:00Z",
  "lote_id": 25,
  "created_at": "2024-01-15T09:00:00Z"
}
```

---

### `POST /api/v1/inspecciones/{id}/hallazgos`

Registra un hallazgo en una inspección.

**Autenticación:** Requerida

**Permisos:** `inspecciones:crear`

**Request Body:**
```json
{
  "arbol_id": 1234,
  "tipo": "plaga",
  "problema_id": 15,
  "severidad": "leve",
  "ubicacion_arbol": "hojas_inferiores",
  "porcentaje_afectado": 10,
  "observaciones": "Presencia de trips en hojas nuevas",
  "fotos": [
    "https://storage.agrogrid.io/inspecciones/789/foto1.jpg"
  ],
  "requiere_tratamiento": true,
  "prioridad": "media"
}
```

**Response 201:**
```json
{
  "id": 1001,
  "inspeccion_id": 789,
  "arbol_id": 1234,
  "created_at": "2024-01-15T09:15:00Z"
}
```

---
