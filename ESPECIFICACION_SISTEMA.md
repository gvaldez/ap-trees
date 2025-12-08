### 7.4 💧 Módulo de Infraestructura Hídrica y Riego

> **Sistema completo para gestionar fuentes de agua, distribución y fertirriego**

Este módulo permite mapear y monitorear toda la infraestructura hídrica de la finca, ya que problemas en el sistema de riego impactan directamente la salud de los árboles.

#### Componentes del Sistema Hídrico

```
┌─────────────────────────────────────────────────────────────────┐
│              INFRAESTRUCTURA HÍDRICA DE LA FINCA                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐         │
│  │   FUENTES   │    │   ALMACEN   │    │   DISTRIB   │         │
│  │   DE AGUA   │───▶│   AMIENTO   │───▶│   UCIÓN     │         │
│  └─────────────┘    └─────────────┘    └─────────────┘         │
│        │                  │                  │                  │
│   ┌────┴────┐       ┌────┴────┐       ┌────┴────┐              │
│   │ Pozo    │       │ Tanque  │       │ Válvulas│              │
│   │ Captación│       │ Reserv. │       │ Tuberías│              │
│   │ Río/Canal│       │ Laguna  │       │ Goteros │              │
│   └─────────┘       └─────────┘       └─────────┘              │
│                           │                  │                  │
│                           ▼                  ▼                  │
│                    ┌─────────────┐    ┌─────────────┐          │
│                    │   FERTIRR   │    │   ÁRBOL     │          │
│                    │   IEGO      │───▶│   🌳        │          │
│                    └─────────────┘    └─────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

#### Registro de Activos Hídricos

| Tipo de Activo | Datos Registrados | Monitoreo |
|----------------|-------------------|-----------|
| **Pozo** | Profundidad, caudal, bomba, nivel freático | Nivel de agua, horas bomba |
| **Captación** | Tipo (lluvia/río), capacidad, filtros | Volumen captado |
| **Tanque/Reservorio** | Capacidad (m³), material, ubicación | Nivel actual, calidad |
| **Bomba** | Potencia, marca, fecha instalación | Estado, horas funcionamiento |
| **Línea principal** | Diámetro, material, longitud, presión | Fugas, presión |
| **Válvulas** | Tipo, ubicación, sectores que controla | Estado abierto/cerrado |
| **Goteros/Aspersores** | Tipo, caudal, cantidad por árbol | Obstrucciones |

#### Modelo de Datos - Infraestructura Hídrica

```sql
-- Fuentes de agua
CREATE TABLE fuentes_agua (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    tipo VARCHAR(30), -- 'pozo', 'captacion_lluvia', 'captacion_rio', 'red_municipal'
    nombre VARCHAR(100),
    ubicacion_lat DECIMAL(10,8),
    ubicacion_lon DECIMAL(11,8),
    capacidad_m3_hora DECIMAL(10,2),
    profundidad_m DECIMAL(6,2), -- para pozos
    estado VARCHAR(20), -- 'activo', 'mantenimiento', 'inactivo'
    fecha_instalacion DATE,
    notas TEXT
);

-- Equipos de bombeo
CREATE TABLE bombas (
    id SERIAL PRIMARY KEY,
    fuente_id INTEGER REFERENCES fuentes_agua(id),
    marca VARCHAR(50),
    modelo VARCHAR(50),
    potencia_hp DECIMAL(6,2),
    caudal_lph DECIMAL(10,2),
    presion_psi DECIMAL(6,2),
    horas_funcionamiento INTEGER DEFAULT 0,
    fecha_instalacion DATE,
    proximo_mantenimiento DATE,
    estado VARCHAR(20)
);

-- Almacenamiento
CREATE TABLE almacenamiento_agua (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    tipo VARCHAR(30), -- 'tanque_elevado', 'tanque_superficie', 'reservorio', 'laguna'
    nombre VARCHAR(100),
    capacidad_m3 DECIMAL(10,2),
    nivel_actual_porcentaje DECIMAL(5,2),
    material VARCHAR(50),
    ubicacion_lat DECIMAL(10,8),
    ubicacion_lon DECIMAL(11,8)
);

