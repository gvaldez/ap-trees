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