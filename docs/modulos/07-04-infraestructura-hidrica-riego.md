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

## 📚 Documentos relacionados

- [Impacto del riego en la salud del árbol](07-03-salud-fenologia.md)
- [Programación de tareas de riego](07-06-planificacion-semanal.md)
- [Inventario de repuestos de riego](07-08-compras-inventario.md)

---

> Navegación: [← Anterior](07-03-salud-fenologia.md) | [📑 Índice](../README.md) | [Siguiente →](07-05-aplicaciones-calculo-dosis.md)
