## 18. Plan de MVP y Roadmap

> **Planificación de desarrollo por fases con priorización de funcionalidades**

Este documento define el camino desde el concepto hasta el lanzamiento del producto en producción, estructurado en 5 fases incrementales con entregables claros y medibles. Cada fase está diseñada para construir sobre la anterior, garantizando un desarrollo sostenible y enfocado en el valor para el usuario.

---

## 18.1 Criterios de Priorización

Para decidir qué funcionalidades incluir en cada fase, utilizamos los siguientes criterios de evaluación:

### 🎯 Valor para el Usuario
**Peso: 40%**

¿Qué tan crítica es esta funcionalidad para resolver el problema principal del usuario?

- **Alto**: Funcionalidad CORE sin la cual el sistema no tiene sentido (Vista de Cuadrícula, Registro de Árboles)
- **Medio**: Mejora significativa de la experiencia o eficiencia (Aplicaciones, Tareas)
- **Bajo**: Funcionalidad "nice to have" que puede esperar (White-label, Analytics avanzados)

### 🔧 Complejidad Técnica
**Peso: 25%**

¿Cuánto esfuerzo de desarrollo requiere?

- **Baja**: 1-2 sprints, tecnología conocida
- **Media**: 2-4 sprints, integración con servicios externos
- **Alta**: 4+ sprints, IA/ML, procesamiento de imágenes

### 🔗 Dependencias
**Peso: 20%**

¿Qué otros módulos o funcionalidades necesita?

- **Sin dependencias**: Puede desarrollarse de forma independiente
- **Dependencias internas**: Requiere otros módulos del sistema
- **Dependencias externas**: Requiere APIs o servicios de terceros

### 💎 Diferenciador Competitivo
**Peso: 15%**

¿Esta funcionalidad nos distingue de la competencia?

- **Alto**: Funcionalidad única o significativamente mejor que competidores (Vista Cuadrícula, Offline-first)
- **Medio**: Paridad competitiva necesaria
- **Bajo**: Funcionalidad estándar que todos tienen

### Matriz de Priorización

| Funcionalidad | Valor | Complejidad | Dependencias | Diferenciador | Puntuación | Fase |
|---------------|-------|-------------|--------------|---------------|------------|------|
| Vista de Cuadrícula | Alto (10) | Media (6) | Sin dep (10) | Alto (10) | **9.0** | 1 |
| Registro de Árboles | Alto (10) | Baja (8) | Sin dep (10) | Medio (5) | **8.8** | 1 |
| Inspecciones | Alto (9) | Baja (8) | Árboles (8) | Medio (6) | **8.3** | 1 |
| App Móvil básica | Alto (9) | Media (7) | Backend (7) | Alto (8) | **8.1** | 1 |
| Autenticación | Alto (10) | Baja (9) | Sin dep (10) | Bajo (3) | **8.0** | 1 |
| Tareas y Planificación | Medio (7) | Baja (8) | Árboles (8) | Medio (6) | **7.3** | 2 |
| Aplicaciones | Medio (8) | Media (6) | Inventario (6) | Medio (6) | **6.9** | 2 |
| Cosecha | Medio (7) | Media (7) | Árboles (8) | Bajo (4) | **6.7** | 3 |
| Reportes | Medio (6) | Media (7) | Todos (5) | Medio (5) | **6.0** | 3 |
| Integración IA/ML | Bajo (5) | Alta (3) | Imágenes (4) | Alto (10) | **5.1** | 4 |
| Drones/Satélite | Bajo (4) | Alta (3) | IA (3) | Alto (9) | **4.5** | 4 |
| White-label | Bajo (3) | Media (5) | Admin (6) | Medio (7) | **4.8** | 5 |

---

## 18.2 Fases de Desarrollo

### 📦 FASE 1: MVP Core (12 semanas)

**Objetivo**: Un usuario puede registrar su finca, mapear árboles, hacer inspecciones y ver el estado en cuadrícula.

**Duración**: 12 semanas (6 sprints de 2 semanas)

**Equipo**: 4-5 personas (1 Backend, 1 Frontend, 1 Mobile, 1 QA)

#### ✅ Incluido en Fase 1

**🔐 Autenticación y Seguridad**
- Login/logout con email y contraseña
- JWT + refresh tokens
- Roles básicos (Admin, Manager, Worker)
- Recuperación de contraseña por email
- Sesiones con expiración configurable

**🏢 Multi-tenant Básico**
- Crear tenant (organización/finca)
- Configuración básica del tenant
- Invitar usuarios al tenant
- Gestión básica de permisos

**🌾 Setup del Campo**
- Crear finca con ubicación GPS
- Definir sectores y lotes
- Registrar árboles individualmente (especie, edad, ubicación)
- Importar árboles desde CSV (masivo)
- Generación automática de árboles por cuadrícula (filas x columnas)
- Asignar códigos QR a cada árbol

**🔲 Vista de Cuadrícula** (CORE)
- Visualización de árboles en formato cuadrícula
- Colores por estado de salud (verde, amarillo, rojo, gris)
- Click en celda para ver detalle del árbol
- Filtros básicos (por sector, lote, estado)
- Zoom in/out
- Exportar vista como PNG/PDF

**🌱 Salud y Fenología**
- Cambiar estado de salud de un árbol (sano, enfermo, tratamiento, muerto)
- Registrar etapa fenológica (floración, cuajado, desarrollo fruto, cosecha)
- Historial básico de cambios de estado
- Notas por árbol

