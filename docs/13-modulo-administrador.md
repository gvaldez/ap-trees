## 13. 🔧 Módulo Administrador - Panel Interno AgroGrid

> **💼 Backoffice exclusivo del equipo AgroGrid** para gestión de la plataforma, tenants, catálogos globales y configuración del sistema.

Este módulo es el **panel de control interno** que permite al equipo de AgroGrid administrar toda la plataforma SaaS, gestionar clientes (tenants), configurar catálogos compartidos y monitorear la salud del sistema.

### 🔐 Acceso Restringido

- Solo accesible por personal interno de AgroGrid
- Requiere autenticación de dos factores (2FA)
- Logs de auditoría completos de todas las acciones
- Subdomain dedicado: `admin.agrogrid.io`

---

## 13.1 Roles Internos

### Jerarquía de Permisos Internos

```
┌──────────────────────────────────────────────────────────┐
│                     EQUIPO AGROGRID                      │
├──────────────────────────────────────────────────────────┤
│  SuperAdmin (Fundadores/CTO)                             │
│  ├── Acceso total al sistema                             │
│  ├── Gestión de roles internos                           │
│  ├── Configuración de seguridad                          │
│  └── Acceso a producción                                 │
│                                                           │
│  Gerente de Operaciones                                  │
│  ├── Gestión de tenants                                  │
│  ├── Configuración de planes                             │
│  ├── Reportes ejecutivos                                 │
│  └── No tiene acceso a código/servidores                 │
│                                                           │
│  Soporte Técnico (Niveles 1, 2, 3)                       │
│  ├── Sistema de tickets                                  │
│  ├── Acceso temporal a tenants (con auditoría)           │
│  ├── Consulta de logs y métricas                         │
│  └── Base de conocimiento                                │
│                                                           │
│  Equipo de Ventas                                        │
│  ├── Creación de demos                                   │
│  ├── Consulta de métricas comerciales                    │
│  ├── Configuración de pruebas gratuitas                  │
│  └── Solo lectura en configuración                       │
│                                                           │
│  Agrónomo/Técnico Senior                                 │
│  ├── Gestión de catálogos globales                       │
│  ├── Plagas y enfermedades                               │
│  ├── Cultivos y fenología                                │
│  └── Productos fitosanitarios                            │
└──────────────────────────────────────────────────────────┘
```

---

## 13.2 Gestión de Tenants

### 13.2.1 Crear Nuevo Tenant/Cliente

```
┌────────────────────────────────────────────────────────┐
│  CREAR NUEVO TENANT                                    │
├────────────────────────────────────────────────────────┤
│                                                         │
│  Información Básica:                                   │
│  ├── Nombre de la organización                         │
│  ├── Slug/Subdomain (empresa.agrogrid.io)             │
│  ├── País y timezone                                   │
│  ├── Idioma principal                                  │
│  └── Logo de la empresa                                │
│                                                         │
│  Plan y Límites:                                       │
│  ├── Plan: [Starter/Professional/Enterprise]           │
│  ├── Límite de árboles                                 │
│  ├── Límite de usuarios                                │
│  ├── Límite de storage (GB)                            │
│  ├── Límite de API calls/mes                           │
│  └── Features habilitados                              │
│                                                         │
│  Contacto Principal:                                   │
│  ├── Nombre del propietario                            │
│  ├── Email (será admin del tenant)                     │
│  ├── Teléfono                                           │
│  └── Rol en la empresa                                 │
│                                                         │
│  Facturación:                                          │
│  ├── Método de pago (Tarjeta/Transferencia/Factura)   │
│  ├── Ciclo de facturación (Mensual/Anual)             │
│  ├── Descuento aplicado (%)                            │
│  └── Fecha de inicio de facturación                    │
│                                                         │
│  [Cancelar]  [Crear Tenant]                            │
└────────────────────────────────────────────────────────┘
```

### 13.2.2 Acciones sobre Tenants

