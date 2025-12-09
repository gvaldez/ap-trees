# API - Aplicaciones

> Endpoints para aplicaciones fitosanitarias y cálculo de dosis

## Resumen

Gestión de aplicaciones de productos fitosanitarios, fertilizantes y bioestimulantes con cálculo automático de dosis.

---

## Endpoints

### `GET /api/v1/aplicaciones`

Lista aplicaciones del tenant.

**Autenticación:** Requerida

**Permisos:** `aplicaciones:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| finca_id | integer | No | Filtrar por finca |
| estado | string | No | programada, en_progreso, completada, cancelada |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |

**Response 200:**
```json
{
  "data": [
    {
      "id": 456,
      "fecha_programada": "2024-01-18",
      "tipo": "fitosanitaria",
      "metodo": "foliar",
      "lote": {
        "id": 25,
        "nombre": "Lote Norte A"
      },
      "productos_count": 2,
      "arboles_objetivo": 50,
      "estado": "programada",
      "aprobado": true
    }
  ]
}
```

---

### `POST /api/v1/aplicaciones`

Crea/programa una nueva aplicación.

**Autenticación:** Requerida

**Permisos:** `aplicaciones:crear`

**Request Body:**
```json
{
  "fecha_programada": "2024-01-18",
  "tipo": "fitosanitaria",
  "metodo": "foliar",
  "lote_id": 25,
  "problema_objetivo_id": 15,
  "arboles_objetivo": [1234, 1235, 1236],
  "productos": [
    {
      "producto_id": 101,
      "cantidad": 500,
      "unidad": "ml",
      "orden_mezcla": 1
    },
    {
      "producto_id": 102,
      "cantidad": 200,
      "unidad": "ml",
      "orden_mezcla": 2
    }
  ],
  "volumen_total_mezcla_litros": 100,
  "observaciones": "Aplicar en horas de la mañana"
}
```

**Response 201:**
```json
{
  "id": 456,
  "fecha_programada": "2024-01-18",
  "estado": "programada",
  "requiere_aprobacion": true,
  "created_at": "2024-01-15T18:00:00Z"
}
```

---

### `POST /api/v1/aplicaciones/calcular-dosis`

Calcula dosis recomendadas para una aplicación.

**Autenticación:** Requerida

**Permisos:** `aplicaciones:crear`

**Request Body:**
```json
{
  "cultivo_codigo": "AGU",
  "problema_id": 15,
  "producto_id": 101,
  "arboles_count": 50,
  "metodo": "foliar",
  "litros_por_arbol": 2
}
```

**Response 200:**
```json
{
  "producto": {
    "id": 101,
    "nombre": "Insecticida XYZ",
    "concentracion": "480 g/L"
  },
  "dosis_recomendada": {
    "dosis_por_litro_ml": 5,
    "dosis_minima_ml": 4,
    "dosis_maxima_ml": 6
  },
  "calculo": {
    "arboles": 50,
    "litros_por_arbol": 2,
    "volumen_total_litros": 100,
    "producto_total_ml": 500,
    "producto_total_litros": 0.5
  },
  "costo_estimado": {
    "costo_producto": 75.50,
    "costo_por_arbol": 1.51,
    "moneda": "USD"
  },
  "recomendaciones": [
    "Aplicar en horas de baja radiación solar",
    "No aplicar si se prevé lluvia en las próximas 4 horas"
  ]
}
```

---

### `PUT /api/v1/aplicaciones/{id}/ejecutar`

Marca una aplicación como ejecutada.

**Autenticación:** Requerida

**Permisos:** `aplicaciones:actualizar`

**Request Body:**
```json
{
  "fecha_ejecutada": "2024-01-18T07:30:00Z",
  "ejecutado_por_id": 45,
  "condiciones_climaticas": {
    "temperatura_c": 24,
    "humedad_porcentaje": 65,
    "viento_kmh": 8
  },
  "arboles_completados": [1234, 1235, 1236],
  "observaciones": "Aplicación completada sin incidencias"
}
```

**Response 200:**
```json
{
  "id": 456,
  "estado": "completada",
  "fecha_ejecutada": "2024-01-18T07:30:00Z",
  "updated_at": "2024-01-18T08:00:00Z"
}
```

---
