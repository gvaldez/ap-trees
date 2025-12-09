### 7.3 🌱 Módulo de Salud y Fenología

> **Sistema integral para monitoreo del estado fitosanitario y seguimiento de etapas de desarrollo de los árboles**

Este módulo es crítico para la toma de decisiones agronómicas, ya que permite detectar problemas tempranamente y hacer seguimiento del ciclo productivo de cada árbol.

#### Estados de Salud Detallados

| Código | Nombre | Emoji | Descripción | Acciones Recomendadas |
|--------|--------|-------|-------------|----------------------|
| **SAL** | Saludable | 🟢 | Árbol sin problemas visibles. Follaje verde, vigor normal, sin síntomas de plagas o enfermedades. | Monitoreo rutinario |
| **OBS** | Observación | 🟡 | Síntomas leves que requieren seguimiento. Puede ser inicio de problema o condición temporal. | Inspección cada 3-5 días |
| **RIE** | Riesgo | 🟠 | Problema detectado que podría agravarse. Síntomas moderados de plaga/enfermedad o déficit. | Evaluar tratamiento en 48h |
| **CRI** | Crítico | 🔴 | Estado grave que amenaza la vida del árbol. Requiere intervención inmediata. | Tratamiento urgente |
| **TRA** | En Tratamiento | 🔵 | Árbol bajo tratamiento activo (aplicación química, poda, riego correctivo). | Seguimiento post-tratamiento |
| **REC** | Recuperación | 🟣 | Mejorando tras tratamiento. Síntomas en remisión. | Monitoreo cercano |
| **MUE** | Muerto | ⚫ | Árbol sin viabilidad. Completamente seco o con muerte de tejidos irreversible. | Remoción programada |
| **REM** | Removido | ⚪ | Árbol eliminado físicamente de la plantación. | Registro histórico |
| **NUE** | Nuevo | 🟢 | Árbol recién plantado (menos de 6 meses). En establecimiento. | Cuidados especiales |

#### Diagrama de Transiciones de Estado Permitidas

```
┌─────────────────────────────────────────────────────────────────┐
│           TRANSICIONES DE ESTADO DE SALUD                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                         ┌─────────┐                             │
│                    ┌───▶│   NUE   │────┐                        │
│                    │    │  Nuevo  │    │                        │
│                    │    └─────────┘    ▼                        │
│                    │                ┌─────────┐                 │
│    ┌───────────────┴───────────────▶│   SAL   │◀─┐             │
│    │                                │Saludable│  │             │
│    │                                └─────────┘  │             │
│    │                                   │  ▲      │             │
│    │                                   ▼  │      │             │
│    │                                ┌─────────┐  │             │
│    │                           ┌───▶│   OBS   │──┘             │
│    │                           │    │Observ.  │                │
│    │                           │    └─────────┘                │
│    │                           │       │  ▲                     │
│    │                           │       ▼  │                     │
│    │                           │    ┌─────────┐                │
│    │                           │    │   RIE   │                │
│    │                           │    │ Riesgo  │                │
│    │                           │    └─────────┘                │
│    │                           │       │  ▲                     │
│    │                           │       ▼  │                     │
│    │                           │    ┌─────────┐                │
│    │                           │    │   CRI   │                │
│    │                           │    │ Crítico │                │
│    │                           │    └─────────┘                │
│    │                           │       │  ▲                     │
│    │                           │       ▼  │                     │
│    │    ┌─────────┐         ┌─────────┐  │                     │
│    │    │   REC   │◀────────│   TRA   │──┘                     │
│    │    │Recuper. │         │Tratam.  │                        │
│    │    └─────────┘         └─────────┘                        │
│    │       │                     │                              │
│    └───────┘                     │                              │
│                                  ▼                              │
│                              ┌─────────┐                        │
│                              │   MUE   │                        │
│                              │ Muerto  │                        │
│                              └─────────┘                        │
│                                  │                              │
│                                  ▼                              │
│                              ┌─────────┐                        │
│                              │   REM   │                        │
│                              │Removido │                        │
│                              └─────────┘                        │
│                                                                 │
│  Reglas:                                                        │
│  • NUE solo puede ir a SAL (después de 6 meses)                │
│  • Cualquier estado (excepto REM/MUE) puede ir a TRA           │
│  • Solo desde TRA se puede ir a REC                            │
│  • MUE es terminal (solo va a REM)                             │
│  • REM es final (no hay transiciones desde aquí)               │
└─────────────────────────────────────────────────────────────────┘
```

