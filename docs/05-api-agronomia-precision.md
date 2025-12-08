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

> Navegación: [← Anterior](04-catalogo-plagas-enfermedades.md)[📑 Índice](README.md) | [Siguiente →](06-objetivos-sistema.md)
