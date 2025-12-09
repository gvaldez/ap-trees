# API - Reportes

> Endpoints para generación y exportación de reportes

## Resumen

Sistema de generación de reportes con capacidad de exportación a PDF y Excel.

---

## Endpoints

### `GET /api/v1/reportes/tipos`

Lista tipos de reportes disponibles.

**Autenticación:** Requerida

**Permisos:** `reportes:leer`

**Response 200:**
```json
{
  "data": [
    {
      "codigo": "produccion",
      "nombre": "Reporte de Producción",
      "categoria": "produccion",
      "descripcion": "Análisis de cosechas y rendimiento",
      "parametros_requeridos": ["fecha_inicio", "fecha_fin"],
      "formatos_disponibles": ["pdf", "excel", "json"]
    },
    {
      "codigo": "salud_arboles",
      "nombre": "Reporte de Salud de Árboles",
      "categoria": "sanidad",
      "descripcion": "Estado fitosanitario de árboles",
      "parametros_requeridos": ["finca_id"],
      "formatos_disponibles": ["pdf", "excel", "json"]
    }
  ]
}
```

---

### `POST /api/v1/reportes/generar`

Genera un reporte.

**Autenticación:** Requerida

**Permisos:** `reportes:leer` o `reportes:avanzados`

**Request Body:**
```json
{
  "tipo_reporte": "produccion",
  "parametros": {
    "finca_id": 5,
    "fecha_inicio": "2024-01-01",
    "fecha_fin": "2024-01-31",
    "agrupar_por": "lote"
  },
  "formato": "json"
}
```

**Response 200:**
```json
{
  "reporte_id": "rep-20240115-001",
  "tipo": "produccion",
  "fecha_generacion": "2024-01-15T23:00:00Z",
  "parametros": {
    "finca_id": 5,
    "fecha_inicio": "2024-01-01",
    "fecha_fin": "2024-01-31"
  },
  "datos": {
    "resumen": {
      "cosecha_total_kg": 4500,
      "valor_total": 22500.00,
      "arboles_cosechados": 850,
      "promedio_kg_arbol": 5.29
    },
    "por_lote": [...]
  }
}
```

---

### `POST /api/v1/reportes/exportar`

Exporta un reporte a PDF o Excel.

**Autenticación:** Requerida

**Permisos:** `reportes:exportar`

**Request Body:**
```json
{
  "tipo_reporte": "produccion",
  "parametros": {
    "finca_id": 5,
    "fecha_inicio": "2024-01-01",
    "fecha_fin": "2024-01-31"
  },
  "formato": "pdf",
  "opciones": {
    "incluir_graficas": true,
    "orientacion": "horizontal",
    "tamano_hoja": "letter"
  }
}
```

**Response 200:**
```json
{
  "reporte_id": "rep-20240115-001",
  "formato": "pdf",
  "url_descarga": "https://storage.agrogrid.io/reportes/rep-20240115-001.pdf",
  "expira_en": "2024-01-16T23:00:00Z",
  "tamano_bytes": 245678
}
```

---

### `GET /api/v1/reportes/predefinidos/dashboard`

Obtiene datos para dashboard principal.

**Autenticación:** Requerida

**Permisos:** `reportes:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| finca_id | integer | No | Filtrar por finca |

**Response 200:**
```json
{
  "finca_id": 5,
  "fecha_actualizacion": "2024-01-15T23:00:00Z",
  "resumen": {
    "arboles_totales": 3450,
    "arboles_saludables": 3200,
    "tasa_salud": 92.75,
    "produccion_mes_kg": 1250,
    "valor_produccion": 6250.00
  },
  "alertas_activas": [
    {
      "tipo": "plaga",
      "mensaje": "15 árboles con síntomas de trips",
      "prioridad": "media",
      "lote_id": 25
    }
  ],
  "proximas_tareas": [
    {
      "id": 890,
      "titulo": "Inspección Lote Norte A",
      "fecha": "2024-01-16T08:00:00Z",
      "tipo": "inspeccion"
    }
  ]
}
```

---
