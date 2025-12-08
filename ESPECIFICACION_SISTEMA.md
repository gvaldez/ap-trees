# 🥑 AgroGrid - Sistema de Gestión de Finca de Aguacates

## Especificación Técnica del Sistema de Control Árbol por Árbol

---

## 1. Resumen Ejecutivo

AgroGrid es un sistema SaaS integral para la gestión de fincas de aguacates que permite el seguimiento y control árbol por árbol. Inspirado en las mejores prácticas de soluciones líderes como **Croptracker**, **Hectre**, **Outfield**, **Map My Crop** y **Green Atlas**, este sistema ofrece trazabilidad completa, optimización de recursos y análisis predictivo para maximizar la productividad de tu cultivo.

---

## 2. Objetivos del Sistema

### 2.1 Objetivo General
Proporcionar una plataforma digital que permita el monitoreo, gestión y análisis de cada árbol de aguacate en la finca, optimizando la producción y reduciendo costos operativos.

### 2.2 Objetivos Específicos
- ✅ Registrar y geolocalizar cada árbol individualmente
- ✅ Monitorear la salud y estado fitosanitario por árbol
- ✅ Controlar aplicaciones de insumos (riego, fertilizantes, pesticidas)
- ✅ Registrar y proyectar cosechas por árbol
- ✅ Gestionar mano de obra y actividades de campo
- ✅ Generar reportes de trazabilidad para certificaciones
- ✅ Predecir rendimientos mediante análisis de datos históricos
- ✅ **Visualizar el estado de la finca en formato de cuadrícula (filas x columnas)**

---

## 3. Módulos del Sistema

### 3.1 📍 Módulo de Mapeo y Geolocalización

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
  "coordenadas": {
    "latitud": 4.7110,
    "longitud": -74.0721
  },
  "variedad": "Hass",
  "fecha_siembra": "2019-03-15",
  "patron": "Criollo",
  "sector": "Norte",
  "lote": "A",
  "fila": 12,
  "posicion": 34,
  "estado": "Productivo",
  "edad_anos": 6
}
```

---

### 3.2 🔲 Módulo de Vista de Cuadrícula (CORE)

> **💡 Funcionalidad inspirada en el método tradicional de hoja cuadriculada**, digitalizada para visualización rápida del estado de toda la finca.

Este módulo es el **corazón visual del sistema**, permitiendo ver el estado de cada árbol en una cuadrícula interactiva de filas y columnas, tal como se hace tradicionalmente en papel pero con capacidades digitales avanzadas.

#### Características Principales

| Funcionalidad | Descripción |
|---------------|-------------|
| **Cuadrícula Interactiva** | Visualización de árboles en formato fila × columna por lote |
| **Código de Colores** | Estado visual inmediato de cada árbol |
| **Filtros Temporales** | Ver estado actual o histórico (semana, mes, año) |
| **Zoom y Navegación** | Desde vista general hasta árbol individual |
| **Actualización en Campo** | Modificar estado directamente desde app móvil |
| **Mapas de Calor** | Identificar zonas problemáticas rápidamente |
| **Comparación Temporal** | Antes/después para análisis de propagación |

#### Código de Colores por Estado

```
┌─────────────────────────────────────────────────────────────┐
│  LEYENDA DE ESTADOS                                         │
├─────────────────────────────────────────────────────────────┤
│  🟢 Verde      │ Saludable - Sin problemas                  │
│  🟡 Amarillo   │ Atención - Requiere monitoreo              │
│  🟠 Naranja    │ Advertencia - Intervención próxima         │
│  🔴 Rojo       │ Crítico - Intervención inmediata           │
│  ⚫ Negro      │ Muerto/Removido                            │
│  🔵 Azul       │ En tratamiento activo                      │
│  🟣 Morado     │ Recién plantado/En desarrollo              │
│  ⬜ Blanco     │ Sin inspeccionar                           │
└─────────────────────────────────────────────────────────────┘
```

#### Ejemplo de Vista de Cuadrícula - Lote A

```
                    LOTE A - Sector Norte
                    Fecha: 2025-12-08
    
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
      ⚠️ ALERTA: Posible foco de plaga detectado en zona [F1-3, C5-8]