| Acción | Descripción | Rol requerido |
|--------|-------------|---------------|
| **Activar** | Permitir acceso al tenant | Operaciones |
| **Suspender** | Deshabilitar temporalmente (impago, investigación) | Operaciones |
| **Cancelar** | Terminar cuenta (retención de datos 30 días) | SuperAdmin |
| **Upgrade** | Cambiar a plan superior | Operaciones/Ventas |
| **Downgrade** | Cambiar a plan inferior (con validación de límites) | Operaciones |
| **Extender trial** | Agregar días de prueba gratuita | Ventas |
| **Ajustar límites** | Modificar cuotas sin cambiar plan | Operaciones |
| **Acceso de soporte** | Generar token temporal para entrar al tenant | Soporte L2/L3 |

### 13.2.3 Métricas por Tenant

Vista consolidada de uso y actividad:

```
TENANT: Finca Paraíso (finca-paraiso.agrogrid.io)
Plan: Professional | Estado: Activo | MRR: $199

┌─────────────────────────────────────────────────────────┐
│  MÉTRICAS DE USO                                        │
├─────────────────────────────────────────────────────────┤
│  📊 Recursos:                                           │
│  ├── Árboles: 3,847 / 5,000 (77%)                      │
│  ├── Usuarios activos: 8 / 10                          │
│  ├── Storage: 4.3 GB / 50 GB                           │
│  ├── API calls (mes): 127,543 / 500,000                │
│  └── Imágenes drone procesadas: 23 (incluidas)         │
│                                                          │
│  👥 Actividad (últimos 30 días):                        │
│  ├── Logins: 247                                        │
│  ├── Inspecciones registradas: 1,542                    │
│  ├── Aplicaciones fitosanitarias: 34                    │
│  ├── Reportes generados: 89                             │
│  └── Tickets de soporte: 2 (resueltos)                 │
│                                                          │
│  💰 Facturación:                                        │
│  ├── Próximo cobro: 2026-01-15                          │
│  ├── Método: Visa •••• 4242                             │
│  ├── MRR: $199 USD                                      │
│  ├── LTV estimado: $7,164                               │
│  └── Días como cliente: 347                             │
│                                                          │
│  🚨 Alertas:                                            │
│  └── ⚠️ Próximo a límite de árboles (77%)               │
│                                                          │
│  [Ver Detalles] [Ajustar Plan] [Acceso Soporte]        │
└─────────────────────────────────────────────────────────┘
```

---

## 13.3 Catálogos Globales

Gestión centralizada de datos compartidos por todos los tenants.

### 13.3.1 Cultivos

```sql
-- Tabla de cultivos globales
CREATE TABLE global_cultivos (
    id VARCHAR(50) PRIMARY KEY,  -- 'aguacate', 'limon', 'durazno'
    nombre_cientifico VARCHAR(200),
    nombres_comunes JSONB,  -- {"es": "Aguacate", "en": "Avocado"}
    familia VARCHAR(100),
    categoria VARCHAR(50),  -- 'frutal', 'citrico', 'tropical'
    imagen_url TEXT,
    activo BOOLEAN DEFAULT true,
    metadata JSONB,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Variedades por cultivo
CREATE TABLE global_variedades (
    id SERIAL PRIMARY KEY,
    cultivo_id VARCHAR(50) REFERENCES global_cultivos(id),
    nombre VARCHAR(100),
    descripcion TEXT,
    caracteristicas JSONB,
    activo BOOLEAN DEFAULT true
);
```

**Panel de Gestión:**
- CRUD completo de cultivos
- Asignar variedades a cada cultivo
- Configurar etapas fenológicas base
- Cargar imágenes de referencia
- Activar/desactivar cultivos

### 13.3.2 Plagas y Enfermedades