-- Sectores de riego
CREATE TABLE sectores_riego (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    nombre VARCHAR(100),
    lotes_ids INTEGER[], -- lotes que cubre
    valvula_principal_id INTEGER,
    tipo_riego VARCHAR(30), -- 'goteo', 'microaspersion', 'aspersion', 'gravedad'
    caudal_total_lph DECIMAL(10,2),
    tiempo_riego_min INTEGER,
    frecuencia_dias INTEGER
);

-- Registro de riegos
CREATE TABLE registros_riego (
    id SERIAL PRIMARY KEY,
    sector_id INTEGER REFERENCES sectores_riego(id),
    fecha_inicio TIMESTAMP,
    fecha_fin TIMESTAMP,
    volumen_m3 DECIMAL(10,2),
    tipo VARCHAR(20), -- 'programado', 'manual', 'emergencia'
    con_fertirriego BOOLEAN DEFAULT false,
    mezcla_aplicada_id INTEGER,
    operador_id INTEGER REFERENCES usuarios(id),
    observaciones TEXT
);

-- Alertas de infraestructura
CREATE TABLE alertas_infraestructura (
    id SERIAL PRIMARY KEY,
    activo_tipo VARCHAR(30), -- 'bomba', 'valvula', 'tuberia', 'tanque'
    activo_id INTEGER,
    tipo_alerta VARCHAR(50), -- 'fuga', 'baja_presion', 'bomba_fallo', 'nivel_bajo'
    severidad VARCHAR(20),
    fecha_deteccion TIMESTAMP,
    fecha_resolucion TIMESTAMP,
    descripcion TEXT,
    afecta_lotes INTEGER[]
);
```

#### Vista de Infraestructura en Mapa

```
┌──────���──────────────────────────────────────────────────────────┐
│  🗺️ MAPA DE INFRAESTRUCTURA - Finca Los Alamos                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│      ⛰️ Zona Alta                                                │
│           │                                                     │
│        [💧Captación]───────────┐                                │
│           │                    │                                │
│        [🏊Reservorio 500m³]────┤                                │
│           │ 87%                │                                │
│      [🔌Bomba 1]              [🔌Bomba 2]                       │
│           │ ✅                  │ ⚠️                            │
│           └────────┬───────────┘                                │
│                    │                                            │
│           [Línea Principal 4"]                                  │
│                    │                                            │
│    ┌───────────────┼───────────────┐                           │
│    │               │               │                            │
│  [🚿Sector A]   [🚿Sector B]   [🚿Sector C]                     │
│    Goteo         Goteo          Micro                          │
│    ✅ 45psi      ✅ 42psi       ⚠️ 35psi                        │
│    │               │               │                            │
│  ┌─┴─┐          ┌──┴──┐        ┌──┴──┐                         │
│  │🌳│          │🌳🌳│        │🌳🌳🌳│                          │
│  │🌳│          │🌳🌳│        │🌳🌳🌳│  ← Árboles                │
│  └───┘          └────┘        └─────┘                          │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│ 📊 Estado: Reservorio 87% | Bomba 1 ✅ | Bomba 2 ⚠️ Revisar     │
│ ⚠️ Alerta: Sector C baja presión - Posible fuga línea 3        │
└─────────────────────────────────────────────────────────────────┘
```

#### Correlación Riego ↔ Salud de Árboles

El sistema detecta automáticamente correlaciones:

```yaml
alerta_correlacion:
  tipo: "problema_riego_afecta_arboles"
  fecha: "2025-12-08"
  hallazgo: |
    Detectados 15 árboles con estrés hídrico (🟡🟠) en Lote B, Filas 5-8
    Coincide con:
    - Sector de riego B con presión 35 psi (normal: 45 psi)
    - Última alerta de fuga en línea secundaria hace 3 días
  arboles_afectados: [B-5-1, B-5-2, B-5-3, ... , B-8-10]
  recomendacion: |
    1. Revisar línea secundaria Sector B para detectar fuga
    2. Programar riego de emergencia manual
    3. Reinspeccionar árboles en 5 días
```

---

### 7.5 🧪 Módulo de Aplicaciones con Cálculo de Dosis

> **Gestión completa de productos, cálculo automático de dosis y formulación de mezclas**

#### Catálogo de Productos

```sql
CREATE TABLE productos_agricolas (
    id SERIAL PRIMARY KEY,
    tipo VARCHAR(30), -- 'insecticida', 'fungicida', 'herbicida', 'fertilizante', 'bioestimulante', 'coadyuvante'
    nombre_comercial VARCHAR(100),
    ingrediente_activo VARCHAR(150),
    concentracion VARCHAR(50), -- ej: "480 g/L", "70% WP"
    presentacion VARCHAR(50), -- 'liquido', 'polvo', 'granulado'
    unidad_medida VARCHAR(20), -- 'ml', 'g', 'kg', 'L'
    fabricante VARCHAR(100),
    registro_sanitario VARCHAR(50),
    categoria_toxicologica VARCHAR(20), -- 'I', 'II', 'III', 'IV'
    periodo_carencia_dias INTEGER,
    periodo_reingreso_horas INTEGER,
    precio_unitario DECIMAL(10,2),
    stock_actual DECIMAL(10,2),
    stock_minimo DECIMAL(10,2),
    activo BOOLEAN DEFAULT true
);

-- Dosis recomendadas por cultivo y problema
CREATE TABLE dosis_recomendadas (
    id SERIAL PRIMARY KEY,
    producto_id INTEGER REFERENCES productos_agricolas(id),
    cultivo_id VARCHAR(50),
    plaga_enfermedad_id INTEGER REFERENCES catalogo_plagas(id),
    aplicacion_tipo VARCHAR(30), -- 'foliar', 'suelo', 'fertirriego', 'drench'
    dosis_minima DECIMAL(10,4),
    dosis_maxima DECIMAL(10,4),
    dosis_recomendada DECIMAL(10,4),
    unidad_dosis VARCHAR(30), -- 'ml/L', 'g/L', 'kg/ha', 'ml/árbol'
    frecuencia_aplicacion VARCHAR(50), -- 'cada 7 días', 'cada 14 días'
    max_aplicaciones_ciclo INTEGER,
    notas TEXT
);

-- Registro de aplicaciones
CREATE TABLE aplicaciones (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    fecha_programada DATE,
    fecha_ejecutada TIMESTAMP,
    tipo VARCHAR(30), -- 'fitosanitaria', 'fertilizacion', 'bioestimulante'
    metodo VARCHAR(30), -- 'foliar', 'fertirriego', 'drench', 'aplicacion_directa'
    estado VARCHAR(20), -- 'programada', 'en_progreso', 'completada', 'cancelada'
    problema_objetivo_id INTEGER, -- plaga/enfermedad que se trata
    creado_por INTEGER REFERENCES usuarios(id),
    aprobado_por INTEGER REFERENCES usuarios(id)
);

-- Mezcla/receta de la aplicación
CREATE TABLE aplicacion_mezcla (
    id SERIAL PRIMARY KEY,
    aplicacion_id INTEGER REFERENCES aplicaciones(id),
    producto_id INTEGER REFERENCES productos_agricolas(id),
    cantidad_producto DECIMAL(10,3),
    unidad VARCHAR(20),
    orden_mezcla INTEGER, -- orden en que se agrega a la mezcla
    notas TEXT
);

-- Árboles/lotes objetivo
CREATE TABLE aplicacion_objetivos (
    id SERIAL PRIMARY KEY,
    aplicacion_id INTEGER REFERENCES aplicaciones(id),
    tipo_objetivo VARCHAR(20), -- 'arbol', 'lote', 'sector'
    objetivo_id INTEGER,
    cantidad_aplicada DECIMAL(10,3),
    completado BOOLEAN DEFAULT false,
    ejecutado_por INTEGER REFERENCES usuarios(id),
    fecha_ejecucion TIMESTAMP
);
```

#### Calculadora de Dosis

```
┌─────────────────────────────────────────────────────────────────┐
│  🧮 CALCULADORA DE DOSIS - Nueva Aplicación                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📋 DATOS DE LA APLICACIÓN                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tipo: [Fitosanitaria ▼]  Método: [Foliar ▼]                ││
│  │ Problema: [Trips - AGU-PL-001 ▼]                            ││
│  │                                                             ││
│  │ Área de aplicación:                                         ││
│  │ ○ Lote completo: [Lote A ▼] → 500 árboles                  ││
│  │ ● Selección: 45 árboles seleccionados en cuadrícula        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 CÁLCULO DE VOLUMEN                                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Árboles a tratar: 45                                        ││
│  │ Volumen por árbol: [3] L (según edad/tamaño)               ││
│  │ ─────────────────────────────────────                       ││
│  │ Volumen total de mezcla: 135 L                              ││
│  │ Tanques de 200L necesarios: 1 (usar 135L)                  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🧪 PRODUCTOS Y DOSIS                                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ [+ Agregar producto]                                        ││
│  │                                                             ││
│  │ 1. Spinosad (Success 480 SC)                                ││
│  │    Dosis recomendada: 0.3 ml/L (rango: 0.2-0.4 ml/L)       ││
│  │    Para 135L → 40.5 ml                            [Calcular]││
│  │                                                             ││
│  │ 2. Coadyuvante (Inex-A)                                     ││
│  │    Dosis: 0.5 ml/L                                          ││
│  │    Para 135L → 67.5 ml                            [Calcular]││
│  │                                                             ││
│  │ 3. [+ Agregar otro producto]                                ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📝 RESUMEN DE MEZCLA                                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Para preparar 135 L de mezcla:                              ││
│  │                                                             ││
│  │ 1. Llenar tanque con 100 L de agua                          ││
│  │ 2. Agregar 40.5 ml de Success 480 SC, agitar               ││
│  │ 3. Agregar 67.5 ml de Inex-A, agitar                        ││
│  │ 4. Completar a 135 L con agua                               ││
│  │                                                             ││
│  │ ⚠️ EPP requerido: Guantes, máscara, gafas                   ││
│  │ ⏱️ Periodo de reingreso: 4 horas                            ││
│  │ 📅 Periodo de carencia: 7 días                              ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💰 COSTO ESTIMADO                                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Success 480 SC: 40.5 ml × $0.85/ml = $34.43                ││
│  │ Inex-A: 67.5 ml × $0.05/ml = $3.38                         ││
│  │ ─────────────────────────────────────                       ││
│  │ Total productos: $37.81                                     ││
│  │ Costo por árbol: $0.84                                      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [Guardar borrador] [Crear y asignar tareas] [Programar]       │
└─────────────────────────────────────────────────────────────────┘
```

#### Generación de Instrucciones para Trabajadores

Al crear una aplicación, el sistema genera instrucciones detalladas:

```yaml
tarea_aplicacion:
  id: "APL-2025-0089"
  titulo: "Aplicación foliar contra Trips - Lote A zona crítica"
  fecha_programada: "2025-12-09"
  asignado_a: "Juan Pérez"
  
  instructivo:
    preparacion:
      - "Verificar EPP completo: guantes nitrilo, máscara con filtro, gafas, overol"
      - "Llevar bomba de espalda 20L calibrada"
      - "Recoger productos en bodega (ya medidos en envases)"
    
    mezcla:
      volumen_total: "20 L por carga (7 cargas total)"
      pasos:
        - orden: 1
          accion: "Llenar bomba con 15 L de agua limpia"
        - orden: 2
          accion: "Agregar 6 ml de Success 480 SC (medir con jeringa)"
          producto: "Success 480 SC"
          cantidad: "6 ml"
        - orden: 3
          accion: "Agitar por 30 segundos"
        - orden: 4
          accion: "Agregar 10 ml de Inex-A"
          producto: "Inex-A"
          cantidad: "10 ml"
        - orden: 5
          accion: "Completar a 20 L y agitar"
    
    aplicacion:
      arboles_objetivo: "45 árboles marcados en cuadrícula (zona roja)"
      ubicacion: "Lote A, Filas 1-3, Columnas 5-8"
      tecnica: "Aplicar cubriendo follaje completo, énfasis en envés de hojas"
      volumen_por_arbol: "3 L aproximadamente"
      hora_recomendada: "6:00-9:00 AM o 5:00-7:00 PM (evitar sol directo)"
    
    seguridad:
      epp_obligatorio: ["Guantes nitrilo", "Máscara con filtro", "Gafas", "Overol manga larga"]
      periodo_reingreso: "4 horas - señalizar área"
      primeros_auxilios: "En caso de contacto con piel, lavar con agua abundante"
      telefono_emergencia: "+52 123 456 7890"
    
    registro:
      - "Marcar en app cada árbol aplicado"
      - "Tomar foto de mezcla preparada"
      - "Reportar cualquier anomalía"
```

---

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

### 7.7 📱 Aplicación Móvil de Campo (Ligera)

> **App minimalista enfocada en captura rápida de datos y consulta de instrucciones**

#### Principios de Diseño

| Principio | Implementación |
|-----------|----------------|
| **Ultra ligera** | APK < 15 MB, funciona en 3G |
| **Offline first** | Sincroniza cuando hay conexión |
| **Mínimos clics** | Máximo 3 taps para cualquier acción |
| **Batería** | Optimizada para uso prolongado en campo |
| **Legible** | Texto grande, iconos claros, contraste alto |

#### Funcionalidades CORE (únicas)

```
┌─────────────────────────────────────────┐
│  📱 AGROGRID CAMPO v1.0                 │
│  👤 Juan Pérez | Finca Los Alamos       │
├─────────────────────────────────────────┤
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  📋 MIS TAREAS HOY                  ││
│  │                                     ││
│  │  🔴 Aplicación Trips (urgente)      ││
│  │     Lote A, 45 árboles              ││
│  │     [Ver instrucciones] [Iniciar]   ││
│  │                                     ││
│  │  🟡 Revisión sistema riego          ││
│  │     Sector B                        ││
│  │     [Ver instrucciones] [Iniciar]   ││
│  │                                     ││
│  │  🟢 Corte de pasto                  ││
│  │     Sector A                        ││
│  │     [Ver instrucciones] [Iniciar]   ││
│  └─────────────────────────────────────┘│
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  🌳 REGISTRAR ÁRBOL                 ││
│  │  [Escanear QR] [Buscar por fila]    ││
│  └─────────────────────────────────────┘│
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  📊 CUADRÍCULA RÁPIDA               ││
│  │  [Ver Lote A] [Ver Lote B]          ││
│  └─────────────────────────────────────┘│
│                                         │
│  ─────────────────────────────────────  │
│  🔄 Última sync: Hace 5 min | ☁️ Online │
└─────────────────────────────────────────┘
```

#### Vista de Tarea con Instructivo

```
┌─────────────────────────────────────────┐
│  ← TAREA: Aplicación Trips              │
├─────────────────────────────────────────┤
│                                         │
│  📍 Ubicación: Lote A, F1-3, C5-8       │
│  🌳 Árboles: 45                         │
│  ⏱️ Tiempo est: 3 horas                 │
│                                         │
│  ═══════════════════════════════════════│
│  📋 INSTRUCCIONES                       │
│  ═══════════════════════════════════════│
│                                         │
│  1️⃣ PREPARACIÓN                         │
│  ─────────────────────────────────────  │
│  ☐ Verificar EPP completo               │
│  ☐ Recoger productos en bodega          │
│  ☐ Llevar bomba 20L calibrada           │
│                                         │
│  2️⃣ MEZCLA (por cada 20L)               │
│  ─────────────────────────────────────  │
│  • Agua: 15 L primero                   │
│  • Success 480 SC: 6 ml ← [🖼️ Ver foto] │
│  • Agitar 30 seg                        │
│  • Inex-A: 10 ml                        │
│  • Completar a 20 L                     │
│                                         │
│  3️⃣ APLICACIÓN                          │
│  ─────────────────────────────────────  │
│  • 3 L por árbol aprox                  │
│  • Cubrir follaje completo              │
│  • Énfasis envés de hojas               │
│                                         │
│  ⚠️ SEGURIDAD                           │
│  ─────────────────────────────────────  │
│  🧤 Guantes  👓 Gafas  😷 Máscara       │
│  Reingreso: 4 horas                     │
│                                         │
│  [📷 Foto mezcla] [▶️ INICIAR TAREA]    │
└─────────────────────────────────────────┘
```

#### Registro Rápido de Árbol

```
┌─────────────────────────────────────────┐
│  🌳 ÁRBOL A-3-7                         │
│  Lote A | Fila 3 | Col 7                │
├─────────────────────────────────────────┤
│                                         │
│  Estado actual: 🟢 Saludable            │
│  Última inspección: Hace 5 días         │
│                                         │
│  ═══════════════════════════════════════│
│  CAMBIAR ESTADO:                        │
│  ═══════════════════════════════════════│
│                                         │
│  [🟢]  [🟡]  [🟠]  [🔴]  [🔵]           │
│  Sano  Atenc Adver Crít  Trat           │
│        ↑                                │
│  Seleccionado: 🟡 Atención              │
│                                         │
│  ═══════════════════════════════════════│
│  ¿QUÉ PROBLEMA? (opcional)              │
│  ═══════════════════════════════════════│
│                                         │
│  [🐛 Plaga]  [🦠 Enfermedad]            │
│  [💧 Riego] [❓ Otro]                   │
│                                         │
│  📝 Nota rápida:                        │
│  ┌─────────────────────────────────────┐│
│  │ Hojas con manchas amarillas...      ││
│  └─────────────────────────────────────┘│
│                                         │
│  [📷 Tomar foto]                        │
│                                         │
│  [← Anterior] [💾 GUARDAR] [Siguiente →]│
│                                         │
│  Progreso: Árbol 12 de 45               │
│  ████████░░░░░░░░ 27%                   │
└─────────────────────────────────────────┘
```

#### Modo Inspección Rápida (Fila por Fila)

```
┌─────────────────────────────────────────┐
│  🚶 INSPECCIÓN RÁPIDA - Lote A          │
├─────────────────────────────────────────┤
│                                         │
│  Fila actual: 3 de 8                    │
│                                         │
│  Columna:  1   2   3   4   5   6   7    │
│           [🟢][🟢][🟢][🟡][🟡][ ▶️][ ]   │
│                              ↑          │
│                          Actual: 6      │
│                                         │
│  Árbol F3-C6:                           │
│  ┌─────────────────────────────────────┐│
│  │ Toca para cambiar estado:           ││
│  │                                     ││
│  │ [🟢] [🟡] [🟠] [🔴] [🔵]            ││
│  │                                     ││
│  │ [📷] [📝 Nota] [⏭️ Saltar]          ││
│  └─────────────────────────────────────┘│
│                                         │
│  ─────────────────────────────────────  │
│  Total inspeccionados: 41/80            │
│  ████████████████░░░░ 51%               │
│                                         │
│  [⏸️ Pausar] [📊 Resumen] [✅ Finalizar]│
└─────────────────────────────────────────┘
```

#### Sincronización y Modo Offline

```yaml
sincronizacion:
  datos_offline_disponibles:
    - cuadricula_estado_actual
    - tareas_asignadas_con_instructivos
    - catalogo_plagas_basico
    - lista_productos_frecuentes
  
  datos_que_se_sincronizan:
    - cambios_estado_arboles
    - fotos_capturadas
    - tareas_completadas
    - notas_agregadas
  
  estrategia_sync:
    - "Sync automático cada 15 min si hay WiFi"
    - "Sync manual con botón si hay 3G/4G"
    - "Cola de cambios pendientes visible"
    - "Conflictos: último cambio gana con log"
```