**🔍 Inspecciones**
- Registrar inspección de árbol (fecha, inspector, observaciones)
- Subir fotos (hasta 5 por inspección)
- Diagnóstico manual de problemas
- Listar historial de inspecciones por árbol

**📱 App Móvil Básica (React Native)**
- Login/logout
- Ver lista de árboles
- Escanear código QR de árbol
- Registrar inspección rápida
- Tomar y subir fotos
- Cambiar estado de árbol
- Funcionalidad offline básica (leer datos, hacer inspecciones, sincronizar cuando hay conexión)

#### ❌ Excluido para Fases Futuras

- 🤖 Integración IA/ML para detección automática
- 🛰️ Integración con drones y satélites
- 📊 Reportes avanzados y analytics
- 💰 Módulo financiero (costos, ventas)
- 📦 Inventario completo de insumos
- 🔔 Notificaciones push y alertas automáticas
- 🌐 White-label y personalización de marca
- 📋 Planificación y asignación de tareas
- 🧪 Aplicaciones fitosanitarias con cálculo de dosis

#### 🎯 Resultado Esperado Fase 1

Al final de la Fase 1, un productor debería poder:
1. Registrar su finca en menos de 30 minutos
2. Importar 1000 árboles desde Excel en menos de 5 minutos
3. Ver todos sus árboles en la vista de cuadrícula
4. Usar la app móvil para hacer inspecciones sin conexión
5. Cambiar el estado de un árbol y ver el cambio reflejado en tiempo real en la cuadrícula

---

### 📋 FASE 2: Operaciones (8 semanas)

**Objetivo**: Gestionar tareas, aplicaciones fitosanitarias y alertas del día a día.

**Duración**: 8 semanas (4 sprints de 2 semanas)

**Equipo**: 5-6 personas (2 Backend, 1 Frontend, 1 Mobile, 1 QA, 0.5 DevOps)

#### ✅ Incluido en Fase 2

**📋 Planificación y Tareas**
- Crear tareas (poda, aplicación, riego, cosecha)
- Asignar tareas a usuarios específicos
- Fecha estimada y fecha real de completado
- Estado de tarea (pendiente, en progreso, completada, cancelada)
- Vista de calendario semanal
- Filtros por tipo de tarea, responsable, sector

**🧪 Aplicaciones Fitosanitarias**
- Catálogo de productos (insecticidas, fungicidas, fertilizantes)
- Crear aplicación (producto, dosis, fecha, árboles/sector objetivo)
- Cálculo automático de dosis según superficie/número de árboles
- Flujo de aprobación (crear → aprobar → ejecutar)
- Registro de aplicación ejecutada (fecha real, cantidad usada)
- Historial de aplicaciones por árbol

**⚠️ Alertas y Notificaciones**
- Crear alerta manual (plaga detectada, equipo dañado, etc.)
- Alertas automáticas básicas (árbol sin inspección en X días, tarea vencida)
- Asignar responsable a alerta
- Resolver/cerrar alerta con notas
- Vista de alertas activas en dashboard

**💧 Riego Básico**
- Definir sectores de riego
- Registrar evento de riego (sector, fecha, duración, tipo)
- Historial de riegos por sector
- Correlación básica entre riego y salud de árboles

**📱 App Móvil Mejorada**
- Ver tareas asignadas del día
- Completar tarea desde móvil
- Registrar aplicación fitosanitaria
- Ver alertas activas
- Mejorar sincronización offline (cola de acciones, resolución de conflictos)

#### 🎯 Resultado Esperado Fase 2

Al final de la Fase 2, un productor debería poder:
1. Planificar las tareas de la semana y asignarlas a su equipo
2. Registrar una aplicación de fungicida a un sector específico
3. Recibir alertas cuando un árbol lleva 15+ días sin inspección
4. Ver en el móvil las tareas del día y completarlas
5. Generar un reporte básico de aplicaciones del mes

---

### 📊 FASE 3: Análisis y Cosecha (8 semanas)

**Objetivo**: Registrar producción, analizar datos históricos y generar reportes de gestión.

**Duración**: 8 semanas (4 sprints de 2 semanas)

**Equipo**: 5-6 personas (2 Backend, 1 Frontend, 1 Mobile, 1 QA, 0.5 BI/Data)

#### ✅ Incluido en Fase 3

**🍎 Registro de Cosecha**
- Definir temporadas de cosecha
- Registrar cosecha por árbol (peso, calidad, calibre)
- Registro masivo de cosecha por lote
- Clasificación de fruta (exportación, mercado local, descarte)
- Resumen de producción por árbol, lote, sector, finca
- Comparación año contra año

**📦 Inventario de Insumos**
- Catálogo de insumos (productos fitosanitarios, fertilizantes, herramientas)
- Registro de entradas (compras) y salidas (consumo)
- Stock actual y alertas de stock bajo
- Movimientos de inventario
- Órdenes de compra básicas

**📊 Reportes y Dashboards**
- Reporte de salud: % árboles por estado en periodo
- Reporte de producción: kg/árbol, kg/ha, distribución por calidad
- Reporte comparativo: año actual vs años anteriores
- Exportar reportes a Excel/PDF
- Dashboard principal con KPIs (producción, salud promedio, tareas completadas)

**🔲 Cuadrícula Avanzada**
- Múltiples modos de visualización (salud, producción, fenología)
- Comparación temporal (ver cuadrícula en fecha pasada vs hoy)
- Detección de patrones (árboles enfermos en cluster)
- Exportar secuencia temporal como GIF

