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

## 📚 Documentos relacionados

- [Productos recomendados por plaga](../04-catalogo-plagas-enfermedades.md)
- [Diagnóstico de plagas y enfermedades](07-03-salud-fenologia.md)
- [Inventario de agroquímicos](07-08-compras-inventario.md)
- [Programación de aplicaciones](07-06-planificacion-semanal.md)

---

> Navegación: [← Anterior](07-04-infraestructura-hidrica-riego.md) | [📑 Índice](../README.md) | [Siguiente →](07-06-planificacion-semanal.md)
