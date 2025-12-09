### 7.1 📍 Módulo de Mapeo y Geolocalización

> **Sistema completo de geolocalización precisa y organización espacial de los árboles en la finca**

Este módulo es fundamental para el sistema, ya que establece la ubicación exacta de cada árbol y permite visualizar toda la información en el contexto geográfico de la finca.

#### Jerarquía Organizacional

```
┌─────────────────────────────────────────────────────────────────┐
│              ESTRUCTURA ORGANIZACIONAL ESPACIAL                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  🏢 TENANT (Organización)                                        │
│      └─ Finca Los Alamos                                        │
│         └─ Finca El Paraíso                                     │
│         └─ ...                                                  │
│                                                                 │
│  🏞️ FINCA (Propiedad agrícola)                                  │
│      └─ Ubicación GPS central                                   │
│      └─ Polígono delimitador                                    │
│      └─ Área total (hectáreas)                                  │
│                                                                 │
│  📍 SECTOR (Agrupación por topografía/manejo)                   │
│      └─ Sector Norte (Parte alta)                               │
│      └─ Sector Sur (Parte baja)                                 │
│      └─ Polígono de sector                                      │
│                                                                 │
│  🗺️ LOTE (Unidad de plantación homogénea)                       │
│      └─ Lote A (Aguacate Hass, año 2019)                        │
│      └─ Lote B (Aguacate Hass, año 2020)                        │
│      └─ Cultivo, variedad, año                                  │
│      └─ Configuración de cuadrícula                             │
│                                                                 │
│  🌳 ÁRBOL (Individuo georeferenciado)                            │
│      └─ Coordenadas GPS precisas                                │
│      └─ Código único                                            │
│      └─ Posición en cuadrícula (fila, columna)                  │
│      └─ Historial completo                                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### Modelo de Datos SQL Completo

```sql
-- Fincas (propiedades agrícolas)
CREATE TABLE fincas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    nombre VARCHAR(100) NOT NULL,
    ubicacion_centro GEOGRAPHY(POINT, 4326),
    poligono_delimitador GEOGRAPHY(POLYGON, 4326),
    area_hectareas DECIMAL(10,2),
    altitud_msnm INTEGER,
    clima VARCHAR(50),
    tipo_suelo VARCHAR(100),
    direccion TEXT,
    propietario VARCHAR(200),
    encargado_id INTEGER REFERENCES usuarios(id),
    fecha_registro DATE DEFAULT CURRENT_DATE,
    activo BOOLEAN DEFAULT true,
    notas TEXT
);

-- Sectores (agrupaciones dentro de finca)
CREATE TABLE sectores (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    nombre VARCHAR(100) NOT NULL,
    poligono GEOGRAPHY(POLYGON, 4326),
    area_hectareas DECIMAL(10,2),
    descripcion TEXT,
    topografia VARCHAR(50), -- 'plano', 'pendiente_suave', 'pendiente_media', 'pendiente_fuerte'
    exposicion VARCHAR(20), -- 'norte', 'sur', 'este', 'oeste'
    orden_visualizacion INTEGER DEFAULT 0
);

-- Lotes (unidades de plantación homogénea)
CREATE TABLE lotes (
    id SERIAL PRIMARY KEY,
    sector_id INTEGER REFERENCES sectores(id),
    finca_id INTEGER REFERENCES fincas(id),
    nombre VARCHAR(100) NOT NULL,
    codigo VARCHAR(20) UNIQUE,
    poligono GEOGRAPHY(POLYGON, 4326),
    area_hectareas DECIMAL(10,2),
    cultivo_id VARCHAR(50),
    variedad VARCHAR(100),
    patron VARCHAR(100), -- portainjerto
    ano_plantacion INTEGER,
    fecha_plantacion DATE,
    distancia_entre_filas DECIMAL(5,2), -- metros
    distancia_entre_arboles DECIMAL(5,2), -- metros
    arboles_programados INTEGER, -- árboles que debería haber
    arboles_actuales INTEGER, -- árboles existentes actualmente
    configuracion_cuadricula JSONB, -- { "filas": 20, "columnas": 25, "origen": {...} }
    estado VARCHAR(20), -- 'activo', 'en_renovacion', 'inactivo'
    notas TEXT
);

