# API - Catálogos

> Endpoints para catálogos del sistema (cultivos, plagas, productos, etc.)

## Resumen

Catálogos compartidos de la plataforma con información agronómica base.

---

## Endpoints

### `GET /api/v1/catalogos/cultivos`

Lista cultivos disponibles.

**Autenticación:** Requerida

**Permisos:** Ninguno (público para usuarios autenticados)

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| tipo | string | No | frutal_hueso, frutal_pepita, citrico, tropical |
| buscar | string | No | Búsqueda por nombre |
| activo | boolean | No | Filtrar por estado activo |

**Response 200:**
```json
{
  "data": [
    {
      "codigo": "AGU",
      "nombre": "Aguacate",
      "nombre_cientifico": "Persea americana",
      "familia": "Lauraceae",
      "tipo": "frutal_hueso",
      "ciclo_anos": 1.0,
      "variedades": [
        "Hass",
        "Fuerte",
        "Criollo"
      ],
      "descripcion": "Fruto de alto valor nutricional y comercial",
      "imagen_url": "https://storage.agrogrid.io/cultivos/aguacate.jpg",
      "activo": true
    }
  ]
}
```

---

### `GET /api/v1/catalogos/cultivos/{codigo}`

Obtiene detalles de un cultivo.

**Autenticación:** Requerida

**Permisos:** Ninguno

**Response 200:**
```json
{
  "codigo": "AGU",
  "nombre": "Aguacate",
  "nombre_cientifico": "Persea americana",
  "familia": "Lauraceae",
  "tipo": "frutal_hueso",
  "ciclo_anos": 1.0,
  "variedades": ["Hass", "Fuerte", "Criollo"],
  "etapas_fenologicas": [
    {
      "codigo": "REP",
      "nombre": "Reposo",
      "descripcion": "Periodo de dormancia",
      "duracion_dias_aprox": 30
    },
    {
      "codigo": "BRO",
      "nombre": "Brotación",
      "descripcion": "Emisión de nuevos brotes",
      "duracion_dias_aprox": 15
    }
  ],
  "plagas_comunes": [15, 17, 22],
  "enfermedades_comunes": [5, 8],
  "requerimientos": {
    "temperatura_min_c": 15,
    "temperatura_max_c": 30,
    "precipitacion_mm_anual": 1200,
    "altitud_min_msnm": 800,
    "altitud_max_msnm": 2200,
    "ph_suelo_min": 6.0,
    "ph_suelo_max": 7.0
  }
}
```

---

### `GET /api/v1/catalogos/plagas`

Lista plagas del catálogo.

**Autenticación:** Requerida

**Permisos:** Ninguno

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| cultivo_codigo | string | No | Filtrar por cultivo |
| tipo | string | No | insecto, acaro, nematodo |
| buscar | string | No | Búsqueda por nombre |

**Response 200:**
```json
{
  "data": [
    {
      "id": 15,
      "codigo": "AGU-PL-001",
      "nombre_comun": "Trips",
      "nombre_cientifico": "Frankliniella spp.",
      "tipo": "insecto",
      "cultivos_afectados": ["AGU", "MAN", "LIM"],
      "descripcion": "Insecto pequeño que daña flores y frutos",
      "sintomas": [
        "Manchas plateadas en hojas",
        "Deformación de frutos",
        "Caída de flores"
      ],
      "control_recomendado": [
        "Monitoreo con trampas",
        "Control biológico",
        "Insecticidas selectivos"
      ],
      "productos_recomendados": [101, 103],
      "imagen_url": "https://storage.agrogrid.io/plagas/trips.jpg"
    }
  ]
}
```

---

### `GET /api/v1/catalogos/enfermedades`

Lista enfermedades del catálogo.

**Autenticación:** Requerida

**Permisos:** Ninguno

**Response 200:**
```json
{
  "data": [
    {
      "id": 5,
      "codigo": "AGU-ENF-001",
      "nombre_comun": "Antracnosis",
      "nombre_cientifico": "Colletotrichum gloeosporioides",
      "tipo": "hongo",
      "cultivos_afectados": ["AGU", "MAN"],
      "descripcion": "Enfermedad fúngica que afecta frutos",
      "sintomas": [
        "Manchas negras circulares en frutos",
        "Lesiones hundidas",
        "Pudrición del fruto"
      ],
      "control_recomendado": [
        "Poda sanitaria",
        "Aplicación de fungicidas",
        "Manejo de humedad"
      ],
      "productos_recomendados": [102, 104],
      "imagen_url": "https://storage.agrogrid.io/enfermedades/antracnosis.jpg"
    }
  ]
}
```

---

### `GET /api/v1/catalogos/productos-fitosanitarios`

Lista productos fitosanitarios del catálogo.

**Autenticación:** Requerida

**Permisos:** Ninguno

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| tipo | string | No | insecticida, fungicida, herbicida, etc. |
| buscar | string | No | Búsqueda por nombre |

**Response 200:**
```json
{
  "data": [
    {
      "id": 101,
      "tipo": "insecticida",
      "nombre_comercial": "Insecticida XYZ",
      "ingrediente_activo": "Imidacloprid",
      "concentracion": "350 SC",
      "fabricante": "AgroTech S.A.",
      "registro_sanitario": "RS-12345",
      "categoria_toxicologica": "III",
      "periodo_carencia_dias": 14,
      "modo_accion": "Sistémico",
      "plagas_controladas": [15, 17],
      "cultivos_autorizados": ["AGU", "MAN", "LIM"]
    }
  ]
}
```

---

### `GET /api/v1/catalogos/unidades-medida`

Lista unidades de medida del sistema.

**Autenticación:** Requerida

**Permisos:** Ninguno

**Response 200:**
```json
{
  "data": [
    {
      "codigo": "kg",
      "nombre": "Kilogramo",
      "tipo": "peso",
      "simbolo": "kg"
    },
    {
      "codigo": "L",
      "nombre": "Litro",
      "tipo": "volumen",
      "simbolo": "L"
    },
    {
      "codigo": "ha",
      "nombre": "Hectárea",
      "tipo": "area",
      "simbolo": "ha",
      "equivalencia_m2": 10000
    }
  ]
}
```

---
