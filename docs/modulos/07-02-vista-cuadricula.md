### 7.2 🔲 Módulo de Vista de Cuadrícula (CORE)

> **💡 Funcionalidad inspirada en el método tradicional de hoja cuadriculada**, digitalizada para visualización rápida del estado de toda la finca.

Este módulo es el **corazón visual del sistema**, permitiendo ver el estado de cada árbol en una cuadrícula interactiva de filas y columnas. Es la herramienta más usada por los ingenieros agrónomos para identificar rápidamente problemas y tomar decisiones.

#### Concepto Visual Mejorado

```
┌─────────────────────────────────────────────────────────────────┐
│  🔲 VISTA DE CUADRÍCULA - Lote A                                │
│  Sector Norte - Aguacate Hass - Plantación 2019                 │
│  📅 Fecha: 2025-12-08 14:35 | 🛰️ Última actualización: 2025-12-07│
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Modo: [Estado de Salud ▼]  Fecha: [2025-12-08 ▼]  [Filtros⚙️] │
│                                                                 │
│        Col→  1    2    3    4    5    6    7    8    9   10    │
│      ┌────────────────────────────────────────────────────────┐ │
│ Fil 1│  🟢   🟢   🟢   🟢   🟡   🟡   🔴   🔴   🟢   🟢        │ │
│ Fil 2│  🟢   🟢   🟢   🟡   🟡   🔴   🔴   🟠   🟢   🟢        │ │
│ Fil 3│  🟢   🟢   🟡   🟡   🔴   🔴   🟠   🟠   🟢   🟢        │ │
│ Fil 4│  🟢   🟢   🟢   🟡   🟠   🟠   🟢   🟢   🟢   🟢        │ │
│ Fil 5│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢        │ │
│ Fil 6│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢        │ │
│ Fil 7│  🟢   🟢   🔵   🔵   🟢   🟢   🟢   🟢   🟣   🟣        │ │
│ Fil 8│  🟢   🟢   🔵   🔵   🟢   🟢   🟢   🟢   🟣   🟣        │ │
│ Fil 9│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢        │ │
│Fil 10│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢        │ │
│      └────────────────────────────────────────────────────────┘ │
│                                                                 │
│  📊 RESUMEN                                                     │
│  Total: 100 árboles                                             │
│  🟢 Saludables (SAL): 74 (74%)                                  │
│  🟡 Observación (OBS): 6 (6%)                                   │
│  🟠 Riesgo (RIE): 5 (5%)                                        │
│  🔴 Crítico (CRI): 5 (5%)                                       │
│  🔵 En tratamiento (TRA): 4 (4%)                                │
│  🟣 En recuperación (REC): 4 (4%)                               │
│  ⚫ Muerto (MUE): 2 (2%)                                        │
│                                                                 │
│  ⚠️ ALERTAS AUTOMÁTICAS                                         │
│  🔍 Posible foco de Phytophthora en zona [F1-3, C5-8]          │
│     → 5 árboles críticos agrupados                              │
│     → Recomendación: Aplicación urgente de fungicida           │
│  🔍 Patrón de estrés hídrico en Fila 7                         │
│     → Verificar sistema de riego                                │
│                                                                 │
│  [Ver Mapa 🗺️] [Comparar Fechas 📊] [Exportar 📥] [Imprimir 🖨️]│
└─────────────────────────────────────────────────────────────────┘
```

#### Modos de Visualización

| Modo | Descripción | Esquema de Color | Actualización |
|------|-------------|------------------|---------------|
| **Estado de Salud** | Estado general del árbol | 9 estados (SAL, OBS, RIE, CRI, TRA, REC, MUE, REM, NUE) | Manual + IA |
| **Etapa Fenológica** | Fase de desarrollo actual | Por cultivo (ej: REP, BRO, FLO, CUA, MAD, COS) | Manual |
| **NDVI** | Vigor vegetativo | Gradiente continuo (0.0-1.0) | Drone/Satélite |
| **NDRE** | Índice red-edge | Gradiente continuo | Drone multiespectral |
| **Producción** | Rendimiento por árbol | kg: bajo (<50) amarillo, medio (50-100) verde, alto (>100) azul | Manual |
| **Edad del Árbol** | Años desde siembra | Gradiente temporal | Automático |
| **Última Inspección** | Días desde última revisión | Gradiente temporal (verde <7d, amarillo <14d, rojo >14d) | Automático |
| **Riesgo de Plagas** | Probabilidad de infección | Calculado por IA | Automático diario |
| **Estrés Hídrico (CWSI)** | Nivel de estrés por agua | Gradiente: azul (sin estrés) a rojo (estrés alto) | Cámara térmica |
| **Programación Cosecha** | Árboles listos para cosechar | Verde (listo), amarillo (próximo), gris (no listo) | Fenología + días |