#### Etapas Fenológicas - Aguacate (Completas)

```typescript
type EtapaFenologicaAguacate = 
  | 'REP'  // Reposo
  | 'BRO'  // Brotación
  | 'VEG'  // Crecimiento Vegetativo
  | 'IFL'  // Inducción Floral
  | 'FLO'  // Floración
  | 'CUA'  // Cuajado
  | 'DES'  // Desarrollo de Fruto
  | 'MAD'  // Maduración
  | 'COS'; // Listo para Cosecha

interface EtapaFenologica {
  codigo: string;
  nombre: string;
  descripcion: string;
  duracion_aprox_dias: number;
  cuidados_especiales: string[];
  indicadores_visuales: string[];
  riesgos_comunes: string[];
}

const ETAPAS_AGUACATE: Record<EtapaFenologicaAguacate, EtapaFenologica> = {
  REP: {
    codigo: 'REP',
    nombre: 'Reposo',
    descripcion: 'Periodo de baja actividad vegetativa. Preparación para nuevo ciclo.',
    duracion_aprox_dias: 30,
    cuidados_especiales: [
      'Reducir riego',
      'Evitar aplicaciones foliares innecesarias',
      'Preparar terreno'
    ],
    indicadores_visuales: [
      'Crecimiento mínimo',
      'Pocas hojas nuevas'
    ],
    riesgos_comunes: ['Sequía excesiva', 'Ataques de barrenador']
  },
  BRO: {
    codigo: 'BRO',
    nombre: 'Brotación',
    descripcion: 'Emergencia de nuevos brotes vegetativos. Hojas tiernas.',
    duracion_aprox_dias: 20,
    cuidados_especiales: [
      'Incrementar riego gradualmente',
      'Fertilización nitrogenada',
      'Protección contra plagas'
    ],
    indicadores_visuales: [
      'Brotes verdes claros',
      'Hojas jóvenes expandiéndose'
    ],
    riesgos_comunes: ['Trips', 'Minador de la hoja', 'Ácaros']
  },
  VEG: {
    codigo: 'VEG',
    nombre: 'Crecimiento Vegetativo',
    descripcion: 'Desarrollo activo de ramas y follaje. Acumulación de reservas.',
    duracion_aprox_dias: 60,
    cuidados_especiales: [
      'Riego regular',
      'Fertilización completa (NPK)',
      'Control de malezas'
    ],
    indicadores_visuales: [
      'Follaje denso verde oscuro',
      'Crecimiento de ramas'
    ],
    riesgos_comunes: ['Deficiencia nutricional', 'Pudrición radicular']
  },
  IFL: {
    codigo: 'IFL',
    nombre: 'Inducción Floral',
    descripcion: 'Formación de botones florales. Diferenciación de yemas.',
    duracion_aprox_dias: 30,
    cuidados_especiales: [
      'Reducir nitrógeno',
      'Aplicar potasio y boro',
      'Manejo de estrés controlado'
    ],
    indicadores_visuales: [
      'Yemas florales hinchadas',
      'Cambio de coloración en yemas'
    ],
    riesgos_comunes: ['Desequilibrio nutricional', 'Estrés hídrico extremo']
  },
  FLO: {
    codigo: 'FLO',
    nombre: 'Floración',
    descripcion: 'Apertura de flores. Polinización activa.',
    duracion_aprox_dias: 45,
    cuidados_especiales: [
      'Evitar aplicaciones durante floración',
      'Fomentar polinizadores',
      'Riego ligero y constante'
    ],
    indicadores_visuales: [
      'Panículas florales abiertas',
      'Flores amarillo-verdosas',
      'Actividad de abejas'
    ],
    riesgos_comunes: ['Lluvia excesiva', 'Vientos fuertes', 'Antracnosis']
  },
  CUA: {
    codigo: 'CUA',
    nombre: 'Cuajado',
    descripcion: 'Formación inicial del fruto. Alta caída natural.',
    duracion_aprox_dias: 30,
    cuidados_especiales: [
      'Evitar estrés hídrico',
      'Aplicar calcio y boro',
      'Proteger de plagas'
    ],
    indicadores_visuales: [
      'Frutos pequeños (2-3 cm)',
      'Alta caída de frutillos',
      'Frutos verdes brillantes'
    ],
    riesgos_comunes: ['Caída excesiva', 'Trips del aguacate', 'Araña roja']
  },
  DES: {
    codigo: 'DES',
    nombre: 'Desarrollo de Fruto',
    descripcion: 'Crecimiento del fruto. Acumulación de aceite.',
    duracion_aprox_dias: 90,
    cuidados_especiales: [
      'Riego constante',
      'Fertilización balanceada',
      'Monitoreo de plagas'
    ],
    indicadores_visuales: [
      'Frutos 5-15 cm',
      'Color verde oscuro',
      'Forma definitiva del fruto'
    ],
    riesgos_comunes: ['Barrenador del fruto', 'Antracnosis', 'Phytophthora']
  },
  MAD: {
    codigo: 'MAD',
    nombre: 'Maduración',
    descripcion: 'Madurez fisiológica alcanzada. Fruto listo para corte.',
    duracion_aprox_dias: 30,
    cuidados_especiales: [
      'Reducir riego',
      'Preparar para cosecha',
      'Proteger de pájaros'
    ],
    indicadores_visuales: [
      'Tamaño final',
      'Cambio de brillo',
      'Materia seca >23%'
    ],
    riesgos_comunes: ['Daño mecánico', 'Robo', 'Caída prematura']
  },
  COS: {
    codigo: 'COS',
    nombre: 'Listo para Cosecha',
    descripcion: 'Fruto en ventana de cosecha óptima.',
    duracion_aprox_dias: 20,
    cuidados_especiales: [
      'Programar cuadrillas',
      'Cosechar con pedúnculo',
      'Evitar golpes'
    ],
    indicadores_visuales: [
      'Color maduro',
      'Desprendimiento fácil'
    ],
    riesgos_comunes: ['Sobremaduración', 'Daño en cosecha']
  }
};
```

