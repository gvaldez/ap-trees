## 14. 👥 Módulo Usuarios - Roles y Permisos del Tenant

> **🏢 Sistema de roles y permisos para clientes de AgroGrid**, diseñado para equipos agrícolas con diferentes niveles de acceso y responsabilidades.

Este módulo define la estructura de usuarios dentro de cada tenant (cliente), permitiendo una gestión granular de permisos y accesos según el rol de cada persona en la operación agrícola.

---

## 14.1 Descripción General

Cada tenant de AgroGrid puede tener múltiples usuarios con diferentes roles y responsabilidades. El sistema de permisos está diseñado para reflejar la jerarquía típica de una operación agrícola moderna.

### Características Clave

- **Jerarquía de roles flexible** adaptada a la estructura organizacional
- **Permisos granulares** por funcionalidad y módulo
- **Gestión de equipos** y asignación de fincas/lotes
- **Invitaciones por email** con onboarding guiado
- **Acceso por nivel** de responsabilidad
- **Auditoría completa** de acciones por usuario

---

## 14.2 Jerarquía de Roles dentro del Tenant

```
┌──────────────────────────────────────────────────────────────┐
│                    TENANT: Finca El Paraíso                  │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  👤 Propietario/Admin del Tenant                             │
│  │  (Juan Pérez - propietario@fincaparaiso.com)             │
│  ├── Acceso total a la organización                          │
│  ├── Gestión de usuarios y permisos                          │
│  ├── Configuración del tenant                                │
│  ├── Facturación y suscripción                               │
│  └── Sin restricciones                                       │
│                                                               │
│  ├─► 👔 Gerente/Admin de Finca (2 usuarios)                  │
│  │   │  (María González, Carlos Ruiz)                        │
│  │   ├── Gestión de fincas asignadas                         │
│  │   ├── Reportes y dashboards                               │
│  │   ├── Aprobación de tareas y compras                      │
│  │   └── Configuración operativa                             │
│  │                                                            │
│  │   ├─► 🌱 Agrónomo/Técnico (3 usuarios)                    │
│  │   │   │  (Ana Martínez, Luis Torres, Sofia Ramírez)      │
│  │   │   ├── Planificación de actividades                    │
│  │   │   ├── Diagnóstico y prescripciones                    │
│  │   │   ├── Análisis de datos                               │
│  │   │   └── Configuración de alertas                        │
│  │   │                                                        │
│  │   │   ├─► 🚜 Supervisor de Campo (2 usuarios)            │
│  │   │   │   │  (Pedro Sánchez, Roberto Díaz)               │
│  │   │   │   ├── Asignación de tareas a operarios           │
│  │   │   │   ├── Validación de trabajo realizado             │
│  │   │   │   ├── Reportes de campo                           │
│  │   │   │   └── Coordinación de equipos                     │
│  │   │   │                                                    │
│  │   │   │   ├─► 👷 Operario/Trabajador (8 usuarios)        │
│  │   │   │   │   ├── App móvil simplificada                  │
│  │   │   │   │   ├── Ejecutar tareas asignadas               │
│  │   │   │   │   ├── Registrar inspecciones                  │
│  │   │   │   │   └── Reportar hallazgos                      │
│  │   │   │                                                    │
│  │   │   ├─► 🚁 Operador Drone (1 usuario)                  │
│  │   │       │  (Miguel Herrera)                             │
│  │   │       ├── Subir y procesar imágenes                   │
│  │   │       ├── Configurar análisis                         │
│  │   │       └── Ver resultados de IA                        │
│  │   │                                                        │
│  │   └─► 👁️ Solo Lectura/Invitado (3 usuarios)             │
│  │       │  (Inversionistas, consultores externos)           │
│  │       ├── Ver dashboards y reportes                       │
│  │       ├── Sin capacidad de edición                        │
│  │       └── Acceso temporal o permanente                    │
│                                                               │
│  Total usuarios: 20 / 25 (plan Professional)                │
└──────────────────────────────────────────────────────────────┘
```

---

## 14.3 Descripción Detallada de Roles

### 14.3.1 👤 Propietario/Admin del Tenant

