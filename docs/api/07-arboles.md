# API - Árboles

> Endpoints para gestión de árboles individuales

## Resumen

Los árboles son la unidad más granular de gestión en AgroGrid, con seguimiento individual de ubicación, salud, fenología y producción.

---

## Endpoints

### `GET /api/v1/lotes/{lote_id}/arboles`

Lista árboles de un lote.

**Autenticación:** Requerida

**Permisos:** `arboles:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| page | integer | No | Número de página |
| size | integer | No | Elementos por página |
| estado | string | No | Filtrar por estado: SAL, OBS, RIE, CRI, etc. |
| buscar | string | No | Búsqueda por código o QR |

**Response 200:**
```json
{
  "data": [
    {
      "id": 1234,
      "codigo": "A-001",
      "qr_code": "FLE-SN-LNA-A001",
      "lote_id": 25,
      "posicion": {
        "fila": 1,
        "columna": 1,
        "latitud": 4.9175,
        "longitud": -75.0778
      },
      "estado_actual": {
        "codigo": "SAL",
        "nombre": "Saludable",
        "desde": "2024-01-01T00:00:00Z"
      },
      "etapa_fenologica": {
        "codigo": "FLO",
        "nombre": "Floración"
      },
      "en_produccion": true,
      "fecha_plantacion": "2020-03-15",
      "edad_anos": 3.8
    }
  ],
  "meta": {
    "page": 1,
    "size": 50,
    "total_elements": 280
  }
}
```

---

### `POST /api/v1/lotes/{lote_id}/arboles`

Crea/registra un nuevo árbol.

**Autenticación:** Requerida

**Permisos:** `arboles:crear`

**Request Body:**
```json
{
  "codigo": "A-281",
  "fecha_plantacion": "2024-01-15",
  "posicion": {
    "fila": 10,
    "columna": 28,
    "latitud": 4.9180,
    "longitud": -75.0785
  },
  "estado_inicial": "NUE",
  "observaciones": "Plántula injertada de vivero certificado"
}
```

**Response 201:**
```json
{
  "id": 1281,
  "codigo": "A-281",
  "qr_code": "FLE-SN-LNA-A281",
  "lote_id": 25,
  "created_at": "2024-01-15T17:00:00Z"
}
```

---

### `GET /api/v1/arboles/{id}`

Obtiene detalles completos de un árbol.

**Autenticación:** Requerida

**Permisos:** `arboles:leer`

**Response 200:**
```json
{
  "id": 1234,
  "codigo": "A-001",
  "qr_code": "FLE-SN-LNA-A001",
  "lote": {
    "id": 25,
    "nombre": "Lote Norte A",
    "sector": "Sector Norte",
    "finca": "Finca La Esperanza"
  },
  "cultivo": {
    "codigo": "AGU",
    "nombre": "Aguacate",
    "variedad": "Hass"
  },
  "posicion": {
    "fila": 1,
    "columna": 1,
    "latitud": 4.9175,
    "longitud": -75.0778,
    "altitud_msnm": 1205
  },
  "estado_actual": {
    "codigo": "SAL",
    "nombre": "Saludable",
    "desde": "2024-01-01T00:00:00Z",
    "dias_en_estado": 15
  },
  "etapa_fenologica_actual": {
    "codigo": "FLO",
    "nombre": "Floración",
    "desde": "2024-01-10T00:00:00Z"
  },
  "salud": {
    "vigor": "alto",
    "follaje": "verde_intenso",
    "ultima_inspeccion": "2024-01-12T10:00:00Z",
    "problemas_activos": []
  },
  "fecha_plantacion": "2020-03-15",
  "edad_anos": 3.8,
  "en_produccion": true,
  "produccion": {
    "cosecha_ultima_kg": 4.5,
    "cosecha_acumulada_kg": 52.3,
    "rendimiento_promedio_kg": 13.08
  },
  "imagenes": [
    {
      "url": "https://storage.agrogrid.io/arboles/1234/2024-01-12.jpg",
      "fecha": "2024-01-12T10:00:00Z",
      "tipo": "inspeccion"
    }
  ],
  "activo": true,
  "created_at": "2020-03-20T00:00:00Z",
  "updated_at": "2024-01-15T10:00:00Z"
}
```

---

### `PUT /api/v1/arboles/{id}`

Actualiza información de un árbol.

**Autenticación:** Requerida

**Permisos:** `arboles:actualizar`

**Request Body:** (parcial)
```json
{
  "posicion": {
    "latitud": 4.9176,
    "longitud": -75.0779
  },
  "observaciones": "Coordenadas GPS actualizadas"
}
```

**Response 200:**
```json
{
  "id": 1234,
  "updated_at": "2024-01-15T17:30:00Z"
}
```

---

### `PUT /api/v1/arboles/{id}/estado`

Actualiza el estado de salud de un árbol.

**Autenticación:** Requerida

**Permisos:** `arboles:actualizar`

**Request Body:**
```json
{
  "nuevo_estado": "OBS",
  "motivo": "Síntomas leves de clorosis en hojas nuevas",
  "observaciones": "Requiere seguimiento en 3 días",
  "requiere_accion": true,
  "prioridad": "media"
}
```

**Response 200:**
```json
{
  "id": 1234,
  "estado_anterior": "SAL",
  "estado_actual": "OBS",
  "fecha_cambio": "2024-01-15T17:45:00Z"
}
```

---

### `GET /api/v1/arboles/{id}/historial`

Obtiene el historial completo de cambios de estado.

**Autenticación:** Requerida

**Permisos:** `arboles:leer`

**Response 200:**
```json
{
  "arbol_id": 1234,
  "historial": [
    {
      "id": 5678,
      "estado_anterior": "SAL",
      "estado_nuevo": "OBS",
      "fecha_cambio": "2024-01-15T17:45:00Z",
      "motivo": "Síntomas leves de clorosis",
      "usuario": {
        "id": 42,
        "nombre": "Juan Pérez"
      },
      "inspeccion_id": 123
    },
    {
      "id": 5677,
      "estado_anterior": "NUE",
      "estado_nuevo": "SAL",
      "fecha_cambio": "2020-09-15T00:00:00Z",
      "motivo": "Árbol establecido exitosamente",
      "usuario": {
        "id": 40,
        "nombre": "María López"
      }
    }
  ]
}
```

---

### `GET /api/v1/arboles/buscar`

Búsqueda rápida de árbol por código o QR.

**Autenticación:** Requerida

**Permisos:** `arboles:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| q | string | Sí | Código o QR del árbol |

**Response 200:**
```json
{
  "id": 1234,
  "codigo": "A-001",
  "qr_code": "FLE-SN-LNA-A001",
  "lote": "Lote Norte A",
  "estado": "SAL",
  "posicion": {
    "fila": 1,
    "columna": 1
  }
}
```

---

### `POST /api/v1/arboles/actualizacion-masiva`

Actualiza múltiples árboles en una sola operación.

**Autenticación:** Requerida

**Permisos:** `arboles:actualizar`

**Request Body:**
```json
{
  "arboles_ids": [1234, 1235, 1236],
  "actualizacion": {
    "etapa_fenologica": "FLO",
    "observaciones": "Floración iniciada en todo el lote"
  }
}
```

**Response 200:**
```json
{
  "actualizados": 3,
  "errores": [],
  "timestamp": "2024-01-15T18:00:00Z"
}
```

---
