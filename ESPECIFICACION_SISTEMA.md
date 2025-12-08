# 🌳 AgroGrid SaaS - Sistema de Gestión de Fincas Frutícolas

## Plataforma Multi-Cultivo de Control Árbol por Árbol

---

## 1. Resumen Ejecutivo

**AgroGrid** es una plataforma SaaS multi-tenant para la gestión integral de fincas frutícolas, permitiendo el seguimiento y control árbol por árbol. Diseñado como un servicio comercializable dentro de soluciones IoT para el agro, soporta múltiples tipos de cultivos (aguacate, durazno, manzana, limón, cítricos, etc.) con catálogos configurables de plagas, enfermedades y estados fenológicos por especie.

### Características Distintivas

| Característica | Descripción |
|----------------|-------------|
| **Multi-Tenant** | Múltiples clientes/fincas en una sola instancia |
| **Multi-Cultivo** | Soporte para diferentes especies frutícolas |
| **Catálogos Configurables** | Plagas, enfermedades y fenología por cultivo |
| **API de Agronomía de Precisión** | Integración con drones y cámaras multiespectrales |
| **Vista de Cuadrícula** | Visualización rápida estilo hoja cuadriculada |
| **White-Label Ready** | Personalizable para reventa |

### Inspiración y Benchmarks

Sistema inspirado en las mejores prácticas de:
- **Croptracker** - Trazabilidad y cumplimiento
- **Hectre** - Gestión de cosecha y calidad
- **Outfield** - Mapeo con drones y IA
- **Map My Crop** - Monitoreo satelital por árbol
- **Green Atlas** - Conteo de frutos con imaging
- **eOrchard** - Gestión integral de huertos

---

## 2. Modelo de Negocio SaaS

### 2.1 Arquitectura Multi-Tenant

```
┌─────────────────────────────────────────────────────────────────┐
│                    AGROGRID CLOUD (SaaS)                        │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐              │
│  │  Tenant A   │  │  Tenant B   │  │  Tenant C   │   ...        │
│  │  Finca      │  │  Finca      │  │  Finca      │              │
│  │  Aguacates  │  │  Manzanas   │  │  Cítricos   │              │
│  │  Colombia   │  │  Chile      │  │  México     │              │
│  └─────────────┘  └─────────────┘  └─────────────┘              │
├─────────────────────────────────────────────────────────────────┤
│                    SERVICIOS COMPARTIDOS                        │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│  │ Catálogo │ │   API    │ │    IA    │ │ Drones   │           │
│  │  Plagas  │ │  REST    │ │  ML/CV   │ │ Imagery  │           │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘           │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 Planes de Suscripción

| Plan | Árboles | Usuarios | Características | Precio/mes |
|------|---------|----------|-----------------|------------|
| **Starter** | Hasta 500 | 3 | Básico + Cuadrícula | $49 USD |
| **Professional** | Hasta 5,000 | 10 | + Reportes + API | $199 USD |
| **Enterprise** | Ilimitado | Ilimitado | + Drones + IA + White-label | Contactar |
| **IoT Bundle** | Ilimitado | Ilimitado | + Hardware + Soporte dedicado | Personalizado |

### 2.3 Jerarquía de Datos Multi-Tenant

```
Organización (Tenant)
├── Configuración (cultivos habilitados, branding)
├── Usuarios y Roles
├── Fincas
│   ├── Finca 1 (Aguacates)
│   │   ├── Sectores
│   │   │   └── Lotes
│   │   │       └── Árboles
│   │   └── Cultivo: Aguacate Hass
│   │       └── Catálogo: Plagas Aguacate
│   │
│   └── Finca 2 (Mixta)
│       ├── Sector Cítricos
│       │   └── Cultivo: Limón
│       └── Sector Frutales
│           └── Cultivo: Durazno
└── Integraciones (Drones, Sensores, ERP)
```

---

## 3. Catálogo Multi-Cultivo

### 3.1 Cultivos Soportados

| Cultivo | Nombre Científico | Variedades Comunes | Estado |
|---------|-------------------|-------------------|--------|
| 🥑 **Aguacate** | *Persea americana* | Hass, Fuerte, Criollo, Reed | ✅ Completo |
| 🍑 **Durazno** | *Prunus persica* | Elberta, O'Henry, Redhaven | ✅ Completo |
| 🍎 **Manzana** | *Malus domestica* | Gala, Fuji, Granny Smith | ✅ Completo |
| 🍋 **Limón** | *Citrus limon* | Eureka, Lisboa, Meyer | ✅ Completo |
| 🍊 **Naranja** | *Citrus sinensis* | Valencia, Navel, Sanguina | ✅ Completo |
| 🍐 **Pera** | *Pyrus communis* | Bartlett, Anjou, Bosc | 🔄 En desarrollo |
| 🍒 **Cereza** | *Prunus avium* | Bing, Rainier, Sweetheart | 🔄 En desarrollo |
| 🫒 **Olivo** | *Olea europaea* | Arbequina, Picual, Hojiblanca | 📋 Planificado |

### 3.2 Estructura del Catálogo de Cultivos

```json
{
  "cultivo_id": "aguacate",
  "nombre_comun": "Aguacate",
  "nombre_cientifico": "Persea americana",
  "familia": "Lauraceae",
  "variedades": [
    {
      "id": "hass",
      "nombre": "Hass",
      "caracteristicas": {
        "piel": "rugosa, oscura",
        "peso_promedio_g": "200-300",
        "tiempo_madurez_meses": "12-18"
      }
    }
  ],
  "etapas_fenologicas": ["reposo", "brotacion", "floracion", "cuajado", "desarrollo_fruto", "madurez", "cosecha"],
  "plagas_comunes": ["trips", "arana_roja", "barrenador"],
  "enfermedades_comunes": ["phytophthora", "antracnosis", "roña"],
  "calibres": [...],
  "requerimientos": {
    "temperatura_optima_c": "20-25",
    "precipitacion_mm_ano": "1200-1800",
    "ph_suelo": "5.5-7.0"
  }
}
```

---

## 4. Catálogo de Plagas y Enfermedades

### 4.1 Modelo de Datos

```sql
-- Catálogo maestro de plagas (compartido entre tenants)
CREATE TABLE catalogo_plagas (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(50) UNIQUE NOT NULL,
    nombre_comun VARCHAR(100) NOT NULL,
    nombre_cientifico VARCHAR(150),
    tipo VARCHAR(20) CHECK (tipo IN ('plaga', 'enfermedad', 'deficiencia', 'fisiopatia')),
    categoria VARCHAR(50), -- 'insecto', 'acaro', 'hongo', 'bacteria', 'virus', 'nematodo'
    descripcion TEXT,
    sintomas TEXT,
    imagen_url VARCHAR(500),
    activo BOOLEAN DEFAULT true
);