**📱 App Móvil - Cosecha**
- Registrar cosecha desde móvil (árbol, peso, calidad)
- Usar QR para identificar árbol rápidamente
- Acumular registros offline y sincronizar

#### 🎯 Resultado Esperado Fase 3

Al final de la Fase 3, un productor debería poder:
1. Registrar la cosecha de 5000 árboles en una temporada
2. Ver la producción promedio por árbol y detectar árboles de bajo rendimiento
3. Comparar la producción de esta temporada vs la anterior
4. Controlar el inventario de insumos y recibir alertas de reposición
5. Generar un reporte ejecutivo mensual con todos los KPIs

---

### 🤖 FASE 4: Inteligencia (10 semanas)

**Objetivo**: Integrar IA, drones, imágenes satelitales y automatizaciones avanzadas.

**Duración**: 10 semanas (5 sprints de 2 semanas)

**Equipo**: 7-8 personas (2 Backend, 1 Frontend, 1 Mobile, 1 ML Engineer, 1 QA, 0.5 DevOps, 0.5 Data Analyst)

#### ✅ Incluido en Fase 4

**🛰️ Integración Drones y Satélite**
- Upload de imágenes multiespectrales (drone o satélite)
- Procesamiento de índices NDVI y NDRE
- Asignación de valores espectrales a cada árbol (por geolocalización)
- Generación automática de alertas por anomalías espectrales
- Visualización de mapa de calor sobre cuadrícula

**🤖 IA y Machine Learning**
- Modelo de detección de plagas en fotos (TensorFlow/PyTorch)
- Clasificación automática de enfermedades
- Predicción de propagación de plagas
- Recomendaciones automáticas de tratamiento
- Estimación de cosecha basada en histórico + condiciones

**💰 Módulo Financiero**
- Registro de costos por actividad (mano de obra, insumos, servicios)
- Registro de ventas de cosecha
- Cálculo de rentabilidad por árbol, lote, sector
- Proyecciones financieras basadas en datos históricos
- Análisis de ROI de inversiones

**🔔 Notificaciones Avanzadas**
- Notificaciones push en app móvil
- Notificaciones por email
- Integración con WhatsApp Business API
- Configuración de reglas de notificación personalizadas
- Resumen diario/semanal por email

#### 🎯 Resultado Esperado Fase 4

Al final de la Fase 4, un productor debería poder:
1. Subir imágenes de drones y obtener automáticamente el estado de salud de cada árbol
2. Recibir alertas automáticas cuando el sistema detecta una plaga en una foto
3. Ver recomendaciones de tratamiento basadas en IA
4. Calcular la rentabilidad exacta de cada lote
5. Recibir notificaciones push cuando se completa una tarea crítica

---

### 🏢 FASE 5: Enterprise (8 semanas)

**Objetivo**: Features para clientes grandes, white-label y modelo de reventa.

**Duración**: 8 semanas (4 sprints de 2 semanas)

**Equipo**: 6-7 personas (2 Backend, 1 Frontend, 1 Mobile, 1 DevOps, 1 QA, 0.5 PM)

#### ✅ Incluido en Fase 5

**🔧 Plataforma de Administración**
- Dashboard de administración de tenants
- Gestión de facturación y suscripciones
- Sistema de tickets de soporte
- Monitoreo de uso y performance por tenant
- Métricas de negocio (MRR, churn, activación)

**🌐 White-Label**
- Configuración de branding por tenant (logo, colores, nombre)
- Dominio personalizado (miagrogrid.com)
- Templates de emails personalizados
- App móvil con branding del cliente (iOS/Android)
- Documentación personalizada

**🔗 Integraciones**
- API pública REST con documentación OpenAPI
- Webhooks para eventos del sistema
- Integración con ERPs comunes (SAP, Oracle)
- Integración con sistemas de contabilidad (QuickBooks, Xero)
- SDK para desarrolladores

**📈 Analytics Avanzados**
- Benchmarking (comparar con productores similares)
- Predicciones de mercado (precios, demanda)
- Análisis de tendencias multi-finca
- Exportación de datos para BI externo

#### 🎯 Resultado Esperado Fase 5

Al final de la Fase 5, la plataforma debería poder:
1. Venderse en modo white-label a consultoras o cooperativas
2. Conectarse con sistemas ERP y contables del cliente
3. Ofrecer una API pública para integraciones custom
4. Comparar métricas de un productor con el promedio del sector
5. Generar revenue recurrente con múltiples canales de venta

---

## 18.3 Timeline Visual