**Perfil:** Dueño de la finca o CEO de la organización agrícola.

**Responsabilidades:**
- Toma de decisiones estratégicas
- Configuración inicial del sistema
- Gestión del equipo completo
- Control de gastos y presupuestos
- Supervisión general de operaciones

**Acceso:**
- ✅ **TOTAL** a todos los módulos y funcionalidades
- ✅ Gestión de usuarios (invitar, editar roles, eliminar)
- ✅ Configuración del tenant (nombre, logo, preferencias)
- ✅ Facturación y cambio de plan
- ✅ Integraciones y APIs
- ✅ Exportación masiva de datos
- ✅ Configuración de alertas globales

**Limitaciones:**
- ❌ Ninguna (acceso sin restricciones)

**Ejemplo de caso de uso:**
> Juan, propietario de 3 fincas de aguacate, revisa dashboards ejecutivos, aprueba compras importantes, invita nuevos agrónomos al sistema y configura alertas críticas a su email.

---

### 14.3.2 👔 Gerente/Administrador de Finca

**Perfil:** Gerente operativo, administrador de finca o jefe de producción.

**Responsabilidades:**
- Operación diaria de una o varias fincas
- Supervisión de equipos técnicos
- Aprobación de actividades y gastos
- Análisis de reportes de producción
- Coordinación con agrónomos

**Acceso:**
- ✅ Dashboard completo de fincas asignadas
- ✅ Reportes y análisis avanzados
- ✅ Gestión de inventario y compras
- ✅ Planificación de actividades
- ✅ Aprobación de tareas planificadas
- ✅ Configuración de lotes y sectores
- ✅ Ver todos los árboles y su estado
- ✅ Configurar alertas para sus fincas
- ⚠️ **Limitado** a fincas específicas asignadas

**Limitaciones:**
- ❌ No puede gestionar usuarios del tenant
- ❌ No puede ver facturación
- ❌ No puede cambiar configuración global del tenant
- ❌ No puede acceder a fincas no asignadas

**Ejemplo de caso de uso:**
> María, gerente de 2 fincas, revisa el dashboard de producción, aprueba un plan de aplicación de fungicidas propuesto por el agrónomo, y genera un reporte mensual de costos para Juan.

---

### 14.3.3 🌱 Agrónomo/Técnico

**Perfil:** Ingeniero agrónomo, técnico especializado o asesor técnico.

**Responsabilidades:**
- Diagnóstico de plagas y enfermedades
- Prescripción de tratamientos
- Planificación técnica de actividades
- Análisis de datos agronómicos
- Asesoría técnica al equipo

**Acceso:**
- ✅ Vista de cuadrícula y mapa
- ✅ Registro y diagnóstico de problemas
- ✅ Crear prescripciones de aplicaciones
- ✅ Planificación semanal de tareas
- ✅ Ver análisis de imágenes de drone
- ✅ Configurar alertas fenológicas
- ✅ Reportes técnicos (salud, NDVI, fenología)
- ✅ Gestión de catálogo de productos del tenant
- ⚠️ **No puede aprobar** compras importantes

**Limitaciones:**
- ❌ No puede eliminar fincas o lotes
- ❌ No puede gestionar usuarios
- ❌ No puede ver costos detallados (solo cantidades)
- ❌ No puede cambiar configuración de infraestructura

**Ejemplo de caso de uso:**
> Ana, agrónoma, detecta en la cuadrícula un posible foco de Phytophthora. Registra el diagnóstico en 15 árboles afectados, prescribe una aplicación de fungicida específico con dosis calculadas y planifica la tarea para el próximo martes.

---

### 14.3.4 🚜 Supervisor de Campo

**Perfil:** Capataz, supervisor de cuadrilla o jefe de campo.

**Responsabilidades:**
- Coordinación de trabajadores de campo
- Asignación diaria de tareas
- Validación de trabajo completado
- Reporte de avances
- Control de asistencia