-- Relación plaga-cultivo con información específica
CREATE TABLE plaga_cultivo (
    id SERIAL PRIMARY KEY,
    plaga_id INTEGER REFERENCES catalogo_plagas(id),
    cultivo_id VARCHAR(50) NOT NULL,
    severidad_tipica VARCHAR(20), -- 'leve', 'moderada', 'severa', 'critica'
    frecuencia VARCHAR(20), -- 'rara', 'ocasional', 'frecuente', 'muy_frecuente'
    epoca_riesgo VARCHAR(100), -- 'verano', 'todo_año', 'epoca_lluvias'
    productos_recomendados JSONB,
    umbral_economico TEXT,
    notas_especificas TEXT
);

-- Estados de severidad por árbol
CREATE TABLE estados_arbol (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE NOT NULL,
    nombre VARCHAR(50) NOT NULL,
    color_hex VARCHAR(7) NOT NULL,
    icono VARCHAR(10),
    descripcion TEXT,
    accion_requerida TEXT,
    orden_severidad INTEGER -- 1=mejor, 10=peor
);
```

### 4.2 Catálogo de Plagas por Cultivo

#### 🥑 Aguacate

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `AGU-PL-001` | Trips (*Scirtothrips perseae*) | Plaga | Cicatrices en frutos, deformación | Moderada | Spinosad, Abamectina |
| `AGU-PL-002` | Araña roja (*Oligonychus punicae*) | Plaga | Amarillamiento hojas, telarañas | Moderada | Abamectina, Azufre |
| `AGU-PL-003` | Barrenador del hueso | Plaga | Galerías en semilla y pulpa | Severa | Clorpirifos, trampas |
| `AGU-PL-004` | Mosca del fruto | Plaga | Larvas en pulpa, pudrición | Severa | Spinosad, trampeo |
| `AGU-EN-001` | Phytophthora (*P. cinnamomi*) | Enfermedad | Marchitez, pudrición raíz | Crítica | Fosetil Al, Metalaxil |
| `AGU-EN-002` | Antracnosis (*Colletotrichum*) | Enfermedad | Manchas negras en frutos | Moderada | Cobre, Mancozeb |
| `AGU-EN-003` | Roña (*Sphaceloma perseae*) | Enfermedad | Costras en frutos y hojas | Leve | Cobre, Oxicloruro |
| `AGU-EN-004` | Fusariosis | Enfermedad | Marchitez vascular | Severa | Sin control químico efectivo |
| `AGU-DF-001` | Deficiencia de Zinc | Deficiencia | Hojas pequeñas, entrenudos cortos | Leve | Sulfato de Zinc foliar |
| `AGU-DF-002` | Deficiencia de Hierro | Deficiencia | Clorosis intervenal | Moderada | Quelatos de Fe |

#### 🍑 Durazno

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `DUR-PL-001` | Polilla oriental (*Grapholita molesta*) | Plaga | Galerías en brotes y frutos | Severa | Confusión sexual, Clorpirifos |
| `DUR-PL-002` | Pulgón verde | Plaga | Hojas enrolladas, melaza | Moderada | Imidacloprid, jabón potásico |
| `DUR-PL-003` | Mosca de la fruta | Plaga | Larvas en frutos | Severa | Spinosad, trampeo masivo |
| `DUR-EN-001` | Torque (*Taphrina deformans*) | Enfermedad | Hojas deformadas, rojizas | Moderada | Cobre en dormancia |
| `DUR-EN-002` | Monilia (*Monilinia fructicola*) | Enfermedad | Pudrición parda en frutos | Severa | Iprodione, Ciprodinil |
| `DUR-EN-003` | Oídio | Enfermedad | Polvo blanco en hojas | Leve | Azufre, Trifloxistrobin |
| `DUR-EN-004` | Gomosis bacteriana | Enfermedad | Exudado gomoso en tronco | Moderada | Cobre, poda sanitaria |

#### 🍎 Manzana

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `MAN-PL-001` | Carpocapsa (*Cydia pomonella*) | Plaga | Galerías hacia semillas | Severa | Confusión sexual, Clorpirifos |
| `MAN-PL-002` | Áfidos | Plaga | Hojas enrolladas, melaza | Moderada | Imidacloprid, depredadores |
| `MAN-PL-003` | Araña roja europea | Plaga | Bronceado de hojas | Moderada | Abamectina, Hexitiazox |
| `MAN-EN-001` | Sarna (*Venturia inaequalis*) | Enfermedad | Manchas oliváceas en hojas/frutos | Severa | Captan, Difenoconazol |
| `MAN-EN-002` | Oídio | Enfermedad | Polvo blanco en brotes | Moderada | Azufre, Miclobutanil |
| `MAN-EN-003` | Fuego bacteriano (*Erwinia*) | Enfermedad | Brotes quemados, exudado | Crítica | Cobre, eliminar tejido infectado |
| `MAN-EN-004` | Podredumbre amarga | Enfermedad | Manchas hundidas en frutos | Moderada | Captan, manejo postcosecha |

#### 🍋 Limón / Cítricos

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `CIT-PL-001` | Minador de hojas (*Phyllocnistis citrella*) | Plaga | Galerías serpenteantes en hojas | Moderada | Abamectina, Imidacloprid |
| `CIT-PL-002` | Psílido asiático (*Diaphorina citri*) | Plaga | Vector de HLB, brotes amarillos | Crítica | Imidacloprid, monitoreo intensivo |
| `CIT-PL-003` | Mosca blanca | Plaga | Fumagina, debilitamiento | Moderada | Aceites, Buprofezin |
| `CIT-PL-004` | Cochinilla acanalada | Plaga | Colonias en ramas, melaza | Moderada | Aceite, Buprofezin |
| `CIT-EN-001` | HLB/Huanglongbing | Enfermedad | Brotes amarillos, frutos deformes | Crítica | Sin cura - erradicación |
| `CIT-EN-002` | Gomosis (*Phytophthora*) | Enfermedad | Exudado en tronco, muerte | Severa | Fosetil Al, drenaje |
| `CIT-EN-003` | Mancha grasienta | Enfermedad | Manchas amarillas aceitosas | Leve | Cobre preventivo |
| `CIT-EN-004` | Cancrosis | Enfermedad | Lesiones elevadas en hojas/frutos | Moderada | Cobre, material certificado |

### 4.3 Estados del Árbol (Universales)

```yaml
estados_arbol:
  - codigo: "SAL"
    nombre: "Saludable"
    color: "#22C55E"  # Verde
    icono: "🟢"
    descripcion: "Árbol sin problemas detectados"
    accion: "Continuar monitoreo rutinario"
    severidad: 1

  - codigo: "ATE"
    nombre: "Atención"
    color: "#EAB308"  # Amarillo
    icono: "🟡"
    descripcion: "Síntomas leves detectados, requiere seguimiento"
    accion: "Inspección detallada en próxima visita"
    severidad: 2

  - codigo: "ADV"
    nombre: "Advertencia"
    color: "#F97316"  # Naranja
    icono: "🟠"
    descripcion: "Problema confirmado, intervención próxima"
    accion: "Programar tratamiento esta semana"
    severidad: 3

  - codigo: "CRI"
    nombre: "Crítico"
    color: "#EF4444"  # Rojo
    icono: "🔴"
    descripcion: "Requiere intervención inmediata"
    accion: "Tratamiento urgente hoy"
    severidad: 4

  - codigo: "TRA"
    nombre: "En Tratamiento"
    color: "#3B82F6"  # Azul
    icono: "🔵"
    descripcion: "Tratamiento activo en curso"
    accion: "Monitorear efectividad del tratamiento"
    severidad: 2

  - codigo: "REC"
    nombre: "En Recuperación"
    color: "#06B6D4"  # Cyan
    icono: "🩵"
    descripcion: "Post-tratamiento, en observación"
    accion: "Verificar recuperación completa"
    severidad: 2

  - codigo: "JOV"
    nombre: "Juvenil/Desarrollo"
    color: "#A855F7"  # Morado
    icono: "🟣"
    descripcion: "Árbol joven, aún no productivo"
    accion: "Cuidados de establecimiento"
    severidad: 1

  - codigo: "MUE"
    nombre: "Muerto/Removido"
    color: "#1F2937"  # Negro/Gris oscuro
    icono: "⚫"
    descripcion: "Árbol muerto o eliminado"
    accion: "Planificar replante si aplica"
    severidad: 5

  - codigo: "SIN"
    nombre: "Sin Inspeccionar"
    color: "#D1D5DB"  # Gris claro
    icono: "⬜"
    descripcion: "Pendiente de primera inspección"
    accion: "Incluir en próxima ronda"
    severidad: 0