```

#### Análisis de Propagación de Plagas

La cuadrícula permite identificar patrones de propagación:

```
HISTÓRICO DE PROPAGACIÓN - Phytophthora (Últimas 4 semanas)

Semana 1          Semana 2          Semana 3          Semana 4
┌──────────┐      ┌──────────┐      ┌──────────┐      ┌──────────┐
│🟢🟢🟢🟢🟢│      │🟢🟢🟢🟢🟢│      │🟢🟢🟢🟡🟡│      │🟢🟢🟡🟡🔴│
│🟢🟢🟢🟢🟢│      │🟢🟢🟢🟡🟢│      │🟢🟢🟡🔴🟡│      │🟢🟡🟡🔴🔴│
│🟢🟢🟢🟢🟢│      │🟢🟢🟡🟢🟢│      │🟢🟡🔴🟡🟢│      │🟡🟡🔴🟠🟢│
│🟢🟢🟢🟢🟢│      │🟢🟢🟢🟢🟢│      │🟢🟢🟡🟢🟢│      │🟢🟢🟡🟢🟢│
│🟢🟢🟢🟢🟢│      │🟢🟢🟢🟢🟢│      │🟢🟢🟢🟢🟢│      │🟢🟢🟢🟢🟢│
└──────────┘      └──────────┘      └──────────┘      └──────────┘
Foco inicial:     Expansión:        Propagación:      Estado actual:
F3,C3             4 árboles         9 árboles         15 árboles
                  
📈 Velocidad de propagación: ~4 árboles/semana
🧭 Dirección: Noreste → Suroeste
💡 Recomendación: Aplicar fungicida en perímetro + 2 filas buffer
```

#### Capas de Visualización

| Capa | Descripción | Uso Principal |
|------|-------------|---------------|
| **Estado Fitosanitario** | Salud general del árbol | Inspección diaria |
| **Fenología** | Etapa de desarrollo | Planificación de cosecha |
| **Producción** | kg por árbol (gradiente) | Análisis de rendimiento |
| **Riego** | Estado de humedad | Gestión hídrica |
| **Última Inspección** | Días desde última visita | Planificación de rondas |
| **Edad** | Años desde siembra | Gestión de renovación |
| **Tratamientos** | Aplicaciones activas | Control fitosanitario |

#### Interacciones en la Cuadrícula

**Click/Tap en un árbol:**
```
┌─────────────────────────────────────────┐
│  🌳 Árbol AGC-001-A-0315                │
├─────────────────────────────────────────┤
│  📍 Fila: 3  |  Columna: 15             │
│  🏷️ Variedad: Hass                      │
│  📅 Edad: 6 años                        │
│  🌡️ Estado: 🟠 Advertencia              │
│  🐛 Problema: Trips (leve)              │
│  📆 Última inspección: Hace 2 días      │
│  💊 Tratamiento: Spinosad (en curso)    │
├─────────────────────────────────────────┤
│  [📝 Actualizar] [📷 Ver fotos]         │
│  [📊 Historial] [🗺️ Ver en mapa]        │
└─────────────────────────────────────────┘
```

**Selección múltiple (arrastrar):**
- Seleccionar zona para aplicación masiva
- Marcar área afectada por plaga
- Asignar tarea a trabajador
- Generar reporte de zona

#### Filtros y Búsqueda

```yaml
filtros_disponibles:
  estado:
    - Todos
    - Solo críticos (🔴)
    - Requieren atención (🟡🟠🔴)
    - En tratamiento (🔵)
    - Saludables (🟢)
  
  temporal:
    - Hoy
    - Esta semana
    - Este mes
    - Rango personalizado
    - Comparar dos fechas
  
  fenologia:
    - En floración
    - Con fruto
    - En cosecha
    - Reposo
  
  produccion:
    - Alto rendimiento (>100kg)
    - Rendimiento medio (50-100kg)
    - Bajo rendimiento (<50kg)
    - Sin producción
  
  busqueda:
    - Por ID de árbol
    - Por fila/columna
    - Por problema específico