#### Otros Cultivos Soportados

| Cultivo | Variedades Principales | Etapas Fenológicas | Duración Ciclo |
|---------|------------------------|-------------------|----------------|
| **Mango** | Tommy Atkins, Kent, Keitt, Ataulfo | REP, BRO, VEG, IFL, FLO, CUA, DES, MAD, COS | 4-5 meses |
| **Cítricos** | Naranja Valencia, Limón Tahití, Mandarina | REP, BRO, FLO, CUA, DES1, DES2, MAD, COS | 6-12 meses |
| **Manzana** | Red Delicious, Granny Smith, Gala | REP, BRO, FLO, CUA, DES, MAD, COS | 4-5 meses |
| **Durazno** | Jarillo, Rubidoux, Diamante | REP, BRO, FLO, CUA, DES, MAD, COS | 3-4 meses |
| **Café** | Arábica, Robusta, Castillo | FLO, CUA, DES, MAD, COS, REP | 7-9 meses |

#### Modelo de Datos SQL Completo

```sql
-- Historial de estados de salud
CREATE TABLE salud_registros (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id),
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    estado_anterior VARCHAR(10),
    estado_nuevo VARCHAR(10) NOT NULL,
    
    -- Diagnóstico
    sintomas TEXT[],
    plaga_detectada_id INTEGER REFERENCES catalogo_plagas(id),
    enfermedad_detectada_id INTEGER REFERENCES catalogo_enfermedades(id),
    severidad VARCHAR(20), -- 'leve', 'moderada', 'severa'
    porcentaje_afectacion DECIMAL(5,2),
    
    -- Contexto
    ubicacion_sintomas VARCHAR(100), -- 'hojas', 'tronco', 'raices', 'frutos'
    fotos TEXT[], -- URLs de fotos
    
    -- Responsable
    registrado_por INTEGER REFERENCES usuarios(id),
    metodo_deteccion VARCHAR(30), -- 'inspeccion_visual', 'analisis_ia', 'drone', 'laboratorio'
    
    -- Acciones tomadas
    requiere_accion BOOLEAN DEFAULT false,
    accion_tomada TEXT,
    tratamiento_aplicado_id INTEGER REFERENCES aplicaciones(id),
    
    observaciones TEXT
);

-- Historial de etapas fenológicas
CREATE TABLE fenologia_registros (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id),
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    etapa_anterior VARCHAR(10),
    etapa_nueva VARCHAR(10) NOT NULL,
    
    -- Detalles de la etapa
    porcentaje_completitud DECIMAL(5,2), -- % de avance en la etapa (0-100)
    dias_en_etapa INTEGER,
    fecha_esperada_cambio DATE, -- predicción de cuándo cambiará de etapa
    
    -- Producción (para etapas productivas)
    numero_frutos INTEGER,
    tamano_promedio_frutos_cm DECIMAL(6,2),
    
    -- Observaciones
    registrado_por INTEGER REFERENCES usuarios(id),
    fotos TEXT[],
    notas TEXT
);

-- Índices de vegetación (histórico)
CREATE TABLE indices_vegetacion (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id),
    fecha DATE NOT NULL,
    fuente VARCHAR(30), -- 'satelite', 'drone', 'camara_multiespectral'
    
    -- Índices principales
    ndvi DECIMAL(6,4),      -- Normalized Difference Vegetation Index (-1 a 1)
    ndre DECIMAL(6,4),      -- Normalized Difference Red Edge Index
    gndvi DECIMAL(6,4),     -- Green NDVI
    cwsi DECIMAL(6,4),      -- Crop Water Stress Index (0 a 1)
    lai DECIMAL(6,3),       -- Leaf Area Index
    
    -- Metadata
    resolucion_cm DECIMAL(6,2),
    confianza DECIMAL(5,4),
    
    UNIQUE(arbol_id, fecha, fuente)
);

-- Alertas de salud
CREATE TABLE alertas_salud (
    id SERIAL PRIMARY KEY,
    
    -- Alcance de la alerta
    tipo_alcance VARCHAR(20), -- 'arbol', 'lote', 'sector', 'finca'
    arbol_id INTEGER REFERENCES arboles(id),
    lote_id INTEGER REFERENCES lotes(id),
    sector_id INTEGER REFERENCES sectores(id),
    finca_id INTEGER REFERENCES fincas(id),
    arboles_afectados INTEGER[],
    
    -- Tipo y severidad
    tipo_alerta VARCHAR(50), -- 'estado_critico', 'foco_plaga', 'estres_hidrico', 'deficit_nutricional'
    severidad VARCHAR(20), -- 'baja', 'media', 'alta', 'critica'
    prioridad INTEGER DEFAULT 3, -- 1=urgente, 2=alta, 3=media, 4=baja
    
    -- Detalles
    titulo VARCHAR(200),
    descripcion TEXT,
    problema_detectado VARCHAR(100),
    plaga_enfermedad_id INTEGER,
    
    -- Recomendaciones
    accion_recomendada TEXT,
    urgencia_dias INTEGER, -- en cuántos días debe atenderse
    
    -- Estado de la alerta
    estado VARCHAR(20) DEFAULT 'activa', -- 'activa', 'en_atencion', 'resuelta', 'falsa_alarma'
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    fecha_atencion TIMESTAMP,
    fecha_resolucion TIMESTAMP,
    atendida_por INTEGER REFERENCES usuarios(id),
    
    -- Seguimiento
    aplicacion_realizada_id INTEGER REFERENCES aplicaciones(id),
    resultado TEXT,
    
    -- Metadata de detección
    detectado_por VARCHAR(30), -- 'sistema_ia', 'inspeccion_manual', 'analisis_patron'
    confianza DECIMAL(5,4)
);

-- Índices para consultas rápidas
CREATE INDEX idx_salud_arbol_fecha ON salud_registros(arbol_id, fecha DESC);
CREATE INDEX idx_salud_estado ON salud_registros(estado_nuevo);
CREATE INDEX idx_fenologia_arbol_fecha ON fenologia_registros(arbol_id, fecha DESC);
CREATE INDEX idx_fenologia_etapa ON fenologia_registros(etapa_nueva);
CREATE INDEX idx_indices_arbol_fecha ON indices_vegetacion(arbol_id, fecha DESC);
CREATE INDEX idx_alertas_estado ON alertas_salud(estado);
CREATE INDEX idx_alertas_severidad ON alertas_salud(severidad, estado);
```