**Acceso:**
- ✅ Ver tareas planificadas
- ✅ Asignar tareas a operarios específicos
- ✅ Ver ubicación y estado de árboles
- ✅ Validar y cerrar tareas completadas
- ✅ Registrar inspecciones rápidas
- ✅ App móvil con funciones de supervisión
- ✅ Reportes de avance diario
- ⚠️ **No puede crear** planes de aplicación

**Limitaciones:**
- ❌ No puede diagnosticar ni prescribir
- ❌ No puede ver análisis de costos
- ❌ No puede modificar estructura de finca/lotes
- ❌ No puede acceder a módulo de compras
- ❌ No puede ver información de facturación

**Ejemplo de caso de uso:**
> Pedro revisa en su tablet las 12 tareas pendientes para hoy, asigna 5 operarios a la inspección del Sector Norte, valida el trabajo de ayer (340 árboles inspeccionados) y reporta un hallazgo urgente al agrónomo.

---

### 14.3.5 👷 Operario/Trabajador de Campo

**Perfil:** Trabajador de campo, operador, jornalero o personal de ejecución.

**Responsabilidades:**
- Ejecutar tareas asignadas
- Inspeccionar árboles según indicaciones
- Registrar hallazgos simples
- Reportar problemas detectados
- Trabajar en campo con app móvil

**Acceso:**
- ✅ **App móvil simplificada** (interfaz básica)
- ✅ Ver tareas asignadas a su persona
- ✅ Escanear QR de árboles
- ✅ Registrar inspección simple (estado de salud)
- ✅ Tomar fotos de hallazgos
- ✅ Marcar tareas como completadas
- ✅ Modo offline (sincronización posterior)
- ⚠️ **Solo sus tareas** asignadas

**Limitaciones:**
- ❌ No puede ver información de otros operarios
- ❌ No puede planificar ni crear tareas
- ❌ No puede diagnosticar problemas complejos
- ❌ No puede acceder a módulo web completo
- ❌ No puede ver reportes ni dashboards
- ❌ No puede ver información de costos

**Ejemplo de caso de uso:**
> José, con su celular, abre la app AgroGrid, ve que tiene asignadas 150 inspecciones en Lote A-3. Escanea el QR del primer árbol, registra "Estado: Bueno", toma una foto de frutos, y marca como completado. Detecta un árbol con hojas amarillas, reporta "Problema detectado" y sube una foto.

---

### 14.3.6 🚁 Operador de Drone/Técnico de Imágenes

**Perfil:** Piloto de drones, técnico en teledetección o especialista en imágenes.

**Responsabilidades:**
- Captura de imágenes aéreas
- Procesamiento de imágenes multiespectrales
- Configuración de análisis de IA
- Interpretación de índices de vegetación
- Mantenimiento de drones

**Acceso:**
- ✅ Módulo de drones y vuelos
- ✅ Subir imágenes (RAW, TIFF, JPEG)
- ✅ Configurar parámetros de procesamiento
- ✅ Ver resultados de análisis (NDVI, GNDVI, etc.)
- ✅ Generar mapas de salud
- ✅ Crear alertas desde análisis de imágenes
- ✅ Ver historial de vuelos
- ⚠️ **No puede** modificar tratamientos

**Limitaciones:**
- ❌ No puede prescribir tratamientos
- ❌ No puede gestionar tareas de campo
- ❌ No puede ver información de costos
- ❌ Acceso limitado a módulos no relacionados con imágenes

**Ejemplo de caso de uso:**
> Miguel sube 847 imágenes del vuelo del lunes sobre el Sector Norte. Configura el análisis NDVI con umbral de alerta <0.6, el sistema procesa las imágenes en 30 minutos y genera 23 alertas en la zona noroeste que Miguel envía al agrónomo para revisión.

---

### 14.3.7 👁️ Solo Lectura/Invitado

**Perfil:** Inversionista, consultor externo, auditor, asesor temporal o stakeholder.

**Responsabilidades:**
- Revisión de reportes
- Análisis de resultados
- Auditoría externa
- Consultoría puntual

**Acceso:**
- ✅ Ver dashboards principales
- ✅ Ver reportes generados
- ✅ Ver mapas y estado de fincas
- ✅ Exportar reportes en PDF
- ✅ Ver métricas de producción
- ⚠️ **Solo lectura** en todo el sistema