```sql
CREATE TABLE global_plagas_enfermedades (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(50) UNIQUE,  -- 'AG-001', 'LM-034'
    nombre_comun VARCHAR(200),
    nombre_cientifico VARCHAR(200),
    tipo VARCHAR(50),  -- 'plaga', 'enfermedad', 'deficiencia'
    categoria VARCHAR(100),  -- 'insecto', 'hongo', 'bacteria', 'virus'
    descripcion TEXT,
    sintomas TEXT[],
    condiciones_favorables TEXT,
    imagenes_url TEXT[],
    activo BOOLEAN DEFAULT true,
    severidad_default VARCHAR(20) DEFAULT 'media'
);

-- Relación con cultivos afectados
CREATE TABLE global_plaga_cultivo (
    plaga_id INT REFERENCES global_plagas_enfermedades(id),
    cultivo_id VARCHAR(50) REFERENCES global_cultivos(id),
    severidad_tipica VARCHAR(20),  -- 'baja', 'media', 'alta', 'critica'
    epoca_critica VARCHAR(100),  -- 'Primavera', 'Verano', 'Todo el año'
    PRIMARY KEY (plaga_id, cultivo_id)
);
```

**Funcionalidades:**
- Agregar nueva plaga/enfermedad al sistema
- Asociar con cultivos específicos
- Subir imágenes de identificación
- Definir tratamientos recomendados
- Configurar alertas automáticas

### 13.3.3 Productos Fitosanitarios Base

```sql
CREATE TABLE global_productos_fitosanitarios (
    id SERIAL PRIMARY KEY,
    nombre_comercial VARCHAR(200),
    ingrediente_activo VARCHAR(200),
    fabricante VARCHAR(200),
    tipo VARCHAR(50),  -- 'insecticida', 'fungicida', 'herbicida', etc.
    formulacion VARCHAR(100),  -- 'EC', 'WP', 'SC', etc.
    concentracion VARCHAR(50),
    modo_accion VARCHAR(100),
    dosis_recomendada JSONB,  -- {"min": 2, "max": 4, "unidad": "L/ha"}
    intervalo_seguridad_dias INT,  -- PHI: Pre-harvest interval
    toxicidad VARCHAR(50),  -- 'I', 'II', 'III', 'IV'
    registro_sanitario VARCHAR(100),
    aprobado BOOLEAN DEFAULT false,
    activo BOOLEAN DEFAULT true
);
```

**Panel de Productos:**
- Catálogo de productos comerciales
- Ingredientes activos y formulaciones
- Dosis recomendadas por cultivo
- Intervalos de seguridad
- Compatibilidades entre productos

### 13.3.4 Unidades de Medida

Catálogo estandarizado de unidades:
- Área: ha, m², acre
- Volumen: L, gal, m³
- Peso: kg, lb, ton
- Concentración: %, ppm, g/L
- Distancia: m, km, ft

---

## 13.4 Facturación y Planes

### 13.4.1 Configuración de Planes

```
┌──────────────────────────────────────────────────────────┐
│  GESTIÓN DE PLANES                                       │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  PLAN: Professional                                      │
│  ├── Precio mensual: $199 USD                            │
│  ├── Precio anual: $1,990 USD (descuento 17%)            │
│  ├── Límites:                                            │
│  │   ├── Árboles: 5,000                                  │
│  │   ├── Usuarios: 10                                    │
│  │   ├── Storage: 50 GB                                  │
│  │   └── API calls: 500,000/mes                          │
│  ├── Features incluidos:                                 │
│  │   ✅ Vista de cuadrícula                              │
│  │   ✅ Mapeo GPS                                        │
│  │   ✅ App móvil                                        │
│  │   ✅ Reportes avanzados                               │
│  │   ✅ API REST                                         │
│  │   ✅ Análisis de imágenes (5/mes)                     │
│  │   ❌ White-label                                      │
│  │   ❌ Soporte dedicado                                 │
│  │   ❌ Análisis de drones ilimitados                    │
│  └── Soporte: Email (48h response)                       │
│                                                           │
│  [Editar] [Duplicar] [Desactivar]                        │
└──────────────────────────────────────────────────────────┘
```

### 13.4.2 Historial de Facturación