-- Árboles (individuos georeferenciados)
CREATE TABLE arboles (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    finca_id INTEGER REFERENCES fincas(id),
    sector_id INTEGER REFERENCES sectores(id),
    lote_id INTEGER REFERENCES lotes(id),
    
    -- Identificación
    codigo VARCHAR(50) UNIQUE NOT NULL, -- ej: "AGC-001-A-0234"
    qr_code VARCHAR(100), -- código QR único
    nfc_tag VARCHAR(100), -- ID de tag NFC
    
    -- Geolocalización
    ubicacion GEOGRAPHY(POINT, 4326) NOT NULL,
    altitud_msnm DECIMAL(7,2),
    precision_gps_metros DECIMAL(5,2),
    metodo_registro VARCHAR(30), -- 'gps_manual', 'gps_rtk', 'csv_import', 'cuadricula', 'drone'
    
    -- Posición en cuadrícula
    fila INTEGER,
    columna INTEGER,
    
    -- Datos del árbol
    cultivo_id VARCHAR(50),
    variedad VARCHAR(100),
    patron VARCHAR(100),
    fecha_siembra DATE,
    fecha_registro DATE DEFAULT CURRENT_DATE,
    
    -- Estado actual
    estado_vital VARCHAR(20) DEFAULT 'vivo', -- 'vivo', 'muerto', 'removido', 'nuevo'
    estado_salud VARCHAR(10), -- 'SAL', 'OBS', 'RIE', 'CRI', 'TRA', 'REC'
    etapa_fenologica VARCHAR(10),
    
    -- Metadatos
    registrado_por INTEGER REFERENCES usuarios(id),
    foto_url VARCHAR(255),
    notas TEXT,
    
    -- Índices de calidad
    ultimo_ndvi DECIMAL(5,4),
    fecha_ultimo_ndvi DATE,
    
    -- Timestamps
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Historial de árboles (auditoría de cambios)
CREATE TABLE arboles_historial (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id),
    tipo_cambio VARCHAR(50), -- 'creacion', 'actualizacion_ubicacion', 'cambio_estado', 'remocion'
    fecha_cambio TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    usuario_id INTEGER REFERENCES usuarios(id),
    datos_anteriores JSONB,
    datos_nuevos JSONB,
    observaciones TEXT
);

-- Índices espaciales PostGIS
CREATE INDEX idx_fincas_ubicacion ON fincas USING GIST(ubicacion_centro);
CREATE INDEX idx_fincas_poligono ON fincas USING GIST(poligono_delimitador);
CREATE INDEX idx_sectores_poligono ON sectores USING GIST(poligono);
CREATE INDEX idx_lotes_poligono ON lotes USING GIST(poligono);
CREATE INDEX idx_arboles_ubicacion ON arboles USING GIST(ubicacion);

-- Índices de búsqueda
CREATE INDEX idx_arboles_codigo ON arboles(codigo);
CREATE INDEX idx_arboles_qr ON arboles(qr_code);
CREATE INDEX idx_arboles_lote ON arboles(lote_id);
CREATE INDEX idx_arboles_estado ON arboles(estado_salud);
CREATE INDEX idx_arboles_cuadricula ON arboles(lote_id, fila, columna);
```

#### Sistema de Generación de Códigos de Árbol

**Formato del código:** `{CULTIVO}-{FINCA}-{LOTE}-{SECUENCIA}`

Ejemplo: `AGC-001-A-0234`
- `AGC`: Aguacate
- `001`: Finca ID
- `A`: Lote
- `0234`: Número secuencial del árbol

```typescript
// Función TypeScript para generar código de árbol
interface CodigoArbolParams {
  cultivoAbrev: string;    // 'AGC', 'MAN', 'CIT', etc.
  fincaId: number;
  loteNombre: string;
  secuencia: number;
}

function generarCodigoArbol(params: CodigoArbolParams): string {
  const { cultivoAbrev, fincaId, loteNombre, secuencia } = params;
  
  const fincaPadded = fincaId.toString().padStart(3, '0');
  const secuenciaPadded = secuencia.toString().padStart(4, '0');
  
  return `${cultivoAbrev}-${fincaPadded}-${loteNombre}-${secuenciaPadded}`;
}