#### Esquemas de Color Configurables (TypeScript)

```typescript
// Modo: Estado de Salud
type EstadoSalud = 'SAL' | 'OBS' | 'RIE' | 'CRI' | 'TRA' | 'REC' | 'MUE' | 'REM' | 'NUE';

interface EsquemaColorSalud {
  [key: string]: {
    color: string;
    emoji: string;
    nombre: string;
    descripcion: string;
  };
}

const COLORES_SALUD: EsquemaColorSalud = {
  'SAL': { 
    color: '#22c55e', 
    emoji: '🟢', 
    nombre: 'Saludable',
    descripcion: 'Árbol sin problemas detectados'
  },
  'OBS': { 
    color: '#eab308', 
    emoji: '🟡', 
    nombre: 'Observación',
    descripcion: 'Requiere monitoreo cercano'
  },
  'RIE': { 
    color: '#f97316', 
    emoji: '🟠', 
    nombre: 'Riesgo',
    descripcion: 'Problema detectado, requiere acción pronto'
  },
  'CRI': { 
    color: '#ef4444', 
    emoji: '🔴', 
    nombre: 'Crítico',
    descripcion: 'Requiere atención inmediata'
  },
  'TRA': { 
    color: '#3b82f6', 
    emoji: '🔵', 
    nombre: 'En Tratamiento',
    descripcion: 'Aplicación o intervención en curso'
  },
  'REC': { 
    color: '#a855f7', 
    emoji: '🟣', 
    nombre: 'Recuperación',
    descripcion: 'Mejorando tras tratamiento'
  },
  'MUE': { 
    color: '#1f2937', 
    emoji: '⚫', 
    nombre: 'Muerto',
    descripcion: 'Árbol no recuperable'
  },
  'REM': { 
    color: '#9ca3af', 
    emoji: '⚪', 
    nombre: 'Removido',
    descripcion: 'Árbol eliminado físicamente'
  },
  'NUE': { 
    color: '#84cc16', 
    emoji: '🟢', 
    nombre: 'Nuevo',
    descripcion: 'Plantado recientemente'
  }
};

// Modo: Etapa Fenológica (ejemplo Aguacate)
type EtapaFenologicaAguacate = 'REP' | 'BRO' | 'VEG' | 'IFL' | 'FLO' | 'CUA' | 'DES' | 'MAD' | 'COS';

const COLORES_FENOLOGIA_AGUACATE = {
  'REP': { color: '#8b4513', emoji: '🟤', nombre: 'Reposo' },
  'BRO': { color: '#90ee90', emoji: '🟢', nombre: 'Brotación' },
  'VEG': { color: '#228b22', emoji: '💚', nombre: 'Crecimiento Vegetativo' },
  'IFL': { color: '#ffb6c1', emoji: '🌸', nombre: 'Inducción Floral' },
  'FLO': { color: '#ff69b4', emoji: '🌺', nombre: 'Floración' },
  'CUA': { color: '#98fb98', emoji: '🟢', nombre: 'Cuajado' },
  'DES': { color: '#7cfc00', emoji: '🍏', nombre: 'Desarrollo Fruto' },
  'MAD': { color: '#32cd32', emoji: '🥑', nombre: 'Maduración' },
  'COS': { color: '#006400', emoji: '🎯', nombre: 'Listo Cosecha' }
};

// Modo: NDVI (gradiente continuo)
function obtenerColorNDVI(ndvi: number): string {
  // NDVI varía de -1 a 1, pero para vegetación: 0.2 a 0.9
  const normalizado = Math.max(0, Math.min(1, (ndvi - 0.2) / 0.7));
  
  if (normalizado < 0.33) {
    // Rojo a Amarillo (bajo vigor)
    const r = 255;
    const g = Math.floor(255 * (normalizado / 0.33));
    return `rgb(${r}, ${g}, 0)`;
  } else if (normalizado < 0.67) {
    // Amarillo a Verde claro
    const r = Math.floor(255 * (1 - (normalizado - 0.33) / 0.34));
    const g = 255;
    return `rgb(${r}, ${g}, 0)`;
  } else {
    // Verde claro a Verde oscuro (alto vigor)
    const g = 255;
    const b = Math.floor(128 * ((normalizado - 0.67) / 0.33));
    return `rgb(0, ${g}, ${b})`;
  }
}

// Función para obtener emoji NDVI
function obtenerEmojiNDVI(ndvi: number): string {
  if (ndvi < 0.3) return '🔴';
  if (ndvi < 0.5) return '🟠';
  if (ndvi < 0.65) return '🟡';
  if (ndvi < 0.75) return '🟢';
  return '🟢';
}
```