**Limitaciones:**
- ❌ No puede editar ningún dato
- ❌ No puede crear tareas ni planes
- ❌ No puede ver información financiera sensible
- ❌ No puede gestionar usuarios
- ❌ No puede acceder a configuraciones
- ❌ No puede ver datos de costos (opcional según configuración)

**Ejemplo de caso de uso:**
> Un consultor externo contratado por 3 meses recibe acceso de "Solo Lectura" para revisar los dashboards de producción, analizar tendencias de salud de los cultivos y generar recomendaciones en un informe externo, sin poder modificar datos del sistema.

---

## 14.4 Matriz de Permisos Detallada

### Tabla de Permisos por Funcionalidad

| Funcionalidad | Propietario | Gerente | Agrónomo | Supervisor | Operario | Drone Op. | Lectura |
|---------------|:-----------:|:-------:|:--------:|:----------:|:--------:|:---------:|:-------:|
| **GESTIÓN DE TENANT** |
| Configurar tenant | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Ver facturación | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Cambiar plan | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Gestionar usuarios | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Invitar usuarios | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **FINCAS Y ESTRUCTURA** |
| Crear finca | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Editar finca | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| Eliminar finca | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Crear sectores/lotes | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| Registrar árboles | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ |
| Editar árboles | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ |
| Eliminar árboles | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| Ver mapa de finca | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ |
| Ver cuadrícula | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ |
| **INSPECCIONES Y SALUD** |
| Ver estado de árboles | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ |
| Registrar inspección | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Diagnosticar problemas | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Ver historial de salud | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| Configurar alertas | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **PLANIFICACIÓN Y TAREAS** |
| Ver planificación | ✅ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ | ✅ |
| Crear plan de actividad | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Aprobar plan | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| Asignar tareas | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Ver tareas asignadas | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ |
| Completar tareas | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ |
| **APLICACIONES** |
| Ver aplicaciones | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ | ✅ |
| Prescribir tratamiento | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Calcular dosis | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Registrar aplicación | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| Ver historial fitosanitario | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **DRONES E IMÁGENES** |
| Ver vuelos | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ✅ |
| Crear vuelo | ✅ | ✅ | ⚠️ | ❌ | ❌ | ✅ | ❌ |
| Subir imágenes | ✅ | ✅ | ⚠️ | ❌ | ❌ | ✅ | ❌ |
| Configurar análisis | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ |
| Ver resultados NDVI | ✅ | ✅ | ✅ | ⚠️ | ❌ | ✅ | ✅ |
| **INVENTARIO Y COMPRAS** |
| Ver inventario | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ | ✅ |
| Registrar compra | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| Aprobar compra | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Ver costos | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ⚠️ |
| Registrar consumo | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| **REPORTES Y ANÁLISIS** |
| Ver dashboards | ✅ | ✅ | ✅ | ⚠️ | ❌ | ⚠️ | ✅ |
| Generar reportes | ✅ | ✅ | ✅ | ⚠️ | ❌ | ⚠️ | ✅ |
| Exportar datos | ✅ | ✅ | ✅ | ❌ | ❌ | ⚠️ | ⚠️ |
| Ver análisis financiero | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| **RIEGO** |
| Ver sistema de riego | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ | ✅ |
| Configurar riego | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Activar/desactivar válvulas | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Ver historial riego | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **COSECHA** |
| Ver datos de cosecha | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ | ✅ |
| Registrar cosecha | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Ver análisis de rendimiento | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ | ✅ |

**Leyenda:**
- ✅ Acceso completo
- ⚠️ Acceso parcial o con restricciones
- ❌ Sin acceso

---

## 14.5 Flujo de Invitación de Usuarios

### 14.5.1 Proceso de Invitación