#### Indicadores de Monitoreo con Métodos y Umbrales

| Indicador | Método de Captura | Frecuencia | Umbral Normal | Umbral Alerta | Acción |
|-----------|-------------------|------------|---------------|---------------|--------|
| **Estado Visual** | Inspección + App móvil | Semanal | SAL | OBS, RIE, CRI | Investigar causa |
| **NDVI** | Drone/Satélite | Quincenal | 0.65-0.85 | <0.5 o >0.9 | Revisar nutrición/riego |
| **NDRE** | Cámara multiespectral | Quincenal | 0.5-0.7 | <0.4 | Déficit de nitrógeno |
| **CWSI** | Cámara térmica | Semanal (época seca) | 0.0-0.3 | >0.5 | Riego inmediato |
| **LAI** | Análisis de imagen | Mensual | 3-5 | <2 o >7 | Evaluar manejo |
| **Plagas** | Trampas + Inspección | Semanal | <5 individuos/trampa | >20 individuos | Aplicación |
| **Enfermedades** | Inspección + Fotos + IA | Semanal | Sin síntomas | Lesiones visibles | Tratamiento |
| **Crecimiento** | Medición manual | Mensual | Según edad | <50% esperado | Evaluar nutrición |
| **Producción** | Conteo frutos | Por etapa | Según variedad | <60% esperado | Ajustar manejo |