```
ROADMAP AGROGRID 2025-2026
═══════════════════════════════════════════════════════════════

Q1 2025          Q2 2025          Q3 2025          Q4 2025          Q1 2026          Q2 2026
│                │                │                │                │                │
├────────────────┼────────────────┼────────────────┼────────────────┼────────────────┤
│                │                │                │                │                │
│  FASE 1: MVP CORE (10 semanas)  │                │                │                │
│  ════════════════════════════   │                │                │                │
│  🔐 Auth                         │                │                │                │
│  🏢 Multi-tenant                 │                │                │                │
│  🌾 Setup Campo                  │                │                │                │
│  🔲 Cuadrícula ★                 │                │                │                │
│  🌱 Salud/Fenología              │                │                │                │
│  🔍 Inspecciones                 │                │                │                │
│  📱 App Móvil básica             │                │                │                │
│                                  │                │                │                │
├──────────────────────────────────┼────────────────┼────────────────┼────────────────┤
│                                  │                │                │                │
│                  FASE 2: OPERACIONES (8 semanas)  │                │                │
│                  ════════════════════════════════  │                │                │
│                  📋 Tareas y Planificación         │                │                │
│                  🧪 Aplicaciones                   │                │                │
│                  ⚠️  Alertas                        │                │                │
│                  💧 Riego básico                   │                │                │
│                  📱 App Móvil v2                   │                │                │
│                                                    │                │                │
├────────────────────────────────────────────────────┼────────────────┼────────────────┤
│                                                    │                │                │
│                    FASE 3: ANÁLISIS Y COSECHA (8 semanas)           │                │
│                    ═══════════════════════════════════════           │                │
│                    🍎 Registro Cosecha                               │                │
│                    📦 Inventario                                     │                │
│                    📊 Reportes y Dashboards                          │                │
│                    🔲 Cuadrícula avanzada                            │                │
│                                                                      │                │
├──────────────────────────────────────────────────────────────────────┼────────────────┤
│                                                                      │                │
│                                  FASE 4: INTELIGENCIA (10 semanas)  │                │
│                                  ═══════════════════════════════════ │                │
│                                  🛰️  Drones y Satélite              │                │
│                                  🤖 IA/ML                            │                │
│                                  💰 Financiero                       │                │
│                                  🔔 Notificaciones                   │                │
│                                                                      │                │
├──────────────────────────────────────────────────────────────────────┼────────────────┤
│                                                                      │                │
│                                              FASE 5: ENTERPRISE (8 semanas)           │
│                                              ════════════════════════════════         │
│                                              🔧 Admin Platform                        │
│                                              🌐 White-Label                           │
│                                              🔗 Integraciones                         │
│                                              📈 Analytics Avanzados                   │
│                                                                                       │
└───────────────────────────────────────────────────────────────────────────────────────┘

HITOS CLAVE:
═══════════════
  ★  MVP Release (Fin Q2 2025)        - Primera versión vendible
  ⚡ Operations Ready (Fin Q3 2025)   - Gestión completa del día a día
  📈 Analytics Ready (Fin Q4 2025)    - Análisis y reportes completos
  🤖 AI-Powered (Q1 2026)             - IA y automatización inteligente
  🏢 Enterprise Ready (Q2 2026)       - White-label y multi-canal

RECURSOS CLAVE POR FASE:
═════════════════════════
Fase 1: 4-5 personas  |  Fase 2: 5-6 personas  |  Fase 3: 5-6 personas
Fase 4: 7-8 personas  |  Fase 5: 6-7 personas

TOTAL DURACIÓN: 44 semanas (~11 meses desde kick-off hasta Enterprise Ready)
```

---

## 18.4 Backlog MVP - Desglose por Sprints

### Sprint 1-2: Fundamentos (4 semanas)

**Objetivo**: Tener la base técnica lista (autenticación, base de datos, estructura de proyectos).

| Historia de Usuario | Story Points | Módulo | Prioridad |
|---------------------|--------------|--------|-----------|
| Configurar proyecto Spring Boot con estructura modular | 5 | Backend | P0 |
| Implementar autenticación JWT con refresh tokens | 8 | Auth | P0 |
| CRUD de usuarios con roles (Admin, Manager, Worker) | 5 | Auth | P0 |
| Implementar modelo de datos multi-tenant (schema por tenant) | 8 | DB | P0 |
| Crear entidades JPA: Tenant, User, Finca, Sector, Lote, Tree | 8 | Backend | P0 |
| Setup Next.js 14 con App Router + autenticación | 5 | Frontend | P0 |
| Pantalla de login con validación | 3 | Frontend | P0 |
| Pantalla de registro de tenant | 5 | Frontend | P0 |
| Setup React Native con navegación y estructura básica | 5 | Mobile | P0 |
| Configurar CI/CD básico (GitHub Actions) | 3 | DevOps | P1 |
| **Total Sprint 1-2** | **55** | | |

**Entregables**:
- ✅ Usuario puede registrarse y hacer login
- ✅ JWT funcionando con expiración y refresh
- ✅ Base de datos PostgreSQL con PostGIS instalado
- ✅ Proyectos Frontend y Mobile con login funcional

---

### Sprint 3-4: Setup del Campo (4 semanas)

**Objetivo**: Permitir al usuario configurar su finca completa (sectores, lotes, árboles).

| Historia de Usuario | Story Points | Módulo | Prioridad |
|---------------------|--------------|--------|-----------|
| API REST: CRUD de Fincas | 5 | Backend | P0 |
| API REST: CRUD de Sectores | 5 | Backend | P0 |
| API REST: CRUD de Lotes | 5 | Backend | P0 |
| API REST: CRUD de Árboles con geolocalización | 8 | Backend | P0 |
| API: Importación masiva de árboles desde CSV | 8 | Backend | P0 |
| API: Generación automática de árboles por cuadrícula | 8 | Backend | P0 |
| Endpoint: Generar códigos QR para árboles | 5 | Backend | P1 |
| Frontend: Wizard de setup paso a paso | 13 | Frontend | P0 |
| Frontend: Formulario de creación de finca | 5 | Frontend | P0 |
| Frontend: Formulario de sectores y lotes | 5 | Frontend | P0 |
| Frontend: Mapa interactivo con Mapbox para ubicar árboles | 8 | Frontend | P0 |
| Frontend: Importador de CSV con validación | 8 | Frontend | P0 |
| Frontend: Generador de cuadrícula visual | 8 | Frontend | P0 |
| Mobile: Pantalla de lista de fincas | 3 | Mobile | P1 |
| **Total Sprint 3-4** | **94** | | |