```
┌──────────────────────────────────────────────────────────┐
│  PASO 1: Propietario invita nuevo usuario               │
├──────────────────────────────────────────────────────────┤
│  Formulario de invitación:                               │
│  ├── Email del invitado                                  │
│  ├── Nombre completo                                     │
│  ├── Rol: [Seleccionar rol ▼]                           │
│  ├── Fincas asignadas: [☑ Finca Norte] [☐ Finca Sur]   │
│  ├── Mensaje personalizado (opcional)                    │
│  └── [Enviar Invitación]                                 │
└──────────────────────────────────────────────────────────┘
                    ⬇️
┌──────────────────────────────────────────────────────────┐
│  PASO 2: Sistema envía email de invitación              │
├──────────────────────────────────────────────────────────┤
│  Para: maria.gonzalez@example.com                        │
│  Asunto: Te han invitado a Finca El Paraíso en AgroGrid │
│                                                          │
│  Hola María,                                             │
│                                                          │
│  Juan Pérez te ha invitado a unirte a su organización   │
│  "Finca El Paraíso" en AgroGrid como Gerente de Finca.  │
│                                                          │
│  [Aceptar Invitación]                                    │
│                                                          │
│  Esta invitación expira en 7 días.                      │
└──────────────────────────────────────────────────────────┘
                    ⬇️
┌──────────────────────────────────────────────────────────┐
│  PASO 3: Usuario acepta y crea cuenta                   │
├──────────────────────────────────────────────────────────┤
│  Registro:                                               │
│  ├── Email: maria.gonzalez@example.com (prellenado)     │
│  ├── Crear contraseña: [•••••••]                        │
│  ├── Confirmar contraseña: [•••••••]                    │
│  ├── Teléfono (opcional): [+57 ...]                     │
│  └── [Crear Cuenta]                                      │
└──────────────────────────────────────────────────────────┘
                    ⬇️
┌──────────────────────────────────────────────────────────┐
│  PASO 4: Onboarding guiado según rol                    │
├──────────────────────────────────────────────────────────┤
│  Bienvenida María! Como Gerente de Finca tendrás:       │
│  ✓ Acceso a 1 finca asignada                            │
│  ✓ Dashboards de producción                             │
│  ✓ Gestión de tareas y aprobaciones                     │
│                                                          │
│  Tour rápido:                                            │
│  1️⃣ Tu dashboard principal                              │
│  2️⃣ Cómo ver el estado de la finca                      │
│  3️⃣ Cómo aprobar planes de actividad                    │
│  4️⃣ Cómo generar reportes                               │
│                                                          │
│  [Comenzar Tour] [Omitir e Ir al Dashboard]             │
└──────────────────────────────────────────────────────────┘
```

### 14.5.2 Estados de Invitación

| Estado | Descripción | Acciones disponibles |
|--------|-------------|----------------------|
| **Pendiente** | Invitación enviada, no aceptada | Reenviar email, Cancelar |
| **Expirada** | 7 días sin aceptar | Reenviar nueva invitación |
| **Aceptada** | Usuario creó cuenta y tiene acceso | Ver perfil, Editar permisos |
| **Cancelada** | Invitación cancelada antes de aceptar | Enviar nueva si es necesario |

---

## 14.6 Modelo de Datos de Usuarios y Permisos

```sql
-- Usuarios dentro de un tenant
CREATE TABLE tenant_users (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    email VARCHAR(255) UNIQUE NOT NULL,
    nombre VARCHAR(200),
    telefono VARCHAR(50),
    rol VARCHAR(50),  -- 'propietario', 'gerente', 'agronomo', etc.
    avatar_url TEXT,
    activo BOOLEAN DEFAULT true,
    email_verificado BOOLEAN DEFAULT false,
    ultimo_login TIMESTAMP,
    preferencias JSONB,  -- idioma, timezone, notificaciones
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    UNIQUE (tenant_id, email)
);

-- Asignación de fincas a usuarios
CREATE TABLE user_finca_assignment (
    user_id INT REFERENCES tenant_users(id),
    finca_id INT REFERENCES fincas(id),
    permisos_especificos JSONB,  -- permisos custom para esta finca
    created_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (user_id, finca_id)
);

-- Invitaciones pendientes
CREATE TABLE user_invitations (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    email VARCHAR(255) NOT NULL,
    nombre VARCHAR(200),
    rol VARCHAR(50),
    token VARCHAR(100) UNIQUE NOT NULL,
    invitado_por INT REFERENCES tenant_users(id),
    mensaje TEXT,
    fincas_asignadas INT[],
    estado VARCHAR(50) DEFAULT 'pendiente',  -- pendiente, aceptada, expirada, cancelada
    expira_en TIMESTAMP,
    aceptada_en TIMESTAMP,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Log de actividad de usuarios
CREATE TABLE user_activity_log (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    user_id INT REFERENCES tenant_users(id),
    accion VARCHAR(100),  -- 'login', 'crear_tarea', 'editar_arbol', etc.
    modulo VARCHAR(50),  -- 'fincas', 'planificacion', 'reportes', etc.
    recurso_tipo VARCHAR(50),  -- 'arbol', 'tarea', 'aplicacion'
    recurso_id INT,
    detalles JSONB,
    ip_address INET,
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Permisos personalizados por usuario (excepciones)
CREATE TABLE user_custom_permissions (
    user_id INT REFERENCES tenant_users(id),
    permiso VARCHAR(100),  -- 'fincas.crear', 'aplicaciones.prescribir', etc.
    habilitado BOOLEAN DEFAULT true,
    razon TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (user_id, permiso)
);
```