```

---

## 5. API de Agronomía de Precisión

### 5.1 Integración con Drones y Cámaras Multiespectrales

```
┌─────────────────────────────────────────────────────────────────┐
│              ECOSISTEMA DE AGRONOMÍA DE PRECISIÓN               │
├──────────────────────────────────────────���──────────────────────┤
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │   DRONES    │    │  CÁMARAS    │    │  SENSORES   │         │
│  │   DJI/etc   │    │Multiespectral│    │    IoT     │         │
│  └──────┬──────┘    └──────┬──────┘    └──────┬──────┘         │
│         │                  │                  │                 │
│         └─────────────┬────┴─────────────────┘                 │
│                       ▼                                         │
│         ┌─────────────────────────────┐                        │
│         │    PRECISION AG API         │                        │
│         │    /api/v1/precision        │                        │
│         └─────────────┬───────────────┘                        │
│                       │                                         │
│    ┌─────────────────┼─────────────────┐                       │
│    ▼                 ▼                 ▼                       │
│ ┌──────────┐   ┌──────────┐   ┌──────────┐                    │
│ │  NDVI    │   │  NDRE    │   │ Thermal  │                    │
│ │ Analysis │   │ Analysis │   │ Analysis │                    │
│ └──────────┘   └──────────┘   └──────────┘                    │
│         │            │              │                          │
│         └────────────┼──────────────┘                          │
│                      ▼                                          │
│         ┌─────────────────────────────┐                        │
│         │    ML/AI PROCESSING         │                        │
│         │  - Detección de estrés      │                        │
│         │  - Predicción de plagas     │                        │
│         │  - Estimación de cosecha    │                        │
│         │  - Mapeo de variabilidad    │                        │
│         └─────────────┬───────────────┘                        │
│                       ▼                                         │
│         ┌─────────────────────────────┐                        │
│         │       AGROGRID CORE         │                        │
│         │  - Actualización cuadrícula │                        │
│         │  - Alertas automáticas      │                        │
│         │  - Recomendaciones          │                        │
│         └─────────────────────────────┘                        │
└─────────────────────────────────────────────────────────────────┘
```

### 5.2 Endpoints de la API de Precisión

```yaml
openapi: 3.0.0
info:
  title: AgroGrid Precision Agriculture API
  version: 1.0.0
  description: API para integración con sistemas de agronomía de precisión