```

#### Exportación y Reportes

| Formato | Contenido | Uso |
|---------|-----------|-----|
| **PDF Visual** | Cuadrícula con colores | Impresión para campo |
| **Excel** | Matriz con datos | Análisis en hojas de cálculo |
| **PNG/JPG** | Imagen de la cuadrícula | Reportes y presentaciones |
| **GIF Animado** | Evolución temporal | Análisis de propagación |
| **CSV** | Datos crudos | Integración con otros sistemas |

#### Integración con App Móvil

```
┌─────────────────────────────────────┐
│  📱 MODO INSPECCIÓN DE CAMPO        │
├─────────────────────────────────────┤
│                                     │
│  Lote: A    Fila actual: 3          │
│                                     │
│  ← [🟢] [🟢] [🟡] [🟠] [🔴] →       │
│       1     2     3     4    5      │
│            ↑                        │
│       Posición actual               │
│                                     │
│  Árbol F3-C3:                       │
│  ┌─────────────────────────────┐    │
│  │ Estado actual: 🟡 Atención  │    │
│  │                             │    │
│  │ Cambiar a:                  │    │
│  │ [🟢] [🟡] [🟠] [🔴] [🔵]    │    │
│  │                             │    │
│  │ Problema detectado:         │    │
│  │ [Seleccionar plaga/enf...▼] │    │
│  │                             │    │
│  │ [📷 Tomar foto]             │    │
│  │ [💬 Agregar nota]           │    │
│  └─────────────────────────────┘    │
│                                     │
│  [← Anterior] [Guardar] [Siguiente →]│
└─────────────────────────────────────┘
```

#### Beneficios Clave

| Beneficio | Impacto |
|-----------|---------|
| **Detección temprana** | Identificar focos de problemas antes de que se propaguen |
| **Planificación visual** | Asignar tareas por zonas de manera intuitiva |
| **Análisis de patrones** | Entender cómo se mueven las plagas/enfermedades |
| **Comunicación efectiva** | Mostrar estado de la finca a cualquier persona |
| **Decisiones rápidas** | Vista de 500+ árboles en una sola pantalla |
| **Historial visual** | Comparar estado actual vs histórico |
| **Reducción de tiempo** | De horas revisando datos a segundos con la cuadrícula |

---

### 3.3 🌱 Módulo de Salud y Fenología

#### Seguimiento Fenológico
- **Etapas Registradas:**
  - Reposo vegetativo
  - Brotación
  - Floración (% de floración)
  - Cuajado de frutos
  - Desarrollo del fruto
  - Madurez fisiológica
  - Cosecha

#### Monitoreo de Salud
| Indicador | Método de Captura | Frecuencia |
|-----------|-------------------|------------|
| Estado general | Inspección visual + app | Semanal |
| Plagas detectadas | Fotos + IA | Según hallazgo |
| Enfermedades | Diagnóstico en campo | Según hallazgo |
| Índice de verdor (NDVI) | Drone/Satélite | Quincenal |
| Estrés hídrico | Sensores IoT | Tiempo real |

#### Alertas Automáticas
- 🔴 **Crítico:** Árbol requiere intervención inmediata
- 🟠 **Advertencia:** Anomalía detectada, monitorear
- 🟢 **Normal:** Sin problemas detectados

---

### 3.4 💧 Módulo de Riego y Fertirriego

#### Funcionalidades
- Programación de riego por sectores/árboles
- Registro de aplicaciones de fertilizantes
- Integración con sensores de humedad del suelo
- Cálculo de requerimientos hídricos según etapa fenológica
- Historial de aplicaciones por árbol

#### Esquema de Fertirriego
```yaml
plan_fertirriego:
  etapa: "Desarrollo de fruto"
  frecuencia: "Semanal"
  elementos:
    - nombre: "Nitrógeno (N)"
      cantidad: "120 kg/ha/año"
      fuente: "Urea"
    - nombre: "Fósforo (P)"
      cantidad: "40 kg/ha/año"
      fuente: "DAP"
    - nombre: "Potasio (K)"
      cantidad: "200 kg/ha/año"
      fuente: "KCl"
    - nombre: "Calcio (Ca)"
      cantidad: "80 kg/ha/año"
      fuente: "Nitrato de Calcio"
    - nombre: "Boro (B)"
      cantidad: "2 kg/ha/año"
      fuente: "Ácido Bórico"