---

## 14.7 Configuración de Notificaciones por Rol

### 14.7.1 Preferencias de Notificaciones

```
┌─────────────────────────────────────────────────────────┐
│  CONFIGURACIÓN DE NOTIFICACIONES                        │
│  Usuario: María González (Gerente de Finca)            │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Canal de notificaciones:                               │
│  ☑ Email: maria.gonzalez@example.com                   │
│  ☑ Notificaciones push (app móvil)                     │
│  ☑ SMS: +57 300 123 4567 (solo urgente)                │
│  ☐ WhatsApp Business                                    │
│                                                          │
│  ──────────────────────────────────────────────────────│
│  Eventos a notificar:                                   │
│                                                          │
│  🌱 SALUD Y ALERTAS                                     │
│  ├─ ☑ Nueva alerta crítica           [Email + Push]    │
│  ├─ ☑ Alerta media/alta               [Push]           │
│  ├─ ☐ Alerta baja                     [Ninguno]        │
│  └─ ☑ Resumen diario de alertas       [Email 8:00am]   │
│                                                          │
│  📋 TAREAS Y PLANIFICACIÓN                              │
│  ├─ ☑ Tarea requiere aprobación       [Email + Push]   │
│  ├─ ☑ Tarea completada importante     [Push]           │
│  ├─ ☐ Todas las tareas completadas    [Ninguno]        │
│  └─ ☑ Resumen semanal de tareas       [Email Lunes]    │
│                                                          │
│  💰 COMPRAS Y COSTOS                                    │
│  ├─ ☑ Compra requiere aprobación      [Email]          │
│  ├─ ☑ Presupuesto excedido            [Email + SMS]    │
│  └─ ☑ Informe mensual de costos       [Email día 1]    │
│                                                          │
│  🚁 DRONES E IMÁGENES                                   │
│  ├─ ☑ Análisis completado             [Push]           │
│  ├─ ☐ Nuevas alertas desde drone      [Push]           │
│  └─ ☐ Cada vuelo programado           [Ninguno]        │
│                                                          │
│  👥 EQUIPO                                              │
│  ├─ ☑ Nuevo usuario agregado          [Email]          │
│  ├─ ☐ Usuario completó tarea          [Ninguno]        │
│  └─ ☑ Reporte de actividad semanal    [Email Viernes]  │
│                                                          │
│  [Guardar Preferencias]                                 │
└─────────────────────────────────────────────────────────┘
```

### 14.7.2 Notificaciones Recomendadas por Rol