**Entregables**:
- ✅ Usuario puede crear una finca completa
- ✅ Importar 1000 árboles desde Excel en <5 minutos
- ✅ Generar automáticamente árboles en formato cuadrícula (ej: 20 filas x 50 columnas)
- ✅ Ver árboles en un mapa con Mapbox

---

### Sprint 5-6: Cuadrícula e Inspecciones (4 semanas)

**Objetivo**: Vista de cuadrícula funcional (CORE del MVP) + inspecciones desde web y móvil.

| Historia de Usuario | Story Points | Módulo | Prioridad |
|---------------------|--------------|--------|-----------|
| API: Endpoint de vista de cuadrícula con filtros | 8 | Backend | P0 |
| API: Cambiar estado de árbol (sano, enfermo, muerto) | 5 | Backend | P0 |
| API REST: CRUD de inspecciones | 5 | Backend | P0 |
| API: Upload de fotos de inspección (S3 o equivalente) | 5 | Backend | P0 |
| Frontend: Componente cuadrícula interactiva con Konva/Canvas | 13 | Frontend | P0 |
| Frontend: Panel lateral con detalle de árbol | 8 | Frontend | P0 |
| Frontend: Filtros de cuadrícula (sector, lote, estado) | 5 | Frontend | P0 |
| Frontend: Formulario de inspección con upload de fotos | 5 | Frontend | P0 |
| Frontend: Historial de inspecciones por árbol | 5 | Frontend | P1 |
| Frontend: Exportar cuadrícula como PNG/PDF | 5 | Frontend | P1 |
| Mobile: Pantalla de lista de árboles con búsqueda | 5 | Mobile | P0 |
| Mobile: Pantalla de detalle de árbol | 5 | Mobile | P0 |
| Mobile: Formulario de inspección con cámara | 8 | Mobile | P0 |
| Mobile: Escaneo de código QR para identificar árbol | 5 | Mobile | P0 |
| Mobile: Implementar sincronización offline básica (SQLite local) | 13 | Mobile | P0 |
| **Total Sprint 5-6** | **100** | | |

**Entregables**:
- ✅ Vista de cuadrícula funcional en web con colores por estado
- ✅ Click en un árbol abre panel con detalle e historial
- ✅ App móvil puede escanear QR y hacer inspecciones offline
- ✅ Sincronización automática cuando vuelve la conexión
- ✅ Exportar vista de cuadrícula como imagen

---

### Resumen de Story Points MVP

| Sprint | Semanas | Story Points | Equipo Sugerido | Velocidad Esperada |
|--------|---------|--------------|-----------------|-------------------|
| Sprint 1-2 | 4 | 55 | 4 personas | ~14 puntos/sem |
| Sprint 3-4 | 4 | 94 | 5 personas | ~24 puntos/sem |
| Sprint 5-6 | 4 | 100 | 5 personas | ~25 puntos/sem |
| **Total MVP** | **12 sem** | **249** | **5 personas** | **~21 puntos/sem** |

> **Nota**: Con un equipo de 5 personas y velocidad de 21 puntos/semana, el MVP tomaría aproximadamente **12 semanas** en condiciones ideales. Se recomienda agregar 2-4 semanas de buffer para imprevistos, testing adicional y refinamiento.

---

## 18.5 Entregables por Fase

### Tabla de Entregables

| Fase | Componente | Features | Prioridad | Estado |
|------|-----------|----------|-----------|---------|
| **1: MVP** | Backend API | Auth, Multi-tenant, CRUD Árboles, Inspecciones, Cuadrícula API | P0 | Pendiente |
| | Web App | Login, Setup Wizard, Vista Cuadrícula, Inspecciones, Exportar PNG | P0 | Pendiente |
| | Mobile App | Login, Lista árboles, Escaneo QR, Inspecciones, Offline | P0 | Pendiente |
| | Base de Datos | PostgreSQL + PostGIS, Schema multi-tenant | P0 | Pendiente |
| | | | | |
| **2: Operaciones** | Backend API | Tareas, Aplicaciones, Alertas, Riego | P0 | Pendiente |
| | Web App | Calendario tareas, Formulario aplicaciones, Dashboard alertas | P0 | Pendiente |
| | Mobile App | Tareas del día, Registrar aplicación, Sync mejorado | P0 | Pendiente |
| | | | | |
| **3: Análisis** | Backend API | Cosecha, Inventario, Reportes, Cuadrícula temporal | P0 | Pendiente |
| | Web App | Registro cosecha, Control inventario, Reportes Excel/PDF | P0 | Pendiente |
| | Mobile App | Registro cosecha rápido con QR | P1 | Pendiente |
| | | | | |
| **4: Inteligencia** | Backend API | Procesamiento imágenes, Modelos ML, Financiero, Webhooks | P1 | Pendiente |
| | ML Pipeline | Detección plagas, Clasificación, Predicción | P1 | Pendiente |
| | Web App | Upload drones, Mapa de calor NDVI, Recomendaciones IA | P1 | Pendiente |
| | Notificaciones | Push, Email, WhatsApp | P1 | Pendiente |
| | | | | |
| **5: Enterprise** | Admin Platform | Gestión tenants, Facturación, Soporte, Monitoreo | P1 | Pendiente |
| | White-Label | Branding, Dominio custom, Emails personalizados | P2 | Pendiente |
| | API Pública | REST API, Documentación OpenAPI, SDK | P1 | Pendiente |
| | Integraciones | Webhooks, ERP, Contabilidad | P2 | Pendiente |

