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

> Navegación: [← Anterior](modulos/07-10-reportes-integrados.md)[📑 Índice](README.md) | [Siguiente →](09-plan-implementacion.md)