#### Integración con IA

```typescript
interface ResultadoAnalisisIA {
  arbol_id: number;
  fecha_analisis: Date;
  tipo_analisis: 'deteccion_plaga' | 'evaluacion_salud' | 'conteo_frutos' | 'segmentacion_follaje';
  
  // Entrada
  imagenes: {
    url: string;
    tipo: 'rgb' | 'multiespectral' | 'termica';
    fecha_captura: Date;
  }[];
  
  // Resultados
  confianza: number; // 0-1
  clasificacion: {
    categoria: string;
    probabilidad: number;
    bbox?: { x: number; y: number; w: number; h: number };
  }[];
  
  // Diagnóstico automático
  estado_salud_sugerido?: 'SAL' | 'OBS' | 'RIE' | 'CRI';
  plagas_detectadas?: {
    plaga_id: number;
    nombre: string;
    confianza: number;
    severidad: 'leve' | 'moderada' | 'severa';
  }[];
  enfermedades_detectadas?: {
    enfermedad_id: number;
    nombre: string;
    confianza: number;
    severidad: 'leve' | 'moderada' | 'severa';
  }[];
  
  // Conteos
  numero_frutos?: number;
  area_foliar_m2?: number;
  
  // Recomendaciones
  requiere_revision_humana: boolean;
  recomendaciones: string[];
  
  // Metadata
  modelo_ia: string;
  version_modelo: string;
  tiempo_procesamiento_ms: number;
}

// Flujo de análisis con IA
interface FlujoAnalisisIA {
  pasos: {
    paso: number;
    accion: string;
    responsable: 'sistema' | 'usuario' | 'ingeniero';
  }[];
}

const FLUJO_ANALISIS_IA: FlujoAnalisisIA = {
  pasos: [
    {
      paso: 1,
      accion: "Drone captura imágenes RGB + multiespectral del lote",
      responsable: "sistema"
    },
    {
      paso: 2,
      accion: "Procesamiento de imágenes: ortomosaico + georreferenciación",
      responsable: "sistema"
    },
    {
      paso: 3,
      accion: "IA detecta árboles y calcula índices (NDVI, NDRE)",
      responsable: "sistema"
    },
    {
      paso: 4,
      accion: "IA analiza cada árbol: color follaje, densidad, síntomas",
      responsable: "sistema"
    },
    {
      paso: 5,
      accion: "Sistema clasifica árboles por estado de salud",
      responsable: "sistema"
    },
    {
      paso: 6,
      accion: "Algoritmo de clustering detecta focos de problemas",
      responsable: "sistema"
    },
    {
      paso: 7,
      accion: "Sistema genera alertas automáticas con recomendaciones",
      responsable: "sistema"
    },
    {
      paso: 8,
      accion: "Ingeniero agrónomo revisa alertas en dashboard",
      responsable: "ingeniero"
    },
    {
      paso: 9,
      accion: "Si es necesario: inspección en campo para confirmar diagnóstico",
      responsable: "usuario"
    },
    {
      paso: 10,
      accion: "Decisión de tratamiento y programación de aplicación",
      responsable: "ingeniero"
    }
  ]
};
```

#### Diagrama de Flujo - Análisis con IA