```

---

### 3.5 🧪 Módulo de Aplicaciones Fitosanitarias

#### Control de Plagas y Enfermedades Comunes en Aguacate
| Problema | Síntomas | Producto Recomendado |
|----------|----------|---------------------|
| Trips | Cicatrices en frutos | Spinosad |
| Araña roja | Amarillamiento hojas | Abamectina |
| Phytophthora | Marchitez, pudrición raíz | Fosetil Aluminio |
| Antracnosis | Manchas en frutos | Cobre + Mancozeb |
| Barrenador | Galerías en tronco | Clorpirifos |

#### Registro de Aplicaciones
- Fecha y hora de aplicación
- Producto aplicado (nombre comercial e ingrediente activo)
- Dosis utilizada
- Árboles/lotes tratados
- Operario responsable
- Condiciones climáticas
- Período de carencia
- Documentos adjuntos (facturas, fichas técnicas)

---

### 3.6 🍃 Módulo de Cosecha

#### Funcionalidades
- Registro de cosecha por árbol/lote
- Peso y cantidad de frutos por árbol
- Clasificación por calibre y calidad
- Asignación de cosechadores
- Cálculo de rendimiento (kg/árbol, kg/ha)
- Trazabilidad desde árbol hasta empaque

#### Calibres de Aguacate Hass
| Calibre | Peso (g) | Unidades/Caja 4kg |
|---------|----------|-------------------|
| 12 | 306-365 | 12 |
| 14 | 266-305 | 14 |
| 16 | 236-265 | 16 |
| 18 | 211-235 | 18 |
| 20 | 190-210 | 20 |
| 22 | 171-189 | 22 |
| 24 | 156-170 | 24 |

#### Métricas de Productividad
```
📊 Dashboard de Cosecha
├── Producción total temporada: 45,000 kg
├── Promedio por árbol: 90 kg
├── Mejor árbol: AGC-001-A-0234 (156 kg)
├── Árboles bajo rendimiento (<50kg): 23 (4.6%)
└── Distribución por calibre:
    ├── Cal 12-16: 35%
    ├── Cal 18-20: 45%
    └── Cal 22-24: 20%