paths:
  /api/v1/precision/flights:
    post:
      summary: Registrar nuevo vuelo de drone
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                finca_id: { type: integer }
                drone_model: { type: string }
                camera_type: { type: string, enum: [RGB, Multispectral, Thermal] }
                flight_date: { type: string, format: date-time }
                coverage_area_ha: { type: number }
                altitude_m: { type: number }
                overlap_percent: { type: number }

  /api/v1/precision/imagery/upload:
    post:
      summary: Subir imágenes de vuelo
      requestBody:
        content:
          multipart/form-data:
            schema:
              type: object
              properties:
                flight_id: { type: integer }
                images: { type: array, items: { type: string, format: binary } }
                metadata: { type: object }

  /api/v1/precision/analysis/ndvi:
    post:
      summary: Solicitar análisis NDVI
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                flight_id: { type: integer }
                lote_ids: { type: array, items: { type: integer } }
                generate_tree_level: { type: boolean }

  /api/v1/precision/analysis/results/{analysis_id}:
    get:
      summary: Obtener resultados de análisis
      responses:
        200:
          content:
            application/json:
              schema:
                type: object
                properties:
                  analysis_id: { type: integer }
                  status: { type: string }
                  ndvi_map_url: { type: string }
                  tree_health_scores:
                    type: array
                    items:
                      type: object
                      properties:
                        arbol_id: { type: integer }
                        ndvi_value: { type: number }
                        health_score: { type: number }
                        anomaly_detected: { type: boolean }
                        recommended_action: { type: string }

  /api/v1/precision/predictions/pest-risk:
    get:
      summary: Predicción de riesgo de plagas
      parameters:
        - name: lote_id
          in: query
          schema: { type: integer }
        - name: days_ahead
          in: query
          schema: { type: integer, default: 7 }
      responses:
        200:
          content:
            application/json:
              schema:
                type: object
                properties:
                  predictions:
                    type: array
                    items:
                      type: object
                      properties:
                        plaga_codigo: { type: string }
                        risk_level: { type: string }
                        probability: { type: number }
                        affected_zone: { type: object }
                        preventive_actions: { type: array }

  /api/v1/precision/grid/auto-update:
    post:
      summary: Actualizar cuadrícula automáticamente desde análisis
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                analysis_id: { type: integer }
                confidence_threshold: { type: number, default: 0.8 }
                create_alerts: { type: boolean, default: true }