```
┌─────────────────────────────────────────────────────────────────┐
│          FLUJO DE ANÁLISIS AUTOMÁTICO CON IA                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  🚁 Vuelo Drone                                                 │
│      │                                                          │
│      │ Captura: RGB + Multiespectral                           │
│      ▼                                                          │
│  📊 Procesamiento                                               │
│      │                                                          │
│      │ - Ortomosaico                                           │
│      │ - Georreferenciación                                    │
│      │ - Cálculo NDVI/NDRE                                     │
│      ▼                                                          │
│  🤖 IA - Detección Árboles                                      │
│      │                                                          │
│      │ Modelo: YOLO v8                                         │
│      │ Confianza > 0.9                                         │
│      ▼                                                          │
│  🧠 IA - Análisis Individual                                    │
│      │                                                          │
│      │ Por cada árbol:                                         │
│      │ - Color follaje                                         │
│      │ - Densidad                                              │
│      │ - Síntomas visuales                                     │
│      │ - Comparación temporal                                  │
│      ▼                                                          │
│  🎯 Clasificación Estado                                        │
│      │                                                          │
│      ├─ SAL (78%)                                              │
│      ├─ OBS (12%)                                              │
│      ├─ RIE (7%)                                               │
│      └─ CRI (3%)                                               │
│      ▼                                                          │
│  🔍 Detección de Patrones                                       │
│      │                                                          │
│      │ Clustering espacial                                     │
│      │ Focos de plagas/enfermedades                            │
│      ▼                                                          │
│  ⚠️ Generación de Alertas                                       │
│      │                                                          │
│      │ - Foco crítico en F2-4, C5-8                            │
│      │ - Recomendación: Inspección urgente                     │
│      │ - Prioridad: ALTA                                       │
│      ▼                                                          │
│  👨‍🌾 Dashboard Ingeniero                                         │
│      │                                                          │
│      │ Revisión y validación                                   │
│      ▼                                                          │
│  🔬 Inspección Campo (si necesario)                             │
│      │                                                          │
│      │ Confirmar diagnóstico                                   │
│      ▼                                                          │
│  💉 Decisión Tratamiento                                        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Reglas de Alertas Automáticas

```yaml
reglas_alertas:
  
  # Alerta por árbol individual crítico
  arbol_critico:
    condicion: "estado_salud = 'CRI'"
    severidad: "alta"
    titulo: "Árbol en estado crítico - {codigo}"
    descripcion: "El árbol {codigo} ha sido marcado como crítico. Requiere atención inmediata."
    accion_recomendada: "Inspección inmediata y diagnóstico detallado"
    urgencia_dias: 1
    notificar:
      - ingeniero_responsable
      - supervisor_finca
  
  # Alerta por foco de plagas/enfermedades
  foco_problema:
    condicion: "3 o más árboles RIE/CRI adyacentes"
    severidad: "critica"
    titulo: "Posible foco de {problema} en {lote}"
    descripcion: "Detectados {n} árboles con problemas en área {ubicacion}"
    accion_recomendada: |
      1. Inspección inmediata de la zona
      2. Diagnóstico preciso del problema
      3. Aplicación de tratamiento según protocolo
      4. Delimitar área de cuarentena si es necesario
    urgencia_dias: 2
    notificar:
      - ingeniero_responsable
      - supervisor_finca
      - gerente_produccion
  
  # Alerta por NDVI bajo
  ndvi_bajo:
    condicion: "ndvi < 0.5 por 2 mediciones consecutivas"
    severidad: "media"
    titulo: "NDVI bajo en árbol {codigo}"
    descripcion: "NDVI: {valor}. Posible estrés nutricional o hídrico."
    accion_recomendada: |
      1. Verificar sistema de riego
      2. Revisar plan de fertilización
      3. Descartar enfermedades radiculares
    urgencia_dias: 7
    notificar:
      - ingeniero_responsable
  
  # Alerta por árbol sin inspección
  sin_inspeccion:
    condicion: "ultima_inspeccion > 21 días"
    severidad: "baja"
    titulo: "Árbol sin inspección reciente"
    descripcion: "El árbol {codigo} no ha sido inspeccionado en {dias} días"
    accion_recomendada: "Programar inspección rutinaria"
    urgencia_dias: 7
    notificar:
      - supervisor_campo
  
  # Alerta por estrés hídrico
  estres_hidrico:
    condicion: "cwsi > 0.6"
    severidad: "alta"
    titulo: "Estrés hídrico detectado en {lote}"
    descripcion: "CWSI: {valor}. {n} árboles con estrés hídrico severo."
    accion_recomendada: |
      1. Verificar funcionamiento del sistema de riego
      2. Programar riego de emergencia si es necesario
      3. Revisar en 48 horas
    urgencia_dias: 1
    notificar:
      - ingeniero_responsable
      - encargado_riego
  
  # Alerta por fenología desincronizada
  fenologia_atrasada:
    condicion: "dias_en_etapa > duracion_esperada * 1.5"
    severidad: "media"
    titulo: "Árbol con fenología atrasada"
    descripcion: "Árbol en etapa {etapa} por {dias} días (esperado: {esperado})"
    accion_recomendada: "Evaluar factores limitantes: nutrición, agua, salud"
    urgencia_dias: 14
    notificar:
      - ingeniero_responsable