Vista por tenant de todas las transacciones:
- Fecha de transacción
- Concepto (suscripción, upgrade, add-on)
- Monto cobrado
- Estado (exitoso, fallido, reembolsado)
- Método de pago
- Factura generada (PDF)

### 13.4.3 Upgrades y Downgrades

**Proceso de Upgrade:**
1. Cliente solicita desde su panel o ventas lo ofrece
2. Sistema calcula cargo prorrateado
3. Validación de método de pago
4. Aplicación inmediata de nuevos límites
5. Facturación ajustada

**Proceso de Downgrade:**
1. Validar que uso actual cumpla con límites del nuevo plan
2. Si excede límites:
   - Opción 1: Posponer downgrade hasta reducir uso
   - Opción 2: Forzar eliminación de datos (con confirmación)
3. Aplicar al final del período actual
4. Notificar al cliente

---

## 13.5 Soporte Técnico

### 13.5.1 Sistema de Tickets

```
┌─────────────────────────────────────────────────────────┐
│  TICKET #1847                                           │
├─────────────────────────────────────────────────────────┤
│  Estado: 🟡 En progreso                                  │
│  Prioridad: Alta                                        │
│  Categoría: Problema técnico                            │
│  Tenant: Finca El Bosque                                │
│  Usuario reporta: María García (Agrónoma)               │
│  Asignado a: Carlos Ruiz (Soporte L2)                   │
│  Creado: 2025-12-08 14:23                               │
│  SLA: Responder en 4h (quedan 2h 15m)                   │
│                                                          │
│  ──────────────────────────────────────────────────────│
│  PROBLEMA:                                              │
│  "Las imágenes de drone subidas ayer no han terminado   │
│  de procesarse. Muestra 'En cola' desde hace 18 horas." │
│                                                          │
│  ──────────────────────────────────────────────────────│
│  CONVERSACIÓN:                                          │
│                                                          │
│  Carlos [15:40]: Revisé los logs. El job de             │
│  procesamiento falló por timeout. Reintentando...       │
│                                                          │
│  Sistema [15:42]: Procesamiento reiniciado              │
│  Job ID: proc_28476_retry                               │
│                                                          │
│  Carlos [16:05]: Completado. Por favor revisar:         │
│  https://finca-el-bosque.agrogrid.io/drones/flight-847  │
│                                                          │
│  ──────────────────────────────────────────────────────│
│  [Ver Logs del Sistema] [Acceso Temporal al Tenant]    │
│  [Agregar Nota] [Cambiar Estado] [Escalar a L3]        │
└─────────────────────────────────────────────────────────┘
```

### 13.5.2 Acceso de Soporte a Tenants

**Protocolo de acceso temporal:**
1. Ticket debe estar abierto y asignado
2. Soporte L2/L3 solicita acceso temporal
3. Se genera token de acceso con:
   - Duración: 2 horas (extendible)
   - Permisos: Solo lectura (o específicos según necesidad)
   - Auditoría: Cada acción es registrada
4. Notificación al propietario del tenant
5. Acceso automáticamente revocado al expirar

### 13.5.3 Base de Conocimiento Interna

Documentación para el equipo de soporte:
- Problemas comunes y soluciones
- Guías de troubleshooting
- Scripts de diagnóstico
- Escalamiento a desarrollo
- FAQs internas

### 13.5.4 Logs de Actividad

Registro de todas las acciones en el sistema:
- Acciones de usuarios (quién, qué, cuándo)
- Cambios en configuración
- Errores y excepciones
- Performance de APIs
- Jobs de background

---

## 13.6 Configuración de Plataforma

### 13.6.1 Integraciones Globales

**Proveedores de Drones:**
```javascript
{
  "drone_providers": [
    {
      "id": "dji_terra",
      "name": "DJI Terra",
      "api_endpoint": "https://api.dji.com/v2",
      "auth_type": "oauth2",
      "supported_formats": ["JPG", "TIFF", "DNG"],
      "active": true
    },
    {
      "id": "pix4d",
      "name": "Pix4D Cloud",
      "api_endpoint": "https://cloud.pix4d.com/api",
      "auth_type": "api_key",
      "supported_formats": ["JPG", "TIFF"],
      "active": true
    }
  ]
}
```