#### Modelo de Datos SQL

```sql
-- Estado actual de cada árbol (para cuadrícula)
CREATE TABLE arboles_estado (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id) UNIQUE,
    
    -- Estado de salud
    estado_salud VARCHAR(10) DEFAULT 'SAL',
    fecha_cambio_salud TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    salud_observaciones TEXT,
    
    -- Etapa fenológica
    etapa_fenologica VARCHAR(10),
    fecha_cambio_fenologia TIMESTAMP,
    
    -- Índices de vegetación (último valor)
    ndvi DECIMAL(5,4),
    fecha_ndvi DATE,
    ndre DECIMAL(5,4),
    gndvi DECIMAL(5,4),
    
    -- Producción
    produccion_kg_actual DECIMAL(8,2),
    fecha_ultima_cosecha DATE,
    
    -- Alertas activas
    tiene_alertas BOOLEAN DEFAULT false,
    alertas_activas INTEGER[] DEFAULT '{}',
    
    -- Metadata
    ultima_inspeccion TIMESTAMP,
    inspeccionado_por INTEGER REFERENCES usuarios(id),
    
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Índices
CREATE INDEX idx_arboles_estado_salud ON arboles_estado(estado_salud);
CREATE INDEX idx_arboles_estado_arbol ON arboles_estado(arbol_id);
CREATE INDEX idx_arboles_estado_alertas ON arboles_estado(tiene_alertas);

-- Vista materializada para renderizado rápido de cuadrícula
CREATE MATERIALIZED VIEW vista_cuadricula AS
SELECT 
    l.id AS lote_id,
    l.nombre AS lote_nombre,
    a.id AS arbol_id,
    a.codigo AS arbol_codigo,
    a.fila,
    a.columna,
    ae.estado_salud,
    ae.etapa_fenologica,
    ae.ndvi,
    ae.ndre,
    ae.produccion_kg_actual,
    ae.tiene_alertas,
    ae.ultima_inspeccion,
    EXTRACT(EPOCH FROM (CURRENT_TIMESTAMP - ae.ultima_inspeccion))/86400 AS dias_sin_inspeccion,
    a.fecha_siembra,
    EXTRACT(YEAR FROM AGE(CURRENT_DATE, a.fecha_siembra)) AS edad_anos
FROM arboles a
JOIN lotes l ON a.lote_id = l.id
LEFT JOIN arboles_estado ae ON a.id = ae.arbol_id
WHERE a.estado_vital = 'vivo'
ORDER BY l.id, a.fila, a.columna;

-- Índice para búsquedas rápidas
CREATE INDEX idx_vista_cuadricula_lote ON vista_cuadricula(lote_id, fila, columna);

-- Refrescar vista materializada (ejecutar periódicamente)
REFRESH MATERIALIZED VIEW vista_cuadricula;
```

#### Interacciones de Usuario