---

## 18.6 Métricas del MVP

### Métricas de Desarrollo

| Métrica | Valor MVP | Fase 2 | Fase 3 | Fase 4 | Fase 5 |
|---------|-----------|--------|--------|--------|--------|
| **Duración** | 10-12 semanas | +8 sem | +8 sem | +10 sem | +8 sem |
| **Sprints** | 6 sprints | +4 | +4 | +5 | +4 |
| **Story Points** | ~250 puntos | +180 | +160 | +220 | +150 |
| **Endpoints API** | ~40 | +25 | +20 | +30 | +15 |
| **Pantallas Web** | ~10 | +8 | +10 | +8 | +6 |
| **Pantallas Mobile** | ~6 | +5 | +4 | +3 | +2 |
| **Modelos DB** | ~12 entidades | +8 | +6 | +10 | +5 |
| **Tests Unitarios** | ~200 tests | +150 | +100 | +180 | +80 |
| **Cobertura Código** | >70% | >75% | >80% | >80% | >80% |

### Métricas de Performance

| Métrica | Objetivo MVP | Forma de Medición |
|---------|--------------|-------------------|
| **Tiempo de carga cuadrícula** | < 2 segundos | Lighthouse, tiempo hasta interactive |
| **Tiempo importar 1000 árboles** | < 5 minutos | Medición end-to-end desde upload |
| **Tiempo completar setup inicial** | < 30 minutos | Tiempo desde registro hasta primera cuadrícula |
| **App móvil: tiempo de sync** | < 10 segundos | 100 inspecciones offline → online |
| **Disponibilidad del sistema** | > 99% | Uptime monitoring (UptimeRobot, Datadog) |
| **Tiempo de respuesta API (p95)** | < 500ms | Métricas de backend (New Relic, Prometheus) |

### Métricas de Negocio (Post-Launch)

| Métrica | Objetivo Mes 1 | Objetivo Mes 3 | Objetivo Mes 6 |
|---------|----------------|----------------|----------------|
| **Usuarios activos** | 10 | 50 | 150 |
| **Fincas registradas** | 15 | 75 | 200 |
| **Árboles en sistema** | 50,000 | 250,000 | 750,000 |
| **Inspecciones/mes** | 500 | 3,000 | 10,000 |
| **Tasa de retención** | 80% | 85% | 90% |
| **NPS** | > 40 | > 50 | > 60 |

---

## 18.7 Riesgos y Mitigaciones

### Tabla de Riesgos

| Riesgo | Probabilidad | Impacto | Severidad | Mitigación |
|--------|--------------|---------|-----------|------------|
| **Complejidad de PostGIS** | Media | Alto | �� Alta | Spike técnico en Sprint 0, contratar consultor externo si es necesario |
| **Scope creep en MVP** | Alta | Medio | 🟡 Media | Definition of Done estricta, Product Owner con poder de veto |
| **Performance de cuadrícula con 10k+ árboles** | Media | Alto | 🔴 Alta | Virtualización de canvas, paginación en backend, caché Redis |
| **Sincronización offline compleja** | Alta | Alto | 🔴 Alta | Usar biblioteca probada (WatermelonDB), limitar features offline inicialmente |
| **Adopción móvil en campo** | Media | Alto | 🔴 Alta | UX testing con usuarios reales, onboarding guiado, soporte dedicado |
| **Integración con drones** | Media | Medio | 🟡 Media | Validar con partners de drones antes de Fase 4, tener plan B con upload manual |
| **Escalabilidad multi-tenant** | Baja | Alto | 🟡 Media | Pruebas de carga desde MVP, arquitectura horizontal scale-out |
| **Rotación del equipo** | Baja | Alto | 🟡 Media | Documentación exhaustiva, pair programming, knowledge sharing semanal |
| **Retraso en dependencias externas** | Media | Medio | 🟡 Media | Identificar dependencias críticas temprano, tener proveedores alternativos |

### Plan de Contingencia

#### Si nos retrasamos en el MVP (>2 semanas de retraso):
1. **Reducir scope**:
   - Mover exportar PNG/PDF a Fase 2
   - Simplificar wizard de setup (modo experto directo)
   - Posponer generación de QR

2. **Aumentar recursos**:
   - Contratar 1 desarrollador adicional temporal
   - Reducir ceremonias (daily de 15 min → 10 min)

3. **Re-priorizar**:
   - Focus absoluto en cuadrícula + inspecciones
   - Features de Mobile mínimas (solo lectura)

#### Si PostGIS resulta muy complejo:
1. **Alternativa 1**: Usar biblioteca de geometría en memoria (JTS Topology Suite)
2. **Alternativa 2**: Pre-calcular posiciones en formato JSON, usar búsqueda by-coordinates
3. **Alternativa 3**: Usar servicio externo (Google Maps API, Mapbox Tilesets)

#### Si sincronización offline no funciona:
1. **Plan B**: App requiere conexión (modo online-only) en MVP
2. **Plan C**: Solo lectura offline, escritura requiere conexión
3. Mover funcionalidad offline completa a Fase 2

---