| Tipo de Notificación | Propietario | Gerente | Agrónomo | Supervisor | Operario | Drone Op. |
|----------------------|:-----------:|:-------:|:--------:|:----------:|:--------:|:---------:|
| Alerta crítica plaga | ✅ Email+SMS | ✅ Email+Push | ✅ Email+Push | ⚠️ Push | ❌ | ⚠️ Push |
| Tarea asignada | ❌ | ⚠️ Push | ⚠️ Push | ✅ Push | ✅ Push | ⚠️ Push |
| Tarea completada | ❌ | ⚠️ Push | ⚠️ Push | ✅ Push | ❌ | ❌ |
| Compra requiere aprobación | ✅ Email | ✅ Email+Push | ❌ | ❌ | ❌ | ❌ |
| Análisis drone listo | ⚠️ Email | ✅ Push | ✅ Push | ❌ | ❌ | ✅ Push |
| Resumen diario | ✅ Email | ✅ Email | ⚠️ Email | ⚠️ Push | ❌ | ❌ |
| Resumen semanal | ✅ Email | ✅ Email | ✅ Email | ⚠️ Email | ❌ | ⚠️ Email |
| Presupuesto excedido | ✅ SMS+Email | ✅ Email | ❌ | ❌ | ❌ | ❌ |
| Sistema caído | ✅ SMS | ❌ | ❌ | ❌ | ❌ | ❌ |

---

## 14.8 Gestión de Usuarios - Panel de Administración

### 14.8.1 Lista de Usuarios

