## 8. Arquitectura Técnica

### 8.1 Stack Tecnológico

```
Frontend:
├── Web: Next.js 14 (App Router)
├── Móvil: React Native / Expo
├── Mapas: Mapbox GL JS
└── Gráficos: D3.js / Recharts

Backend:
├── API: Java 17 + Spring Boot 3.2
├── Seguridad: Spring Security 6 + JWT
├── ORM: Spring Data JPA + Hibernate
├── Base de datos: PostgreSQL 15 + PostGIS + TimescaleDB
├── Cache: Redis
├── Cola: RabbitMQ
├── Storage: S3-compatible (MinIO / AWS S3)
└── Docs API: SpringDoc OpenAPI (Swagger)

ML/AI (Microservicio separado):
├── Framework: Python + FastAPI
├── ML: PyTorch / TensorFlow
├── Procesamiento: GDAL, Rasterio
├── Modelos: YOLO (detección), CNN (clasificación)
└── MLOps: MLflow

Infraestructura:
├── Cloud: AWS / GCP / Azure
├── Containers: Docker + Kubernetes
├── CDN: CloudFlare
├── CI/CD: GitHub Actions
└── Monitoring: Grafana + Prometheus
```

---

### 8.2 Arquitectura de Seguridad

#### 8.2.1 Flujo de Autenticación JWT

```
┌─────────────────────────────────────────────────────────────────┐
│                    FLUJO DE AUTENTICACIÓN                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. LOGIN                                                       │
│  ┌─────────┐         POST /api/auth/login         ┌──────────┐ │
│  │ Cliente │────────────────────────────────────▶  │ Spring   │ │
│  │ (Web/   │  {email, password}                    │ Security │ │
│  │  App)   │                                       └────┬─────┘ │
│  └─────────┘                                            │       │
│      │                                                  ▼       │
│      │                                        ┌─────────────────┤
│      │                                        │ Valida en DB    ││
│      │                                        │ usuarios table  ││
│      │                                        └────────┬────────┤
│      │                                                 │        │
│      │◀────────────────────────────────────────────────┘        │
│      │  200 OK:                                                │
│      │  {                                                      │
│      │    "accessToken": "eyJhbGc...",   (15 min)             │
│      │    "refreshToken": "dGhpc2lz...", (7 días)             │
│      │    "tokenType": "Bearer",                              │
│      │    "user": {...}                                        │
│      │  }                                                      │
│      │                                                         │
│  2. SOLICITUDES AUTENTICADAS                                   │
│  ┌─────────┐    GET /api/fincas                 ┌──────────┐  │
│  │ Cliente │────────────────────────────────────▶│  API     │  │
│  │         │    Header: Authorization: Bearer... │  Rest    │  │
│  └─────────┘                                     └────┬─────┘  │
│      │                                                │        │
│      │                                                ▼        │
│      │                                    ┌───────────────────┤│
│      │                                    │ JwtAuthFilter     │││
│      │                                    │ - Valida token    │││
│      │                                    │ - Extrae usuario  │││
│      │                                    │ - Set tenant ctx  │││
│      │                                    └──────┬────────────┤│
│      │◀───────────────────────────────────────────┘           │
│      │  200 OK + data                                         │
│      │                                                         │
│  3. REFRESH TOKEN                                             │
│  ┌─────────┐    POST /api/auth/refresh         ┌──────────┐  │
│  │ Cliente │────────────────────────────────────▶│ Auth     │  │
│  │         │    {refreshToken: "..."}            │ Service  │  │
│  └─────────┘                                     └────┬─────┘  │
│      │                                                │        │
│      │                                                ▼        │
│      │                                    ┌───────────────────┤│
│      │                                    │ Valida refresh    │││
│      │                                    │ en tabla sesiones │││
│      │                                    └──────┬────────────┤│
│      │◀───────────────────────────────────────────┘           │
│      │  Nuevo accessToken + refreshToken                      │
│      │                                                         │
│  4. LOGOUT                                                    │
│  ┌─────────┐    POST /api/auth/logout          ┌──────────┐  │
│  │ Cliente │────────────────────────────────────▶│ Auth     │  │
│  │         │    Header: Authorization: Bearer... │ Service  │  │
│  └─────────┘                                     └────┬─────┘  │
│      │                                                │        │
│      │                                                ▼        │
│      │                                    ┌───────────────────┤│
│      │                                    │ Invalida sesión   │││
│      │                                    │ Elimina tokens    │││
│      │                                    └──────┬────────────┤│
│      │◀───────────────────────────────────────────┘           │
│      │  200 OK                                                │
└─────────────────────────────────────────────────────────────────┘
```

#### 8.2.2 Estructura de Tokens

**Access Token JWT** (15 minutos de validez):
```json
{
  "sub": "usuario_id",
  "email": "juan@finca.com",
  "tenant_id": "uuid-tenant",
  "roles": ["ROLE_TENANT_MANAGER"],
  "finca_id": 123,
  "iat": 1701234567,
  "exp": 1701235467
}
```

**Refresh Token** (7 días de validez):
- Token opaco almacenado en tabla `sesiones`
- Permite renovar accessToken sin reautenticación
- Se invalida en logout o cambio de contraseña

#### 8.2.3 Configuración Spring Security

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .sessionManagement(session -> 
                session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**", "/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasAnyRole("SUPER_ADMIN", "ADMIN_SOPORTE")
                .requestMatchers("/api/tenant/**").hasRole("TENANT_OWNER")
                .anyRequest().authenticated()
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);
        
        return http.build();
    }
}
```

#### 8.2.4 Row Level Security (RLS) Multi-Tenant

PostgreSQL RLS garantiza aislamiento de datos a nivel de base de datos:

```sql
-- Configurar variable de sesión con tenant_id
SET app.current_tenant = 'uuid-del-tenant';

-- Política RLS aplicada automáticamente
CREATE POLICY tenant_isolation ON fincas
    USING (tenant_id = current_setting('app.current_tenant')::UUID);

-- Spring Boot establece tenant context automáticamente en cada request
```

---

### 8.3 Modelo de Datos Completo

#### 8.3.1 Multi-Tenant Base

```sql
-- ============================================================================
-- MULTI-TENANT BASE
-- ============================================================================

-- Tenant (Cliente/Organización)
CREATE TABLE tenants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nombre VARCHAR(100) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    plan_id INTEGER REFERENCES planes(id),
    razon_social VARCHAR(150),
    rfc VARCHAR(20),
    direccion TEXT,
    ciudad VARCHAR(100),
    pais VARCHAR(50) DEFAULT 'México',
    telefono VARCHAR(20),
    email_contacto VARCHAR(100),
    configuracion JSONB DEFAULT '{}',
    activo BOOLEAN DEFAULT true,
    fecha_registro TIMESTAMP DEFAULT NOW(),
    fecha_activacion TIMESTAMP,
    fecha_suspension TIMESTAMP,
    motivo_suspension TEXT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_tenants_slug ON tenants(slug);