```

### 5.3 Índices Espectrales Soportados

| Índice | Nombre | Fórmula | Uso Principal |
|--------|--------|---------|---------------|
| **NDVI** | Normalized Difference Vegetation Index | (NIR - Red) / (NIR + Red) | Salud general, vigor |
| **NDRE** | Normalized Difference Red Edge | (NIR - RedEdge) / (NIR + RedEdge) | Estrés temprano, N |
| **GNDVI** | Green NDVI | (NIR - Green) / (NIR + Green) | Clorofila, N |
| **SAVI** | Soil Adjusted Vegetation Index | ((NIR - Red) / (NIR + Red + L)) * (1 + L) | Áreas con suelo expuesto |
| **CWSI** | Crop Water Stress Index | Basado en temperatura canopy | Estrés hídrico |
| **EVI** | Enhanced Vegetation Index | G * ((NIR - Red) / (NIR + C1*Red - C2*Blue + L)) | Alta biomasa |

### 5.4 Flujo de Datos Automatizado

```
1. CAPTURA
   Drone vuela sobre la finca
   ↓
2. UPLOAD
   Imágenes subidas vía API
   ↓
3. PROCESAMIENTO
   - Ortomosaico
   - Cálculo de índices
   - Segmentación por árbol
   ↓
4. ANÁLISIS ML
   - Clasificación de salud
   - Detección de anomalías
   - Predicción de problemas
   ↓
5. INTEGRACIÓN
   - Actualización de cuadrícula
   - Generación de alertas
   - Recomendaciones
   ↓
6. ACCIÓN
   - Notificación a usuario
   - Asignación de tareas
   - Registro de intervención
