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

### 3.2 🌱 Módulo de Salud y Fenología

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

### 3.3 💧 Módulo de Riego y Fertirriego

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

### 3.4 🧪 Módulo de Aplicaciones Fitosanitarias

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

### 3.5 🍃 Módulo de Cosecha

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

### 3.6 👷 Módulo de Gestión de Personal

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

### 3.7 📊 Módulo de Reportes y Análisis

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

#### KPIs Principales
- 🎯 Rendimiento promedio (kg/árbol)
- 🎯 Costo por kg producido
- 🎯 % de fruta exportable
- 🎯 Eficiencia de mano de obra (kg/hora-hombre)
- 🎯 Consumo hídrico (m³/kg producido)
- 🎯 Índice de incidencia de plagas

---

### 3.8 📱 Aplicación Móvil de Campo

#### Características
- ✅ Funciona offline (sincroniza al tener conexión)
- ✅ Escaneo de QR/NFC de árboles
- ✅ Captura de fotos georreferenciadas
- ✅ Registro rápido de actividades
- ✅ Navegación GPS hasta el árbol
- ✅ Alertas y notificaciones push
- ✅ Disponible para iOS y Android

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
- [ ] Aplicación móvil con funcionalidad offline
- [ ] Registro de cosechas
- [ ] Dashboard básico

### Fase 2: Core (3 meses)
- [ ] Módulo de aplicaciones fitosanitarias
- [ ] Gestión de riego y fertirriego
- [ ] Módulo de personal
- [ ] Reportes avanzados

### Fase 3: Avanzado (3 meses)
- [ ] Integración con drones e imágenes satelitales
- [ ] IA para detección de plagas
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
*Versión 1.0*