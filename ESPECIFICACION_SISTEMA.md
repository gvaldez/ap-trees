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


### 7.4 💧 Módulo de Infraestructura Hídrica y Riego

> **Sistema completo para gestionar fuentes de agua, distribución y fertirriego**

Este módulo permite mapear y monitorear toda la infraestructura hídrica de la finca, ya que problemas en el sistema de riego impactan directamente la salud de los árboles.

#### Componentes del Sistema Hídrico

```
┌─────────────────────────────────────────────────────────────────┐
│              INFRAESTRUCTURA HÍDRICA DE LA FINCA                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │   FUENTES   │    │   ALMACEN   │    │   DISTRIB   │         │
│  │   DE AGUA   │───▶│   AMIENTO   │───▶│   UCIÓN     │         │
│  └─────────────┘    └─────────────┘    └─────────────┘         │
│        │                  │                  │                  │
│   ┌────┴────┐       ┌────┴────┐       ┌────┴────┐              │
│   │ Pozo    │       │ Tanque  │       │ Válvulas│              │
│   │ Captación│       │ Reserv. │       │ Tuberías│              │
│   │ Río/Canal│       │ Laguna  │       │ Goteros │              │
│   └─────────┘       └─────────┘       └─────────┘              │
│                           │                  │                  │
│                           ▼                  ▼                  │
│                    ┌─────────────┐    ┌─────────────┐          │
│                    │   FERTIRR   │    │   ÁRBOL     │          │
│                    │   IEGO      │───▶│   🌳        │          │
│                    └─────────────┘    └─────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

#### Registro de Activos Hídricos

| Tipo de Activo | Datos Registrados | Monitoreo |
|----------------|-------------------|-----------|
| **Pozo** | Profundidad, caudal, bomba, nivel freático | Nivel de agua, horas bomba |
| **Captación** | Tipo (lluvia/río), capacidad, filtros | Volumen captado |
| **Tanque/Reservorio** | Capacidad (m³), material, ubicación | Nivel actual, calidad |
| **Bomba** | Potencia, marca, fecha instalación | Estado, horas funcionamiento |
| **Línea principal** | Diámetro, material, longitud, presión | Fugas, presión |
| **Válvulas** | Tipo, ubicación, sectores que controla | Estado abierto/cerrado |
| **Goteros/Aspersores** | Tipo, caudal, cantidad por árbol | Obstrucciones |

#### Modelo de Datos - Infraestructura Hídrica

```sql
-- Fuentes de agua
CREATE TABLE fuentes_agua (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    tipo VARCHAR(30), -- 'pozo', 'captacion_lluvia', 'captacion_rio', 'red_municipal'
    nombre VARCHAR(100),
    ubicacion_lat DECIMAL(10,8),
    ubicacion_lon DECIMAL(11,8),
    capacidad_m3_hora DECIMAL(10,2),
    profundidad_m DECIMAL(6,2), -- para pozos
    estado VARCHAR(20), -- 'activo', 'mantenimiento', 'inactivo'
    fecha_instalacion DATE,
    notas TEXT
);

-- Equipos de bombeo
CREATE TABLE bombas (
    id SERIAL PRIMARY KEY,
    fuente_id INTEGER REFERENCES fuentes_agua(id),
    marca VARCHAR(50),
    modelo VARCHAR(50),
    potencia_hp DECIMAL(6,2),
    caudal_lph DECIMAL(10,2),
    presion_psi DECIMAL(6,2),
    horas_funcionamiento INTEGER DEFAULT 0,
    fecha_instalacion DATE,
    proximo_mantenimiento DATE,
    estado VARCHAR(20)
);

-- Almacenamiento
CREATE TABLE almacenamiento_agua (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    tipo VARCHAR(30), -- 'tanque_elevado', 'tanque_superficie', 'reservorio', 'laguna'
    nombre VARCHAR(100),
    capacidad_m3 DECIMAL(10,2),
    nivel_actual_porcentaje DECIMAL(5,2),
    material VARCHAR(50),
    ubicacion_lat DECIMAL(10,8),
    ubicacion_lon DECIMAL(11,8)
);

-- Sectores de riego
CREATE TABLE sectores_riego (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    nombre VARCHAR(100),
    lotes_ids INTEGER[], -- lotes que cubre
    valvula_principal_id INTEGER,
    tipo_riego VARCHAR(30), -- 'goteo', 'microaspersion', 'aspersion', 'gravedad'
    caudal_total_lph DECIMAL(10,2),
    tiempo_riego_min INTEGER,
    frecuencia_dias INTEGER
);

-- Registro de riegos
CREATE TABLE registros_riego (
    id SERIAL PRIMARY KEY,
    sector_id INTEGER REFERENCES sectores_riego(id),
    fecha_inicio TIMESTAMP,
    fecha_fin TIMESTAMP,
    volumen_m3 DECIMAL(10,2),
    tipo VARCHAR(20), -- 'programado', 'manual', 'emergencia'
    con_fertirriego BOOLEAN DEFAULT false,
    mezcla_aplicada_id INTEGER,
    operador_id INTEGER REFERENCES usuarios(id),
    observaciones TEXT
);