```

---

## 6. Objetivos del Sistema

### 6.1 Objetivo General
Proporcionar una plataforma SaaS multi-tenant que permita a empresas agrícolas y proveedores de servicios IoT gestionar fincas frutícolas con monitoreo árbol por árbol, catálogos configurables por cultivo e integración con tecnologías de agricultura de precisión.

### 6.2 Objetivos Específicos
- ✅ Soportar múltiples clientes (tenants) en una sola instancia
- ✅ Permitir configuración de cultivos, plagas y estados por cliente
- ✅ Registrar y geolocalizar cada árbol individualmente
- ✅ Monitorear la salud y estado fitosanitario por árbol
- ✅ Visualizar el estado de la finca en formato de cuadrícula (filas x columnas)
- ✅ Integrar con drones y cámaras multiespectrales vía API
- ✅ Controlar aplicaciones de insumos (riego, fertilizantes, pesticidas)
- ✅ Registrar y proyectar cosechas por árbol
- ✅ Gestionar mano de obra y actividades de campo
- ✅ Generar reportes de trazabilidad para certificaciones
- ✅ Predecir rendimientos y riesgos mediante ML/IA

---

## 7. Módulos del Sistema

### 7.1 📍 Módulo de Mapeo y Geolocalización

| Funcionalidad | Descripción |
|---------------|-------------|
| **Registro de Árboles** | Alta individual con coordenadas GPS precisas |
| **Mapas Interactivos** | Visualización de la finca con capas de información |
| **Sectores y Lotes** | Organización jerárquica: Finca → Sector → Lote → Árbol |
| **Integración Satelital** | Importación de imágenes satelitales/drones |
| **QR/NFC por Árbol** | Etiquetado físico para escaneo en campo |

#### Datos por Árbol:
```json
{
  "arbol_id": "AGC-001-A-0234",
  "tenant_id": "finca_los_alamos",
  "coordenadas": {
    "latitud": 4.7110,
    "longitud": -74.0721
  },
  "cultivo": "aguacate",
  "variedad": "Hass",
  "fecha_siembra": "2019-03-15",
  "patron": "Criollo",
  "sector": "Norte",
  "lote": "A",
  "fila": 12,
  "columna": 34,
  "estado_actual": "SAL",
  "ultimo_ndvi": 0.78,
  "edad_anos": 6
}
```

---

### 7.2 🔲 Módulo de Vista de Cuadrícula (CORE)

> **💡 Funcionalidad inspirada en el método tradicional de hoja cuadriculada**, digitalizada para visualización rápida del estado de toda la finca.

Este módulo es el **corazón visual del sistema**, permitiendo ver el estado de cada árbol en una cuadrícula interactiva de filas y columnas.

#### Ejemplo de Vista de Cuadrícula - Lote A

```
                    LOTE A - Sector Norte - Aguacate Hass
                    Fecha: 2025-12-08 | NDVI promedio: 0.72
    
        Col→  1    2    3    4    5    6    7    8    9   10
      ┌─────────────────────────────────────────────────────────┐
Fila 1│  🟢   🟢   🟢   🟢   🟡   🟡   🔴   🔴   🟢   🟢  │
Fila 2│  🟢   🟢   🟢   🟡   🟡   🔴   🔴   🟠   🟢   🟢  │
Fila 3│  🟢   🟢   🟡   🟡   🔴   🔴   🟠   🟠   🟢   🟢  │
Fila 4│  🟢   🟢   🟢   🟡   🟠   🟠   🟢   🟢   🟢   🟢  │
Fila 5│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢  │
Fila 6│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢  │
Fila 7│  🟢   🟢   🔵   🔵   🟢   🟢   🟢   🟢   🟣   🟣  │
Fila 8│  🟢   🟢   🔵   🔵   🟢   🟢   🟢   🟢   🟣   🟣  │
      └─────────────────────────────────────────────────────────┘
      
      📊 Resumen: 80 árboles | 🟢 58 | 🟡 6 | 🟠 5 | 🔴 5 | 🔵 4 | 🟣 4
      ⚠️ ALERTA: Posible foco de Phytophthora en zona [F1-3, C5-8]
      🛰️ Última actualización drone: 2025-12-07