**APIs Satelitales:**
- Sentinel Hub
- Planet Labs
- Landsat
- MODIS

### 13.6.2 Configuración de Modelos de IA/ML

```
┌─────────────────────────────────────────────────────────┐
│  MODELOS DE MACHINE LEARNING                            │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  DETECCIÓN DE PLAGAS - YOLO v8                          │
│  ├── Versión: v8.2-custom-agro                          │
│  ├── Última actualización: 2025-11-15                   │
│  ├── Accuracy: 94.3%                                    │
│  ├── Clases detectadas: 47                              │
│  ├── Uso (mes actual): 12,847 inferencias               │
│  └── Estado: ✅ Activo                                   │
│                                                          │
│  CLASIFICACIÓN NDVI                                     │
│  ├── Versión: CNN-ResNet50-modified                     │
│  ├── Última actualización: 2025-10-03                   │
│  ├── F1-Score: 0.91                                     │
│  ├── Categorías: 5 (Muy bajo a Muy alto)                │
│  └── Estado: ✅ Activo                                   │
│                                                          │
│  CONTEO DE FRUTOS                                       │
│  ├── Versión: FruitCounter v3.1                         │
│  ├── Estado: 🔧 En entrenamiento                         │
│  ├── Cultivos soportados: 6                             │
│  └── Release estimado: 2026-01-20                       │
│                                                          │
│  [Entrenar Nuevo Modelo] [Ver Métricas] [Rollback]     │
└─────────────────────────────────────────────────────────┘
```

### 13.6.3 Parámetros del Sistema

Configuración global de comportamiento:
- Timeout de sesiones
- Límites de rate limiting
- Tamaño máximo de archivos
- Retención de datos
- Políticas de backup
- Configuración de emails
- Webhooks globales

### 13.6.4 Feature Flags

Control de funcionalidades en producción:

| Feature | Descripción | Estado | Tenants |
|---------|-------------|--------|---------|
| `drone_analysis_v2` | Nueva versión del análisis de drones | Beta | 5 tenants |
| `mobile_offline_mode` | Modo offline en app móvil | GA | Todos |
| `ai_prescription` | Prescripciones automáticas con IA | Alpha | 2 tenants |
| `multi_currency` | Soporte para múltiples monedas | Dev | Ninguno |
| `blockchain_traceability` | Trazabilidad con blockchain | Planned | - |

---

## 13.7 White-Label / Branding

### 13.7.1 Configuración por Tenant

```
┌──────────────────────────────────────────────────────────┐
│  BRANDING: Finca Paraíso                                 │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  Dominio Personalizado:                                  │
│  ├── URL: app.fincaparaiso.com                           │
│  ├── SSL: ✅ Válido hasta 2026-08-15                     │
│  └── DNS: ✅ Configurado correctamente                   │
│                                                           │
│  Identidad Visual:                                       │
│  ├── Logo principal: [📁 logo.svg] [Cambiar]             │
│  ├── Favicon: [📁 favicon.ico] [Cambiar]                 │
│  ├── Color primario: #2E7D32 [●]                         │
│  ├── Color secundario: #558B2F [●]                       │
│  └── Tipografía: Roboto                                  │
│                                                           │
│  Emails:                                                 │
│  ├── Remitente: "Finca Paraíso" <no-reply@finca...>     │
│  ├── Template: Custom (basado en branding)               │
│  └── Footer: Logo + info de contacto                     │
│                                                           │
│  Reportes:                                               │
│  ├── Header: Logo + nombre de finca                      │
│  ├── Footer: Powered by AgroGrid (oculto en Enterprise)  │
│  └── Marca de agua: Configurable                         │
│                                                           │
│  [Vista Previa] [Aplicar Cambios] [Restaurar Default]   │
└──────────────────────────────────────────────────────────┘
```