```
┌──────────────────────────────────────────────────────────────┐
│  GESTIÓN DE USUARIOS - Finca El Paraíso                     │
│  20 usuarios activos / 25 disponibles (Plan Professional)   │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  [+ Invitar Usuario]  [🔽 Exportar Lista]  [⚙️ Configurar]  │
│                                                               │
│  Filtros: [Todos ▼] [Todas las fincas ▼]  🔍 Buscar...      │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │ Nombre          │ Rol        │ Fincas │ Último acceso │  │
│  ├────────────────────────────────────────────────────────┤  │
│  │ 👤 Juan Pérez   │ Propietario│ Todas  │ Hace 2h     ⚙️│  │
│  │ 👔 María G.     │ Gerente    │ 1      │ Hace 30m    ⚙️│  │
│  │ 👔 Carlos Ruiz  │ Gerente    │ 2      │ Hace 1h     ⚙️│  │
│  │ 🌱 Ana Martínez │ Agrónomo   │ Todas  │ Hace 15m    ⚙️│  │
│  │ 🌱 Luis Torres  │ Agrónomo   │ 1      │ Hace 4h     ⚙️│  │
│  │ 🌱 Sofia R.     │ Agrónomo   │ 1      │ Ayer        ⚙️│  │
│  │ 🚜 Pedro S.     │ Supervisor │ 1      │ Hace 1h     ⚙️│  │
│  │ 🚜 Roberto D.   │ Supervisor │ 1      │ Hace 45m    ⚙️│  │
│  │ 👷 José López   │ Operario   │ 1      │ Hace 2h     ⚙️│  │
│  │ 👷 Miguel V.    │ Operario   │ 1      │ Hace 3h     ⚙️│  │
│  │ ... (10 operarios más)                               │  │
│  │ 🚁 Miguel H.    │ Drone Op.  │ Todas  │ Hace 1d     ⚙️│  │
│  │ 👁️ Investor LLC │ Lectura    │ Todas  │ Hace 2d     ⚙️│  │
│  └────────────────────────────────────────────────────────┘  │
│                                                               │
│  📧 INVITACIONES PENDIENTES (2)                              │
│  ┌────────────────────────────────────────────────────────┐  │
│  │ agrónoma2@example.com │ Agrónomo │ Enviado hace 3d  ❌│  │
│  │ operario8@example.com │ Operario │ Enviado hace 1d  ❌│  │
│  └────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### 14.8.2 Editar Usuario

```
┌─────────────────────────────────────────────────────────┐
│  EDITAR USUARIO: María González                         │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Información básica:                                    │
│  ├── Nombre: [María González           ]               │
│  ├── Email: maria.gonzalez@example.com (no editable)   │
│  ├── Teléfono: [+57 300 123 4567       ]               │
│  └── Estado: ● Activo [Desactivar usuario]             │
│                                                          │
│  Rol y permisos:                                        │
│  ├── Rol: [Gerente de Finca ▼]                         │
│  └── Permisos personalizados:                           │
│      ☐ Puede eliminar lotes                             │
│      ☑ Puede ver costos detallados                      │
│      ☐ Acceso a módulo de facturación                   │
│                                                          │
│  Fincas asignadas:                                      │
│  ├── ☑ Finca Norte (Sector A, B, C)                    │
│  ├── ☐ Finca Sur                                        │
│  └── ☐ Finca Este                                       │
│                                                          │
│  Notificaciones:                                        │
│  └── [Configurar preferencias →]                        │
│                                                          │
│  Estadísticas:                                          │
│  ├── Miembro desde: 2025-03-15 (268 días)              │
│  ├── Último acceso: Hace 30 minutos                     │
│  ├── Logins totales: 247                                │
│  ├── Tareas completadas: 89                             │
│  └── Inspecciones registradas: 1,542                    │
│                                                          │
│  [Guardar Cambios]  [Cancelar]  [Eliminar Usuario]     │
└─────────────────────────────────────────────────────────┘
```

---

## 14.9 Casos de Uso por Rol

### Caso 1: Propietario - Revisión Ejecutiva Semanal

**Actor:** Juan Pérez (Propietario)
**Frecuencia:** Semanal
**Tiempo:** 30 minutos

**Flujo:**
1. Login al sistema
2. Ve dashboard ejecutivo con métricas clave
3. Revisa resumen de alertas críticas (si hay)
4. Analiza reporte de costos vs presupuesto
5. Verifica estado de tareas importantes
6. Revisa notificaciones pendientes de aprobación
7. Genera reporte mensual para inversionistas

---

### Caso 2: Agrónomo - Diagnóstico y Prescripción

**Actor:** Ana Martínez (Agrónoma)
**Frecuencia:** Diaria
**Tiempo:** 2-3 horas

**Flujo:**
1. Revisa alertas nuevas del día
2. Analiza cuadrícula de lotes con problemas
3. Ve imágenes de drone del Sector Norte
4. Identifica foco de Phytophthora en 15 árboles
5. Crea diagnóstico: "Phytophthora cinnamomi - Severidad Alta"
6. Prescribe tratamiento: Fosetil-Al 80% WP
7. Calcula dosis: 3 kg/ha = 450g por árbol afectado
8. Crea tarea de aplicación para Supervisor
9. Configura alerta de seguimiento en 7 días
10. Envía notificación al Gerente

---

### Caso 3: Operario - Ejecución de Inspección

**Actor:** José López (Operario)
**Frecuencia:** Diaria
**Tiempo:** 6-8 horas

**Flujo:**
1. Abre app móvil en campo
2. Ve lista de tareas asignadas: "Inspeccionar Lote A-3"
3. Inicia tarea (GPS marca inicio)
4. Escanea QR del árbol #A3-001
5. Registra estado: Bueno
6. Toma foto de frutos
7. Repite para 150 árboles
8. Detecta árbol con problema (hojas amarillas)
9. Reporta: "Problema detectado - Hojas amarillas"
10. Sube 3 fotos
11. Finaliza jornada (sincroniza offline data)

---

### Caso 4: Supervisor - Coordinación de Equipo

**Actor:** Pedro Sánchez (Supervisor)
**Frecuencia:** Diaria
**Tiempo:** Full-time

**Flujo:**
1. Revisa tareas planificadas para hoy
2. Ve 5 tareas pendientes
3. Asigna 3 operarios a inspección de Sector Norte
4. Asigna 2 operarios a aplicación de fungicida
5. Monitorea progreso en tiempo real
6. Valida tareas completadas de ayer
7. Detecta un reporte de problema urgente
8. Notifica al agrónomo
9. Cierra tareas validadas
10. Genera reporte diario de avance

---

## 📚 Documentos Relacionados

- [Módulo Administrador](13-modulo-administrador.md) - Panel interno del equipo AgroGrid
- [Customer Journeys](15-customer-journeys.md) - Flujos detallados de usuario
- [App Móvil de Campo](modulos/07-07-app-movil-campo.md) - Funcionalidades de la app para operarios

---

> Navegación: [← Anterior](13-modulo-administrador.md) | [📑 Índice](README.md) | [Siguiente →](15-customer-journeys.md)