```

---

### 3.7 👷 Módulo de Gestión de Personal

#### Funcionalidades
- Registro de trabajadores y roles
- Asignación de tareas por lote/árbol
- Control de asistencia y horas trabajadas
- Rendimiento por trabajador (kg cosechados, árboles podados, etc.)
- Cálculo de nómina por actividad

#### Actividades Típicas
- Cosecha
- Poda de formación
- Poda sanitaria
- Aplicación de agroquímicos
- Fertilización manual
- Limpieza de platos
- Monitoreo fitosanitario
- Instalación/mantenimiento de riego

---

### 3.8 📊 Módulo de Reportes y Análisis

#### Reportes Disponibles
| Reporte | Periodicidad | Formato |
|---------|--------------|---------|
| Inventario de árboles | Mensual | PDF/Excel |
| Estado fitosanitario | Semanal | Dashboard |
| Aplicaciones realizadas | Por evento | PDF |
| Producción por cosecha | Por cosecha | PDF/Excel |
| Rendimiento por árbol | Anual | Excel |
| Trazabilidad completa | Por lote | PDF |
| Costos operativos | Mensual | Dashboard |
| Proyección de cosecha | Trimestral | Dashboard |
| **Cuadrícula de estado** | **Diario/Semanal** | **PDF/PNG/GIF** |
| **Mapa de calor por zona** | **Semanal** | **Dashboard** |
| **Análisis de propagación** | **Por evento** | **PDF/GIF** |

#### KPIs Principales
- 🎯 Rendimiento promedio (kg/árbol)
- 🎯 Costo por kg producido
- 🎯 % de fruta exportable
- 🎯 Eficiencia de mano de obra (kg/hora-hombre)
- 🎯 Consumo hídrico (m³/kg producido)
- 🎯 Índice de incidencia de plagas
- 🎯 **Velocidad de propagación de problemas**
- 🎯 **% de cobertura de inspección semanal**

---

### 3.9 📱 Aplicación Móvil de Campo

#### Características
- ✅ Funciona offline (sincroniza al tener conexión)
- ✅ Escaneo de QR/NFC de árboles
- ✅ Captura de fotos georreferenciadas
- ✅ Registro rápido de actividades
- ✅ Navegación GPS hasta el árbol
- ✅ Alertas y notificaciones push
- ✅ Disponible para iOS y Android
- ✅ **Vista de cuadrícula optimizada para móvil**
- ✅ **Modo inspección fila por fila**
- ✅ **Actualización rápida de estado con un tap**

---

## 4. Integraciones

### 4.1 Hardware Compatible
| Dispositivo | Uso |
|-------------|-----|
| Drones DJI | Mapeo y análisis NDVI |
| Sensores IoT (humedad, temperatura) | Monitoreo en tiempo real |
| Estaciones meteorológicas | Datos climáticos locales |
| Básculas digitales | Peso en cosecha |
| Impresoras de etiquetas | QR codes |

### 4.2 Integraciones de Software
- **ERP:** SAP, Oracle, Odoo
- **Contabilidad:** QuickBooks, Siigo
- **Mapas:** Google Maps, Mapbox
- **Imágenes satelitales:** Sentinel-2, Planet Labs
- **Certificaciones:** GlobalG.A.P., USDA Organic, Rainforest Alliance

---

## 5. Arquitectura Técnica

### 5.1 Stack Tecnológico Recomendado
```
Frontend:
├── Web: React.js / Next.js
├── Móvil: React Native / Flutter
└── Mapas: Leaflet / Mapbox GL

Backend:
├── API: Node.js (Express) / Python (FastAPI)
├── Base de datos: PostgreSQL + PostGIS
├── Cache: Redis
└── Cola de mensajes: RabbitMQ

Infraestructura:
├── Cloud: AWS / GCP / Azure
├── CDN: CloudFlare
├── Storage: S3 / GCS
└── CI/CD: GitHub Actions
```

### 5.2 Modelo de Datos Simplificado
```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Finca     │────▶│   Sector    │────▶│    Lote     │
└─────────────┘     └─────────────┘     └─────────────┘
                                               │
                                               ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│ Aplicación  │◀────│   Árbol     │────▶│  Cosecha    │
└─────────────┘     └─────────────┘     └─────────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
       ┌──────────┐ ┌──────────┐ ┌──────────┐
       │ Fenología│ │  Salud   │ │  Fotos   │
       └──────────┘ └──────────┘ └──────────┘
              │            │
              └─────┬──────┘
                    ▼
            ┌──────────────┐
            │  Historial   │
            │  Cuadrícula  │
            │  (Snapshots) │
            └──────────────┘
```

### 5.3 Modelo de Datos - Vista Cuadrícula

```sql
-- Tabla para snapshots históricos de la cuadrícula
CREATE TABLE grid_snapshots (
    id SERIAL PRIMARY KEY,
    lote_id INTEGER REFERENCES lotes(id),
    fecha_snapshot TIMESTAMP DEFAULT NOW(),
    tipo_snapshot VARCHAR(20), -- 'manual', 'automatico', 'inspeccion'
    creado_por INTEGER REFERENCES usuarios(id)
);

-- Detalle de cada celda en el snapshot
CREATE TABLE grid_snapshot_celdas (
    id SERIAL PRIMARY KEY,
    snapshot_id INTEGER REFERENCES grid_snapshots(id),
    arbol_id INTEGER REFERENCES arboles(id),
    fila INTEGER NOT NULL,
    columna INTEGER NOT NULL,
    estado VARCHAR(20), -- 'saludable', 'atencion', 'advertencia', 'critico', etc.
    problema_id INTEGER REFERENCES problemas(id),
    notas TEXT
);

