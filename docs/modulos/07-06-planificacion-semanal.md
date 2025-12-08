### 7.6 📋 Módulo de Planificación Semanal y Mantenimiento

> **Planificación inteligente de labores periódicas, generación automática de tareas y lista de compras**

#### Tipos de Labores de Mantenimiento

```yaml
labores_mantenimiento:
  infraestructura:
    - codigo: "MNT-CAM"
      nombre: "Mantenimiento de caminos"
      descripcion: "Reparación de baches, limpieza de cunetas"
      frecuencia_default: "mensual"
      duracion_estimada_hrs: 8
      requiere_insumos: true
      insumos_tipicos: ["Grava", "Material de relleno"]
    
    - codigo: "MNT-CER"
      nombre: "Revisión de cercas"
      descripcion: "Reparación de cercas perimetrales"
      frecuencia_default: "quincenal"
      duracion_estimada_hrs: 4
      requiere_insumos: true
      insumos_tipicos: ["Alambre", "Grapas", "Postes"]
    
    - codigo: "MNT-CAP"
      nombre: "Limpieza captación de agua"
      descripcion: "Limpieza de filtros y canales de captación"
      frecuencia_default: "semanal"
      duracion_estimada_hrs: 2
      requiere_insumos: false
    
    - codigo: "MNT-TAN"
      nombre: "Limpieza de tanques"
      descripcion: "Vaciado y limpieza de tanques de almacenamiento"
      frecuencia_default: "trimestral"
      duracion_estimada_hrs: 6
      requiere_insumos: true
      insumos_tipicos: ["Cloro", "Cepillos"]
  
  vegetacion:
    - codigo: "MNT-PAS"
      nombre: "Corte de pasto/maleza"
      descripcion: "Control de maleza entre hileras y orillas"
      frecuencia_default: "quincenal"
      duracion_estimada_hrs: 16
      requiere_insumos: true
      insumos_tipicos: ["Combustible desbrozadora", "Hilo nylon"]
    
    - codigo: "MNT-POD"
      nombre: "Poda de formación"
      descripcion: "Poda de árboles jóvenes"
      frecuencia_default: "anual"
      duracion_estimada_hrs: 40
      requiere_insumos: true
      insumos_tipicos: ["Pasta cicatrizante", "Herramientas"]
  
  equipo:
    - codigo: "MNT-BOM"
      nombre: "Mantenimiento bombas"
      descripcion: "Revisión y mantenimiento preventivo de bombas"
      frecuencia_default: "mensual"
      duracion_estimada_hrs: 3
      requiere_insumos: true
      insumos_tipicos: ["Aceite", "Filtros"]
    
    - codigo: "MNT-RIE"
      nombre: "Revisión sistema de riego"
      descripcion: "Limpieza de filtros, revisión de goteros"
      frecuencia_default: "semanal"
      duracion_estimada_hrs: 4
      requiere_insumos: true
      insumos_tipicos: ["Goteros repuesto", "Conectores"]
```

#### Programación de Labores Periódicas

```sql
-- Plantillas de labores periódicas
CREATE TABLE labores_periodicas (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    codigo_labor VARCHAR(20),
    nombre VARCHAR(100),
    descripcion TEXT,
    frecuencia VARCHAR(20), -- 'diaria', 'semanal', 'quincenal', 'mensual', 'trimestral', 'anual'
    dia_semana_preferido INTEGER, -- 0=domingo, 1=lunes, etc
    semana_mes_preferida INTEGER, -- 1,2,3,4 para frecuencias mensuales
    duracion_estimada_hrs DECIMAL(5,2),
    trabajadores_requeridos INTEGER DEFAULT 1,
    prioridad VARCHAR(20) DEFAULT 'normal', -- 'alta', 'normal', 'baja'
    activa BOOLEAN DEFAULT true,
    ultima_ejecucion DATE,
    proxima_ejecucion DATE
);

-- Insumos requeridos por labor
CREATE TABLE labor_insumos (
    id SERIAL PRIMARY KEY,
    labor_id INTEGER REFERENCES labores_periodicas(id),
    producto_id INTEGER REFERENCES productos_agricolas(id),
    cantidad_estimada DECIMAL(10,2),
    unidad VARCHAR(20),
    es_opcional BOOLEAN DEFAULT false
);

-- Tareas generadas
CREATE TABLE tareas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID,
    finca_id INTEGER,
    tipo VARCHAR(30), -- 'aplicacion', 'mantenimiento', 'inspeccion', 'cosecha', 'otro'
    origen VARCHAR(30), -- 'programada', 'manual', 'alerta', 'planificacion_semanal'
    labor_periodica_id INTEGER REFERENCES labores_periodicas(id),
    aplicacion_id INTEGER REFERENCES aplicaciones(id),
    titulo VARCHAR(200),
    descripcion TEXT,
    instructivo JSONB, -- instrucciones detalladas
    fecha_programada DATE,
    hora_inicio TIME,
    duracion_estimada_hrs DECIMAL(5,2),
    prioridad VARCHAR(20),
    estado VARCHAR(20), -- 'pendiente', 'asignada', 'en_progreso', 'completada', 'cancelada'
    asignado_a INTEGER REFERENCES usuarios(id),
    completada_por INTEGER REFERENCES usuarios(id),
    fecha_completada TIMESTAMP,
    notas_ejecucion TEXT,
    fotos_evidencia TEXT[] -- URLs de fotos
);
```