// Ejemplo de uso
const codigo = generarCodigoArbol({
  cultivoAbrev: 'AGC',
  fincaId: 1,
  loteNombre: 'A',
  secuencia: 234
});
// Resultado: "AGC-001-A-0234"
```

#### Métodos de Registro de Árboles

| Método | Descripción | Precisión | Uso recomendado |
|--------|-------------|-----------|------------------|
| **GPS Manual** | Operador registra cada árbol con GPS de mano | ±3-5 metros | Fincas pequeñas, registro inicial |
| **GPS RTK** | GPS de precisión centimétrica con corrección | ±2 cm | Fincas grandes, plantaciones nuevas |
| **Importación CSV** | Carga masiva desde archivo con coordenadas | Según fuente | Migración de datos existentes |
| **Generación por Cuadrícula** | Cálculo automático basado en configuración | ±50 cm | Plantaciones regulares, rápida implementación |
| **Imagen de Drone** | Detección automática con IA desde ortofotos | ±10 cm | Actualización masiva, verificación |

#### Formato de Importación Masiva (CSV)

```csv
codigo,finca_id,lote_nombre,fila,columna,latitud,longitud,cultivo,variedad,fecha_siembra
AGC-001-A-0001,1,A,1,1,4.711000,-74.072100,aguacate,Hass,2019-03-15
AGC-001-A-0002,1,A,1,2,4.711010,-74.072090,aguacate,Hass,2019-03-15
AGC-001-A-0003,1,A,1,3,4.711020,-74.072080,aguacate,Hass,2019-03-15
AGC-001-A-0004,1,A,2,1,4.711030,-74.072100,aguacate,Hass,2019-03-15
```

#### Parámetros para Generación Automática por Cuadrícula

```typescript
interface ConfiguracionCuadricula {
  loteId: number;
  
  // Punto de origen (esquina noroeste)
  origen: {
    latitud: number;
    longitud: number;
  };
  
  // Dimensiones
  numeroFilas: number;
  numeroColumnas: number;
  
  // Espaciamiento (metros)
  distanciaEntreFilas: number;      // ej: 7.0
  distanciaEntreArboles: number;    // ej: 5.0
  
  // Orientación
  azimutFilas: number;              // Ángulo respecto al norte (0-360)
  
  // Opciones
  generarQR: boolean;
  metodoNumeracion: 'serpentina' | 'lineal';
}

// Ejemplo de configuración
const config: ConfiguracionCuadricula = {
  loteId: 5,
  origen: {
    latitud: 4.711000,
    longitud: -74.072100
  },
  numeroFilas: 20,
  numeroColumnas: 25,
  distanciaEntreFilas: 7.0,
  distanciaEntreArboles: 5.0,
  azimutFilas: 45,  // Plantación en dirección noreste
  generarQR: true,
  metodoNumeracion: 'serpentina'
};
```

#### Sistema QR/NFC por Árbol

**Diseño de etiqueta física:**

```
┌─────────────────────────────┐
│  FINCA LOS ALAMOS           │
│                             │
│  ███████████████████████    │
│  ██ ▄▄▄▄▄ █▀█▄▄█ ▄▄▄▄▄ ██   │  ← QR Code
│  ██ █   █ █ █▄ █ █   █ ██   │
│  ██ █▄▄▄█ █▄ ▄▀█ █▄▄▄█ ██   │
│  ███████████████████████    │
│                             │
│  Árbol: AGC-001-A-0234      │
│  Lote A - Fila 2 - Col 34   │
│  Aguacate Hass              │
│  🌳 Sembrado: Mar 2019      │
│                             │
│  [Escanear para ver ficha]  │
└─────────────────────────────┘
```

**Datos codificados en QR:**

```json
{
  "tipo": "arbol",
  "codigo": "AGC-001-A-0234",
  "url": "https://app.aptrees.com/tree/AGC-001-A-0234",
  "finca_id": 1,
  "lote": "A",
  "fila": 2,
  "columna": 34
}
```

#### Capas del Mapa Interactivo

| Capa | Descripción | Datos visualizados |
|------|-------------|-------------------|
| **Árboles** | Posición exacta de cada árbol | Puntos con color según estado |
| **Lotes** | Delimitación de lotes | Polígonos con nombres |
| **Sectores** | Áreas de sectores | Polígonos con transparencia |
| **Infraestructura Hídrica** | Tuberías, válvulas, tanques | Líneas y símbolos |
| **Estado de Salud** | Mapa de calor de salud | Gradiente de colores |
| **NDVI** | Índice de vegetación | Gradiente verde-rojo |
| **Imagen Satelital** | Ortofoto de fondo | Raster georreferenciado |
| **Imagen Drone** | Ortofoto detallada | Raster alta resolución |
| **Relieve** | Curvas de nivel | Líneas de elevación |
| **Aplicaciones** | Áreas de aplicación recientes | Polígonos temporales |

#### API de Geolocalización

```yaml
# Endpoints principales

GET /api/v1/fincas/{fincaId}/mapa
  descripcion: Obtener todos los elementos del mapa de una finca
  respuesta:
    finca:
      id: 1
      nombre: "Los Alamos"
      centro: { lat: 4.7110, lon: -74.0721 }
      zoom_inicial: 16
    sectores: [...]
    lotes: [...]
    arboles: [...]
    infraestructura: [...]