### 13.7.2 Templates de Emails

Personalización de comunicaciones:
- Email de bienvenida
- Recuperación de contraseña
- Alertas de sistema
- Reportes automáticos
- Invitaciones de usuarios
- Notificaciones de facturación

---

## 13.8 Monitoreo

### 13.8.1 Dashboard de Salud de Plataforma

```
┌──────────────────────────────────────────────────────────┐
│  🏥 SALUD DE LA PLATAFORMA                               │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  Estado General: ✅ Operacional                           │
│  Última verificación: hace 30 segundos                   │
│                                                           │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  SERVICIOS CRÍTICOS                                 │ │
│  ├─────────────────────────────────────────────────────┤ │
│  │  API Backend      [●] Healthy    Latencia: 45ms    │ │
│  │  Base de Datos    [●] Healthy    CPU: 34% Mem: 62% │ │
│  │  Cache (Redis)    [●] Healthy    Hit rate: 94.3%   │ │
│  │  Queue (RabbitMQ) [●] Healthy    Pending: 127      │ │
│  │  Storage (S3)     [●] Healthy    98.7% uptime      │ │
│  │  ML Service       [●] Healthy    Queue: 3 jobs     │ │
│  │  CDN              [●] Healthy    Origin hits: 8.2% │ │
│  └─────────────────────────────────────────────────────┘ │
│                                                           │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  MÉTRICAS GLOBALES (Últimas 24h)                   │ │
│  ├─────────────────────────────────────────────────────┤ │
│  │  Tenants activos:     143 (+2)                      │ │
│  │  Usuarios online:     1,847                         │ │
│  │  Requests/min:        8,432 (normal)                │ │
│  │  Error rate:          0.03% (excelente)             │ │
│  │  Avg response time:   127ms (bueno)                 │ │
│  │  Storage usado:       2.4 TB / 10 TB                │ │
│  │  Jobs procesados:     47,342                        │ │
│  │  ML inferences:       15,623                        │ │
│  └─────────────────────────────────────────────────────┘ │
│                                                           │
│  🚨 ALERTAS ACTIVAS: 2                                    │
│  ├── ⚠️ WARNING: DB replica lag 15s (umbral: 10s)        │
│  └── ℹ️ INFO: Scheduled maintenance en 72 horas          │
│                                                           │
│  [Ver Logs] [Métricas Detalladas] [Configurar Alertas]  │
└──────────────────────────────────────────────────────────┘
```

### 13.8.2 Métricas de Uso Global

Análisis agregado de todos los tenants:
- Árboles totales en el sistema
- Usuarios activos diarios (DAU)
- Inspecciones registradas por día
- API calls por servicio
- Uso de storage por región
- Jobs de ML procesados
- Distribución geográfica de tenants

### 13.8.3 Alertas del Sistema

Configuración de notificaciones automáticas:

| Alerta | Condición | Canal | Responsable |
|--------|-----------|-------|-------------|
| **Service Down** | Servicio no responde > 2min | PagerDuty + Slack | DevOps |
| **High Error Rate** | Errors > 1% durante 5min | Slack | Backend Team |
| **DB Performance** | Query time > 2s | Email | DBA |
| **Storage Critical** | Storage > 90% | Email + Slack | Ops |
| **Security Alert** | Login fallidos > 10 | Email | Security Team |
| **Payment Failed** | Cargo rechazado | Email | Billing Team |

### 13.8.4 Performance y Errores

Dashboard de APM (Application Performance Monitoring):
- Traces de transacciones
- Flamegraphs de performance
- Slow queries
- Memory leaks
- Error tracking con stack traces
- Real User Monitoring (RUM)

---

## 13.9 Modelo de Datos - Configuraciones Internas

