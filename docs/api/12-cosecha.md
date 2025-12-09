# API - Cosecha

> Endpoints para registro y análisis de cosechas

## Resumen

Registro de cosechas por árbol, lote o finca, con análisis de producción y rendimiento.

---

## Endpoints

### `GET /api/v1/cosecha/registros`

Lista registros de cosecha.

**Autenticación:** Requerida

**Permisos:** `cosecha:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| finca_id | integer | No | Filtrar por finca |
| lote_id | integer | No | Filtrar por lote |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |

**Response 200:**
```json
{
  "data": [
    {
      "id": 678,
      "fecha": "2024-01-15",
      "lote": {
        "id": 25,
        "nombre": "Lote Norte A"
      },
      "arboles_cosechados": 50,
      "cantidad_total_kg": 225.5,
      "promedio_kg_arbol": 4.51,
      "calidad": "primera",
      "cosechador": {
        "id": 45,
        "nombre": "Carlos Ramírez"
      }
    }
  ]
}
```

---

### `POST /api/v1/cosecha/registros`

Registra una nueva cosecha.

**Autenticación:** Requerida

**Permisos:** `cosecha:crear`

**Request Body:**
```json
{
  "fecha": "2024-01-15",
  "lote_id": 25,
  "tipo_registro": "individual",
  "arboles": [
    {
      "arbol_id": 1234,
      "cantidad_kg": 4.5,
      "calidad": "primera",
      "observaciones": "Frutos de buen tamaño"
    },
    {
      "arbol_id": 1235,
      "cantidad_kg": 5.2,
      "calidad": "primera"
    }
  ],
  "cosechador_id": 45,
  "condiciones_climaticas": {
    "temperatura_c": 22,
    "humedad_porcentaje": 60
  },
  "observaciones": "Cosecha matutina"
}
```

**Response 201:**
```json
{
  "id": 678,
  "fecha": "2024-01-15",
  "arboles_cosechados": 2,
  "cantidad_total_kg": 9.7,
  "created_at": "2024-01-15T21:00:00Z"
}
```

---

### `GET /api/v1/cosecha/reportes/produccion`

Genera reporte de producción.

**Autenticación:** Requerida

**Permisos:** `cosecha:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| finca_id | integer | No | Filtrar por finca |
| lote_id | integer | No | Filtrar por lote |
| fecha_desde | date | Sí | Fecha inicial |
| fecha_hasta | date | Sí | Fecha final |
| agrupar_por | string | No | lote, mes, semana (default: lote) |

**Response 200:**
```json
{
  "periodo": {
    "fecha_inicio": "2024-01-01",
    "fecha_fin": "2024-01-31"
  },
  "resumen": {
    "cosecha_total_kg": 4500,
    "arboles_cosechados": 850,
    "promedio_kg_arbol": 5.29,
    "dias_cosecha": 12,
    "valor_estimado": 22500.00,
    "moneda": "USD"
  },
  "por_lote": [
    {
      "lote_id": 25,
      "lote_nombre": "Lote Norte A",
      "cosecha_kg": 1200,
      "arboles": 280,
      "promedio_kg_arbol": 4.29,
      "valor_estimado": 6000.00
    }
  ],
  "por_calidad": {
    "primera": 3600,
    "segunda": 750,
    "tercera": 150
  }
}
```

---

### `GET /api/v1/cosecha/arboles/{arbol_id}/historial`

Obtiene historial de cosecha de un árbol.

**Autenticación:** Requerida

**Permisos:** `cosecha:leer`

**Response 200:**
```json
{
  "arbol_id": 1234,
  "codigo": "A-001",
  "historial": [
    {
      "id": 678,
      "fecha": "2024-01-15",
      "cantidad_kg": 4.5,
      "calidad": "primera",
      "cosechador": "Carlos Ramírez"
    },
    {
      "id": 650,
      "fecha": "2023-12-01",
      "cantidad_kg": 4.8,
      "calidad": "primera",
      "cosechador": "María López"
    }
  ],
  "estadisticas": {
    "total_cosechas": 15,
    "total_kg": 68.5,
    "promedio_kg": 4.57,
    "mejor_cosecha_kg": 5.8,
    "ultima_cosecha": "2024-01-15"
  }
}
```

---