```

#### Funcionalidades de Cuadrícula
- Código de colores configurable por estado
- Capas de visualización (salud, fenología, NDVI, producción)
- Actualización manual desde app móvil
- Actualización automática desde análisis de drone
- Histórico y comparación temporal
- Análisis de propagación de plagas
- Exportación PDF, Excel, PNG, GIF animado

---

### 7.3 🌱 Módulo de Salud y Fenología

#### Etapas Fenológicas por Cultivo

Las etapas se cargan desde el catálogo según el cultivo seleccionado.

#### Monitoreo de Salud
| Indicador | Método de Captura | Frecuencia |
|-----------|-------------------|------------|
| Estado general | Inspección visual + app | Semanal |
| Plagas detectadas | Fotos + IA + Catálogo | Según hallazgo |
| Enfermedades | Diagnóstico + Catálogo | Según hallazgo |
| Índice NDVI | Drone/Satélite | Quincenal |
| Índice NDRE | Cámara multiespectral | Quincenal |
| Estrés hídrico (CWSI) | Cámara térmica | Según necesidad |

---

### 7.4 💧 Módulo de Riego y Fertirriego

*(Funcionalidades mantienen igual - configurable por cultivo)*

---

### 7.5 🧪 Módulo de Aplicaciones Fitosanitarias

#### Integración con Catálogo de Plagas

Al registrar un problema en un árbol:
1. Seleccionar tipo de problema (plaga/enfermedad/deficiencia)
2. Sistema filtra catálogo por cultivo del árbol
3. Usuario selecciona del catálogo o reporta nuevo
4. Sistema sugiere tratamientos recomendados
5. Registro de aplicación con trazabilidad

---

### 7.6 🍃 Módulo de Cosecha

#### Calibres Configurables por Cultivo

Los calibres se cargan desde la configuración del cultivo seleccionado.

---

### 7.7 👷 Módulo de Gestión de Personal

*(Funcionalidades mantienen igual)*

---

### 7.8 📊 Módulo de Reportes y Análisis

#### Reportes Adicionales
| Reporte | Descripción |
|---------|-------------|
| Análisis NDVI por lote | Mapas de calor de índice de vegetación |
| Predicción de plagas | Zonas de riesgo basadas en ML |
| Comparativo multi-cultivo | Rendimiento entre diferentes cultivos |
| Efectividad de tratamientos | Análisis por plaga y producto |

---

### 7.9 📱 Aplicación Móvil de Campo

#### Características Adicionales
- ✅ Selector de cultivo al registrar problemas
- ✅ Catálogo de plagas offline por cultivo
- ✅ Visualización de datos de drone/satélite
- ✅ Captura de fotos para análisis IA

---

### 7.10 🛰️ Módulo de Agronomía de Precisión (Addon)

Este módulo se conecta como addon y proporciona:

| Funcionalidad | Descripción |
|---------------|-------------|
| **Gestión de Vuelos** | Registro y planificación de vuelos de drone |
| **Upload de Imágenes** | Carga masiva de imágenes multiespectrales |
| **Procesamiento** | Generación de ortomosaicos e índices |
| **Análisis por Árbol** | Segmentación y métricas individuales |
| **Detección de Anomalías** | IA para identificar problemas |
| **Predicciones** | ML para riesgo de plagas y estimación de cosecha |
| **Auto-actualización** | Actualiza cuadrícula automáticamente |

---

## 8. Arquitectura Técnica

### 8.1 Stack Tecnológico

```
Frontend:
├── Web: Next.js 14 (App Router)
├── Móvil: React Native / Expo
├── Mapas: Mapbox GL JS
└── Gráficos: D3.js / Recharts

Backend:
├── API: Node.js (NestJS) / Python (FastAPI para ML)
├── Base de datos: PostgreSQL + PostGIS + TimescaleDB
├── Cache: Redis
├── Cola: RabbitMQ / Bull
└── Storage: S3-compatible

ML/AI:
├── Framework: PyTorch / TensorFlow
├── Procesamiento: GDAL, Rasterio
├── Modelos: YOLO (detección), CNN (clasificación)
└── MLOps: MLflow

Infraestructura:
├── Cloud: AWS / GCP / Azure
├── Kubernetes: EKS / GKE
├── CDN: CloudFlare
├── CI/CD: GitHub Actions
└── Monitoring: Grafana + Prometheus
```

### 8.2 Modelo de Datos Multi-Tenant

```sql
-- Tenant (Cliente/Organización)
CREATE TABLE tenants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nombre VARCHAR(100) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    plan VARCHAR(20) DEFAULT 'starter',
    configuracion JSONB,
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Cultivos habilitados por tenant
CREATE TABLE tenant_cultivos (
    tenant_id UUID REFERENCES tenants(id),
    cultivo_id VARCHAR(50) NOT NULL,
    configuracion_especifica JSONB,
    PRIMARY KEY (tenant_id, cultivo_id)
);

-- Todas las tablas principales tienen tenant_id
CREATE TABLE fincas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    nombre VARCHAR(100) NOT NULL,
    ...
);

-- Row Level Security para aislamiento
ALTER TABLE fincas ENABLE ROW LEVEL SECURITY;
CREATE POLICY tenant_isolation ON fincas
    USING (tenant_id = current_setting('app.current_tenant')::UUID);