-- Alertas de infraestructura
CREATE TABLE alertas_infraestructura (
    id SERIAL PRIMARY KEY,
    activo_tipo VARCHAR(30), -- 'bomba', 'valvula', 'tuberia', 'tanque'
    activo_id INTEGER,
    tipo_alerta VARCHAR(50), -- 'fuga', 'baja_presion', 'bomba_fallo', 'nivel_bajo'
    severidad VARCHAR(20),
    fecha_deteccion TIMESTAMP,
    fecha_resolucion TIMESTAMP,
    descripcion TEXT,
    afecta_lotes INTEGER[]
);
```

#### Vista de Infraestructura en Mapa

```
┌──────���──────────────────────────────────────────────────────────┐
│  🗺️ MAPA DE INFRAESTRUCTURA - Finca Los Alamos                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│      ⛰️ Zona Alta                                                │
│           │                                                     │
│        [💧Captación]───────────┐                                │
│           │                    │                                │
│        [🏊Reservorio 500m³]────┤                                │
│           │ 87%                │                                │
│      [🔌Bomba 1]              [🔌Bomba 2]                       │
│           │ ✅                  │ ⚠️                            │
│           └────────┬───────────┘                                │
│                    │                                            │
│           [Línea Principal 4"]                                  │
│                    │                                            │
│    ┌───────────────┼───────────────┐                           │
│    │               │               │                            │
│  [🚿Sector A]   [🚿Sector B]   [🚿Sector C]                     │
│    Goteo         Goteo          Micro                          │
│    ✅ 45psi      ✅ 42psi       ⚠️ 35psi                        │
│    │               │               │                            │
│  ┌─┴─┐          ┌──┴──┐        ┌──┴──┐                         │
│  │🌳│          │🌳🌳│        │🌳🌳🌳│                          │
│  │🌳│          │🌳🌳│        │🌳🌳🌳│  ← Árboles                │
│  └───┘          └────┘        └─────┘                          │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│ 📊 Estado: Reservorio 87% | Bomba 1 ✅ | Bomba 2 ⚠️ Revisar     │
│ ⚠️ Alerta: Sector C baja presión - Posible fuga línea 3        │
└─────────────────────────────────────────────────────────────────┘
```

#### Correlación Riego ↔ Salud de Árboles

El sistema detecta automáticamente correlaciones:

```yaml
alerta_correlacion:
  tipo: "problema_riego_afecta_arboles"
  fecha: "2025-12-08"
  hallazgo: |
    Detectados 15 árboles con estrés hídrico (🟡🟠) en Lote B, Filas 5-8
    Coincide con:
    - Sector de riego B con presión 35 psi (normal: 45 psi)
    - Última alerta de fuga en línea secundaria hace 3 días
  arboles_afectados: [B-5-1, B-5-2, B-5-3, ... , B-8-10]
  recomendacion: |
    1. Revisar línea secundaria Sector B para detectar fuga
    2. Programar riego de emergencia manual
    3. Reinspeccionar árboles en 5 días
```

---

### 7.5 🧪 Módulo de Aplicaciones con Cálculo de Dosis

> **Gestión completa de productos, cálculo automático de dosis y formulación de mezclas**

#### Catálogo de Productos

```sql
CREATE TABLE productos_agricolas (
    id SERIAL PRIMARY KEY,
    tipo VARCHAR(30), -- 'insecticida', 'fungicida', 'herbicida', 'fertilizante', 'bioestimulante', 'coadyuvante'
    nombre_comercial VARCHAR(100),
    ingrediente_activo VARCHAR(150),
    concentracion VARCHAR(50), -- ej: "480 g/L", "70% WP"
    presentacion VARCHAR(50), -- 'liquido', 'polvo', 'granulado'
    unidad_medida VARCHAR(20), -- 'ml', 'g', 'kg', 'L'
    fabricante VARCHAR(100),
    registro_sanitario VARCHAR(50),
    categoria_toxicologica VARCHAR(20), -- 'I', 'II', 'III', 'IV'
    periodo_carencia_dias INTEGER,
    periodo_reingreso_horas INTEGER,
    precio_unitario DECIMAL(10,2),
    stock_actual DECIMAL(10,2),
    stock_minimo DECIMAL(10,2),
    activo BOOLEAN DEFAULT true
);

-- Dosis recomendadas por cultivo y problema
CREATE TABLE dosis_recomendadas (
    id SERIAL PRIMARY KEY,
    producto_id INTEGER REFERENCES productos_agricolas(id),
    cultivo_id VARCHAR(50),
    plaga_enfermedad_id INTEGER REFERENCES catalogo_plagas(id),
    aplicacion_tipo VARCHAR(30), -- 'foliar', 'suelo', 'fertirriego', 'drench'
    dosis_minima DECIMAL(10,4),
    dosis_maxima DECIMAL(10,4),
    dosis_recomendada DECIMAL(10,4),
    unidad_dosis VARCHAR(30), -- 'ml/L', 'g/L', 'kg/ha', 'ml/árbol'
    frecuencia_aplicacion VARCHAR(50), -- 'cada 7 días', 'cada 14 días'
    max_aplicaciones_ciclo INTEGER,
    notas TEXT
);

-- Registro de aplicaciones
CREATE TABLE aplicaciones (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    fecha_programada DATE,
    fecha_ejecutada TIMESTAMP,
    tipo VARCHAR(30), -- 'fitosanitaria', 'fertilizacion', 'bioestimulante'
    metodo VARCHAR(30), -- 'foliar', 'fertirriego', 'drench', 'aplicacion_directa'
    estado VARCHAR(20), -- 'programada', 'en_progreso', 'completada', 'cancelada'
    problema_objetivo_id INTEGER, -- plaga/enfermedad que se trata
    creado_por INTEGER REFERENCES usuarios(id),
    aprobado_por INTEGER REFERENCES usuarios(id)
);

-- Mezcla/receta de la aplicación
CREATE TABLE aplicacion_mezcla (
    id SERIAL PRIMARY KEY,
    aplicacion_id INTEGER REFERENCES aplicaciones(id),
    producto_id INTEGER REFERENCES productos_agricolas(id),
    cantidad_producto DECIMAL(10,3),
    unidad VARCHAR(20),
    orden_mezcla INTEGER, -- orden en que se agrega a la mezcla
    notas TEXT
);

-- Árboles/lotes objetivo
CREATE TABLE aplicacion_objetivos (
    id SERIAL PRIMARY KEY,
    aplicacion_id INTEGER REFERENCES aplicaciones(id),
    tipo_objetivo VARCHAR(20), -- 'arbol', 'lote', 'sector'
    objetivo_id INTEGER,
    cantidad_aplicada DECIMAL(10,3),
    completado BOOLEAN DEFAULT false,
    ejecutado_por INTEGER REFERENCES usuarios(id),
    fecha_ejecucion TIMESTAMP
);
```

#### Calculadora de Dosis

```
┌─────────────────────────────────────────────────────────────────┐
│  🧮 CALCULADORA DE DOSIS - Nueva Aplicación                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📋 DATOS DE LA APLICACIÓN                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tipo: [Fitosanitaria ▼]  Método: [Foliar ▼]                ││
│  │ Problema: [Trips - AGU-PL-001 ▼]                            ││
│  │                                                             ││
│  │ Área de aplicación:                                         ││
│  │ ○ Lote completo: [Lote A ▼] → 500 árboles                  ││
│  │ ● Selección: 45 árboles seleccionados en cuadrícula        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 CÁLCULO DE VOLUMEN                                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Árboles a tratar: 45                                        ││
│  │ Volumen por árbol: [3] L (según edad/tamaño)               ││
│  │ ─────────────────────────────────────                       ││
│  │ Volumen total de mezcla: 135 L                              ││
│  │ Tanques de 200L necesarios: 1 (usar 135L)                  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🧪 PRODUCTOS Y DOSIS                                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ [+ Agregar producto]                                        ││
│  │                                                             ││
│  │ 1. Spinosad (Success 480 SC)                                ││
│  │    Dosis recomendada: 0.3 ml/L (rango: 0.2-0.4 ml/L)       ││
│  │    Para 135L → 40.5 ml                            [Calcular]││
│  │                                                             ││
│  │ 2. Coadyuvante (Inex-A)                                     ││
│  │    Dosis: 0.5 ml/L                                          ││
│  │    Para 135L → 67.5 ml                            [Calcular]││
│  │                                                             ││
│  │ 3. [+ Agregar otro producto]                                ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📝 RESUMEN DE MEZCLA                                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Para preparar 135 L de mezcla:                              ││
│  │                                                             ││
│  │ 1. Llenar tanque con 100 L de agua                          ││
│  │ 2. Agregar 40.5 ml de Success 480 SC, agitar               ││
│  │ 3. Agregar 67.5 ml de Inex-A, agitar                        ││
│  │ 4. Completar a 135 L con agua                               ││
│  │                                                             ││
│  │ ⚠️ EPP requerido: Guantes, máscara, gafas                   ││
│  │ ⏱️ Periodo de reingreso: 4 horas                            ││
│  │ 📅 Periodo de carencia: 7 días                              ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💰 COSTO ESTIMADO                                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Success 480 SC: 40.5 ml × $0.85/ml = $34.43                ││
│  │ Inex-A: 67.5 ml × $0.05/ml = $3.38                         ││
│  │ ─────────────────────────────────────                       ││
│  │ Total productos: $37.81                                     ││
│  │ Costo por árbol: $0.84                                      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [Guardar borrador] [Crear y asignar tareas] [Programar]       │
└─────────────────────────────────────────────────────────────────┘
```

#### Generación de Instrucciones para Trabajadores

Al crear una aplicación, el sistema genera instrucciones detalladas:

```yaml
tarea_aplicacion:
  id: "APL-2025-0089"
  titulo: "Aplicación foliar contra Trips - Lote A zona crítica"
  fecha_programada: "2025-12-09"
  asignado_a: "Juan Pérez"
  
  instructivo:
    preparacion:
      - "Verificar EPP completo: guantes nitrilo, máscara con filtro, gafas, overol"
      - "Llevar bomba de espalda 20L calibrada"
      - "Recoger productos en bodega (ya medidos en envases)"
    
    mezcla:
      volumen_total: "20 L por carga (7 cargas total)"
      pasos:
        - orden: 1
          accion: "Llenar bomba con 15 L de agua limpia"
        - orden: 2
          accion: "Agregar 6 ml de Success 480 SC (medir con jeringa)"
          producto: "Success 480 SC"
          cantidad: "6 ml"
        - orden: 3
          accion: "Agitar por 30 segundos"
        - orden: 4
          accion: "Agregar 10 ml de Inex-A"
          producto: "Inex-A"
          cantidad: "10 ml"
        - orden: 5
          accion: "Completar a 20 L y agitar"
    
    aplicacion:
      arboles_objetivo: "45 árboles marcados en cuadrícula (zona roja)"
      ubicacion: "Lote A, Filas 1-3, Columnas 5-8"
      tecnica: "Aplicar cubriendo follaje completo, énfasis en envés de hojas"
      volumen_por_arbol: "3 L aproximadamente"
      hora_recomendada: "6:00-9:00 AM o 5:00-7:00 PM (evitar sol directo)"
    
    seguridad:
      epp_obligatorio: ["Guantes nitrilo", "Máscara con filtro", "Gafas", "Overol manga larga"]
      periodo_reingreso: "4 horas - señalizar área"
      primeros_auxilios: "En caso de contacto con piel, lavar con agua abundante"
      telefono_emergencia: "+52 123 456 7890"
    
    registro:
      - "Marcar en app cada árbol aplicado"
      - "Tomar foto de mezcla preparada"
      - "Reportar cualquier anomalía"
```

---

### 7.6 📋 Módulo de Planificación Semanal y Mantenimiento

> **Planificación inteligente de labores periódicas, generación automática de tareas y lista de compras**

#### Tipos de Labores de Mantenimiento

```yaml
labores_mantenimiento:
  infraestructura:
    - codigo: "MNT-CAM"
      nombre: "Mantenimiento de caminos"
      descripcion: "Reparación de baches, limpieza de cunetas"
      frecuencia_default: "mensual"
      duracion_estimada_hrs: 8
      requiere_insumos: true
      insumos_tipicos: ["Grava", "Material de relleno"]
    
    - codigo: "MNT-CER"
      nombre: "Revisión de cercas"
      descripcion: "Reparación de cercas perimetrales"
      frecuencia_default: "quincenal"
      duracion_estimada_hrs: 4
      requiere_insumos: true
      insumos_tipicos: ["Alambre", "Grapas", "Postes"]
    
    - codigo: "MNT-CAP"
      nombre: "Limpieza captación de agua"
      descripcion: "Limpieza de filtros y canales de captación"
      frecuencia_default: "semanal"
      duracion_estimada_hrs: 2
      requiere_insumos: false
    
    - codigo: "MNT-TAN"
      nombre: "Limpieza de tanques"
      descripcion: "Vaciado y limpieza de tanques de almacenamiento"
      frecuencia_default: "trimestral"
      duracion_estimada_hrs: 6
      requiere_insumos: true
      insumos_tipicos: ["Cloro", "Cepillos"]
  
  vegetacion:
    - codigo: "MNT-PAS"
      nombre: "Corte de pasto/maleza"
      descripcion: "Control de maleza entre hileras y orillas"
      frecuencia_default: "quincenal"
      duracion_estimada_hrs: 16
      requiere_insumos: true
      insumos_tipicos: ["Combustible desbrozadora", "Hilo nylon"]
    
    - codigo: "MNT-POD"
      nombre: "Poda de formación"
      descripcion: "Poda de árboles jóvenes"
      frecuencia_default: "anual"
      duracion_estimada_hrs: 40
      requiere_insumos: true
      insumos_tipicos: ["Pasta cicatrizante", "Herramientas"]
  
  equipo:
    - codigo: "MNT-BOM"
      nombre: "Mantenimiento bombas"
      descripcion: "Revisión y mantenimiento preventivo de bombas"
      frecuencia_default: "mensual"
      duracion_estimada_hrs: 3
      requiere_insumos: true
      insumos_tipicos: ["Aceite", "Filtros"]
    
    - codigo: "MNT-RIE"
      nombre: "Revisión sistema de riego"
      descripcion: "Limpieza de filtros, revisión de goteros"
      frecuencia_default: "semanal"
      duracion_estimada_hrs: 4
      requiere_insumos: true
      insumos_tipicos: ["Goteros repuesto", "Conectores"]
```

#### Programación de Labores Periódicas

```sql
-- Plantillas de labores periódicas
CREATE TABLE labores_periodicas (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    codigo_labor VARCHAR(20),
    nombre VARCHAR(100),
    descripcion TEXT,
    frecuencia VARCHAR(20), -- 'diaria', 'semanal', 'quincenal', 'mensual', 'trimestral', 'anual'
    dia_semana_preferido INTEGER, -- 0=domingo, 1=lunes, etc
    semana_mes_preferida INTEGER, -- 1,2,3,4 para frecuencias mensuales
    duracion_estimada_hrs DECIMAL(5,2),
    trabajadores_requeridos INTEGER DEFAULT 1,
    prioridad VARCHAR(20) DEFAULT 'normal', -- 'alta', 'normal', 'baja'
    activa BOOLEAN DEFAULT true,
    ultima_ejecucion DATE,
    proxima_ejecucion DATE
);

-- Insumos requeridos por labor
CREATE TABLE labor_insumos (
    id SERIAL PRIMARY KEY,
    labor_id INTEGER REFERENCES labores_periodicas(id),
    producto_id INTEGER REFERENCES productos_agricolas(id),
    cantidad_estimada DECIMAL(10,2),
    unidad VARCHAR(20),
    es_opcional BOOLEAN DEFAULT false
);

-- Tareas generadas
CREATE TABLE tareas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID,
    finca_id INTEGER,
    tipo VARCHAR(30), -- 'aplicacion', 'mantenimiento', 'inspeccion', 'cosecha', 'otro'
    origen VARCHAR(30), -- 'programada', 'manual', 'alerta', 'planificacion_semanal'
    labor_periodica_id INTEGER REFERENCES labores_periodicas(id),
    aplicacion_id INTEGER REFERENCES aplicaciones(id),
    titulo VARCHAR(200),
    descripcion TEXT,
    instructivo JSONB, -- instrucciones detalladas
    fecha_programada DATE,
    hora_inicio TIME,
    duracion_estimada_hrs DECIMAL(5,2),
    prioridad VARCHAR(20),
    estado VARCHAR(20), -- 'pendiente', 'asignada', 'en_progreso', 'completada', 'cancelada'
    asignado_a INTEGER REFERENCES usuarios(id),
    completada_por INTEGER REFERENCES usuarios(id),
    fecha_completada TIMESTAMP,
    notas_ejecucion TEXT,
    fotos_evidencia TEXT[] -- URLs de fotos
);
```

#### Pantalla de Planificación Semanal (Lunes)

```
┌─────────────────────────────────────────────────────────────────┐
│  📅 PLANIFICACIÓN SEMANAL - Semana del 9 al 15 de Diciembre     │
│  Finca: Los Alamos | Generado: Lunes 9 Dic 2025, 6:00 AM        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN DE LA SEMANA                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tareas programadas: 12                                       ││
│  │ Horas totales estimadas: 45 hrs                             ││
│  │ Trabajadores disponibles: 4                                  ││
│  │ Capacidad: 160 hrs (4 × 40hrs) → Utilización: 28%           ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔴 TAREAS URGENTES (vencidas o críticas)                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ⚠️ APL-0089 - Aplicación Trips zona crítica                 ││
│  │    Programada: Hoy | Asignado: Juan Pérez                   ││
│  │    [Ver detalles] [Reasignar]                               ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📋 LABORES PERIÓDICAS ESTA SEMANA                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ☑️ Lun - Limpieza captación de agua (2 hrs)      [Asignar]  ││
│  │ ☑️ Lun - Revisión sistema de riego (4 hrs)       [Asignar]  ││
│  │ ☐ Mar - Corte de pasto Sector A (8 hrs)          [Asignar]  ││
│  │ ☐ Mié - Corte de pasto Sector B (8 hrs)          [Asignar]  ││
│  │ ☐ Jue - Inspección general árboles (6 hrs)       [Asignar]  ││
│  │ ☐ Vie - Mantenimiento bomba #1 (3 hrs)           [Asignar]  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🌳 LABORES DE CULTIVO                                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ☐ Aplicación preventiva Lote B (programada)     [Ver]       ││
│  │ ☐ Fertilización Sector Norte (fertirriego)      [Ver]       ││
│  │ ☐ Re-inspección árboles tratados (seguimiento)  [Ver]       ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🛒 INSUMOS REQUERIDOS ESTA SEMANA                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Producto              │ Necesario │ Stock │ Comprar │ Costo ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ Combustible desbroz.  │ 10 L      │ 3 L   │ 7 L     │ $42   ││
│  │ Hilo nylon 3mm        │ 2 rollos  │ 0     │ 2       │ $24   ││
│  │ Success 480 SC        │ 100 ml    │ 250ml │ -       │ -     ││
│  │ Inex-A                │ 150 ml    │ 500ml │ -       │ -     ││
│  │ Goteros repuesto      │ 20 uds    │ 5     │ 15      │ $15   ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │                                   TOTAL COMPRAS: │ $81      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📋 Generar lista de compras] [📤 Enviar plan a trabajadores]  │
│  [✏️ Ajustar plan] [📊 Ver calendario]                          │
└─────────────────────────────────────────────────────────────────┘
```

#### Lista de Compras Generada

```
┌─────────────────────────────────────────────────────────────────┐
│  🛒 LISTA DE COMPRAS - Semana 9-15 Diciembre 2025               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Generada automáticamente basada en:                            │
│  • Labores programadas de la semana                             │
│  • Stock actual en bodega                                       │
│  • Stock mínimo configurado                                     │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ PARA COMPRAR URGENTE (stock bajo mínimo)                    ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ □ Hilo nylon 3mm × 2 rollos           $24.00                ││
│  │   → Stock: 0 | Mínimo: 2 | Proveedor: AgroInsumos           ││
│  │                                                             ││
│  │ □ Goteros repuesto × 15 uds           $15.00                ││
│  │   → Stock: 5 | Mínimo: 20 | Proveedor: RiegoTec             ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ PARA LABORES DE LA SEMANA                                   ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ □ Combustible desbrozadora × 7 L      $42.00                ││
│  │   → Para: Corte de pasto (Mar-Mié)                          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ RECOMENDADO (stock acercándose al mínimo)                   ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ □ Aceite bomba × 2 L                  $18.00                ││
│  │   → Stock: 3 L | Mínimo: 2 L | Próx. uso: 2 semanas        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  TOTAL URGENTE:        $39.00                                   │
│  TOTAL SEMANA:         $81.00                                   │
│  TOTAL + RECOMENDADO:  $99.00                                   │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  [📧 Enviar a proveedor] [📱 Compartir WhatsApp] [🖨️ Imprimir] │
│  [✏️ Editar lista] [✅ Marcar como comprado]                    │
└─────────────────────────────────────────────────────────────────┘
```

---

### 7.7 📱 Aplicación Móvil de Campo (Ligera)

> **App minimalista enfocada en captura rápida de datos y consulta de instrucciones**

#### Principios de Diseño

| Principio | Implementación |
|-----------|----------------|
| **Ultra ligera** | APK < 15 MB, funciona en 3G |
| **Offline first** | Sincroniza cuando hay conexión |
| **Mínimos clics** | Máximo 3 taps para cualquier acción |
| **Batería** | Optimizada para uso prolongado en campo |
| **Legible** | Texto grande, iconos claros, contraste alto |

#### Funcionalidades CORE (únicas)

```
┌─────────────────────────────────────────┐
│  📱 AGROGRID CAMPO v1.0                 │
│  👤 Juan Pérez | Finca Los Alamos       │
├─────────────────────────────────────────┤
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  📋 MIS TAREAS HOY                  ││
│  │                                     ││
│  │  🔴 Aplicación Trips (urgente)      ││
│  │     Lote A, 45 árboles              ││
│  │     [Ver instrucciones] [Iniciar]   ││
│  │                                     ││
│  │  🟡 Revisión sistema riego          ││
│  │     Sector B                        ││
│  │     [Ver instrucciones] [Iniciar]   ││
│  │                                     ││
│  │  🟢 Corte de pasto                  ││
│  │     Sector A                        ││
│  │     [Ver instrucciones] [Iniciar]   ││
│  └─────────────────────────────────────┘│
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  🌳 REGISTRAR ÁRBOL                 ││
│  │  [Escanear QR] [Buscar por fila]    ││
│  └─────────────────────────────────────┘│
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  📊 CUADRÍCULA RÁPIDA               ││
│  │  [Ver Lote A] [Ver Lote B]          ││
│  └─────────────────────────────────────┘│
│                                         │
│  ─────────────────────────────────────  │
│  🔄 Última sync: Hace 5 min | ☁️ Online │
└─────────────────────────────────────────┘
```

#### Vista de Tarea con Instructivo

```
┌─────────────────────────────────────────┐
│  ← TAREA: Aplicación Trips              │
├─────────────────────────────────────────┤
│                                         │
│  📍 Ubicación: Lote A, F1-3, C5-8       │
│  🌳 Árboles: 45                         │
│  ⏱️ Tiempo est: 3 horas                 │
│                                         │
│  ═══════════════════════════════════════│
│  📋 INSTRUCCIONES                       │
│  ═══════════════════════════════════════│
│                                         │
│  1️⃣ PREPARACIÓN                         │
│  ─────────────────────────────────────  │
│  ☐ Verificar EPP completo               │
│  ☐ Recoger productos en bodega          │
│  ☐ Llevar bomba 20L calibrada           │
│                                         │
│  2️⃣ MEZCLA (por cada 20L)               │
│  ─────────────────────────────────────  │
│  • Agua: 15 L primero                   │
│  • Success 480 SC: 6 ml ← [🖼️ Ver foto] │
│  • Agitar 30 seg                        │
│  • Inex-A: 10 ml                        │
│  • Completar a 20 L                     │
│                                         │
│  3️⃣ APLICACIÓN                          │
│  ─────────────────────────────────────  │
│  • 3 L por árbol aprox                  │
│  • Cubrir follaje completo              │
│  • Énfasis envés de hojas               │
│                                         │
│  ⚠️ SEGURIDAD                           │
│  ─────────────────────────────────────  │
│  🧤 Guantes  👓 Gafas  😷 Máscara       │
│  Reingreso: 4 horas                     │
│                                         │
│  [📷 Foto mezcla] [▶️ INICIAR TAREA]    │
└─────────────────────────────────────────┘
```

#### Registro Rápido de Árbol

```
┌─────────────────────────────────────────┐
│  🌳 ÁRBOL A-3-7                         │
│  Lote A | Fila 3 | Col 7                │
├─────────────────────────────────────────┤
│                                         │
│  Estado actual: 🟢 Saludable            │
│  Última inspección: Hace 5 días         │
│                                         │
│  ═══════════════════════════════════════│
│  CAMBIAR ESTADO:                        │
│  ═══════════════════════════════════════│
│                                         │
│  [🟢]  [🟡]  [🟠]  [🔴]  [🔵]           │
│  Sano  Atenc Adver Crít  Trat           │
│        ↑                                │
│  Seleccionado: 🟡 Atención              │
│                                         │
│  ═══════════════════════════════════════│
│  ¿QUÉ PROBLEMA? (opcional)              │
│  ═══════════════════════════════════════│
│                                         │
│  [🐛 Plaga]  [🦠 Enfermedad]            │
│  [💧 Riego] [❓ Otro]                   │
│                                         │
│  📝 Nota rápida:                        │
│  ┌─────────────────────────────────────┐│
│  │ Hojas con manchas amarillas...      ││
│  └─────────────────────────────────────┘│
│                                         │
│  [📷 Tomar foto]                        │
│                                         │
│  [← Anterior] [💾 GUARDAR] [Siguiente →]│
│                                         │
│  Progreso: Árbol 12 de 45               │
│  ████████░░░░░░░░ 27%                   │
└─────────────────────────────────────────┘
```

#### Modo Inspección Rápida (Fila por Fila)

```
┌─────────────────────────────────────────┐
│  🚶 INSPECCIÓN RÁPIDA - Lote A          │
├─────────────────────────────────────────┤
│                                         │
│  Fila actual: 3 de 8                    │
│                                         │
│  Columna:  1   2   3   4   5   6   7    │
│           [🟢][🟢][🟢][🟡][🟡][ ▶️][ ]   │
│                              ↑          │
│                          Actual: 6      │
│                                         │
│  Árbol F3-C6:                           │
│  ┌─────────────────────────────────────┐│
│  │ Toca para cambiar estado:           ││
│  │                                     ││
│  │ [🟢] [🟡] [🟠] [🔴] [🔵]            ││
│  │                                     ││
│  │ [📷] [📝 Nota] [⏭️ Saltar]          ││
│  └─────────────────────────────────────┘│
│                                         │
│  ─────────────────────────────────────  │
│  Total inspeccionados: 41/80            │
│  ████████████████░░░░ 51%               │
│                                         │
│  [⏸️ Pausar] [📊 Resumen] [✅ Finalizar]│
└─────────────────────────────────────────┘
```

#### Sincronización y Modo Offline

```yaml
sincronizacion:
  datos_offline_disponibles:
    - cuadricula_estado_actual
    - tareas_asignadas_con_instructivos
    - catalogo_plagas_basico
    - lista_productos_frecuentes
  
  datos_que_se_sincronizan:
    - cambios_estado_arboles
    - fotos_capturadas
    - tareas_completadas
    - notas_agregadas
  
  estrategia_sync:
    - "Sync automático cada 15 min si hay WiFi"
    - "Sync manual con botón si hay 3G/4G"
    - "Cola de cambios pendientes visible"
    - "Conflictos: último cambio gana con log"