```

#### Panel de Salud del Árbol (Completo)

```
┌─────────────────────────────────────────────────────────────────┐
│  🌳 PANEL DE SALUD - AGC-001-A-0234                      [X]    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📋 INFORMACIÓN GENERAL                                         │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Lote: A | Fila: 2 | Columna: 34                          │  │
│  │ Cultivo: Aguacate Hass | Patrón: Criollo                 │  │
│  │ Edad: 6 años 9 meses | Plantado: 15/Mar/2019             │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  📊 ESTADO ACTUAL                                               │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Salud: 🟢 Saludable (SAL)                                 │  │
│  │   Desde: 25/Nov/2025 (13 días)                           │  │
│  │                                                           │  │
│  │ Fenología: 🥑 Desarrollo de Fruto (DES)                   │  │
│  │   Desde: 01/Nov/2025 (37 días de 90 esperados)           │  │
│  │   Completitud: 41% ▓▓▓▓▓▓▓▓░░░░░░░                       │  │
│  │   Estimado cambio a MAD: 03/Ene/2026                     │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  📈 ÍNDICES DE VEGETACIÓN                                       │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ NDVI: 0.78 🟢 Excelente  (06/Dic/2025 - Drone)           │  │
│  │   Tendencia: 0.74 → 0.76 → 0.78 ↗                        │  │
│  │                                                           │  │
│  │ NDRE: 0.62 🟢 Normal     (06/Dic/2025)                    │  │
│  │                                                           │  │
│  │ CWSI: 0.15 🟢 Sin estrés (05/Dic/2025 - Térmica)          │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  🔍 ÚLTIMA INSPECCIÓN                                           │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Fecha: 05/Dic/2025 09:30 (hace 3 días)                   │  │
│  │ Inspector: Juan Pérez                                     │  │
│  │                                                           │  │
│  │ Observaciones:                                            │  │
│  │ "Buen desarrollo general. Frutos de 8-10 cm. Sin         │  │
│  │  plagas visibles. Follaje verde oscuro. Continuar        │  │
│  │  con manejo normal."                                      │  │
│  │                                                           │  │
│  │ [📸 Ver Fotos (3)]                                        │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ⚠️ ALERTAS ACTIVAS (0)                                         │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ ✓ Sin alertas activas                                     │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  📊 PRODUCCIÓN                                                  │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Temporada actual (2025):                                  │  │
│  │ • Frutos estimados: 85-95 unidades                        │  │
│  │ • Rendimiento esperado: 90-105 kg                         │  │
│  │                                                           │  │
│  │ Histórico:                                                │  │
│  │ • 2024: 98 kg | 2023: 87 kg | 2022: 72 kg               │  │
│  │ • Tendencia: ↗ Creciente                                 │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  🧪 ÚLTIMA APLICACIÓN                                           │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Fecha: 10/Nov/2025 (hace 28 días)                        │  │
│  │ Tipo: Fertilización foliar                               │  │
│  │ Productos: Nutrifoliar Multi + Aminoácidos                │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  📜 HISTORIAL RECIENTE (últimos 30 días)                        │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ 05/Dic: Inspección → SAL                                  │  │
│  │ 28/Nov: Inspección → SAL                                  │  │
│  │ 21/Nov: Inspección → SAL                                  │  │
│  │ 15/Nov: NDVI: 0.76 (Drone)                                │  │
│  │ 10/Nov: Aplicación foliar (Nutrición)                    │  │
│  │ 07/Nov: Inspección → SAL                                  │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  [📊 Historial Completo] [📈 Gráficas] [🗺️ Ver en Mapa]        │
│  [✏️ Editar] [📝 Nueva Inspección] [🏷️ Imprimir QR]            │
│  [📸 Subir Foto] [⚠️ Reportar Problema]                        │
└─────────────────────────────────────────────────────────────────┘
```

#### API de Salud y Fenología

```yaml
# Obtener estado de salud de un árbol
GET /api/v1/arboles/{arbolId}/salud
  respuesta:
    arbol_id: 234
    codigo: "AGC-001-A-0234"
    estado_actual: "SAL"
    fecha_cambio: "2025-11-25T10:30:00Z"
    dias_en_estado: 13
    historial_reciente:
      - fecha: "2025-12-05"
        estado: "SAL"
        observaciones: "Buen desarrollo..."
        inspector: "Juan Pérez"
    indices:
      ndvi: 0.78
      fecha_ndvi: "2025-12-06"
      cwsi: 0.15
    alertas_activas: []