CREATE INDEX idx_tenants_activo ON tenants(activo);

-- Cultivos habilitados por tenant
CREATE TABLE tenant_cultivos (
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    cultivo_id VARCHAR(50) NOT NULL,
    configuracion_especifica JSONB DEFAULT '{}',
    activo BOOLEAN DEFAULT true,
    fecha_habilitacion TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (tenant_id, cultivo_id)
);

CREATE INDEX idx_tenant_cultivos_tenant ON tenant_cultivos(tenant_id);
```

---

#### 8.3.2 Seguridad y Usuarios

```sql
-- ============================================================================
-- SEGURIDAD Y USUARIOS
-- ============================================================================

-- Usuarios del sistema
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL, -- BCrypt hash
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100),
    telefono VARCHAR(20),
    avatar_url VARCHAR(255),
    
    -- Control de cuenta
    activo BOOLEAN DEFAULT true,
    email_verificado BOOLEAN DEFAULT false,
    email_verification_token VARCHAR(255),
    reset_password_token VARCHAR(255),
    reset_password_expiry TIMESTAMP,
    
    -- Seguridad
    failed_login_attempts INTEGER DEFAULT 0,
    account_locked_until TIMESTAMP,
    ultimo_login TIMESTAMP,
    ultimo_cambio_password TIMESTAMP DEFAULT NOW(),
    requiere_cambio_password BOOLEAN DEFAULT false,
    
    -- Spring Security fields
    account_non_expired BOOLEAN DEFAULT true,
    account_non_locked BOOLEAN DEFAULT true,
    credentials_non_expired BOOLEAN DEFAULT true,
    enabled BOOLEAN DEFAULT true,
    
    -- Auditoría
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    created_by INTEGER REFERENCES usuarios(id),
    updated_by INTEGER REFERENCES usuarios(id)
);

CREATE INDEX idx_usuarios_email ON usuarios(email);
CREATE INDEX idx_usuarios_tenant ON usuarios(tenant_id);
CREATE INDEX idx_usuarios_activo ON usuarios(activo);