```

---


### 7.8 📦 Módulo de Compras e Inventario

> **Control completo de stock de insumos, alertas de bajo inventario, órdenes de compra y gestión de proveedores**

Este módulo gestiona todo el ciclo de vida de los insumos: desde la definición de stock mínimo, alertas automáticas, generación de órdenes de compra, hasta el registro de entradas y salidas.

#### Flujo de Gestión de Inventario

```
┌─────────────────────────────────────────────────────────────────┐
│                   CICLO DE INVENTARIO                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ┌──────────┐     ┌──────────┐     ┌──────────┐               │
│   │ CONSUMO  │     │ ALERTA   │     │  ORDEN   │               │
│   │ en labor │────▶│ stock    │────▶│  compra  │               │
│   └──────────┘     │ bajo     │     └──────────┘               │
│        │           └──────────┘           │                     │
│        │                                  ▼                     │
│        │           ┌──────────┐     ┌──────────┐               │
│        │           │ ENTRADA  │     │ APROBAC  │               │
│        └──────────▶│ bodega   │◀────│ compra   │               │
│                    └──────────┘     └──────────┘               │
│                         │                                       │
│                         ▼                                       │
│                  ┌─────────────┐                                │
│                  │ STOCK       │                                │
│                  │ actualizado │                                │
│                  └─────────────┘                                │
└─────────────────────────────────────────────────────────────────┘
```

#### Categorías de Insumos

| Categoría | Ejemplos | Control de Stock |
|-----------|----------|------------------|
| **Agroquímicos** | Insecticidas, fungicidas, herbicidas | Por unidad/volumen, lote, vencimiento |
| **Fertilizantes** | Granulados, solubles, orgánicos | Por peso, lote |
| **Combustibles** | Gasolina, diesel, gas LP | Por litro |
| **Repuestos riego** | Goteros, conectores, manguera | Por unidad/metro |
| **Herramientas** | Tijeras poda, serruchos, bombas | Por unidad, vida útil |
| **EPP** | Guantes, mascarillas, overoles | Por unidad |
| **Materiales varios** | Alambre, postes, malla sombra | Por unidad/metro/rollo |

#### Modelo de Datos - Inventario y Compras

```sql
-- Categorías de productos (expandido)
CREATE TABLE categorias_insumos (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(50),
    descripcion TEXT,
    requiere_lote BOOLEAN DEFAULT false,
    requiere_vencimiento BOOLEAN DEFAULT false,
    unidad_default VARCHAR(20)
);