# Registrar inspección de salud
POST /api/v1/arboles/{arbolId}/salud/inspeccion
  body:
    estado_nuevo: "OBS"
    sintomas: ["hojas_amarillas", "manchas_foliares"]
    severidad: "leve"
    porcentaje_afectacion: 15
    ubicacion_sintomas: "hojas_jovenes"
    fotos: ["url1.jpg", "url2.jpg"]
    observaciones: "Inicio de clorosis en hojas jóvenes"
    requiere_accion: true
  respuesta:
    registro_id: 5678
    alerta_generada: true
    alerta_id: 123

# Obtener etapa fenológica
GET /api/v1/arboles/{arbolId}/fenologia
  respuesta:
    arbol_id: 234
    etapa_actual: "DES"
    fecha_inicio_etapa: "2025-11-01"
    dias_en_etapa: 37
    duracion_esperada: 90
    completitud: 0.41
    fecha_estimada_cambio: "2026-01-03"
    numero_frutos: 92
    tamano_promedio_cm: 9.5

# Actualizar etapa fenológica
POST /api/v1/arboles/{arbolId}/fenologia/cambiar
  body:
    etapa_nueva: "MAD"
    numero_frutos: 88
    tamano_promedio_cm: 12.5
    observaciones: "Frutos alcanzaron madurez fisiológica"
    fotos: ["fruto1.jpg"]
  respuesta:
    registro_id: 8901
    etapa_actualizada: "MAD"

# Obtener índices de vegetación históricos
GET /api/v1/arboles/{arbolId}/indices
  parametros:
    fecha_inicio: "2025-09-01"
    fecha_fin: "2025-12-08"
    indices: ["ndvi", "ndre", "cwsi"]
  respuesta:
    arbol_id: 234
    periodo: "2025-09-01 a 2025-12-08"
    datos:
      - fecha: "2025-09-15"
        ndvi: 0.72
        ndre: 0.58
        fuente: "drone"
      - fecha: "2025-10-01"
        ndvi: 0.74
        ndre: 0.60
        fuente: "drone"
      - fecha: "2025-10-15"
        ndvi: 0.76
        ndre: 0.62
        cwsi: 0.18
        fuente: "drone"

# Obtener alertas activas
GET /api/v1/alertas
  parametros:
    finca_id: 1
    estado: "activa"
    severidad: ["alta", "critica"]
  respuesta:
    total: 3
    alertas:
      - id: 123
        tipo: "foco_plaga"
        severidad: "alta"
        titulo: "Posible foco de Phytophthora en Lote A"
        arboles_afectados: [234, 235, 236, 244, 245]
        fecha_creacion: "2025-12-07T14:00:00Z"
        urgencia_dias: 2
        estado: "activa"

# Resolver alerta
PATCH /api/v1/alertas/{alertaId}/resolver
  body:
    estado: "resuelta"
    resultado: "Aplicación fungicida realizada. Mejora visible en 5 días."
    aplicacion_id: 567
  respuesta:
    alerta_id: 123
    estado: "resuelta"
    fecha_resolucion: "2025-12-08T10:30:00Z"
```

#### Reportes de Salud Disponibles

| Reporte | Descripción | Periodo | Formato |
|---------|-------------|---------|---------|
| **Estado General Finca** | Resumen de salud de todos los árboles | Actual | PDF, Excel |
| **Evolución Temporal** | Cambios de estado en periodo seleccionado | Personalizable | PDF, Gráficas |
| **Focos de Problemas** | Mapeo de áreas con problemas agrupados | Actual | PDF con mapa |
| **Tendencias NDVI** | Evolución de índices de vegetación | 3-6 meses | PDF, Excel |
| **Eficacia de Tratamientos** | Análisis antes/después de aplicaciones | Por aplicación | PDF |
| **Árbol Individual** | Historial completo de un árbol | Completo | PDF |
| **Comparativo Lotes** | Comparación de salud entre lotes | Actual | PDF, Excel |
| **Predicción de Cosecha** | Estimación basada en fenología | Temporada | PDF, Excel |
| **Alertas Históricas** | Registro de todas las alertas | Personalizable | PDF, Excel |

---

## 📚 Documentos relacionados

- [Vista rápida del estado de salud](07-02-vista-cuadricula.md)
- [Correlación entre riego y salud](07-04-infraestructura-hidrica-riego.md)
- [Catálogo de plagas y enfermedades](../04-catalogo-plagas-enfermedades.md)
- [Aplicaciones basadas en diagnóstico](07-05-aplicaciones-calculo-dosis.md)

---

> Navegación: [← Anterior](07-02-vista-cuadricula.md) | [📑 Índice](../README.md) | [Siguiente →](07-04-infraestructura-hidrica-riego.md)