```sql
-- Staff interno de AgroGrid
CREATE TABLE internal_staff (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    nombre VARCHAR(200),
    rol VARCHAR(50),  -- 'superadmin', 'ops', 'soporte_l1', etc.
    permisos JSONB,
    activo BOOLEAN DEFAULT true,
    two_factor_enabled BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Registro de accesos de soporte a tenants
CREATE TABLE support_access_log (
    id SERIAL PRIMARY KEY,
    staff_id UUID REFERENCES internal_staff(id),
    tenant_id UUID REFERENCES tenants(id),
    motivo TEXT,
    ticket_id INT,
    token_expiration TIMESTAMP,
    permisos_concedidos JSONB,
    acciones_realizadas JSONB[],
    created_at TIMESTAMP DEFAULT NOW(),
    revoked_at TIMESTAMP
);

-- Configuración de planes
CREATE TABLE subscription_plans (
    id VARCHAR(50) PRIMARY KEY,  -- 'starter', 'professional', 'enterprise'
    nombre VARCHAR(100),
    precio_mensual DECIMAL(10,2),
    precio_anual DECIMAL(10,2),
    limites JSONB,  -- {"arboles": 5000, "usuarios": 10, ...}
    features JSONB,  -- ["api_access", "reports", "mobile_app", ...]
    activo BOOLEAN DEFAULT true,
    orden INT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Facturación
CREATE TABLE billing_transactions (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    tipo VARCHAR(50),  -- 'subscription', 'upgrade', 'addon'
    plan_id VARCHAR(50) REFERENCES subscription_plans(id),
    monto DECIMAL(10,2),
    moneda VARCHAR(3) DEFAULT 'USD',
    estado VARCHAR(50),  -- 'pending', 'paid', 'failed', 'refunded'
    metodo_pago VARCHAR(50),
    transaction_id VARCHAR(200),  -- ID de Stripe/PayPal
    factura_url TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    paid_at TIMESTAMP
);

-- Tickets de soporte
CREATE TABLE support_tickets (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    usuario_reporta_id INT,
    asignado_a UUID REFERENCES internal_staff(id),
    prioridad VARCHAR(20),  -- 'baja', 'media', 'alta', 'critica'
    categoria VARCHAR(100),
    estado VARCHAR(50),  -- 'nuevo', 'en_progreso', 'esperando', 'resuelto'
    titulo VARCHAR(200),
    descripcion TEXT,
    sla_respuesta_minutos INT,
    sla_resolucion_minutos INT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    resolved_at TIMESTAMP
);

-- Feature flags
CREATE TABLE feature_flags (
    id VARCHAR(100) PRIMARY KEY,
    nombre VARCHAR(200),
    descripcion TEXT,
    estado VARCHAR(20),  -- 'dev', 'alpha', 'beta', 'ga', 'deprecated'
    habilitado_global BOOLEAN DEFAULT false,
    rollout_percentage INT DEFAULT 0,  -- 0-100
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tenants con feature flags específicos
CREATE TABLE tenant_feature_flags (
    tenant_id UUID REFERENCES tenants(id),
    feature_flag_id VARCHAR(100) REFERENCES feature_flags(id),
    habilitado BOOLEAN DEFAULT true,
    configuracion JSONB,
    PRIMARY KEY (tenant_id, feature_flag_id)
);
```

---

## 13.10 API Endpoints del Backoffice

### Gestión de Tenants

```
GET    /admin/api/tenants              # Listar todos los tenants
POST   /admin/api/tenants              # Crear nuevo tenant
GET    /admin/api/tenants/:id          # Detalles de tenant
PUT    /admin/api/tenants/:id          # Actualizar tenant
DELETE /admin/api/tenants/:id          # Eliminar tenant (soft delete)

POST   /admin/api/tenants/:id/activate    # Activar
POST   /admin/api/tenants/:id/suspend     # Suspender
POST   /admin/api/tenants/:id/upgrade     # Upgrade de plan
POST   /admin/api/tenants/:id/downgrade   # Downgrade de plan

GET    /admin/api/tenants/:id/metrics     # Métricas de uso
GET    /admin/api/tenants/:id/billing     # Historial facturación
```