## 18.8 Criterios de Éxito del MVP

### Criterios Técnicos

✅ **El sistema debe permitir**:
- Registrar una finca con al menos 1000 árboles
- Mostrar la vista de cuadrícula en menos de 2 segundos
- Realizar inspecciones con fotos desde la app móvil
- Funcionar offline (lectura + inspecciones) en la app móvil
- Sincronizar automáticamente al recuperar conexión
- Cambiar el estado de un árbol y reflejarse en tiempo real

✅ **Calidad de código**:
- Cobertura de tests > 70%
- 0 vulnerabilidades críticas (SonarQube, Dependabot)
- 0 bugs P0 en producción después de 2 semanas
- Performance: API p95 < 500ms

### Criterios de Producto

✅ **Usuario debe poder completar estos flujos**:

1. **Setup inicial** (< 30 minutos):
   - Registrarse
   - Crear tenant
   - Crear finca
   - Importar árboles desde CSV
   - Ver cuadrícula con todos los árboles

2. **Inspección desde móvil** (< 2 minutos):
   - Abrir app
   - Escanear QR de árbol
   - Tomar foto
   - Seleccionar estado
   - Guardar inspección (offline ok)

3. **Monitoreo diario** (< 5 minutos):
   - Abrir dashboard web
   - Ver cuadrícula actualizada
   - Identificar árboles con problemas (rojos/amarillos)
   - Click en árbol → ver detalle e historial
   - Cambiar estado de árbol

### Criterios de Negocio

✅ **Validación del mercado**:
- Al menos 10 fincas registradas en el primer mes
- 80% de usuarios completan el setup inicial
- 70% de usuarios activos regresan en la segunda semana
- NPS > 40 (promotores - detractores)
- Al menos 3 casos de éxito documentados

✅ **Feedback positivo en**:
- Facilidad de setup
- Velocidad de la cuadrícula
- Utilidad de la app móvil offline
- Claridad de la información

### Criterios de Go/No-Go para Fase 2

🟢 **GO (iniciar Fase 2)** si:
- Todos los criterios técnicos cumplidos
- Al menos 8/10 usuarios completan setup exitosamente
- 0 bugs P0, < 3 bugs P1
- Feedback cualitativo mayormente positivo

🔴 **NO-GO (refinamiento adicional)** si:
- Bugs críticos sin resolver
- Cuadrícula no carga en < 2 seg con 1000 árboles
- Sincronización offline falla > 20% de las veces
- Usuarios abandonan en el setup (tasa de completado < 60%)

---

## 18.9 Equipo Sugerido

### Composición del Equipo por Fase

#### Fase 1: MVP Core (10 semanas)

| Rol | Cantidad | Dedicación | Responsabilidad Principal |
|-----|----------|------------|---------------------------|
| **Tech Lead / Fullstack Senior** | 1 | 100% | Arquitectura, decisiones técnicas, code reviews, desbloquear al equipo |
| **Backend Developer** | 1-2 | 100% | APIs REST, base de datos, autenticación, lógica de negocio |
| **Frontend Developer** | 1 | 100% | Web app en Next.js, componentes React, integración con backend |
| **Mobile Developer** | 1 | 100% | App React Native, funcionalidad offline, integración con cámara |
| **QA Engineer** | 1 | 50-75% | Testing manual y automatizado, definición de test cases |
| **Product Owner** | 1 | 50% | Priorización, refinamiento de historias, feedback de stakeholders |
| **UX/UI Designer** | 1 | 25% | Wireframes, mockups, design system |
| **DevOps** | 0.5 | 25% (compartido) | CI/CD, ambientes de staging/prod, monitoreo básico |

**Total FTE**: ~5.5 personas

#### Fase 2-3: Operaciones y Análisis (16 semanas)

| Rol | Cantidad | Dedicación | Cambios vs Fase 1 |
|-----|----------|------------|-------------------|
| **Tech Lead / Fullstack Senior** | 1 | 100% | Mantiene |
| **Backend Developer** | 2 | 100% | ➕ +1 para módulo de aplicaciones |
| **Frontend Developer** | 1 | 100% | Mantiene |
| **Mobile Developer** | 1 | 100% | Mantiene |
| **QA Engineer** | 1 | 75-100% | ➕ Aumenta dedicación |
| **Product Owner** | 1 | 50% | Mantiene |
| **UX/UI Designer** | 1 | 25% | Mantiene |
| **DevOps** | 0.5 | 50% | ➕ Aumenta para manejo de staging |

**Total FTE**: ~6.5 personas

#### Fase 4: Inteligencia (10 semanas)

| Rol | Cantidad | Dedicación | Cambios vs Fase 2-3 |
|-----|----------|------------|---------------------|
| **Tech Lead / Fullstack Senior** | 1 | 100% | Mantiene |
| **Backend Developer** | 2 | 100% | Mantiene |
| **Frontend Developer** | 1 | 100% | Mantiene |
| **Mobile Developer** | 1 | 100% | Mantiene |
| **ML Engineer** | 1 | 100% | ➕ Nuevo: modelos IA/ML |
| **Data Analyst** | 0.5 | 50% | ➕ Nuevo: procesamiento de datos |
| **QA Engineer** | 1 | 100% | Mantiene |
| **Product Owner** | 1 | 50% | Mantiene |
| **UX/UI Designer** | 1 | 25% | Mantiene |
| **DevOps** | 1 | 75% | ➕ Aumenta para ML pipeline |

