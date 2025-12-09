# API - Tareas y Planificación

> Endpoints para gestión de tareas y calendario de actividades

## Resumen

Sistema de tareas y planificación semanal para organizar y asignar actividades de campo.

---

## Endpoints

### `GET /api/v1/tareas`

Lista tareas del tenant.

**Autenticación:** Requerida

**Permisos:** `tareas:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| estado | string | No | pendiente, en_progreso, completada, cancelada |
| asignado_a | integer | No | ID del usuario asignado |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |
| prioridad | string | No | baja, media, alta, urgente |
| tipo | string | No | inspeccion, aplicacion, riego, poda, etc. |

**Response 200:**
```json
{
  "data": [
    {
      "id": 890,
      "titulo": "Inspección rutinaria Lote Norte A",
      "tipo": "inspeccion",
      "prioridad": "media",
      "estado": "pendiente",
      "fecha_inicio": "2024-01-16T08:00:00Z",
      "fecha_fin": "2024-01-16T12:00:00Z",
      "asignado_a": {
        "id": 42,
        "nombre": "Juan Pérez"
      },
      "lote": {
        "id": 25,
        "nombre": "Lote Norte A"
      }
    }
  ]
}
```

---

### `POST /api/v1/tareas`

Crea una nueva tarea.

**Autenticación:** Requerida

**Permisos:** `tareas:crear`

**Request Body:**
```json
{
  "titulo": "Poda de formación Lote Sur",
  "descripcion": "Realizar poda de formación en árboles jóvenes",
  "tipo": "poda",
  "prioridad": "media",
  "fecha_inicio": "2024-01-17T07:00:00Z",
  "fecha_fin": "2024-01-17T11:00:00Z",
  "duracion_estimada_horas": 4,
  "asignado_a_id": 45,
  "lote_id": 26,
  "arboles_objetivo": [1300, 1301, 1302],
  "requiere_herramientas": ["tijeras_poda", "escalera"],
  "observaciones": "Llevar equipo de seguridad"
}
```

**Response 201:**
```json
{
  "id": 891,
  "titulo": "Poda de formación Lote Sur",
  "estado": "pendiente",
  "created_at": "2024-01-15T22:00:00Z"
}
```

---

### `GET /api/v1/tareas/{id}`

Obtiene detalles de una tarea.

**Autenticación:** Requerida

**Permisos:** `tareas:leer`

**Response 200:**
```json
{
  "id": 890,
  "titulo": "Inspección rutinaria Lote Norte A",
  "descripcion": "Revisión mensual de salud de árboles",
  "tipo": "inspeccion",
  "prioridad": "media",
  "estado": "completada",
  "fecha_inicio": "2024-01-16T08:00:00Z",
  "fecha_fin": "2024-01-16T12:00:00Z",
  "fecha_inicio_real": "2024-01-16T08:15:00Z",
  "fecha_fin_real": "2024-01-16T11:45:00Z",
  "duracion_real_horas": 3.5,
  "asignado_a": {
    "id": 42,
    "nombre": "Juan Pérez"
  },
  "creado_por": {
    "id": 40,
    "nombre": "María López"
  },
  "lote": {
    "id": 25,
    "nombre": "Lote Norte A"
  },
  "progreso_porcentaje": 100,
  "resultados": {
    "arboles_inspeccionados": 50,
    "hallazgos": 8,
    "inspeccion_id": 789
  },
  "observaciones": "Tarea completada satisfactoriamente",
  "created_at": "2024-01-10T00:00:00Z",
  "updated_at": "2024-01-16T11:50:00Z"
}
```

---

### `PUT /api/v1/tareas/{id}`

Actualiza una tarea.

**Autenticación:** Requerida

**Permisos:** `tareas:actualizar`

**Request Body:** (parcial)
```json
{
  "estado": "en_progreso",
  "fecha_inicio_real": "2024-01-16T08:15:00Z",
  "progreso_porcentaje": 25,
  "observaciones": "Tarea iniciada"
}
```

**Response 200:**
```json
{
  "id": 890,
  "estado": "en_progreso",
  "updated_at": "2024-01-16T08:15:00Z"
}
```

---

### `POST /api/v1/tareas/{id}/completar`

Marca una tarea como completada.

**Autenticación:** Requerida

**Permisos:** `tareas:completar`

**Request Body:**
```json
{
  "fecha_fin_real": "2024-01-16T11:45:00Z",
  "resultados": {
    "arboles_inspeccionados": 50,
    "hallazgos": 8,
    "inspeccion_id": 789
  },
  "observaciones": "Tarea completada, 8 hallazgos registrados",
  "calificacion": 5
}
```

**Response 200:**
```json
{
  "id": 890,
  "estado": "completada",
  "fecha_fin_real": "2024-01-16T11:45:00Z",
  "updated_at": "2024-01-16T11:50:00Z"
}
```

---

### `GET /api/v1/tareas/calendario`

Obtiene calendario de tareas.

**Autenticación:** Requerida

**Permisos:** `tareas:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| fecha_inicio | date | Sí | Inicio del periodo |
| fecha_fin | date | Sí | Fin del periodo |
| usuario_id | integer | No | Filtrar por usuario |
| vista | string | No | dia, semana, mes (default: semana) |

**Response 200:**
```json
{
  "periodo": {
    "fecha_inicio": "2024-01-15",
    "fecha_fin": "2024-01-21",
    "vista": "semana"
  },
  "dias": [
    {
      "fecha": "2024-01-15",
      "dia_semana": "lunes",
      "tareas": [
        {
          "id": 890,
          "titulo": "Inspección Lote Norte A",
          "hora_inicio": "08:00",
          "hora_fin": "12:00",
          "tipo": "inspeccion",
          "prioridad": "media",
          "asignado_a": "Juan Pérez"
        }
      ],
      "carga_trabajo_horas": 4
    }
  ],
  "resumen": {
    "total_tareas": 15,
    "pendientes": 8,
    "en_progreso": 3,
    "completadas": 4,
    "carga_total_horas": 45
  }
}
```

---