```

---

## 9. Plan de Implementación

### Fase 1: MVP (3 meses)
- [ ] Arquitectura multi-tenant básica
- [ ] Módulo de registro de árboles y mapeo
- [ ] **🔲 Vista de cuadrícula básica (PRIORIDAD ALTA)**
- [ ] Catálogo de cultivos (Aguacate inicial)
- [ ] Catálogo de plagas básico
- [ ] Aplicación móvil con funcionalidad offline
- [ ] Dashboard básico

### Fase 2: Core (3 meses)
- [ ] Catálogos completos (Durazno, Manzana, Limón)
- [ ] Módulo de aplicaciones fitosanitarias con catálogo
- [ ] **🔲 Historial de cuadrícula y análisis de propagación**
- [ ] Gestión de riego y fertirriego
- [ ] Módulo de personal
- [ ] API REST completa
- [ ] Planes de suscripción y billing

### Fase 3: Precisión (3 meses)
- [ ] **🛰️ API de Agronomía de Precisión**
- [ ] Integración con drones (DJI SDK)
- [ ] Procesamiento de imágenes multiespectrales
- [ ] Análisis NDVI/NDRE por árbol
- [ ] **🔲 Auto-actualización de cuadrícula desde drone**
- [ ] Modelos ML para detección de anomalías

### Fase 4: IA y Predicción (3 meses)
- [ ] Predicción de riesgo de plagas
- [ ] Estimación de cosecha con ML
- [ ] Predicción de propagación
- [ ] Recomendaciones automatizadas
- [ ] Integración con ERPs

### Fase 5: Escala (Continuo)
- [ ] Nuevos cultivos bajo demanda
- [ ] White-label para revendedores
- [ ] Marketplace de integraciones
- [ ] Expansión internacional

---

## 10. Métricas de Éxito

### Métricas de Producto
| Métrica | Objetivo Año 1 | Objetivo Año 2 |
|---------|----------------|----------------|
| Tenants activos | 20 | 100 |
| Árboles gestionados | 50,000 | 500,000 |
| Adopción app móvil | 80% | 95% |
| Precisión predicción plagas | 75% | 90% |
| Uptime SaaS | 99.5% | 99.9% |

### Métricas de Impacto (por cliente)
| Métrica | Objetivo |
|---------|----------|
| Reducción pérdidas por plagas | 20-30% |
| Aumento rendimiento | 10-15% |
| Reducción tiempo inspección | 60% |
| Detección temprana de problemas | -5 días promedio |

---

## 11. Resumen de Especificación

### Lo que construiremos:

| Componente | Descripción | Prioridad |
|------------|-------------|-----------|
| **Plataforma Multi-Tenant** | Arquitectura SaaS para múltiples clientes | 🔴 Alta |
| **Multi-Cultivo** | Soporte para aguacate, durazno, manzana, limón, etc. | 🔴 Alta |
| **Catálogo de Plagas** | Base de datos de plagas/enfermedades por cultivo | 🔴 Alta |
| **Estados Configurables** | Sistema de estados visuales por árbol | 🔴 Alta |
| **Vista de Cuadrícula** | Visualización estilo hoja cuadriculada | 🔴 Alta |
| **App Móvil** | Aplicación de campo con modo offline | 🔴 Alta |
| **API de Precisión** | Integración con drones/cámaras multiespectrales | 🟠 Media |
| **Análisis NDVI/NDRE** | Procesamiento de imágenes espectrales | 🟠 Media |
| **Predicción ML** | Modelos de riesgo y estimación | 🟡 Futura |
| **White-Label** | Personalización para reventa | 🟡 Futura |

---

## 12. Próximos Pasos

### Documentos a Crear:

1. **📋 Especificación Funcional Detallada**
   - User stories por módulo
   - Wireframes/mockups
   - Flujos de usuario
   - Casos de uso

2. **🗓️ Plan de Proyecto**
   - Cronograma detallado
   - Recursos necesarios
   - Dependencias
   - Riesgos y mitigación

3. **🏗️ Arquitectura Técnica**
   - Diagramas de arquitectura
   - Modelo de datos completo
   - Especificación de APIs
   - Plan de infraestructura

4. **💰 Business Case**
   - Modelo de precios
   - Proyecciones financieras
   - Análisis de competencia

---

**Repositorio:** `gvaldez/ap-trees`

*Documento generado el 2025-12-08*
*Versión 2.0 - Multi-Tenant, Multi-Cultivo, API de Precisión*