-- Índice para búsquedas rápidas por posición
CREATE INDEX idx_grid_posicion ON grid_snapshot_celdas(snapshot_id, fila, columna);
```

---

## 6. Seguridad y Cumplimiento

### 6.1 Seguridad
- 🔒 Autenticación OAuth 2.0 / SSO
- 🔒 Cifrado de datos en tránsito (TLS 1.3) y reposo (AES-256)
- 🔒 Roles y permisos granulares
- 🔒 Auditoría de acciones (logs)
- 🔒 Backups automáticos diarios

### 6.2 Cumplimiento
- ✅ GDPR (protección de datos personales)
- ✅ GlobalG.A.P. (trazabilidad agrícola)
- ✅ ICA (normativa colombiana)
- ✅ FDA (exportación a EE.UU.)

---

## 7. Plan de Implementación

### Fase 1: MVP (3 meses)
- [ ] Módulo de registro de árboles y mapeo básico
- [ ] **🔲 Vista de cuadrícula básica (PRIORIDAD ALTA)**
- [ ] Aplicación móvil con funcionalidad offline
- [ ] **🔲 Actualización de estado en cuadrícula desde móvil**
- [ ] Registro de cosechas
- [ ] Dashboard básico

### Fase 2: Core (3 meses)
- [ ] Módulo de aplicaciones fitosanitarias
- [ ] **🔲 Historial de cuadrícula y comparación temporal**
- [ ] **🔲 Análisis de propagación de plagas**
- [ ] Gestión de riego y fertirriego
- [ ] Módulo de personal
- [ ] Reportes avanzados

### Fase 3: Avanzado (3 meses)
- [ ] Integración con drones e imágenes satelitales
- [ ] IA para detección de plagas
- [ ] **🔲 Predicción de propagación con ML**
- [ ] Predicción de cosechas con ML
- [ ] Integraciones con ERPs

### Fase 4: Optimización (Continuo)
- [ ] Mejoras basadas en feedback
- [ ] Nuevas certificaciones
- [ ] Expansión a otros cultivos

---

## 8. Métricas de Éxito

| Métrica | Objetivo Año 1 | Objetivo Año 2 |
|---------|----------------|----------------|
| Árboles registrados | 100% | 100% |
| Adopción app móvil | 80% usuarios | 95% usuarios |
| Reducción pérdidas | 10% | 20% |
| Aumento rendimiento | 5% | 15% |
| Tiempo registro actividades | -50% | -70% |
| Precisión predicción cosecha | 80% | 90% |
| **Cobertura inspección semanal** | **90%** | **98%** |
| **Tiempo detección de focos** | **-60%** | **-80%** |

---

## 9. Referencias y Benchmarks

Este sistema está inspirado en las mejores prácticas de los líderes del mercado:

| Sistema | Fortaleza Principal | Referencia |
|---------|---------------------|------------|
| **Croptracker** | Trazabilidad y cumplimiento | [croptracker.com](https://www.croptracker.com) |
| **Hectre** | Gestión de cosecha y calidad | [hectre.com](https://hectre.com) |
| **Outfield** | Mapeo con drones y IA | [outfield.xyz](https://outfield.xyz) |
| **Map My Crop** | Monitoreo satelital por árbol | [mapmycrop.com](https://mapmycrop.com) |
| **Green Atlas** | Conteo de frutos con imaging | [greenatlas.com](https://greenatlas.com) |
| **eOrchard** | Gestión integral de huertos | [eorchardapp.com](https://www.eorchardapp.com) |

---

## 10. Contacto y Próximos Pasos

**Repositorio:** `gvaldez/ap-trees`

### Próximas acciones:
1. ⬜ Validar especificación con stakeholders
2. ⬜ Definir prioridades del MVP
3. ⬜ Configurar entorno de desarrollo
4. ⬜ Crear issues para cada módulo
5. ⬜ Iniciar desarrollo del MVP

---

*Documento generado el 2025-12-08*
*Versión 1.1 - Agregado módulo de Vista de Cuadrícula*