#### Pantalla de Planificación Semanal (Lunes)

```
┌─────────────────────────────────────────────────────────────────┐
│  📅 PLANIFICACIÓN SEMANAL - Semana del 9 al 15 de Diciembre     │
│  Finca: Los Alamos | Generado: Lunes 9 Dic 2025, 6:00 AM        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN DE LA SEMANA                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tareas programadas: 12                                       ││
│  │ Horas totales estimadas: 45 hrs                             ││
│  │ Trabajadores disponibles: 4                                  ││
│  │ Capacidad: 160 hrs (4 × 40hrs) → Utilización: 28%           ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔴 TAREAS URGENTES (vencidas o críticas)                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ⚠️ APL-0089 - Aplicación Trips zona crítica                 ││
│  │    Programada: Hoy | Asignado: Juan Pérez                   ││
│  │    [Ver detalles] [Reasignar]                               ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📋 LABORES PERIÓDICAS ESTA SEMANA                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ☑️ Lun - Limpieza captación de agua (2 hrs)      [Asignar]  ││
│  │ ☑️ Lun - Revisión sistema de riego (4 hrs)       [Asignar]  ││
│  │ ☐ Mar - Corte de pasto Sector A (8 hrs)          [Asignar]  ││
│  │ ☐ Mié - Corte de pasto Sector B (8 hrs)          [Asignar]  ││
│  │ ☐ Jue - Inspección general árboles (6 hrs)       [Asignar]  ││
│  │ ☐ Vie - Mantenimiento bomba #1 (3 hrs)           [Asignar]  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🌳 LABORES DE CULTIVO                                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ☐ Aplicación preventiva Lote B (programada)     [Ver]       ││
│  │ ☐ Fertilización Sector Norte (fertirriego)      [Ver]       ││
│  │ ☐ Re-inspección árboles tratados (seguimiento)  [Ver]       ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🛒 INSUMOS REQUERIDOS ESTA SEMANA                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Producto              │ Necesario │ Stock │ Comprar │ Costo ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ Combustible desbroz.  │ 10 L      │ 3 L   │ 7 L     │ $42   ││
│  │ Hilo nylon 3mm        │ 2 rollos  │ 0     │ 2       │ $24   ││
│  │ Success 480 SC        │ 100 ml    │ 250ml │ -       │ -     ││
│  │ Inex-A                │ 150 ml    │ 500ml │ -       │ -     ││
│  │ Goteros repuesto      │ 20 uds    │ 5     │ 15      │ $15   ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │                                   TOTAL COMPRAS: │ $81      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📋 Generar lista de compras] [📤 Enviar plan a trabajadores]  │
│  [✏️ Ajustar plan] [📊 Ver calendario]                          │
└─────────────────────────────────────────────────────────────────┘
```

#### Lista de Compras Generada

```
┌─────────────────────────────────────────────────────────────────┐
│  🛒 LISTA DE COMPRAS - Semana 9-15 Diciembre 2025               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Generada automáticamente basada en:                            │
│  • Labores programadas de la semana                             │
│  • Stock actual en bodega                                       │
│  • Stock mínimo configurado                                     │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ PARA COMPRAR URGENTE (stock bajo mínimo)                    ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ □ Hilo nylon 3mm × 2 rollos           $24.00                ││
│  │   → Stock: 0 | Mínimo: 2 | Proveedor: AgroInsumos           ││
│  │                                                             ││
│  │ □ Goteros repuesto × 15 uds           $15.00                ││
│  │   → Stock: 5 | Mínimo: 20 | Proveedor: RiegoTec             ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ PARA LABORES DE LA SEMANA                                   ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ □ Combustible desbrozadora × 7 L      $42.00                ││
│  │   → Para: Corte de pasto (Mar-Mié)                          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ RECOMENDADO (stock acercándose al mínimo)                   ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ □ Aceite bomba × 2 L                  $18.00                ││
│  │   → Stock: 3 L | Mínimo: 2 L | Próx. uso: 2 semanas        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  TOTAL URGENTE:        $39.00                                   │
│  TOTAL SEMANA:         $81.00                                   │
│  TOTAL + RECOMENDADO:  $99.00                                   │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  [📧 Enviar a proveedor] [📱 Compartir WhatsApp] [🖨️ Imprimir] │
│  [✏️ Editar lista] [✅ Marcar como comprado]                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📚 Documentos relacionados

- [Tareas de mantenimiento de riego](07-04-infraestructura-hidrica-riego.md)
- [Aplicaciones programadas](07-05-aplicaciones-calculo-dosis.md)
- [Lista de compras de insumos](07-08-compras-inventario.md)
- [Ejecución de tareas en campo](07-07-app-movil-campo.md)

---

> Navegación: [← Anterior](07-05-aplicaciones-calculo-dosis.md) | [📑 Índice](../README.md) | [Siguiente →](07-07-app-movil-campo.md)