GET /api/v1/arboles/{codigo}
  descripcion: Obtener datos completos de un árbol por código
  parametros:
    codigo: "AGC-001-A-0234"
  respuesta:
    arbol_id: 234
    codigo: "AGC-001-A-0234"
    ubicacion: { lat: 4.711045, lon: -74.072089 }
    lote: "A"
    fila: 2
    columna: 34
    estado_salud: "SAL"
    ultimo_ndvi: 0.78
    historial_url: "/api/v1/arboles/234/historial"

POST /api/v1/arboles/registrar
  descripcion: Registrar nuevo árbol
  body:
    lote_id: 5
    codigo: "AGC-001-A-0235"
    ubicacion:
      latitud: 4.711050
      longitud: -74.072085
      precision: 0.03
    metodo_registro: "gps_manual"
    fila: 2
    columna: 35
    fecha_siembra: "2019-03-15"

POST /api/v1/lotes/{loteId}/generar-cuadricula
  descripcion: Generar árboles automáticamente por cuadrícula
  body:
    origen: { lat: 4.711000, lon: -74.072100 }
    filas: 20
    columnas: 25
    distancia_filas: 7.0
    distancia_arboles: 5.0
    azimut: 45

GET /api/v1/arboles/buscar
  descripcion: Buscar árboles por múltiples criterios
  parametros:
    lote_id: 5
    estado_salud: "RIE"
    ndvi_min: 0.5
    ndvi_max: 0.7
    bbox: "4.71,−74.08,4.72,-74.07"  # lat_min,lon_min,lat_max,lon_max
```

#### Integración con Imágenes Satelitales/Drone

```typescript
interface ImagenGeorreferenciada {
  id: number;
  tipo: 'satelital' | 'drone' | 'multiespectral' | 'termica';
  fuente: string;                    // 'Sentinel-2', 'DJI Phantom', etc.
  fecha_captura: Date;
  finca_id: number;
  lotes_cubiertos: number[];
  
  // Georreferenciación
  bbox: {
    latitud_min: number;
    latitud_max: number;
    longitud_min: number;
    longitud_max: number;
  };
  
  // Archivos
  url_ortofoto: string;              // GeoTIFF o PNG georreferenciado
  url_thumbnail: string;
  resolucion_cm_pixel: number;       // ej: 5 cm/pixel
  
  // Metadatos
  bandas: string[];                  // ['R', 'G', 'B', 'NIR', 'Red Edge']
  sistema_coordenadas: string;       // 'EPSG:4326'
  
  // Análisis realizados
  ndvi_calculado: boolean;
  arboles_detectados: boolean;
  numero_arboles_detectados?: number;
}

// Procesamiento automático al subir imagen
interface ProcesamientoImagen {
  imagen_id: number;
  tareas: {
    deteccion_arboles: {
      metodo: 'yolo' | 'mask-rcnn';
      confianza_minima: number;       // 0.8
    };
    calculo_indices: {
      ndvi: boolean;
      ndre: boolean;
      gndvi: boolean;
    };
    alineacion_arboles: {
      tolerancia_metros: number;       // 0.5
      actualizar_ubicaciones: boolean;
    };
  };
}
```

#### Formatos de Exportación

| Formato | Uso | Contenido |
|---------|-----|-----------|
| **GeoJSON** | Sistemas GIS, mapas web | Geometrías + atributos completos |
| **CSV** | Excel, análisis estadístico | Tabla con coordenadas y datos |
| **KML** | Google Earth | Visualización 3D con iconos |
| **Shapefile** | ArcGIS, QGIS | Formato estándar GIS profesional |
| **PDF (Mapa)** | Impresión, reportes | Mapa renderizado con leyenda |

**Ejemplo de exportación GeoJSON:**

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [-74.072100, 4.711000]
      },
      "properties": {
        "codigo": "AGC-001-A-0234",
        "cultivo": "aguacate",
        "variedad": "Hass",
        "estado_salud": "SAL",
        "ultimo_ndvi": 0.78,
        "fecha_siembra": "2019-03-15",
        "fila": 2,
        "columna": 34
      }
    }
  ]
}
```

---

## 📚 Documentos relacionados

- [Representación visual de árboles en el mapa](07-02-vista-cuadricula.md)
- [Seguimiento de salud por ubicación](07-03-salud-fenologia.md)

---

> Navegación: [← Anterior](../06-objetivos-sistema.md) | [📑 Índice](../README.md) | [Siguiente →](07-02-vista-cuadricula.md)