-- Productos/Insumos (expandido de productos_agricolas)
CREATE TABLE insumos (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    categoria_id INTEGER REFERENCES categorias_insumos(id),
    codigo_interno VARCHAR(30) UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    unidad_medida VARCHAR(20), -- 'L', 'ml', 'kg', 'g', 'unidad', 'metro', 'rollo'
    unidad_compra VARCHAR(20), -- 'galon', 'bolsa_25kg', 'caja_12', etc
    factor_conversion DECIMAL(10,4), -- unidades por unidad_compra
    
    -- Control de stock
    stock_actual DECIMAL(12,3) DEFAULT 0,
    stock_minimo DECIMAL(12,3),
    stock_maximo DECIMAL(12,3),
    punto_reorden DECIMAL(12,3), -- cuando disparar alerta
    
    -- Precios
    precio_unitario_promedio DECIMAL(12,4),
    ultimo_precio_compra DECIMAL(12,4),
    fecha_ultimo_precio DATE,
    
    -- Para agroquímicos
    ingrediente_activo VARCHAR(150),
    categoria_toxicologica VARCHAR(10),
    registro_sanitario VARCHAR(50),
    
    -- Proveedor preferido
    proveedor_preferido_id INTEGER,
    
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Proveedores
CREATE TABLE proveedores (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    nombre VARCHAR(100) NOT NULL,
    razon_social VARCHAR(150),
    rfc VARCHAR(20),
    direccion TEXT,
    ciudad VARCHAR(100),
    telefono VARCHAR(20),
    email VARCHAR(100),
    contacto_nombre VARCHAR(100),
    contacto_telefono VARCHAR(20),
    dias_credito INTEGER DEFAULT 0,
    notas TEXT,
    activo BOOLEAN DEFAULT true
);

-- Productos que ofrece cada proveedor
CREATE TABLE proveedor_productos (
    id SERIAL PRIMARY KEY,
    proveedor_id INTEGER REFERENCES proveedores(id),
    insumo_id INTEGER REFERENCES insumos(id),
    codigo_proveedor VARCHAR(50), -- código del producto en el proveedor
    precio_lista DECIMAL(12,4),
    unidad_venta VARCHAR(20),
    tiempo_entrega_dias INTEGER,
    es_proveedor_principal BOOLEAN DEFAULT false,
    ultima_compra DATE,
    UNIQUE(proveedor_id, insumo_id)
);

-- Lotes de inventario (para trazabilidad)
CREATE TABLE lotes_inventario (
    id SERIAL PRIMARY KEY,
    insumo_id INTEGER REFERENCES insumos(id),
    numero_lote VARCHAR(50),
    fecha_ingreso DATE,
    fecha_vencimiento DATE,
    cantidad_inicial DECIMAL(12,3),
    cantidad_actual DECIMAL(12,3),
    precio_unitario DECIMAL(12,4),
    orden_compra_id INTEGER,
    ubicacion_bodega VARCHAR(50),
    estado VARCHAR(20) DEFAULT 'disponible' -- 'disponible', 'reservado', 'agotado', 'vencido'
);

-- Movimientos de inventario
CREATE TABLE movimientos_inventario (
    id SERIAL PRIMARY KEY,
    insumo_id INTEGER REFERENCES insumos(id),
    lote_id INTEGER REFERENCES lotes_inventario(id),
    tipo VARCHAR(20), -- 'entrada', 'salida', 'ajuste', 'devolucion', 'merma'
    cantidad DECIMAL(12,3),
    cantidad_anterior DECIMAL(12,3),
    cantidad_posterior DECIMAL(12,3),
    
    -- Referencia al origen
    referencia_tipo VARCHAR(30), -- 'orden_compra', 'tarea', 'aplicacion', 'ajuste_manual'
    referencia_id INTEGER,
    
    costo_unitario DECIMAL(12,4),
    costo_total DECIMAL(12,4),
    
    usuario_id INTEGER REFERENCES usuarios(id),
    fecha TIMESTAMP DEFAULT NOW(),
    notas TEXT
);

-- Órdenes de compra
CREATE TABLE ordenes_compra (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    numero_orden VARCHAR(30) UNIQUE,
    proveedor_id INTEGER REFERENCES proveedores(id),
    
    estado VARCHAR(20), -- 'borrador', 'solicitada', 'aprobada', 'enviada', 'recibida_parcial', 'recibida', 'cancelada'
    
    fecha_solicitud DATE,
    fecha_aprobacion DATE,
    fecha_envio DATE,
    fecha_entrega_esperada DATE,
    fecha_recepcion DATE,
    
    subtotal DECIMAL(12,2),
    impuestos DECIMAL(12,2),
    descuento DECIMAL(12,2),
    total DECIMAL(12,2),
    
    solicitado_por INTEGER REFERENCES usuarios(id),
    aprobado_por INTEGER REFERENCES usuarios(id),
    recibido_por INTEGER REFERENCES usuarios(id),
    
    notas TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Detalle de orden de compra
CREATE TABLE orden_compra_detalle (
    id SERIAL PRIMARY KEY,
    orden_id INTEGER REFERENCES ordenes_compra(id),
    insumo_id INTEGER REFERENCES insumos(id),
    cantidad_solicitada DECIMAL(12,3),
    cantidad_recibida DECIMAL(12,3) DEFAULT 0,
    unidad VARCHAR(20),
    precio_unitario DECIMAL(12,4),
    descuento_porcentaje DECIMAL(5,2) DEFAULT 0,
    subtotal DECIMAL(12,2),
    numero_lote_recibido VARCHAR(50),
    fecha_vencimiento DATE,
    notas TEXT
);

-- Alertas de inventario
CREATE TABLE alertas_inventario (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    insumo_id INTEGER REFERENCES insumos(id),
    tipo VARCHAR(30), -- 'stock_bajo', 'stock_critico', 'proximo_vencer', 'vencido', 'sin_movimiento'
    severidad VARCHAR(20), -- 'info', 'warning', 'critical'
    mensaje TEXT,
    stock_actual DECIMAL(12,3),
    stock_minimo DECIMAL(12,3),
    fecha_creacion TIMESTAMP DEFAULT NOW(),
    fecha_atendida TIMESTAMP,
    atendida_por INTEGER REFERENCES usuarios(id),
    orden_compra_generada_id INTEGER REFERENCES ordenes_compra(id),
    activa BOOLEAN DEFAULT true
);
```

#### Dashboard de Inventario

```
┌─────────────────────────────────────────────────────────────────┐
│  📦 INVENTARIO - Finca Los Alamos                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  🚨 ALERTAS ACTIVAS                                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ 🔴 CRÍTICO (2)                                              ││
│  │    • Hilo nylon 3mm - Stock: 0 | Mínimo: 2 rollos          ││
│  │    • Goteros 4 LPH - Stock: 5 | Mínimo: 50 uds             ││
│  │                                                [Crear OC]  ││
│  │                                                             ││
│  │ 🟡 BAJO (3)                                                 ││
│  │    • Gasolina - Stock: 8 L | Mínimo: 20 L                  ││
│  │    • Success 480 SC - Stock: 150 ml | Mínimo: 200 ml       ││
│  │    • Guantes nitrilo - Stock: 3 pares | Mínimo: 10         ││
│  │                                                             ││
│  │ ⚠️ POR VENCER (1)                                           ││
│  │    • Fungicida XYZ - Lote #2024-089 vence en 15 días       ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 RESUMEN POR CATEGORÍA                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Categoría        │ Items │ Valor Stock │ Bajo Stock │       ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Agroquímicos     │  12   │  $4,520.00  │     2      │ ⚠️    ││
│  │ Fertilizantes    │   8   │  $2,180.00  │     0      │ ✅    ││
│  │ Combustibles     │   3   │    $890.00  │     1      │ ⚠️    ││
│  │ Repuestos riego  │  15   │    $650.00  │     2      │ 🔴    ││
│  │ Herramientas     │   9   │  $1,200.00  │     0      │ ✅    ││
│  │ EPP              │   6   │    $340.00  │     1      │ ⚠️    ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ TOTAL            │  53   │  $9,780.00  │     6      │       ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📋 ÓRDENES DE COMPRA PENDIENTES                                │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ OC-2025-0042 │ AgroInsumos │ $380 │ Aprobada │ Entrega: 10/12││
│  │ OC-2025-0043 │ RiegoTec    │ $125 │ Enviada  │ Entrega: 11/12││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [➕ Nueva OC] [📥 Registrar entrada] [📤 Registrar salida]      │
│  [📊 Reporte kardex] [⚙️ Configurar alertas]                    │
└─────────────────────────────────────────────────────────────────┘
```

#### Pantalla de Creación de Orden de Compra

```
┌─────────────────────────────────────────────────────────────────┐
│  🛒 NUEVA ORDEN DE COMPRA                                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Proveedor: [AgroInsumos del Norte ▼]     📞 555-123-4567       │
│  Días crédito: 30 días                                          │
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  📦 PRODUCTOS                                                   │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ [+ Agregar producto] [📋 Agregar desde alertas]             ││
│  │                                                             ││
│  │ Producto         │ Cant │ Unidad  │ P.Unit │ Subtotal      ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Hilo nylon 3mm   │  5   │ rollo   │ $12.00 │ $60.00    [🗑️]││
│  │ Goteros 4 LPH    │ 100  │ unidad  │  $1.00 │ $100.00   [🗑️]││
│  │ Success 480 SC   │  1   │ litro   │ $85.00 │ $85.00    [🗑️]││
│  │ Gasolina         │ 20   │ litro   │  $6.50 │ $130.00   [🗑️]││
│  │ ────────────────────────────────────────────────────────── ││
│  │                                   Subtotal: │ $375.00      ││
│  │                                   IVA 16%:  │  $60.00      ││
│  │                                   ═════════════════════════││
│  │                                   TOTAL:    │ $435.00      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📅 Fecha entrega esperada: [15/12/2025]                        │
│                                                                 │
│  📝 Notas:                                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Favor entregar en horario de 8am a 2pm                      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [Guardar borrador] [Solicitar aprobación] [📧 Enviar a provee] │
└─────────────────────────────────────────────────────────────────┘
```

#### Configuración de Alertas de Stock

```yaml
configuracion_alertas_stock:
  niveles:
    critico:
      condicion: "stock_actual <= stock_minimo * 0.25"
      color: "rojo"
      notificacion: ["email", "push", "dashboard"]
      auto_generar_oc: true
    
    bajo:
      condicion: "stock_actual <= stock_minimo"
      color: "amarillo"
      notificacion: ["push", "dashboard"]
      auto_generar_oc: false
    
    reorden:
      condicion: "stock_actual <= punto_reorden"
      color: "naranja"
      notificacion: ["dashboard"]
      sugerir_cantidad: "stock_maximo - stock_actual"
  
  vencimiento:
    proximo_vencer:
      dias_antes: 30
      notificacion: ["email", "dashboard"]
      accion_sugerida: "Priorizar uso, revisar aplicaciones programadas"
    
    vencido:
      notificacion: ["email", "push", "dashboard"]
      accion_sugerida: "Retirar de bodega, registrar merma"
  
  frecuencia_revision: "diaria"
  hora_revision: "06:00"
```

---

### 7.9 💰 Módulo de Costos, Ventas y Rentabilidad

> **Sistema completo de costeo por actividad, registro de ventas de cosecha y análisis de rentabilidad hasta nivel de árbol**

Este módulo permite conocer el costo real de producción, registrar las ventas y calcular la rentabilidad a diferentes niveles: finca, lote, temporada y hasta por árbol individual.

#### Estructura de Costos

```
┌─────────────────────────────────────────────────────────────────┐
│              ESTRUCTURA DE COSTOS DE PRODUCCIÓN                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    COSTOS DIRECTOS                          ││
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         ││
│  │  │  MANO DE    │  │  INSUMOS    │  │ SERVICIOS   │         ││
│  │  │  OBRA       │  │             │  │ EXTERNOS    │         ││
│  │  │  INTERNA    │  │             │  │             │         ││
│  │  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘         ││
│  │         │                │                │                 ││
│  │    Jornales         Agroquímicos     Fumigación            ││
│  │    Salarios         Fertilizantes    aérea                 ││
│  │    Prestaciones     Combustibles     Análisis              ││
│  │                     Materiales       suelo                  ││
│  │                                      Cosecha               ││
│  │                                      tercerizada           ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                   COSTOS INDIRECTOS                         ││
│  │  Electricidad │ Agua │ Mantenimiento │ Administrativos     ││
│  └─────────────────────────────────────────────────────────────┘│
│                           │                                     │
│                           ▼                                     │
│               ┌───────────────────────┐                        │
│               │  COSTO TOTAL          │                        │
│               │  PRODUCCIÓN           │                        │
│               └───────────────────────┘                        │
│                           │                                     │
│                           ▼                                     │
│    ┌──────────────────────────────────────────────────────┐    │
│    │  Distribución a: FINCA → LOTE → ÁRBOL               │    │
│    └──────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────────┘
```

#### Tipos de Costo

| Tipo | Descripción | Asignación | Ejemplos |
|------|-------------|------------|----------|
| **Mano de obra interna** | Personal en nómina | Por horas trabajadas en tarea | Salarios, prestaciones, IMSS |
| **Insumos** | Materiales consumidos | Directo a tarea/árbol | Agroquímicos, fertilizantes, gasolina |
| **Servicios externos** | Terceros contratados | Por lote o área atendida | Fumigación aérea, cosecha, fletes |
| **Costos fijos** | No varían con producción | Prorrateo por hectárea/árbol | Renta, seguros, administración |
| **Depreciación** | Activos fijos | Prorrateo mensual | Equipo, infraestructura, vehículos |

#### Modelo de Datos - Costos y Ventas

```sql
-- Configuración de costos de mano de obra
CREATE TABLE costos_mano_obra (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    tipo VARCHAR(30), -- 'jornal', 'salario_mensual', 'destajo'
    nombre VARCHAR(100),
    costo_base DECIMAL(12,2),
    unidad VARCHAR(20), -- 'hora', 'dia', 'mes', 'tarea'
    incluye_prestaciones BOOLEAN DEFAULT false,
    factor_prestaciones DECIMAL(5,4) DEFAULT 1.0, -- ej: 1.35 = 35% de prestaciones
    activo BOOLEAN DEFAULT true,
    vigente_desde DATE,
    vigente_hasta DATE
);

-- Costos fijos periódicos
CREATE TABLE costos_fijos (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    categoria VARCHAR(50), -- 'electricidad', 'agua', 'renta', 'seguros', 'administrativos', 'depreciacion'
    concepto VARCHAR(100),
    monto DECIMAL(12,2),
    periodicidad VARCHAR(20), -- 'mensual', 'bimestral', 'anual'
    metodo_prorrateo VARCHAR(30), -- 'por_hectarea', 'por_arbol', 'por_lote_igual'
    activo BOOLEAN DEFAULT true
);

-- Servicios externos (proveedores de servicios)
CREATE TABLE servicios_externos (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    proveedor_id INTEGER REFERENCES proveedores(id),
    tipo_servicio VARCHAR(50), -- 'fumigacion_aerea', 'cosecha', 'transporte', 'analisis_laboratorio', 'asesoria'
    nombre VARCHAR(100),
    descripcion TEXT,
    unidad_cobro VARCHAR(30), -- 'hectarea', 'arbol', 'kg', 'viaje', 'muestra'
    precio_unitario DECIMAL(12,4),
    activo BOOLEAN DEFAULT true
);

-- Registro de costos por tarea (AMPLIADO)
CREATE TABLE costos_tarea (
    id SERIAL PRIMARY KEY,
    tarea_id INTEGER REFERENCES tareas(id),
    
    -- Mano de obra
    horas_trabajadas DECIMAL(6,2),
    trabajadores_cantidad INTEGER,
    costo_mano_obra_id INTEGER REFERENCES costos_mano_obra(id),
    costo_mano_obra_total DECIMAL(12,2),
    
    -- Insumos (se suman de movimientos_inventario)
    costo_insumos_total DECIMAL(12,2),
    
    -- Servicios externos
    servicio_externo_id INTEGER REFERENCES servicios_externos(id),
    cantidad_servicio DECIMAL(10,2),
    costo_servicio_total DECIMAL(12,2),
    
    -- Totales
    costo_total DECIMAL(12,2),
    
    -- Distribución
    distribuido_a VARCHAR(20), -- 'finca', 'lote', 'arboles'
    lotes_ids INTEGER[],
    arboles_ids INTEGER[],
    
    notas TEXT,
    fecha_registro TIMESTAMP DEFAULT NOW()
);

-- Costos acumulados por árbol (vista materializada o tabla)
CREATE TABLE costos_arbol_periodo (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id),
    periodo_year INTEGER,
    periodo_month INTEGER, -- NULL para acumulado anual
    
    -- Desglose de costos
    costo_mano_obra DECIMAL(12,2) DEFAULT 0,
    costo_insumos DECIMAL(12,2) DEFAULT 0,
    costo_servicios DECIMAL(12,2) DEFAULT 0,
    costo_fijos_prorrateados DECIMAL(12,2) DEFAULT 0,
    
    costo_total DECIMAL(12,2) DEFAULT 0,
    
    -- Para comparativas
    costo_por_kg_producido DECIMAL(12,4), -- se calcula con cosecha
    
    UNIQUE(arbol_id, periodo_year, periodo_month)
);

-- ================================================================
-- COSECHA Y VENTAS
-- ================================================================

-- Temporadas de cosecha
CREATE TABLE temporadas_cosecha (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    nombre VARCHAR(100), -- "Temporada 2025", "Cosecha Otoño 2025"
    fecha_inicio DATE,
    fecha_fin DATE,
    estado VARCHAR(20), -- 'planificada', 'en_curso', 'cerrada'
    notas TEXT
);

-- Registro de cosecha por árbol
CREATE TABLE cosecha_arboles (
    id SERIAL PRIMARY KEY,
    temporada_id INTEGER REFERENCES temporadas_cosecha(id),
    arbol_id INTEGER REFERENCES arboles(id),
    fecha_cosecha DATE,
    
    -- Producción
    cantidad_kg DECIMAL(10,3),
    calidad VARCHAR(20), -- 'premium', 'primera', 'segunda', 'industrial'
    calibre VARCHAR(20), -- tamaño de fruto si aplica
    
    -- Cosechador
    cosechador_id INTEGER REFERENCES usuarios(id),
    
    -- Trazabilidad
    lote_cosecha VARCHAR(50), -- lote/batch de esta cosecha
    contenedor_id VARCHAR(50), -- caja, costal, bin
    
    notas TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Resumen de cosecha por lote
CREATE TABLE cosecha_lote_resumen (
    id SERIAL PRIMARY KEY,
    temporada_id INTEGER REFERENCES temporadas_cosecha(id),
    lote_id INTEGER REFERENCES lotes(id),
    
    -- Totales calculados
    arboles_cosechados INTEGER,
    total_kg DECIMAL(12,3),
    kg_premium DECIMAL(12,3),
    kg_primera DECIMAL(12,3),
    kg_segunda DECIMAL(12,3),
    kg_industrial DECIMAL(12,3),
    
    -- Promedios
    promedio_kg_arbol DECIMAL(8,3),
    
    fecha_inicio_cosecha DATE,
    fecha_fin_cosecha DATE
);

-- Clientes/Compradores
CREATE TABLE clientes (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    nombre VARCHAR(100),
    razon_social VARCHAR(150),
    rfc VARCHAR(20),
    direccion TEXT,
    telefono VARCHAR(20),
    email VARCHAR(100),
    tipo VARCHAR(30), -- 'empacadora', 'exportador', 'mayorista', 'minorista', 'directo'
    activo BOOLEAN DEFAULT true
);

-- Ventas
CREATE TABLE ventas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    temporada_id INTEGER REFERENCES temporadas_cosecha(id),
    
    numero_venta VARCHAR(30) UNIQUE,
    cliente_id INTEGER REFERENCES clientes(id),
    fecha_venta DATE,
    
    -- Totales
    cantidad_total_kg DECIMAL(12,3),
    subtotal DECIMAL(14,2),
    descuentos DECIMAL(12,2) DEFAULT 0,
    impuestos DECIMAL(12,2) DEFAULT 0,
    total DECIMAL(14,2),
    
    -- Pago
    estado_pago VARCHAR(20), -- 'pendiente', 'parcial', 'pagado'
    fecha_pago DATE,
    
    -- Documentos
    factura_numero VARCHAR(50),
    remision_numero VARCHAR(50),
    
    notas TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Detalle de venta
CREATE TABLE venta_detalle (
    id SERIAL PRIMARY KEY,
    venta_id INTEGER REFERENCES ventas(id),
    
    -- Qué se vendió
    lote_cosecha VARCHAR(50), -- referencia a cosecha_arboles.lote_cosecha
    calidad VARCHAR(20),
    calibre VARCHAR(20),
    
    cantidad_kg DECIMAL(10,3),
    precio_kg DECIMAL(10,4),
    subtotal DECIMAL(12,2),
    
    -- Trazabilidad: de qué lotes/árboles vino
    lote_id INTEGER REFERENCES lotes(id),
    arboles_origen INTEGER[] -- IDs de árboles
);

-- Gastos de comercialización (se restan de la venta)
CREATE TABLE gastos_comercializacion (
    id SERIAL PRIMARY KEY,
    venta_id INTEGER REFERENCES ventas(id),
    concepto VARCHAR(100), -- 'flete', 'empaque', 'comision', 'maniobras'
    monto DECIMAL(12,2),
    proveedor VARCHAR(100),
    notas TEXT
);

-- ================================================================
-- VISTAS DE RENTABILIDAD
-- ================================================================

-- Rentabilidad por árbol (vista o tabla calculada)
CREATE TABLE rentabilidad_arbol (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id),
    temporada_id INTEGER REFERENCES temporadas_cosecha(id),
    
    -- Producción
    kg_cosechados DECIMAL(10,3),
    calidad_promedio VARCHAR(20),
    
    -- Ingresos (prorrateados de ventas)
    ingreso_bruto DECIMAL(12,2),
    gastos_comercializacion DECIMAL(12,2),
    ingreso_neto DECIMAL(12,2),
    
    -- Costos del periodo
    costo_mano_obra DECIMAL(12,2),
    costo_insumos DECIMAL(12,2),
    costo_servicios DECIMAL(12,2),
    costo_fijos DECIMAL(12,2),
    costo_total DECIMAL(12,2),
    
    -- Rentabilidad
    utilidad_bruta DECIMAL(12,2), -- ingreso_neto - costo_total
    margen_bruto_porcentaje DECIMAL(6,2),
    
    -- Métricas
    costo_por_kg DECIMAL(10,4),
    ingreso_por_kg DECIMAL(10,4),
    
    -- Clasificación
    es_rentable BOOLEAN,
    ranking_rentabilidad INTEGER, -- 1 = más rentable de la finca
    
    calculado_en TIMESTAMP DEFAULT NOW()
);
```

#### Dashboard de Rentabilidad

```
┌─────────────────────────────────────────────────────────────────┐
│  💰 RENTABILIDAD - Temporada 2025                               │
│  Finca: Los Alamos | Período: Ene-Dic 2025                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN EJECUTIVO                                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │  INGRESOS          │  COSTOS           │  UTILIDAD          ││
│  │  ━━━━━━━━━━━━━━━   │  ━━━━━━━━━━━━━━   │  ━━━━━━━━━━━━━━━  ││
│  │  $485,200          │  $312,450         │  $172,750          ││
│  │  38,500 kg         │                   │  Margen: 35.6%     ││
│  │                    │                   │  ✅ +12% vs 2024   ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📈 DESGLOSE DE COSTOS                                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │  Mano de obra    ████████████████░░░░░░░░  $142,800  45.7% ││
│  │  Insumos         ██████████░░░░░░░░░░░░░░   $78,200  25.0% ││
│  │  Servicios ext   ██████░░░░░░░░░░░░░░░░░░   $52,450  16.8% ││
│  │  Costos fijos    ████░░░░░░░░░░░░░░░░░░░░   $39,000  12.5% ││
│  │  ──────────────────────────────────────────────────────────││
│  │  TOTAL                                     $312,450  100%  ││
│  │                                                             ││
│  │  Costo por kg: $8.11 | Precio venta prom: $12.60/kg        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🌳 RENTABILIDAD POR LOTE                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Lote    │ Árboles │ Producción │ Costo/kg │ Margen │ Acción││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Lote A  │   500   │ 18,200 kg  │  $7.45   │  40.8% │ ✅    ││
│  │ Lote B  │   350   │ 11,800 kg  │  $8.92   │  29.2% │ ⚠️    ││
│  │ Lote C  │   280   │  8,500 kg  │  $9.15   │  27.4% │ ⚠️    ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │                      PROMEDIO:    $8.11     35.6%          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔍 TOP 10 ÁRBOLES MÁS RENTABLES                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ #1 A-5-12  │ 95 kg │ $11.10/kg │ Margen 52% │ $486 utilidad││
│  │ #2 A-3-8   │ 88 kg │ $10.95/kg │ Margen 49% │ $421 utilidad││
│  │ #3 A-7-15  │ 82 kg │ $11.25/kg │ Margen 48% │ $385 utilidad││
│  │ ...                                                [Ver más]││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⚠️ TOP 10 ÁRBOLES MENOS RENTABLES (revisar)                   │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ #1 C-2-5   │ 12 kg │ $28.50/kg │ Margen -126% │ -$198 pérd ││
│  │ #2 B-8-3   │ 18 kg │ $22.10/kg │ Margen -75%  │ -$156 pérd ││
│  │ #3 C-1-9   │ 15 kg │ $21.80/kg │ Margen -73%  │ -$142 pérd ││
│  │ ...                                                [Ver más]││
│  │                                                             ││
│  │ 💡 Recomendación: 12 árboles con rentabilidad negativa      ││
│  │    Evaluar: reemplazo, tratamiento intensivo o eliminación  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📊 Reporte detallado] [📈 Comparar temporadas] [📤 Exportar]  │
└─────────────────────────────────────────────────────────────────┘
```

#### Registro de Costos en Tarea

```
┌─────────────────────────────────────────────────────────────────┐
│  💰 REGISTRAR COSTOS - Tarea: Aplicación Trips Lote A          │
│  Fecha: 09/12/2025 | Estado: Completada                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  👷 MANO DE OBRA INTERNA                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tipo: [Jornal campo ▼]         Costo: $280/día             ││
│  │ Trabajadores: [2]              Horas: [6]                   ││
│  │ ─────────────────────────────────────────────               ││
│  │ Subtotal mano de obra:                      $420.00         ││
│  │ (2 trabajadores × 6 hrs × $35/hr)                          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📦 INSUMOS UTILIZADOS (del inventario)                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Producto         │ Cantidad │ Costo unit │ Subtotal        ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Success 480 SC   │  100 ml  │  $0.85/ml  │ $85.00          ││
│  │ Inex-A           │  150 ml  │  $0.05/ml  │ $7.50           ││
│  │ Gasolina (bomba) │   2 L    │  $6.50/L   │ $13.00          ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Subtotal insumos:                         │ $105.50         ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔧 SERVICIOS EXTERNOS                                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ☐ Esta tarea incluyó servicio externo                      ││
│  │                                                             ││
│  │ Tipo: [Seleccionar... ▼]                                    ││
│  │ Proveedor: [Seleccionar... ▼]                               ││
│  │ Cantidad: [___]  Unidad: [___]  Costo: $0.00               ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📍 DISTRIBUCIÓN DEL COSTO                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ● Distribuir a árboles específicos (45 árboles tratados)   ││
│  │ ○ Distribuir a lote completo (Lote A - 500 árboles)        ││
│  │                                                             ││
│  │ Costo por árbol: $525.50 ÷ 45 = $11.68                     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  RESUMEN DE COSTOS                                              │
│  ───────────────────────────────────────────────────────────────│
│  Mano de obra:        $420.00                                   │
│  Insumos:             $105.50                                   │
│  Servicios externos:    $0.00                                   │
│  ─────────────────────────────────────────────────────────────  │
│  TOTAL TAREA:         $525.50                                   │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  [Guardar costos] [Guardar y cerrar tarea]                     │
└─────────────────────────────────────────────────────────────────┘
```

#### Registro de Venta de Cosecha

```
┌─────────────────────────────────────────────────────────────────┐
│  🛒 NUEVA VENTA - Temporada 2025                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Cliente: [Empacadora del Valle ▼]                              │
│  Fecha venta: [10/12/2025]                                      │
│                                                                 │
│  📦 PRODUCTO VENDIDO                                            │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ [+ Agregar línea]                                           ││
│  │                                                             ││
│  │ Origen      │ Calidad  │ Calibre │   Kg   │ $/kg  │Subtotal││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Lote A      │ Premium  │ Grande  │ 2,500  │$15.00 │$37,500 ││
│  │ Lote A      │ Primera  │ Mediano │ 1,800  │$12.00 │$21,600 ││
│  │ Lote B      │ Primera  │ Mediano │ 1,200  │$12.00 │$14,400 ││
│  │ Lote B      │ Segunda  │ Chico   │   800  │ $8.00 │ $6,400 ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ TOTAL:                           │ 6,300  │       │$79,900 ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💸 GASTOS DE COMERCIALIZACIÓN                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ [+ Agregar gasto]                                           ││
│  │                                                             ││
│  │ Concepto              │ Monto                               ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Flete a empacadora    │ $1,200                              ││
│  │ Empaque (cajas)       │   $630                              ││
│  │ Comisión vendedor 2%  │ $1,598                              ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Total gastos:         │ $3,428                              ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  RESUMEN VENTA                                                  │
│  ───────────────────────────────────────────────────────────────│
│  Venta bruta:          $79,900.00                               │
│  Gastos comercializ:   -$3,428.00                               │
│  ─────────────────────────────────────                          │
│  Ingreso neto:         $76,472.00                               │
│  Precio neto/kg:          $12.14                                │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  [Guardar borrador] [Registrar venta]                          │
└─────────────────────────────────────────────────────────────────┘
```

#### Análisis de Árbol Individual

```
┌─────────────────────────────────────────────────────────────────┐
│  🌳 RENTABILIDAD ÁRBOL A-5-12                                   │
│  Lote A | Fila 5 | Columna 12 | Variedad: Hass | Edad: 8 años  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RANKING: #1 de 1,130 árboles (más rentable)                │
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  TEMPORADA 2025                                                 │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  🍎 PRODUCCIÓN                                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Total cosechado: 95 kg                                      ││
│  │ Calidad: 78% Premium, 22% Primera                          ││
│  │ vs Promedio lote (36 kg): +164% ⬆️                          ││
│  │ vs Promedio finca (34 kg): +179% ⬆️                         ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💰 INGRESOS                                                    │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Venta bruta (95 kg × $12.68 prom):        $1,204.60        ││
│  │ Gastos comercialización prorrateados:        -$52.20       ││
│  │ ─────────────────────────────────────────────               ││
│  │ Ingreso neto:                              $1,152.40        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📉 COSTOS                                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ DESGLOSE DE COSTOS ASIGNADOS                                ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Mano de obra (podas, cosecha, etc):         $285.00        ││
│  │ Insumos (3 aplicaciones, fertilización):    $142.50        ││
│  │ Riego (prorrateo):                           $45.00        ││
│  │ Costos fijos prorrateados:                  $120.00        ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Costo total:                                $592.50        ││
│  │ Costo por kg: $6.24                                        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📈 RENTABILIDAD                                                │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │   UTILIDAD NETA: $559.90  ✅                                ││
│  │   MARGEN: 48.6%                                             ││
│  │                                                             ││
│  │   ████████████████████████░░░░░░░░░░░░░░  Margen 48.6%     ││
│  │                                                             ││
│  │   Comparativa:                                              ││
│  │   • vs Promedio lote A (40.8%): +7.8 pts ⬆️                 ││
│  │   • vs Promedio finca (35.6%): +13.0 pts ⬆️                 ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📜 HISTORIAL DE RENTABILIDAD                                   │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Temporada │ Producción │ Utilidad │ Margen │ Ranking       ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ 2025      │   95 kg    │  $559.90 │ 48.6%  │ #1 de 1,130   ││
│  │ 2024      │   82 kg    │  $445.20 │ 44.2%  │ #3 de 1,125   ││
│  │ 2023      │   71 kg    │  $352.80 │ 41.8%  │ #8 de 1,120   ││
│  │ 2022      │   58 kg    │  $245.50 │ 38.5%  │ #15 de 1,118  ││
│  │ ────────────────────────────────────────────────────────── ││
│  │ Tendencia: ⬆️ Mejorando consistentemente                    ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📋 Ver todas las labores] [📊 Comparar con vecinos]          │
│  [📈 Gráfica histórica] [🖨️ Imprimir ficha]                    │
└─────────────────────────────────────────────────────────────────┘
```

#### Alertas y Recomendaciones Automáticas

```yaml
alertas_rentabilidad:
  arboles_no_rentables:
    condicion: "utilidad_neta < 0 por 2 temporadas consecutivas"
    cantidad_actual: 12
    accion_sugerida: |
      Árboles identificados con pérdida sostenida.
      Opciones:
      1. Análisis de suelo/raíz para identificar problema
      2. Programa de recuperación intensivo
      3. Evaluar reemplazo (costo vs beneficio futuro)
    arboles: ["C-2-5", "B-8-3", "C-1-9", ...]
  
  lotes_bajo_rendimiento:
    condicion: "margen < promedio_finca * 0.75"
    lotes_afectados: ["Lote C"]
    analisis: |
      Lote C presenta margen 27.4% vs promedio 35.6%
      Factores identificados:
      - Mayor incidencia de plagas (15% más aplicaciones)
      - Producción por árbol 18% menor
      - Árboles más jóvenes (promedio 5 años)
    recomendacion: |
      Monitorear siguiente temporada antes de tomar acciones
      drásticas. Se espera mejora natural por maduración.
  
  oportunidades_mejora:
    - tipo: "reduccion_costos"
      hallazgo: "Costo de mano de obra 45.7% del total (arriba de benchmark 40%)"
      sugerencia: "Evaluar eficiencia en labores de cosecha y poda"
    
    - tipo: "mejora_precio"
      hallazgo: "30% de producción se vende como 'Segunda'"
      sugerencia: "Mejorar prácticas para aumentar % Premium y Primera"