-- Roles del sistema
CREATE TABLE roles (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(50) UNIQUE NOT NULL,
    descripcion TEXT,
    nivel VARCHAR(20) NOT NULL, -- 'plataforma' o 'tenant'
    permisos_default INTEGER[], -- IDs de permisos
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Roles iniciales de plataforma
INSERT INTO roles (nombre, descripcion, nivel) VALUES
    ('ROLE_SUPER_ADMIN', 'Administrador total de la plataforma', 'plataforma'),
    ('ROLE_ADMIN_SOPORTE', 'Soporte técnico con acceso multi-tenant', 'plataforma'),
    ('ROLE_ADMIN_VENTAS', 'Equipo de ventas y onboarding', 'plataforma');

-- Roles iniciales de tenant
INSERT INTO roles (nombre, descripcion, nivel) VALUES
    ('ROLE_TENANT_OWNER', 'Dueño/propietario de la organización', 'tenant'),
    ('ROLE_TENANT_MANAGER', 'Gerente con acceso completo', 'tenant'),
    ('ROLE_TENANT_AGRONOMO', 'Agrónomo/ingeniero con acceso técnico', 'tenant'),
    ('ROLE_TENANT_SUPERVISOR', 'Supervisor de campo', 'tenant'),
    ('ROLE_TENANT_OPERARIO', 'Trabajador de campo', 'tenant'),
    ('ROLE_TENANT_VIEWER', 'Solo lectura/consulta', 'tenant');

-- Permisos granulares por módulo
CREATE TABLE permisos (
    id SERIAL PRIMARY KEY,
    modulo VARCHAR(50) NOT NULL, -- 'fincas', 'arboles', 'inspecciones', etc.
    accion VARCHAR(50) NOT NULL, -- 'crear', 'leer', 'actualizar', 'eliminar', 'aprobar'
    nombre VARCHAR(100) UNIQUE NOT NULL, -- 'fincas:crear', 'arboles:eliminar'
    descripcion TEXT,
    activo BOOLEAN DEFAULT true
);

-- Permisos por módulo
INSERT INTO permisos (modulo, accion, nombre, descripcion) VALUES
    -- Fincas
    ('fincas', 'crear', 'fincas:crear', 'Crear nuevas fincas'),
    ('fincas', 'leer', 'fincas:leer', 'Ver información de fincas'),
    ('fincas', 'actualizar', 'fincas:actualizar', 'Editar fincas'),
    ('fincas', 'eliminar', 'fincas:eliminar', 'Eliminar fincas'),
    
    -- Sectores
    ('sectores', 'crear', 'sectores:crear', 'Crear sectores'),
    ('sectores', 'leer', 'sectores:leer', 'Ver sectores'),
    ('sectores', 'actualizar', 'sectores:actualizar', 'Editar sectores'),
    ('sectores', 'eliminar', 'sectores:eliminar', 'Eliminar sectores'),
    
    -- Lotes
    ('lotes', 'crear', 'lotes:crear', 'Crear lotes'),
    ('lotes', 'leer', 'lotes:leer', 'Ver lotes'),
    ('lotes', 'actualizar', 'lotes:actualizar', 'Editar lotes'),
    ('lotes', 'eliminar', 'lotes:eliminar', 'Eliminar lotes'),
    
    -- Árboles
    ('arboles', 'crear', 'arboles:crear', 'Registrar árboles'),
    ('arboles', 'leer', 'arboles:leer', 'Ver árboles'),
    ('arboles', 'actualizar', 'arboles:actualizar', 'Editar árboles'),
    ('arboles', 'eliminar', 'arboles:eliminar', 'Eliminar árboles'),
    
    -- Inspecciones
    ('inspecciones', 'crear', 'inspecciones:crear', 'Crear inspecciones'),
    ('inspecciones', 'leer', 'inspecciones:leer', 'Ver inspecciones'),
    ('inspecciones', 'actualizar', 'inspecciones:actualizar', 'Editar inspecciones'),
    ('inspecciones', 'eliminar', 'inspecciones:eliminar', 'Eliminar inspecciones'),
    
    -- Aplicaciones
    ('aplicaciones', 'crear', 'aplicaciones:crear', 'Programar aplicaciones'),
    ('aplicaciones', 'leer', 'aplicaciones:leer', 'Ver aplicaciones'),
    ('aplicaciones', 'actualizar', 'aplicaciones:actualizar', 'Editar aplicaciones'),
    ('aplicaciones', 'eliminar', 'aplicaciones:eliminar', 'Cancelar aplicaciones'),
    ('aplicaciones', 'aprobar', 'aplicaciones:aprobar', 'Aprobar aplicaciones'),
    
    -- Tareas
    ('tareas', 'crear', 'tareas:crear', 'Crear tareas'),
    ('tareas', 'leer', 'tareas:leer', 'Ver tareas'),
    ('tareas', 'actualizar', 'tareas:actualizar', 'Editar tareas'),
    ('tareas', 'completar', 'tareas:completar', 'Completar tareas'),
    
    -- Inventario
    ('inventario', 'crear', 'inventario:crear', 'Registrar insumos'),
    ('inventario', 'leer', 'inventario:leer', 'Ver inventario'),
    ('inventario', 'actualizar', 'inventario:actualizar', 'Ajustar inventario'),
    ('inventario', 'eliminar', 'inventario:eliminar', 'Eliminar insumos'),
    
    -- Compras
    ('compras', 'crear', 'compras:crear', 'Crear órdenes de compra'),
    ('compras', 'leer', 'compras:leer', 'Ver compras'),
    ('compras', 'aprobar', 'compras:aprobar', 'Aprobar órdenes'),
    ('compras', 'recibir', 'compras:recibir', 'Recibir mercancía'),
    
    -- Cosecha
    ('cosecha', 'crear', 'cosecha:crear', 'Registrar cosecha'),
    ('cosecha', 'leer', 'cosecha:leer', 'Ver registros de cosecha'),
    ('cosecha', 'actualizar', 'cosecha:actualizar', 'Editar cosecha'),
    
    -- Reportes
    ('reportes', 'leer', 'reportes:leer', 'Ver reportes básicos'),
    ('reportes', 'avanzados', 'reportes:avanzados', 'Ver reportes avanzados'),
    ('reportes', 'exportar', 'reportes:exportar', 'Exportar reportes'),
    
    -- Configuración
    ('config', 'leer', 'config:leer', 'Ver configuración'),
    ('config', 'actualizar', 'config:actualizar', 'Editar configuración'),
    
    -- Administración
    ('admin', 'usuarios', 'admin:usuarios', 'Gestionar usuarios'),
    ('admin', 'roles', 'admin:roles', 'Gestionar roles y permisos'),
    ('admin', 'auditoria', 'admin:auditoria', 'Ver logs de auditoría');

-- Relación roles-permisos
CREATE TABLE rol_permisos (
    rol_id INTEGER REFERENCES roles(id) ON DELETE CASCADE,
    permiso_id INTEGER REFERENCES permisos(id) ON DELETE CASCADE,
    created_at TIMESTAMP DEFAULT NOW(),
    PRIMARY KEY (rol_id, permiso_id)
);

CREATE INDEX idx_rol_permisos_rol ON rol_permisos(rol_id);
CREATE INDEX idx_rol_permisos_permiso ON rol_permisos(permiso_id);

-- Asignación de roles a usuarios (con scope por finca)
CREATE TABLE usuario_roles (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER REFERENCES usuarios(id) ON DELETE CASCADE,
    rol_id INTEGER REFERENCES roles(id),
    finca_id INTEGER REFERENCES fincas(id) ON DELETE CASCADE, -- NULL = todas las fincas
    fecha_asignacion TIMESTAMP DEFAULT NOW(),
    asignado_por INTEGER REFERENCES usuarios(id),
    activo BOOLEAN DEFAULT true,
    UNIQUE(usuario_id, rol_id, finca_id)
);

CREATE INDEX idx_usuario_roles_usuario ON usuario_roles(usuario_id);
CREATE INDEX idx_usuario_roles_rol ON usuario_roles(rol_id);
CREATE INDEX idx_usuario_roles_finca ON usuario_roles(finca_id);

-- Sesiones JWT (para refresh tokens)
CREATE TABLE sesiones (
    id SERIAL PRIMARY KEY,
    usuario_id INTEGER REFERENCES usuarios(id) ON DELETE CASCADE,
    refresh_token VARCHAR(255) UNIQUE NOT NULL,
    access_token_jti VARCHAR(255), -- JWT ID del access token actual
    ip_address VARCHAR(50),
    user_agent TEXT,
    dispositivo VARCHAR(100),
    fecha_creacion TIMESTAMP DEFAULT NOW(),
    fecha_expiracion TIMESTAMP NOT NULL,
    fecha_ultimo_uso TIMESTAMP DEFAULT NOW(),
    activa BOOLEAN DEFAULT true
);

CREATE INDEX idx_sesiones_usuario ON sesiones(usuario_id);
CREATE INDEX idx_sesiones_refresh_token ON sesiones(refresh_token);
CREATE INDEX idx_sesiones_activa ON sesiones(activa);

-- Log de auditoría de seguridad
CREATE TABLE audit_log_seguridad (
    id BIGSERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    usuario_id INTEGER REFERENCES usuarios(id),
    accion VARCHAR(50) NOT NULL, -- 'login', 'logout', 'failed_login', 'password_change', etc.
    recurso VARCHAR(100), -- tabla/entidad afectada
    recurso_id INTEGER,
    detalles JSONB,
    ip_address VARCHAR(50),
    user_agent TEXT,
    resultado VARCHAR(20), -- 'exitoso', 'fallido', 'denegado'
    fecha TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_audit_seguridad_usuario ON audit_log_seguridad(usuario_id);
CREATE INDEX idx_audit_seguridad_tenant ON audit_log_seguridad(tenant_id);
CREATE INDEX idx_audit_seguridad_fecha ON audit_log_seguridad(fecha);
CREATE INDEX idx_audit_seguridad_accion ON audit_log_seguridad(accion);
```

---

#### 8.3.3 Jerarquía del Campo

```sql
-- ============================================================================
-- JERARQUÍA DEL CAMPO
-- ============================================================================

-- Fincas (expandido)
CREATE TABLE fincas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    nombre VARCHAR(100) NOT NULL,
    codigo VARCHAR(30) UNIQUE,
    
    -- Ubicación
    direccion TEXT,
    municipio VARCHAR(100),
    estado VARCHAR(100),
    pais VARCHAR(50) DEFAULT 'México',
    latitud DECIMAL(10,8),
    longitud DECIMAL(11,8),
    altitud_msnm INTEGER,
    
    -- Geoespacial
    area_total_ha DECIMAL(10,4),
    perimetro GEOMETRY(POLYGON, 4326), -- PostGIS
    
    -- Características
    tipo_suelo VARCHAR(50),
    ph_promedio DECIMAL(4,2),
    pendiente_promedio DECIMAL(5,2),
    zona_climatica VARCHAR(50),
    
    -- Administración
    propietario_nombre VARCHAR(150),
    propietario_contacto VARCHAR(100),
    encargado_id INTEGER REFERENCES usuarios(id),
    fecha_adquisicion DATE,
    
    -- Sistema
    activa BOOLEAN DEFAULT true,
    configuracion JSONB DEFAULT '{}',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_fincas_tenant ON fincas(tenant_id);
CREATE INDEX idx_fincas_codigo ON fincas(codigo);
CREATE INDEX idx_fincas_perimetro ON fincas USING GIST(perimetro);

-- Row Level Security
ALTER TABLE fincas ENABLE ROW LEVEL SECURITY;
CREATE POLICY tenant_isolation_fincas ON fincas
    USING (tenant_id = current_setting('app.current_tenant')::UUID);

-- Sectores (divisiones dentro de la finca)
CREATE TABLE sectores (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    finca_id INTEGER REFERENCES fincas(id) ON DELETE CASCADE,
    nombre VARCHAR(100) NOT NULL,
    codigo VARCHAR(30),
    
    -- Geoespacial
    area_ha DECIMAL(10,4),
    perimetro GEOMETRY(POLYGON, 4326),
    centroide GEOMETRY(POINT, 4326),
    
    -- Características
    descripcion TEXT,
    tipo_uso VARCHAR(50), -- 'produccion', 'vivero', 'infraestructura', 'conservacion'
    
    -- Sistema
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_sectores_tenant ON sectores(tenant_id);
CREATE INDEX idx_sectores_finca ON sectores(finca_id);
CREATE INDEX idx_sectores_perimetro ON sectores USING GIST(perimetro);

ALTER TABLE sectores ENABLE ROW LEVEL SECURITY;
CREATE POLICY tenant_isolation_sectores ON sectores
    USING (tenant_id = current_setting('app.current_tenant')::UUID);

-- Lotes (unidad de manejo agronómico)
CREATE TABLE lotes (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    finca_id INTEGER REFERENCES fincas(id) ON DELETE CASCADE,
    sector_id INTEGER REFERENCES sectores(id) ON DELETE SET NULL,
    nombre VARCHAR(100) NOT NULL,
    codigo VARCHAR(30),
    
    -- Cultivo
    cultivo_id VARCHAR(50) NOT NULL,
    variedad VARCHAR(100),
    portainjerto VARCHAR(100),
    fecha_siembra DATE,
    edad_anos INTEGER,
    
    -- Geoespacial
    area_ha DECIMAL(10,4),
    perimetro GEOMETRY(POLYGON, 4326),
    centroide GEOMETRY(POINT, 4326),
    
    -- Configuración de plantación
    distancia_entre_filas DECIMAL(5,2), -- metros
    distancia_entre_arboles DECIMAL(5,2), -- metros
    densidad_arboles_ha INTEGER,
    total_arboles INTEGER,
    patron_plantacion VARCHAR(30), -- 'rectangular', 'tresbolillo', 'irregular'
    
    -- Manejo agronómico
    sistema_riego VARCHAR(50),
    tipo_poda VARCHAR(50),
    objetivo_produccion VARCHAR(50), -- 'comercial', 'semilla', 'experimental'
    
    -- Estado
    estado VARCHAR(30) DEFAULT 'activo', -- 'activo', 'en_renovacion', 'abandono'
    notas TEXT,
    
    -- Sistema
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_lotes_tenant ON lotes(tenant_id);
CREATE INDEX idx_lotes_finca ON lotes(finca_id);
CREATE INDEX idx_lotes_sector ON lotes(sector_id);
CREATE INDEX idx_lotes_cultivo ON lotes(cultivo_id);
CREATE INDEX idx_lotes_perimetro ON lotes USING GIST(perimetro);

ALTER TABLE lotes ENABLE ROW LEVEL SECURITY;
CREATE POLICY tenant_isolation_lotes ON lotes
    USING (tenant_id = current_setting('app.current_tenant')::UUID);

-- Árboles (unidad mínima de seguimiento)
CREATE TABLE arboles (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    finca_id INTEGER REFERENCES fincas(id) ON DELETE CASCADE,
    lote_id INTEGER REFERENCES lotes(id) ON DELETE CASCADE,
    
    -- Identificación
    codigo VARCHAR(50) UNIQUE NOT NULL, -- ej: "F1-L2-R3-C5"
    codigo_qr VARCHAR(100),
    fila INTEGER NOT NULL,
    columna INTEGER NOT NULL,
    
    -- Ubicación
    latitud DECIMAL(10,8),
    longitud DECIMAL(11,8),
    ubicacion GEOMETRY(POINT, 4326),
    altitud_msnm INTEGER,
    
    -- Características
    cultivo_id VARCHAR(50) NOT NULL,
    variedad VARCHAR(100),
    portainjerto VARCHAR(100),
    fecha_plantacion DATE,
    edad_anos INTEGER,
    origen VARCHAR(50), -- 'vivero', 'injerto_campo', 'reemplazo'
    
    -- Dimensiones actuales
    altura_cm INTEGER,
    diametro_tronco_cm DECIMAL(5,2),
    diametro_copa_m DECIMAL(5,2),
    
    -- Estado actual (desnormalizado para performance)
    estado_salud VARCHAR(30), -- 'excelente', 'bueno', 'regular', 'malo', 'muerto'
    estado_fenologico VARCHAR(50),
    requiere_atencion BOOLEAN DEFAULT false,
    prioridad_atencion VARCHAR(20), -- 'baja', 'media', 'alta', 'critica'
    
    -- Producción
    productivo BOOLEAN DEFAULT false,
    fecha_primera_produccion DATE,
    produccion_promedio_kg DECIMAL(8,2),
    
    -- Sistema
    activo BOOLEAN DEFAULT true,
    fecha_baja DATE,
    motivo_baja VARCHAR(100), -- 'muerte', 'eliminacion', 'reemplazo'
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    UNIQUE(lote_id, fila, columna)
);

CREATE INDEX idx_arboles_tenant ON arboles(tenant_id);
CREATE INDEX idx_arboles_finca ON arboles(finca_id);
CREATE INDEX idx_arboles_lote ON arboles(lote_id);
CREATE INDEX idx_arboles_codigo ON arboles(codigo);
CREATE INDEX idx_arboles_ubicacion ON arboles USING GIST(ubicacion);
CREATE INDEX idx_arboles_estado_salud ON arboles(estado_salud);
CREATE INDEX idx_arboles_requiere_atencion ON arboles(requiere_atencion) WHERE requiere_atencion = true;

ALTER TABLE arboles ENABLE ROW LEVEL SECURITY;
CREATE POLICY tenant_isolation_arboles ON arboles
    USING (tenant_id = current_setting('app.current_tenant')::UUID);

-- Estado actual del árbol (snapshot para cuadrícula)
CREATE TABLE arboles_estado_actual (
    arbol_id INTEGER PRIMARY KEY REFERENCES arboles(id) ON DELETE CASCADE,
    
    -- Salud
    estado_salud VARCHAR(30),
    color_indicador VARCHAR(7), -- código hex para UI
    indice_salud DECIMAL(5,2), -- 0-100
    
    -- Fenología
    estado_fenologico VARCHAR(50),
    dias_en_estado INTEGER,
    fecha_cambio_estado DATE,
    
    -- Problemas detectados
    tiene_plagas BOOLEAN DEFAULT false,
    plagas_activas INTEGER[],
    tiene_enfermedades BOOLEAN DEFAULT false,
    enfermedades_activas INTEGER[],
    tiene_deficiencias BOOLEAN DEFAULT false,
    
    -- Programación
    proxima_inspeccion DATE,
    proxima_aplicacion DATE,
    proxima_poda DATE,
    
    -- Performance
    cosecha_acumulada_kg DECIMAL(10,2) DEFAULT 0,
    ultima_cosecha DATE,
    
    -- Timestamps
    ultima_inspeccion TIMESTAMP,
    ultima_actualizacion TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_arboles_estado_salud ON arboles_estado_actual(estado_salud);
CREATE INDEX idx_arboles_estado_tiene_plagas ON arboles_estado_actual(tiene_plagas) WHERE tiene_plagas = true;

-- Historial de estados del árbol
CREATE TABLE arboles_historial (
    id BIGSERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id) ON DELETE CASCADE,
    
    -- Estado en ese momento
    estado_salud VARCHAR(30),
    estado_fenologico VARCHAR(50),
    altura_cm INTEGER,
    diametro_tronco_cm DECIMAL(5,2),
    
    -- Evento que generó el cambio
    evento_tipo VARCHAR(50), -- 'inspeccion', 'aplicacion', 'poda', 'cosecha', 'ajuste_manual'
    evento_id INTEGER,
    
    -- Observaciones
    notas TEXT,
    fotos_urls TEXT[],
    
    -- Auditoría
    registrado_por INTEGER REFERENCES usuarios(id),
    fecha_registro TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_arboles_historial_arbol ON arboles_historial(arbol_id);
CREATE INDEX idx_arboles_historial_fecha ON arboles_historial(fecha_registro);

-- Particionamiento para performance (opcional, para grandes volúmenes)
-- CREATE TABLE arboles_historial_2025_q1 PARTITION OF arboles_historial
--     FOR VALUES FROM ('2025-01-01') TO ('2025-04-01');
```

---

#### 8.3.4 Administración de Plataforma

```sql
-- ============================================================================
-- ADMINISTRACIÓN DE PLATAFORMA
-- ============================================================================

-- Planes de suscripción
CREATE TABLE planes (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(50) UNIQUE NOT NULL,
    slug VARCHAR(30) UNIQUE NOT NULL,
    descripcion TEXT,
    
    -- Límites
    max_fincas INTEGER,
    max_hectareas INTEGER,
    max_arboles INTEGER,
    max_usuarios INTEGER,
    
    -- Características
    modulos_incluidos VARCHAR(50)[], -- ['mapeo', 'cuadricula', 'aplicaciones', ...]
    soporte_nivel VARCHAR(30), -- 'email', 'email+chat', 'dedicado'
    almacenamiento_gb INTEGER,
    api_calls_mes INTEGER,
    
    -- Precios
    precio_mensual DECIMAL(10,2),
    precio_anual DECIMAL(10,2),
    costo_hectarea_adicional DECIMAL(8,2),
    costo_usuario_adicional DECIMAL(8,2),
    
    -- Estado
    visible_publico BOOLEAN DEFAULT true,
    permite_trial BOOLEAN DEFAULT true,
    dias_trial INTEGER DEFAULT 14,
    activo BOOLEAN DEFAULT true,
    
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Planes iniciales
INSERT INTO planes (nombre, slug, max_fincas, max_hectareas, max_arboles, max_usuarios, 
    modulos_incluidos, precio_mensual, precio_anual) VALUES
    ('Starter', 'starter', 1, 10, 1000, 3, 
        ARRAY['mapeo', 'cuadricula', 'inspecciones'], 29.00, 290.00),
    ('Professional', 'professional', 3, 50, 5000, 10,
        ARRAY['mapeo', 'cuadricula', 'inspecciones', 'aplicaciones', 'riego', 'inventario'], 
        99.00, 990.00),
    ('Enterprise', 'enterprise', 999, 999, 999999, 999,
        ARRAY['mapeo', 'cuadricula', 'inspecciones', 'aplicaciones', 'riego', 'inventario', 
              'costos', 'reportes_avanzados', 'api_acceso'], 
        299.00, 2990.00);

-- Suscripciones de tenants
CREATE TABLE tenant_suscripciones (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    plan_id INTEGER REFERENCES planes(id),
    
    -- Periodo
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE,
    fecha_renovacion DATE,
    periodo VARCHAR(20), -- 'mensual', 'anual'
    
    -- Estado
    estado VARCHAR(30) DEFAULT 'activa', -- 'trial', 'activa', 'vencida', 'cancelada', 'suspendida'
    auto_renovacion BOOLEAN DEFAULT true,
    
    -- Uso
    hectareas_usadas DECIMAL(10,2) DEFAULT 0,
    arboles_usados INTEGER DEFAULT 0,
    usuarios_usados INTEGER DEFAULT 0,
    
    -- Costos adicionales
    cargo_hectareas_extra DECIMAL(10,2) DEFAULT 0,
    cargo_usuarios_extra DECIMAL(10,2) DEFAULT 0,
    descuento_porcentaje DECIMAL(5,2) DEFAULT 0,
    
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_suscripciones_tenant ON tenant_suscripciones(tenant_id);
CREATE INDEX idx_suscripciones_estado ON tenant_suscripciones(estado);
CREATE INDEX idx_suscripciones_renovacion ON tenant_suscripciones(fecha_renovacion);

-- Facturas
CREATE TABLE facturas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    suscripcion_id INTEGER REFERENCES tenant_suscripciones(id),
    
    -- Factura
    numero_factura VARCHAR(50) UNIQUE NOT NULL,
    fecha_emision DATE NOT NULL,
    fecha_vencimiento DATE NOT NULL,
    
    -- Montos
    subtotal DECIMAL(12,2) NOT NULL,
    impuestos DECIMAL(12,2) DEFAULT 0,
    descuentos DECIMAL(12,2) DEFAULT 0,
    total DECIMAL(12,2) NOT NULL,
    
    -- Conceptos
    conceptos JSONB, -- desglose de cargos
    periodo_facturado VARCHAR(50),
    
    -- Estado
    estado VARCHAR(30) DEFAULT 'pendiente', -- 'pendiente', 'pagada', 'vencida', 'cancelada'
    fecha_pago TIMESTAMP,
    metodo_pago VARCHAR(50),
    referencia_pago VARCHAR(100),
    
    -- Documentos
    pdf_url VARCHAR(255),
    xml_url VARCHAR(255),
    
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_facturas_tenant ON facturas(tenant_id);
CREATE INDEX idx_facturas_numero ON facturas(numero_factura);
CREATE INDEX idx_facturas_estado ON facturas(estado);
CREATE INDEX idx_facturas_vencimiento ON facturas(fecha_vencimiento);

-- Tickets de soporte
CREATE TABLE tickets_soporte (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    usuario_id INTEGER REFERENCES usuarios(id),
    
    -- Ticket
    numero_ticket VARCHAR(30) UNIQUE NOT NULL,
    asunto VARCHAR(200) NOT NULL,
    descripcion TEXT,
    categoria VARCHAR(50), -- 'tecnico', 'facturacion', 'funcionalidad', 'consulta'
    prioridad VARCHAR(20) DEFAULT 'media', -- 'baja', 'media', 'alta', 'critica'
    
    -- Estado
    estado VARCHAR(30) DEFAULT 'abierto', -- 'abierto', 'en_progreso', 'esperando_cliente', 'resuelto', 'cerrado'
    asignado_a INTEGER REFERENCES usuarios(id),
    
    -- Tiempo
    fecha_creacion TIMESTAMP DEFAULT NOW(),
    fecha_primera_respuesta TIMESTAMP,
    fecha_resolucion TIMESTAMP,
    fecha_cierre TIMESTAMP,
    
    -- SLA
    sla_respuesta_horas INTEGER,
    sla_resolucion_horas INTEGER,
    sla_vencido BOOLEAN DEFAULT false,
    
    -- Metadata
    tags VARCHAR(50)[],
    adjuntos_urls TEXT[]
);

CREATE INDEX idx_tickets_tenant ON tickets_soporte(tenant_id);
CREATE INDEX idx_tickets_usuario ON tickets_soporte(usuario_id);
CREATE INDEX idx_tickets_estado ON tickets_soporte(estado);
CREATE INDEX idx_tickets_numero ON tickets_soporte(numero_ticket);

-- Mensajes de tickets
CREATE TABLE ticket_mensajes (
    id SERIAL PRIMARY KEY,
    ticket_id INTEGER REFERENCES tickets_soporte(id) ON DELETE CASCADE,
    usuario_id INTEGER REFERENCES usuarios(id),
    
    -- Mensaje
    mensaje TEXT NOT NULL,
    es_interno BOOLEAN DEFAULT false, -- visible solo para equipo de soporte
    adjuntos_urls TEXT[],
    
    -- Auditoría
    fecha TIMESTAMP DEFAULT NOW(),
    editado BOOLEAN DEFAULT false,
    fecha_edicion TIMESTAMP
);

CREATE INDEX idx_ticket_mensajes_ticket ON ticket_mensajes(ticket_id);
CREATE INDEX idx_ticket_mensajes_fecha ON ticket_mensajes(fecha);

-- Configuración de plataforma
CREATE TABLE configuracion_plataforma (
    clave VARCHAR(100) PRIMARY KEY,
    valor TEXT NOT NULL,
    tipo VARCHAR(30), -- 'string', 'number', 'boolean', 'json'
    categoria VARCHAR(50),
    descripcion TEXT,
    editable BOOLEAN DEFAULT true,
    updated_at TIMESTAMP DEFAULT NOW(),
    updated_by INTEGER REFERENCES usuarios(id)
);

-- Configuraciones iniciales
INSERT INTO configuracion_plataforma (clave, valor, tipo, categoria, descripcion) VALUES
    ('mantenimiento_modo', 'false', 'boolean', 'sistema', 'Modo mantenimiento activo'),
    ('registro_abierto', 'true', 'boolean', 'usuarios', 'Permitir auto-registro de nuevos tenants'),
    ('max_file_upload_mb', '50', 'number', 'sistema', 'Tamaño máximo de archivo en MB'),
    ('smtp_host', 'smtp.example.com', 'string', 'email', 'Servidor SMTP para envío de emails'),
    ('soporte_email', 'soporte@agrogrid.com', 'string', 'contacto', 'Email de soporte'),
    ('trial_dias_default', '14', 'number', 'suscripciones', 'Días de trial por defecto'),
    ('backup_retencion_dias', '30', 'number', 'sistema', 'Días de retención de backups');

-- Log de actividad de administradores
CREATE TABLE admin_activity_log (
    id BIGSERIAL PRIMARY KEY,
    admin_id INTEGER REFERENCES usuarios(id),
    
    -- Acción
    accion VARCHAR(100) NOT NULL, -- 'tenant_suspendido', 'plan_modificado', 'config_cambiada'
    modulo VARCHAR(50),
    descripcion TEXT,
    
    -- Contexto
    tenant_afectado_id UUID REFERENCES tenants(id),
    cambios JSONB, -- before/after de cambios
    
    -- Auditoría
    ip_address VARCHAR(50),
    fecha TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_admin_log_admin ON admin_activity_log(admin_id);
CREATE INDEX idx_admin_log_tenant ON admin_activity_log(tenant_afectado_id);
CREATE INDEX idx_admin_log_fecha ON admin_activity_log(fecha);
```

---

### 8.4 Diagrama de Relaciones

```
┌────────────────────────────────────────────────────────────────────────┐
│                        MODELO DE DATOS - RELACIONES                    │
└────────────────────────────────────────────────────────────────────────┘

                    ┌─────────────────────┐
                    │ ADMINISTRACIÓN      │
                    │  planes             │
                    │  tenant_suscripciones│
                    │  facturas           │
                    │  tickets_soporte    │
                    │  ticket_mensajes    │
                    │  configuracion_plat.│
                    │  admin_activity_log │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ MULTI-TENANT        │
          ┌─────────│  tenants            │─────────┐
          │         │  tenant_cultivos    │         │
          │         └─────────────────────┘         │
          │                                         │
          ▼                                         ▼
┌─────────────────────┐                  ┌─────────────────────┐
│ SEGURIDAD           │                  │ JERARQUÍA CAMPO     │
│  usuarios           │──────────────────│  fincas             │
│  roles              │                  │  sectores           │
│  permisos           │                  │  lotes              │
│  rol_permisos       │                  │  arboles            │
│  usuario_roles      │───┐              │  arboles_estado     │
│  sesiones           │   │              │  arboles_historial  │
│  audit_log_seg.     │   │              └──────────┬──────────┘
└─────────────────────┘   │                         │
                          │                         │
                          │                         ▼
                          │              ┌─────────────────────┐
                          │              │ OPERACIONES CAMPO   │
                          │              │  inspecciones       │
                          │              │  detecciones_plagas │
                          │              │  aplicaciones       │
                          └─────────────▶│  aplicacion_mezcla  │
                                         │  aplicacion_objetivos│
                                         │  registros_riego    │
                                         │  tareas             │
                                         └──────────┬──────────┘
                                                    │
                    ┌───────────────────────────────┼──────────────────┐
                    │                               │                  │
                    ▼                               ▼                  ▼
          ┌─────────────────────┐       ┌─────────────────────┐  ┌───────────────┐
          │ INVENTARIO          │       │ INFRAESTRUCTURA     │  │ PRODUCCIÓN    │
          │  categorias_insumos │       │  fuentes_agua       │  │  cosechas     │
          │  insumos            │       │  bombas             │  │  lotes_cosech │
          │  proveedores        │       │  almacenamiento     │  │  ventas       │
          │  proveedor_productos│       │  sectores_riego     │  │  costos_prod  │
          │  lotes_inventario   │       │  alertas_infraest.  │  └───────────────┘
          │  movimientos_inv.   │       └─────────────────────┘
          │  ordenes_compra     │
          │  orden_compra_det.  │
          │  alertas_inventario │
          └─────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│ CONVENCIONES:                                                          │
│  → : Relación directa (Foreign Key)                                    │
│  tenant_id presente en todas las tablas principales (RLS)              │
│  Índices espaciales en campos GEOMETRY (PostGIS)                       │
│  Particionamiento en tablas de historial y logs                        │
└────────────────────────────────────────────────────────────────────────┘
```

---

### 8.5 Consideraciones de Performance

#### 8.5.1 Índices Recomendados

```sql
-- Índices compuestos para queries frecuentes
CREATE INDEX idx_arboles_lote_salud ON arboles(lote_id, estado_salud);
CREATE INDEX idx_arboles_tenant_finca ON arboles(tenant_id, finca_id) WHERE activo = true;
CREATE INDEX idx_aplicaciones_fecha_estado ON aplicaciones(fecha_programada, estado);
CREATE INDEX idx_movimientos_inv_fecha_tipo ON movimientos_inventario(fecha, tipo);

-- Índices parciales para casos específicos
CREATE INDEX idx_arboles_atencion ON arboles(lote_id, prioridad_atencion) 
    WHERE requiere_atencion = true AND activo = true;

CREATE INDEX idx_facturas_pendientes ON facturas(tenant_id, fecha_vencimiento)
    WHERE estado = 'pendiente';

-- Índices para búsqueda de texto
CREATE INDEX idx_usuarios_nombre_trgm ON usuarios USING gin(nombre gin_trgm_ops);
CREATE INDEX idx_tickets_asunto_trgm ON tickets_soporte USING gin(asunto gin_trgm_ops);
```

#### 8.5.2 Particionamiento de Tablas Históricas

```sql
-- Particionar por rango de fechas (trimestral)
CREATE TABLE arboles_historial (
    id BIGSERIAL,
    arbol_id INTEGER,
    fecha_registro TIMESTAMP,
    ...
) PARTITION BY RANGE (fecha_registro);

-- Crear particiones por trimestre
CREATE TABLE arboles_historial_2025_q1 PARTITION OF arboles_historial
    FOR VALUES FROM ('2025-01-01') TO ('2025-04-01');

CREATE TABLE arboles_historial_2025_q2 PARTITION OF arboles_historial
    FOR VALUES FROM ('2025-04-01') TO ('2025-07-01');

-- Similar para audit_log_seguridad, movimientos_inventario
```

#### 8.5.3 Vistas Materializadas para Cuadrícula

```sql
-- Vista materializada para carga rápida de cuadrícula
CREATE MATERIALIZED VIEW mv_lote_cuadricula AS
SELECT 
    l.id AS lote_id,
    l.tenant_id,
    l.nombre AS lote_nombre,
    a.id AS arbol_id,
    a.codigo AS arbol_codigo,
    a.fila,
    a.columna,
    a.estado_salud,
    ae.color_indicador,
    ae.indice_salud,
    ae.tiene_plagas,
    ae.tiene_enfermedades,
    ae.proxima_inspeccion,
    a.latitud,
    a.longitud
FROM lotes l
JOIN arboles a ON l.id = a.lote_id
LEFT JOIN arboles_estado_actual ae ON a.id = ae.arbol_id
WHERE a.activo = true;

-- Índices en la vista materializada
CREATE INDEX idx_mv_cuadricula_lote ON mv_lote_cuadricula(lote_id);
CREATE INDEX idx_mv_cuadricula_tenant ON mv_lote_cuadricula(tenant_id);

-- Refrescar periódicamente (cada hora o al actualizar estado de árboles)
REFRESH MATERIALIZED VIEW CONCURRENTLY mv_lote_cuadricula;
```

#### 8.5.4 Estrategia de Caché con Redis

```yaml
cache_strategy:
  # Datos casi estáticos (TTL largo)
  catalogos:
    - key: "catalogo:cultivos"
      ttl: 86400  # 24 horas
    - key: "catalogo:plagas:{cultivo_id}"
      ttl: 43200  # 12 horas
  
  # Datos de sesión
  sesiones:
    - key: "session:user:{user_id}"
      ttl: 3600   # 1 hora
    - key: "session:tenant:{tenant_id}"
      ttl: 1800   # 30 minutos
  
  # Datos frecuentes pero mutables
  cuadricula:
    - key: "grid:lote:{lote_id}"
      ttl: 300    # 5 minutos
    - key: "grid:finca:{finca_id}"
      ttl: 600    # 10 minutos
  
  # Invalidación automática en escritura
  invalidate_on:
    - event: "arbol.estado.updated"
      keys: ["grid:lote:{lote_id}", "grid:finca:{finca_id}"]
    - event: "aplicacion.created"
      keys: ["dashboard:pendientes:{tenant_id}"]
```

#### 8.5.5 Connection Pooling con HikariCP

```yaml
# application.yml
spring:
  datasource:
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
      pool-name: AgroGridHikariPool
      
      # PostgreSQL optimizations
      data-source-properties:
        cachePrepStmts: true
        prepStmtCacheSize: 250
        prepStmtCacheSqlLimit: 2048
        useServerPrepStmts: true
        useLocalSessionState: true
        rewriteBatchedStatements: true
        cacheResultSetMetadata: true
        cacheServerConfiguration: true
        elideSetAutoCommits: true
        maintainTimeStats: false
```

---

### 8.6 Configuración Multi-Tenant

#### 8.6.1 Row Level Security (RLS) en PostgreSQL

```sql
-- Habilitar RLS en todas las tablas principales
ALTER TABLE fincas ENABLE ROW LEVEL SECURITY;
ALTER TABLE sectores ENABLE ROW LEVEL SECURITY;
ALTER TABLE lotes ENABLE ROW LEVEL SECURITY;
ALTER TABLE arboles ENABLE ROW LEVEL SECURITY;
ALTER TABLE aplicaciones ENABLE ROW LEVEL SECURITY;
ALTER TABLE insumos ENABLE ROW LEVEL SECURITY;

-- Política genérica de aislamiento
CREATE POLICY tenant_isolation_policy ON fincas
    USING (tenant_id = current_setting('app.current_tenant')::UUID);

-- Aplicar a todas las tablas con tenant_id
DO $$
DECLARE
    tbl RECORD;
BEGIN
    FOR tbl IN 
        SELECT tablename FROM pg_tables 
        WHERE schemaname = 'public' 
        AND tablename NOT IN ('roles', 'permisos', 'planes', 'configuracion_plataforma')
    LOOP
        EXECUTE format('ALTER TABLE %I ENABLE ROW LEVEL SECURITY', tbl.tablename);
        EXECUTE format('CREATE POLICY tenant_isolation_%I ON %I 
            USING (tenant_id = current_setting(''app.current_tenant'')::UUID)', 
            tbl.tablename, tbl.tablename);
    END LOOP;
END $$;
```

#### 8.6.2 TenantContext en Spring Boot

```java
@Component
public class TenantContext {
    private static final ThreadLocal<UUID> CURRENT_TENANT = new ThreadLocal<>();
    
    public static void setTenantId(UUID tenantId) {
        CURRENT_TENANT.set(tenantId);
    }
    
    public static UUID getTenantId() {
        return CURRENT_TENANT.get();
    }
    
    public static void clear() {
        CURRENT_TENANT.remove();
    }
}

@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class TenantFilter extends OncePerRequestFilter {
    
    @Autowired
    private DataSource dataSource;
    
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                   HttpServletResponse response,
                                   FilterChain filterChain) throws ServletException, IOException {
        try {
            // Extraer tenant_id del JWT
            UUID tenantId = extractTenantFromJWT(request);
            
            if (tenantId != null) {
                TenantContext.setTenantId(tenantId);
                
                // Establecer en PostgreSQL session
                try (Connection connection = dataSource.getConnection();
                     Statement stmt = connection.createStatement()) {
                    stmt.execute("SET app.current_tenant = '" + tenantId + "'");
                }
            }
            
            filterChain.doFilter(request, response);
        } finally {
            TenantContext.clear();
        }
    }
}
```

#### 8.6.3 Filtros Automáticos por Tenant

```java
@Entity
@Table(name = "fincas")
@FilterDef(name = "tenantFilter", parameters = @ParamDef(name = "tenantId", type = "uuid-binary"))
@Filter(name = "tenantFilter", condition = "tenant_id = :tenantId")
public class Finca {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    
    @Column(name = "tenant_id", nullable = false)
    private UUID tenantId;
    
    // ... otros campos
}

// Aplicar filtro automáticamente en repositorios
@Component
public class TenantFilterAspect {
    
    @Autowired
    private EntityManager entityManager;
    
    @Before("execution(* com.agrogrid.repository.*.*(..))")
    public void enableTenantFilter() {
        Session session = entityManager.unwrap(Session.class);
        Filter filter = session.enableFilter("tenantFilter");
        filter.setParameter("tenantId", TenantContext.getTenantId());
    }
}
```

#### 8.6.4 Aislamiento de Datos

```
┌─────────────────────────────────────────────────────────────────┐
│             CAPAS DE AISLAMIENTO MULTI-TENANT                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. APPLICATION LAYER (Spring Boot)                             │
│     ┌───────────────────────────────────────────────────┐      │
│     │ TenantFilter → Extrae tenant_id del JWT          │      │
│     │ TenantContext → ThreadLocal storage               │      │
│     │ @PreAuthorize → Validación de acceso             │      │
│     └───────────────────────────────────────────────────┘      │
│                           │                                     │
│                           ▼                                     │
│  2. ORM LAYER (Hibernate)                                       │
│     ┌───────────────────────────────────────────────────┐      │
│     │ @Filter(tenantFilter) → Filtro automático         │      │
│     │ WHERE tenant_id = :tenantId agregado a queries    │      │
│     └───────────────────────────────────────────────────┘      │
│                           │                                     │
│                           ▼                                     │
│  3. DATABASE LAYER (PostgreSQL RLS)                             │
│     ┌───────────────────────────────────────────────────┐      │
│     │ SET app.current_tenant = 'uuid'                   │      │
│     │ RLS Policy aplicada a nivel de fila              │      │
│     │ Previene acceso incluso con SQL directo          │      │
│     └───────────────────────────────────────────────────┘      │
│                                                                 │
│  RESULTADO: Triple capa de seguridad garantiza aislamiento     │
└─────────────────────────────────────────────────────────────────┘
```

---

## Resumen de Arquitectura

El sistema **AgroGrid** está construido sobre una arquitectura moderna y escalable que garantiza:

### ✅ Seguridad Multi-Nivel
- Autenticación JWT con refresh tokens
- Spring Security 6 con roles granulares
- Row Level Security en PostgreSQL
- Aislamiento total de datos por tenant

### ✅ Performance Optimizado
- Vistas materializadas para cuadrícula
- Caché estratégico con Redis
- Índices espaciales PostGIS
- Particionamiento de tablas históricas
- Connection pooling optimizado

### ✅ Escalabilidad
- Arquitectura multi-tenant
- Microservicio separado para ML/AI
- Containers y Kubernetes
- CDN para assets estáticos
- Cola de tareas con RabbitMQ

### ✅ Modelo de Datos Completo
- 40+ tablas con roles, permisos y auditoría
- Jerarquía completa: finca → sector → lote → árbol
- Administración de plataforma integrada
- Facturación y suscripciones
- Sistema de tickets de soporte

---

> Navegación: [← Anterior](modulos/07-10-reportes-integrados.md) | [📑 Índice](README.md) | [Siguiente →](09-plan-implementacion.md)
