# API - Riego

> Endpoints para infraestructura y programación de riego

## Resumen

Gestión de sistemas de riego, programación y registro de riegos ejecutados.

---

## Endpoints

### `GET /api/v1/fincas/{finca_id}/infraestructura-riego`

Lista infraestructura de riego de una finca.

**Autenticación:** Requerida

**Permisos:** `riego:leer`

**Response 200:**
```json
{
  "data": [
    {
      "id": 10,
      "tipo": "goteo",
      "nombre": "Sistema Goteo Lote Norte",
      "lotes": [
        {
          "id": 25,
          "nombre": "Lote Norte A"
        }
      ],
      "capacidad_lh": 500,
      "estado": "operativo",
      "ultima_revision": "2024-01-10T00:00:00Z"
    }
  ]
}
```

---

### `GET /api/v1/riego/programaciones`

Lista programaciones de riego.

**Autenticación:** Requerida

**Permisos:** `riego:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| finca_id | integer | No | Filtrar por finca |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |
| estado | string | No | programado, ejecutado, cancelado |

**Response 200:**
```json
{
  "data": [
    {
      "id": 234,
      "fecha_programada": "2024-01-16T06:00:00Z",
      "duracion_horas": 2,
      "lote_id": 25,
      "sistema_riego_id": 10,
      "litros_programados": 1000,
      "estado": "programado"
    }
  ]
}
```

---

### `POST /api/v1/riego/programaciones`

Programa un nuevo riego.

**Autenticación:** Requerida

**Permisos:** `riego:crear`

**Request Body:**
```json
{
  "fecha_programada": "2024-01-16T06:00:00Z",
  "duracion_horas": 2,
  "lote_id": 25,
  "sistema_riego_id": 10,
  "tipo": "riego_regular",
  "observaciones": "Riego programado automático"
}
```

**Response 201:**
```json
{
  "id": 234,
  "fecha_programada": "2024-01-16T06:00:00Z",
  "estado": "programado",
  "created_at": "2024-01-15T19:00:00Z"
}
```

---

### `POST /api/v1/riego/registros`

Registra un riego ejecutado.

**Autenticación:** Requerida

**Permisos:** `riego:crear`

**Request Body:**
```json
{
  "programacion_id": 234,
  "fecha_inicio": "2024-01-16T06:00:00Z",
  "fecha_fin": "2024-01-16T08:00:00Z",
  "litros_aplicados": 980,
  "presion_bar": 2.5,
  "caudal_lh": 490,
  "observaciones": "Ejecutado según lo programado"
}
```

**Response 201:**
```json
{
  "id": 345,
  "programacion_id": 234,
  "litros_aplicados": 980,
  "created_at": "2024-01-16T08:15:00Z"
}
```

---