```

---

### 7.10 📊 Reportes Integrados

> **Reportes que conectan todos los módulos para visión completa del negocio**

#### Reporte: Estado de Resultados por Temporada

```
┌─────────────────────────────────────────────────────────────────┐
│  📊 ESTADO DE RESULTADOS - Temporada 2025                       │
│  Finca Los Alamos | Enero - Diciembre 2025                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  INGRESOS                                                       │
│  ───────────────────────────────────────────────────────────── │
│  Ventas brutas                                    $485,200.00  │
│    Premium (12,500 kg × $15.20)      $190,000.00              │
│    Primera (18,200 kg × $12.10)      $220,220.00              │
│    Segunda (6,800 kg × $8.50)         $57,800.00              │
│    Industrial (1,000 kg × $5.18)       $5,180.00              │
│                                                                 │
│  (-) Gastos de comercialización                   -$24,150.00  │
│    Fletes                            -$12,400.00              │
│    Empaque                            -$5,850.00              │
│    Comisiones                         -$5,900.00              │
│  ───────────────────────────────────────────────────────────── │
│  INGRESO NETO                                     $461,050.00  │
│                                                                 │
│  COSTOS DE PRODUCCIÓN                                          │
│  ───────────────────────────────────────────────────────────── │
│  Mano de obra                                     $142,800.00  │
│    Labores de cultivo                 $82,400.00              │
│    Cosecha                            $48,200.00              │
│    Mantenimiento                      $12,200.00              │
│                                                                 │
│  Insumos                                           $78,200.00  │
│    Agroquímicos                       $42,500.00              │
│    Fertilizantes                      $28,700.00              │
│    Combustibles                        $4,200.00              │
│    Otros materiales                    $2,800.00              │
│                                                                 │
│  Servicios externos                                $52,450.00  │
│    Fumigación aérea                   $28,000.00              │
│    Análisis laboratorio                $3,450.00              │
│    Asesoría técnica                   $12,000.00              │
│    Otros servicios                     $9,000.00              │
│                                                                 │
│  Costos fijos                                      $39,000.00  │
│    Electricidad/Agua                  $18,000.00              │
│    Mantenimiento infraestructura       $8,500.00              │
│    Seguros                             $6,500.00              │
│    Administrativos                     $6,000.00              │
│  ───────────────────────────────────────────────────────────── │
│  TOTAL COSTOS                                    -$312,450.00  │
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  UTILIDAD OPERATIVA                               $148,600.00  │
│  Margen operativo                                      32.2%   │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  MÉTRICAS CLAVE                                                 │
│  ───────────────────────────────────────────────────────────── │
│  Producción total:        38,500 kg                            │
│  Árboles productivos:     1,130                                │
│  Producción/árbol:        34.1 kg                              │
│  Costo/kg:                $8.11                                │
│  Precio venta prom/kg:    $12.60                               │
│  Utilidad/árbol:          $131.50                              │
│  Utilidad/hectárea:       $14,860                              │
│                                                                 │
│  [📤 Exportar PDF] [📧 Enviar] [📊 Comparar años]              │
└─────────────────────────────────────────────────────────────────┘
```