```yaml
# Click simple en árbol
click:
  accion: "Mostrar panel de detalle"
  contenido:
    - Código del árbol
    - Estado de salud
    - Etapa fenológica
    - NDVI actual
    - Última inspección
    - Alertas activas
    - Historial resumido
    - Botones: [Ver Completo] [Editar] [Registrar Inspección]

# Hover sobre árbol
hover:
  accion: "Mostrar tooltip"
  contenido: "AGC-001-A-0234 | F2-C34 | SAL | NDVI: 0.78 | Última inspección: hace 3 días"
  delay_ms: 300

# Click + Drag (selección múltiple)
drag:
  accion: "Seleccionar rectángulo de árboles"
  opciones:
    - Aplicar operación masiva
    - Exportar selección
    - Crear grupo temporal
    - Programar aplicación

# Click derecho (menú contextual)
right_click:
  opciones:
    - "Ver en mapa"
    - "Ver historial completo"
    - "Registrar inspección"
    - "Marcar para tratamiento"
    - "Agregar nota"
    - "Ver fotos"
    - "Generar QR"

# Atajos de teclado
keyboard_shortcuts:
  "Ctrl + F": "Buscar árbol por código"
  "Ctrl + Click": "Selección múltiple no consecutiva"
  "Shift + Click": "Selección de rango"
  "Escape": "Cancelar selección"
  "F5": "Refrescar datos"
  "Ctrl + E": "Exportar vista actual"
  "Ctrl + P": "Imprimir cuadrícula"
  "Arrow Keys": "Navegar entre árboles"
  "Enter": "Ver detalle del árbol seleccionado"
```

#### Panel de Detalle del Árbol

```
┌─────────────────────────────────────────────────────────────────┐
│  📋 DETALLE DEL ÁRBOL                                    [X]    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  🌳 AGC-001-A-0234                                              │
│  Lote A - Fila 2 - Columna 34                                  │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  📊 ESTADO ACTUAL                                               │
│  Salud: 🟢 Saludable (SAL)                                      │
│  Fenología: 🥑 Desarrollo de Fruto (DES)                        │
│  NDVI: 0.78 (Excelente) 🛰️ hace 1 día                          │
│  Edad: 6 años 9 meses                                           │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  🔍 ÚLTIMA INSPECCIÓN                                           │
│  Fecha: 2025-12-05 09:30                                        │
│  Por: Juan Pérez                                                │
│  Notas: "Buen desarrollo. Frutos 8-10cm. Sin plagas visibles"  │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  ⚠️ ALERTAS (0)                                                 │
│  Sin alertas activas                                            │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  📈 HISTORIAL RECIENTE                                          │
│  2025-12-05: Inspección → SAL                                   │
│  2025-11-28: Inspección → SAL                                   │
│  2025-11-15: NDVI: 0.76                                         │
│  2025-11-10: Aplicación foliar (Nutrición)                     │
│                                                                 │
│  [📸 Ver Fotos (3)] [📊 Historial Completo] [🗺️ Ver en Mapa]   │
│  [✏️ Editar] [📝 Nueva Inspección] [🏷️ Imprimir QR]            │
└─────────────────────────────────────────────────────────────────┘
```

#### Filtros y Búsqueda Disponibles

```yaml
filtros:
  por_estado_salud:
    - SAL (Saludable)
    - OBS (Observación)
    - RIE (Riesgo)
    - CRI (Crítico)
    - TRA (En Tratamiento)
    - REC (Recuperación)
    - Múltiple selección permitida
  
  por_etapa_fenologica:
    - Opciones según cultivo
    - Filtro dinámico
  
  por_rango_ndvi:
    - Slider: 0.0 - 1.0
    - Presets: Bajo (<0.5), Medio (0.5-0.7), Alto (>0.7)
  
  por_ultima_inspeccion:
    - Últimos 7 días
    - Últimos 14 días
    - Últimos 30 días
    - Más de 30 días
    - Nunca inspeccionados
  
  por_produccion:
    - Bajo rendimiento (<50 kg)
    - Rendimiento medio (50-100 kg)
    - Alto rendimiento (>100 kg)
    - Sin datos
  
  por_edad:
    - Rango personalizado (años)
    - Jóvenes (<3 años)
    - Productivos (3-15 años)
    - Maduros (>15 años)
  
  por_alertas:
    - Con alertas activas
    - Sin alertas
    - Por tipo de alerta

busqueda:
  por_codigo:
    - Búsqueda exacta
    - Búsqueda parcial (autocomplete)
  
  por_posicion:
    - Fila específica
    - Columna específica
    - Rango: F1-5, C10-20
  
  avanzada:
    - Combinación de múltiples criterios
    - Guardar búsquedas frecuentes
```