### Catálogos Globales

```
GET    /admin/api/cultivos                # Listar cultivos
POST   /admin/api/cultivos                # Crear cultivo
PUT    /admin/api/cultivos/:id            # Actualizar
DELETE /admin/api/cultivos/:id            # Eliminar

GET    /admin/api/plagas                  # Listar plagas
POST   /admin/api/plagas                  # Crear plaga
PUT    /admin/api/plagas/:id              # Actualizar
DELETE /admin/api/plagas/:id              # Eliminar

POST   /admin/api/plagas/:id/cultivos     # Asociar con cultivos
```

### Soporte

```
GET    /admin/api/tickets                 # Listar tickets
POST   /admin/api/tickets                 # Crear ticket
GET    /admin/api/tickets/:id             # Detalle de ticket
PUT    /admin/api/tickets/:id             # Actualizar ticket

POST   /admin/api/support/access          # Solicitar acceso a tenant
      Body: {tenant_id, motivo, ticket_id}
      Response: {access_token, expires_at}
```

### Monitoreo

```
GET    /admin/api/health                  # Estado de servicios
GET    /admin/api/metrics/global          # Métricas agregadas
GET    /admin/api/metrics/tenants         # Métricas por tenant
GET    /admin/api/logs                    # Logs del sistema
GET    /admin/api/alerts                  # Alertas activas
```

---

## 13.11 Permisos por Rol Interno

| Funcionalidad | SuperAdmin | Ops | Soporte L3 | Soporte L2 | Ventas | Agrónomo |
|---------------|------------|-----|------------|------------|--------|----------|
| Ver todos los tenants | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Crear tenant | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ |
| Modificar tenant | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Suspender/cancelar | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Configurar planes | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Ver facturación | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ |
| Gestionar catálogos | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ |
| Ver tickets | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Asignar tickets | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Acceso temporal a tenant | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Configurar IA/ML | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Feature flags | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Ver monitoreo | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Configurar alertas | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Acceso a producción | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |

---

## 13.12 Diagrama de Arquitectura del Módulo

```
┌────────────────────────────────────────────────────────────────┐
│                    MÓDULO ADMINISTRADOR                        │
│                   (admin.agrogrid.io)                          │
└────────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
┌──────────────┐   ┌─────────────────┐   ┌──────────────┐
│   Gestión    │   │   Catálogos     │   │   Soporte    │
│   Tenants    │   │   Globales      │   │   Técnico    │
└──────┬───────┘   └────────┬────────┘   └──────┬───────┘
       │                    │                    │
       │                    │                    │
       ▼                    ▼                    ▼
┌──────────────────────────────────────────────────────────┐
│              BASE DE DATOS PRINCIPAL                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐              │
│  │ Tenants  │  │ Cultivos │  │ Tickets  │              │
│  │ Plans    │  │ Plagas   │  │ Logs     │              │
│  │ Billing  │  │ Productos│  │ Metrics  │              │
│  └──────────┘  └──────────┘  └──────────┘              │
└──────────────────────────────────────────────────────────┘
       │
       ├─────────────────┬─────────────────┬────────────────┐
       ▼                 ▼                 ▼                ▼
┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐
│ Facturación│   │ Monitoreo  │   │  Feature   │   │   Logs     │
│  (Stripe)  │   │ (Grafana)  │   │   Flags    │   │(CloudWatch)│
└────────────┘   └────────────┘   └────────────┘   └────────────┘
```

---

## 📚 Documentos Relacionados

- [Modelo de Negocio SaaS](02-modelo-negocio-saas.md) - Arquitectura multi-tenant y planes
- [Módulo Usuarios](14-modulo-usuarios.md) - Roles y permisos dentro de tenants
- [Arquitectura Técnica](08-arquitectura-tecnica.md) - Stack tecnológico completo

---

> Navegación: [← Anterior](12-proximos-pasos.md) | [📑 Índice](README.md) | [Siguiente →](14-modulo-usuarios.md)