**Total FTE**: ~8 personas

#### Fase 5: Enterprise (8 semanas)

| Rol | Cantidad | Dedicación | Cambios vs Fase 4 |
|-----|----------|------------|-------------------|
| **Tech Lead / Fullstack Senior** | 1 | 100% | Mantiene |
| **Backend Developer** | 2 | 100% | Mantiene |
| **Frontend Developer** | 1 | 100% | Mantiene |
| **Mobile Developer** | 1 | 100% | Mantiene (white-label mobile) |
| **DevOps Engineer** | 1 | 100% | ➕ Full-time para multi-tenant infra |
| **QA Engineer** | 1 | 100% | Mantiene |
| **Product Manager** | 1 | 75% | ➕ Aumenta para GTM strategy |
| **UX/UI Designer** | 1 | 50% | ➕ Aumenta para white-label designs |

**Total FTE**: ~7.5 personas

### Perfiles Detallados

#### Tech Lead / Fullstack Senior
**Skills requeridas**:
- 5+ años experiencia en desarrollo full-stack
- Expertise en Spring Boot y React/Next.js
- Experiencia con arquitectura multi-tenant
- Conocimiento de PostgreSQL y PostGIS
- Liderazgo técnico y mentoría

**Responsabilidades**:
- Arquitectura y decisiones técnicas de alto nivel
- Code reviews y pair programming
- Desbloquear impedimentos del equipo
- Comunicación con stakeholders técnicos
- Definir estándares de código

#### Backend Developer
**Skills requeridas**:
- 3+ años experiencia con Spring Boot
- Conocimiento sólido de JPA/Hibernate
- Experiencia con PostgreSQL
- Conocimiento de PostGIS (deseable)
- REST API design
- Testing (JUnit, Mockito)

**Responsabilidades**:
- Implementar endpoints REST
- Diseñar y optimizar queries
- Implementar lógica de negocio
- Escribir tests unitarios e integración
- Documentar APIs

#### Frontend Developer
**Skills requeridas**:
- 3+ años experiencia con React
- Conocimiento de Next.js 14 (App Router)
- TypeScript
- Tailwind CSS o similar
- Experiencia con Mapbox o Leaflet
- Testing (Jest, React Testing Library)

**Responsabilidades**:
- Implementar componentes React
- Integrar con APIs backend
- Implementar vista de cuadrícula (Canvas/Konva)
- Manejar estado global (Zustand/Redux)
- Responsive design

#### Mobile Developer
**Skills requeridas**:
- 3+ años experiencia con React Native
- Conocimiento de sincronización offline (WatermelonDB, PouchDB)
- Integración con cámara y GPS
- Expo o React Native CLI
- Testing (Jest, Detox)

**Responsabilidades**:
- Implementar app móvil iOS y Android
- Implementar sincronización offline
- Integración con cámara para fotos
- Escaneo de QR codes
- Performance optimization

#### QA Engineer
**Skills requeridas**:
- 2+ años experiencia en QA
- Testing manual y automatizado
- Herramientas: Selenium, Cypress, Postman
- Conocimiento de metodologías ágiles
- Redacción de casos de prueba

**Responsabilidades**:
- Definir estrategia de testing
- Crear y ejecutar test cases
- Automatizar tests E2E
- Reportar y seguir bugs
- Testing de regresión

### Costos Estimados (Referencia Colombia/LATAM)

| Rol | Tarifa Mensual (USD) | Fase 1 (3 meses) | Fase 2-3 (4 meses) | Fase 4-5 (4.5 meses) |
|-----|---------------------|------------------|-------------------|---------------------|
| Tech Lead | $6,000 | $18,000 | $24,000 | $27,000 |
| Backend Dev (x2) | $4,500 c/u | $13,500 | $36,000 | $40,500 |
| Frontend Dev | $4,000 | $12,000 | $16,000 | $18,000 |
| Mobile Dev | $4,000 | $12,000 | $16,000 | $18,000 |
| ML Engineer | $5,000 | - | - | $22,500 |
| QA Engineer | $3,000 | $4,500 | $10,500 | $13,500 |
| Product Owner | $4,000 | $6,000 | $8,000 | $11,250 |
| UX/UI Designer | $3,500 | $2,625 | $3,500 | $5,250 |
| DevOps | $4,000 | $3,000 | $6,000 | $13,500 |
| **Total por fase** | | **$71,625** | **$120,000** | **$169,500** |

**Total inversión en desarrollo (11 meses)**: ~$361,125 USD

> **Nota**: Estos costos son estimados para talento senior en LATAM. Ajustar según ubicación del equipo y nivel de experiencia.

### Herramientas y Licencias (costos anuales)

| Herramienta | Propósito | Costo Anual |
|-------------|-----------|-------------|
| GitHub Enterprise | Repositorio + CI/CD | $2,100 |
| AWS / GCP | Infraestructura cloud | $12,000 - $24,000 |
| Figma | Diseño | $720 |
| Linear / Jira | Project management | $1,200 |
| Sentry | Error tracking | $1,500 |
| DataDog / New Relic | Monitoring | $3,600 |
| Postman Team | API testing | $360 |
| **Total herramientas** | | **~$21,480 - $33,480** |

---

**Repositorio:** `gvaldez/ap-trees`

*Documento generado el 2025-12-08*
*Versión 1.0 - Plan de MVP y Roadmap*

---

> Navegación: [← Anterior](12-proximos-pasos.md) | [📑 Índice](README.md)