#### Detección Automática de Patrones Espaciales

```typescript
interface PatronDetectado {
  tipo: 'foco_plaga' | 'estres_hidrico' | 'deficit_nutricional' | 'patron_edad';
  severidad: 'baja' | 'media' | 'alta';
  arboles_afectados: number[];
  ubicacion: {
    filas: number[];
    columnas: number[];
    centro: { fila: number; columna: number };
  };
  descripcion: string;
  recomendacion: string;
  confianza: number; // 0-1
  fecha_deteccion: Date;
}

// Ejemplo de patrón detectado
const patron: PatronDetectado = {
  tipo: 'foco_plaga',
  severidad: 'alta',
  arboles_afectados: [234, 235, 236, 244, 245, 246, 254, 255, 256],
  ubicacion: {
    filas: [1, 2, 3],
    columnas: [5, 6, 7, 8],
    centro: { fila: 2, columna: 6 }
  },
  descripcion: 'Cluster de 9 árboles con estado CRI/RIE en área contigua',
  recomendacion: 'Inspección urgente. Posible foco de Phytophthora. Aplicar fungicida sistémico.',
  confianza: 0.87,
  fecha_deteccion: new Date('2025-12-08')
};

// Algoritmo de detección (simplificado)
function detectarPatronesEspaciales(cuadricula: Arbol[][]): PatronDetectado[] {
  const patrones: PatronDetectado[] = [];
  
  // Detectar clusters de árboles con problemas
  const arbolesProblema = cuadricula.flat().filter(a => 
    ['RIE', 'CRI'].includes(a.estado_salud)
  );
  
  // Análisis de vecindad (8 vecinos)
  for (const arbol of arbolesProblema) {
    const vecinos = obtenerVecinosConProblema(arbol, cuadricula);
    if (vecinos.length >= 3) {
      patrones.push(crearPatronFoco(arbol, vecinos));
    }
  }
  
  return patrones;
}
```

#### Comparación Temporal (Lado a Lado)

```
┌─────────────────────────────────────────────────────────────────┐
│  📊 COMPARACIÓN TEMPORAL - Lote A                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  2025-11-15 (hace 23 días)      |  2025-12-08 (hoy)            │
│  ───────────────────────────────┼──────────────────────────     │
│        Col→ 1  2  3  4  5  6    |      Col→ 1  2  3  4  5  6   │
│      ┌─────────────────────     |    ┌─────────────────────    │
│ Fil 1│ 🟢 🟢 🟢 🟢 🟢 🟢         | Fil1│ 🟢 🟢 🟢 🟢 🟡 🟡       │
│ Fil 2│ 🟢 🟢 🟢 🟢 🟡 🟢         | Fil2│ 🟢 🟢 🟢 🟡 🟡 🔴       │
│ Fil 3│ 🟢 🟢 🟢 🟢 🟢 🟢         | Fil3│ 🟢 🟢 🟡 🟡 🔴 🔴       │
│ Fil 4│ 🟢 🟢 🟢 🟢 🟢 🟢         | Fil4│ 🟢 🟢 🟢 🟡 🟠 🟠       │
│      └─────────────────────     |    └─────────────────────    │
│                                 |                              │
│  📊 SAL: 95% | OBS: 5%          |  📊 SAL: 74% | OBS-CRI: 26%  │
│  ⚠️ Sin alertas                 |  ⚠️ 1 foco detectado         │
│                                 |                              │
│  🔍 CAMBIOS DETECTADOS:                                         │
│  • 8 árboles empeoraron de SAL → OBS/RIE/CRI                   │
│  • Foco emergente en zona [F1-3, C5-6]                         │
│  • NDVI promedio bajó de 0.79 a 0.72                           │
│  • Recomendación: Investigar causa (riego, plaga, enfermedad)  │
│                                                                 │
│  [◀ Fecha Anterior] [Reproducir Timeline ▶] [Fecha Siguiente ▶]│
└─────────────────────────────────────────────────────────────────┘
```

