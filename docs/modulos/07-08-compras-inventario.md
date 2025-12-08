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

## 📚 Documentos relacionados

- [Consumo de agroquímicos](07-05-aplicaciones-calculo-dosis.md)
- [Generación de lista de compras](07-06-planificacion-semanal.md)
- [Costos de insumos adquiridos](07-09-costos-ventas-rentabilidad.md)
- [Repuestos de infraestructura](07-04-infraestructura-hidrica-riego.md)

---

> Navegación: [← Anterior](07-07-app-movil-campo.md) | [📑 Índice](../README.md) | [Siguiente →](07-09-costos-ventas-rentabilidad.md)