#### API de Cuadrícula

```yaml
# Obtener cuadrícula de un lote
GET /api/v1/cuadricula/{loteId}
  parametros:
    modo: 'salud' | 'fenologia' | 'ndvi' | 'produccion'
    fecha: '2025-12-08' (opcional, por defecto hoy)
  respuesta:
    lote:
      id: 5
      nombre: "Lote A"
      filas: 20
      columnas: 25
      total_arboles: 500
    modo: "salud"
    fecha: "2025-12-08"
    arboles:
      - fila: 1
        columna: 1
        arbol_id: 234
        codigo: "AGC-001-A-0234"
        valor: "SAL"  # según modo
        color: "#22c55e"
        emoji: "🟢"
        tiene_alertas: false
      - fila: 1
        columna: 2
        arbol_id: 235
        codigo: "AGC-001-A-0235"
        valor: "OBS"
        color: "#eab308"
        emoji: "🟡"
        tiene_alertas: true
    resumen:
      SAL: 370
      OBS: 30
      RIE: 25
      CRI: 10
      TRA: 15
      REC: 10
      MUE: 5
      REM: 0
      NUE: 35
    patrones_detectados:
      - tipo: "foco_plaga"
        severidad: "alta"
        ubicacion: "F1-3, C5-8"
        arboles: 9
        recomendacion: "Aplicación urgente"

# Actualizar estado de árbol desde cuadrícula
PATCH /api/v1/cuadricula/arbol/{arbolId}
  body:
    estado_salud: "RIE"
    observaciones: "Hojas con manchas amarillas"
    inspeccionado_por: 123
  respuesta:
    success: true
    arbol_actualizado: {...}
    cuadricula_actualizada: true

# Comparación temporal
GET /api/v1/cuadricula/{loteId}/comparar
  parametros:
    fecha_inicio: '2025-11-15'
    fecha_fin: '2025-12-08'
    modo: 'salud'
  respuesta:
    fecha_inicio: {...}
    fecha_fin: {...}
    cambios:
      arboles_mejorados: 5
      arboles_empeorados: 8
      arboles_sin_cambio: 487
      cambios_detallados: [...]
```

#### Formatos de Exportación

| Formato | Descripción | Contenido | Uso |
|---------|-------------|-----------|-----|
| **PNG** | Imagen de la cuadrícula | Visualización con colores y leyenda | Reportes, presentaciones |
| **PDF** | Documento imprimible | Cuadrícula + resumen + alertas | Impresión, archivo |
| **Excel** | Hoja de cálculo | Matriz con códigos, estados, NDVI | Análisis offline |
| **GIF Animado** | Secuencia temporal | Evolución de la cuadrícula en el tiempo | Presentaciones, análisis |
| **JSON** | Datos estructurados | Array de árboles con todos los atributos | Integración con otros sistemas |
| **CSV** | Tabla plana | Lista de árboles con coordenadas de cuadrícula | Análisis estadístico |

**Ejemplo de exportación Excel:**

```
| Fila | Columna | Código       | Estado | NDVI | Última Inspección | Alertas |
|------|---------|--------------|--------|------|-------------------|---------|
| 1    | 1       | AGC-001-A-01 | SAL    | 0.78 | 2025-12-05       | 0       |
| 1    | 2       | AGC-001-A-02 | SAL    | 0.79 | 2025-12-05       | 0       |
| 1    | 3       | AGC-001-A-03 | OBS    | 0.68 | 2025-12-03       | 1       |
```

---

## 📚 Documentos relacionados

- [Coordenadas geográficas de cada árbol](07-01-mapeo-geolocalizacion.md)
- [Inspección de árboles en campo](07-07-app-movil-campo.md)
- [Visualización del estado de salud](07-03-salud-fenologia.md)

---

> Navegación: [← Anterior](07-01-mapeo-geolocalizacion.md) | [📑 Índice](../README.md) | [Siguiente →](07-03-salud-fenologia.md)
