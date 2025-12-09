## 19. Catálogos y Datos Semilla

> **Scripts SQL para poblar los catálogos del sistema y datos de prueba**

---

### 19.1 Estructura de Datos Iniciales

Diagrama de dependencias y niveles de datos:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      NIVEL PLATAFORMA (Compartido)                      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌──────────────┐   ┌───────────────┐   ┌──────────────────┐          │
│  │   Roles y    │   │    Planes     │   │  Usuarios Admin  │          │
│  │  Permisos    │   │  Suscripción  │   │   Plataforma     │          │
│  └──────────────┘   └───────────────┘   └──────────────────┘          │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    CATÁLOGOS GLOBALES                           │   │
│  ├─────────────────────────────────────────────────────────────────┤   │
│  │  • Cultivos (22)          • Fenología (~70)                     │   │
│  │  • Plagas (~25)           • Enfermedades (~20)                  │   │
│  │  • Labores (35)           • Estados Salud (9)                   │   │
│  │  • Productos (~30)        • Dosis Recomendadas                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        NIVEL TENANT (Demostración)                      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  Tenant: "Finca Demo AgroGrid"                                   │  │
│  │  • Slug: demo                                                    │  │
│  │  • Plan: Professional                                            │  │
│  │  • Suscripción activa                                            │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  Usuarios por Rol (8)                                            │  │
│  │  • Owner (1)    • Manager (1)    • Agrónomo (1)                 │  │
│  │  • Supervisor (1) • Operario (3) • Viewer (1)                   │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  Finca: "La Esperanza"                                           │  │
│  │  • Ubicación: Fresno, Tolima, Colombia                           │  │
│  │  • Área: 50 ha (42 ha cultivada)                                 │  │
│  │  • 4 Sectores: Norte, Sur, Este, Oeste                           │  │
│  │  • 10 Lotes con diferentes cultivos                              │  │
│  │  • 500 árboles de ejemplo (Lote Norte A)                         │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

### 19.2 Catálogo de Cultivos

Tabla principal de cultivos soportados por la plataforma:

```sql
-- Tabla: catalogo_cultivos
CREATE TABLE IF NOT EXISTS catalogo_cultivos (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    nombre_cientifico VARCHAR(150),
    familia VARCHAR(100),
    tipo VARCHAR(50), -- 'frutal_hueso', 'frutal_pepita', 'citrico', 'tropical', 'cafe', 'otro'
    ciclo_anos DECIMAL(4,1),
    descripcion TEXT,
    imagen_url VARCHAR(500),
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- INSERT de cultivos
INSERT INTO catalogo_cultivos (codigo, nombre, nombre_cientifico, familia, tipo, ciclo_anos, descripcion, activo) VALUES
-- Frutales de hueso
('AGU', 'Aguacate', 'Persea americana', 'Lauraceae', 'frutal_hueso', 1.0, 'Fruto de alto valor nutricional y comercial, adaptable a diversos climas', true),
('MAN', 'Mango', 'Mangifera indica', 'Anacardiaceae', 'tropical', 1.0, 'Fruto tropical de gran demanda, resistente y productivo', true),
('DUR', 'Durazno', 'Prunus persica', 'Rosaceae', 'frutal_hueso', 1.0, 'Frutal caducifolio de clima templado, requiere horas frío', true),
('CIR', 'Ciruela', 'Prunus domestica', 'Rosaceae', 'frutal_hueso', 1.0, 'Frutal versátil para consumo fresco e industrial', true),
('CER', 'Cereza', 'Prunus avium', 'Rosaceae', 'frutal_hueso', 1.0, 'Fruto de alto valor, requiere clima frío para floración', true),

-- Frutales de pepita
('MAZ', 'Manzana', 'Malus domestica', 'Rosaceae', 'frutal_pepita', 1.0, 'Frutal templado de amplia distribución mundial', true),
('PER', 'Pera', 'Pyrus communis', 'Rosaceae', 'frutal_pepita', 1.0, 'Frutal de clima templado, requiere polinización cruzada', true),

-- Cítricos
('NAR', 'Naranja', 'Citrus sinensis', 'Rutaceae', 'citrico', 1.0, 'Cítrico de mayor producción mundial, múltiples variedades', true),
('LIM', 'Limón', 'Citrus limon', 'Rutaceae', 'citrico', 1.0, 'Cítrico ácido de alta demanda, floración continua', true),
('MND', 'Mandarina', 'Citrus reticulata', 'Rutaceae', 'citrico', 1.0, 'Cítrico de fácil pelado, sabor dulce', true),
('TOR', 'Toronja', 'Citrus paradisi', 'Rutaceae', 'citrico', 1.0, 'Cítrico de sabor amargo-dulce, alto contenido vitamina C', true),
('LMA', 'Lima', 'Citrus aurantifolia', 'Rutaceae', 'citrico', 1.0, 'Cítrico ácido, pequeño, aromático', true),

-- Tropicales
('PAP', 'Papaya', 'Carica papaya', 'Caricaceae', 'tropical', 0.75, 'Frutal herbáceo de rápido crecimiento y producción', true),
('GUA', 'Guayaba', 'Psidium guajava', 'Myrtaceae', 'tropical', 1.0, 'Frutal aromático rico en vitamina C', true),
('MAR', 'Maracuyá', 'Passiflora edulis', 'Passifloraceae', 'tropical', 0.5, 'Enredadera frutal de sabor intenso', true),
('PIT', 'Pitahaya', 'Hylocereus undatus', 'Cactaceae', 'tropical', 1.0, 'Cactácea frutal de alto valor ornamental y comercial', true),
('BAN', 'Banano', 'Musa × paradisiaca', 'Musaceae', 'tropical', 0.75, 'Herbácea frutal de producción continua', true),

-- Otros frutales
('UVA', 'Uva', 'Vitis vinifera', 'Vitaceae', 'otro', 1.0, 'Frutal enredadera para mesa y vino', true),
('OLI', 'Olivo', 'Olea europaea', 'Oleaceae', 'otro', 1.0, 'Frutal mediterráneo para aceitunas y aceite', true),
('NUE', 'Nuez', 'Juglans regia', 'Juglandaceae', 'otro', 1.0, 'Frutal de nuez comestible de alto valor', true),
('ALM', 'Almendra', 'Prunus dulcis', 'Rosaceae', 'otro', 1.0, 'Frutal de nuez oleaginosa de clima seco', true),

-- Café y cacao
('CAF', 'Café', 'Coffea arabica', 'Rubiaceae', 'cafe', 1.0, 'Arbusto productor de grano de café, alta altitud', true),
('CAC', 'Cacao', 'Theobroma cacao', 'Malvaceae', 'tropical', 1.0, 'Árbol productor de cacao, requiere sombra y humedad', true);
```


---

### 19.3 Etapas Fenológicas por Cultivo

Tabla de etapas de desarrollo para los principales cultivos:

```sql
-- Tabla: catalogo_fenologia
CREATE TABLE IF NOT EXISTS catalogo_fenologia (
    id SERIAL PRIMARY KEY,
    cultivo_id INTEGER REFERENCES catalogo_cultivos(id) ON DELETE CASCADE,
    codigo VARCHAR(20) NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    orden INTEGER NOT NULL,
    duracion_dias_min INTEGER,
    duracion_dias_max INTEGER,
    color VARCHAR(7),
    emoji VARCHAR(10),
    cuidados JSONB, -- Array de cuidados necesarios
    indicadores JSONB, -- Array de indicadores visuales
    activo BOOLEAN DEFAULT true,
    UNIQUE(cultivo_id, codigo)
);

-- Etapas fenológicas: AGUACATE (9 etapas)
INSERT INTO catalogo_fenologia (cultivo_id, codigo, nombre, descripcion, orden, duracion_dias_min, duracion_dias_max, color, emoji, cuidados, indicadores) 
SELECT id, 'REP', 'Reposo Vegetativo', 'Periodo de menor actividad fisiológica, acumulación de reservas', 1, 30, 60, '#9CA3AF', '😴',
    '["Reducir riego", "Evaluación sanitaria", "Poda de formación si es necesario"]'::jsonb,
    '["Crecimiento vegetativo mínimo", "Hojas maduras", "Sin flores"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'BRO', 'Brotación', 'Aparición de nuevos brotes vegetativos', 2, 15, 30, '#84CC16', '🌱',
    '["Aplicación foliar de nutrientes", "Control preventivo de trips", "Riego moderado"]'::jsonb,
    '["Brotes nuevos color verde claro", "Hojas tiernas en desarrollo"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'VEG', 'Crecimiento Vegetativo', 'Desarrollo activo de follaje y ramas', 3, 45, 90, '#22C55E', '🌿',
    '["Fertilización nitrogenada", "Riego regular", "Control de plagas foliares"]'::jsonb,
    '["Crecimiento rápido", "Hojas verde intenso", "Alargamiento de ramas"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'IFL', 'Inducción Floral', 'Diferenciación de yemas florales', 4, 30, 45, '#F59E0B', '🔶',
    '["Reducción de nitrógeno", "Aplicación de K y P", "Estrés hídrico controlado"]'::jsonb,
    '["Yemas florales visibles", "Reducción de crecimiento vegetativo"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'FLO', 'Floración', 'Apertura de flores y polinización', 5, 20, 40, '#EAB308', '🌸',
    '["Control de trips", "Evitar aplicaciones agresivas", "Riego ligero", "Favorecer polinizadores"]'::jsonb,
    '["Panículas florales abiertas", "Abejas activas", "Aroma característico"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'CUA', 'Cuajado', 'Fijación inicial del fruto', 6, 15, 30, '#10B981', '🟢',
    '["Aplicación de Ca y B", "Riego constante", "Evitar estrés", "Control de aborto de fruto"]'::jsonb,
    '["Frutos pequeños (tamaño arveja)", "Caída natural de flores no fecundadas"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'DES', 'Desarrollo de Fruto', 'Crecimiento y llenado del fruto', 7, 120, 180, '#06B6D4', '🥑',
    '["Fertilización completa NPK", "Riego abundante", "Monitoreo de plagas del fruto"]'::jsonb,
    '["Frutos aumentan tamaño progresivamente", "Color verde oscuro", "Peso creciente"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'MAD', 'Maduración', 'Acumulación de aceites y azúcares', 8, 30, 60, '#8B5CF6', '🟣',
    '["Reducir riego", "Aplicación de K", "Preparar cosecha"]'::jsonb,
    '["Fruto alcanza tamaño final", "Piel puede cambiar tonalidad", "Firmeza característica"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU'
UNION ALL
SELECT id, 'COS', 'Cosecha', 'Recolección del fruto', 9, 15, 30, '#EC4899', '🧺',
    '["Cosecha escalonada", "Manejo cuidadoso", "Post-cosecha rápida"]'::jsonb,
    '["Fruto desprende con facilidad", "Peso y materia seca óptimos"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'AGU';

-- Etapas fenológicas: MANGO (9 etapas similares)
INSERT INTO catalogo_fenologia (cultivo_id, codigo, nombre, descripcion, orden, duracion_dias_min, duracion_dias_max, color, emoji, cuidados, indicadores) 
SELECT id, 'REP', 'Reposo Vegetativo', 'Periodo de descanso antes de floración', 1, 30, 60, '#9CA3AF', '😴',
    '["Poda sanitaria", "Aplicación de inductores florales", "Riego reducido"]'::jsonb,
    '["Hojas maduras", "Sin crecimiento activo"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'BRO', 'Brotación', 'Emisión de brotes nuevos', 2, 15, 25, '#84CC16', '🌱',
    '["Fertilización foliar", "Control de trips y antracnosis", "Riego moderado"]'::jsonb,
    '["Brotes tiernos rojizos", "Hojas nuevas"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'IFL', 'Inducción Floral', 'Formación de yemas florales', 3, 20, 40, '#F59E0B', '🔶',
    '["Aplicación de nitrato de potasio", "Estrés hídrico leve", "Control fitosanitario"]'::jsonb,
    '["Yemas florales hinchadas", "Diferenciación visible"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'FLO', 'Floración', 'Apertura de panículas florales', 4, 25, 40, '#EAB308', '🌸',
    '["Evitar fungicidas agresivos", "Control de oídio", "Riego ligero"]'::jsonb,
    '["Panículas abiertas", "Flores blanco-rosadas", "Fragancia"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'CUA', 'Cuajado', 'Fijación inicial de frutos', 5, 20, 30, '#10B981', '🟢',
    '["Aplicación de Ca-B", "Riego constante", "Control de insectos chupadores"]'::jsonb,
    '["Frutos pequeños (tamaño lenteja)", "Caída natural de flores"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'DES', 'Desarrollo de Fruto', 'Crecimiento del fruto hasta tamaño comercial', 6, 90, 120, '#06B6D4', '🥭',
    '["Fertilización NPK", "Riego abundante", "Control de mosca de la fruta", "Raleo si es necesario"]'::jsonb,
    '["Frutos crecen rápidamente", "Color verde intenso"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'MAD', 'Maduración', 'Cambio de color y acumulación de azúcares', 7, 20, 40, '#8B5CF6', '🟣',
    '["Reducir riego", "Aplicación de K", "Monitoreo de madurez"]'::jsonb,
    '["Cambio de color verde a amarillo/rojo", "Aroma característico", "Ablandamiento"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'COS', 'Cosecha', 'Recolección de frutos', 8, 15, 30, '#EC4899', '🧺',
    '["Cosecha con pedúnculo", "Evitar daños mecánicos", "Clasificación inmediata"]'::jsonb,
    '["Fruto alcanza color y firmeza deseados", "Se desprende fácilmente"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN'
UNION ALL
SELECT id, 'REG', 'Regeneración', 'Nuevo crecimiento vegetativo post-cosecha', 9, 30, 60, '#22C55E', '🌿',
    '["Fertilización nitrogenada", "Riego regular", "Poda de limpieza"]'::jsonb,
    '["Nuevos brotes", "Recuperación del árbol"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'MAN';

-- Etapas fenológicas: CÍTRICOS (Naranja, Limón, Mandarina) - 8 etapas
INSERT INTO catalogo_fenologia (cultivo_id, codigo, nombre, descripcion, orden, duracion_dias_min, duracion_dias_max, color, emoji, cuidados, indicadores)
SELECT c.id, 'REP', 'Reposo', 'Periodo de menor actividad', 1, 30, 60, '#9CA3AF', '😴',
    '["Evaluación sanitaria", "Poda si es necesario", "Riego reducido"]'::jsonb,
    '["Hojas maduras", "Sin floración activa"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'BRO', 'Brotación', 'Emisión de nuevos brotes', 2, 20, 40, '#84CC16', '🌱',
    '["Fertilización foliar", "Control de minador", "Riego moderado"]'::jsonb,
    '["Brotes tiernos", "Hojas nuevas brillantes"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'FLO', 'Floración', 'Producción de flores (azahares)', 3, 20, 40, '#EAB308', '🌸',
    '["Evitar estrés hídrico", "Control preventivo HLB", "Protección de polinizadores"]'::jsonb,
    '["Flores blancas fragantes", "Abejas activas"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'CUA', 'Cuajado', 'Fijación de frutos', 4, 20, 30, '#10B981', '🟢',
    '["Riego constante", "Aplicación de Ca-B", "Evitar temperaturas extremas"]'::jsonb,
    '["Frutos pequeños formándose", "Caída natural de flores"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'DES1', 'Desarrollo Inicial', 'Crecimiento rápido del fruto', 5, 60, 90, '#3B82F6', '🔵',
    '["Fertilización NPK", "Riego abundante", "Control de plagas"]'::jsonb,
    '["Frutos verde oscuro", "Tamaño aumentando"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'DES2', 'Desarrollo Final', 'Fruto alcanza tamaño definitivo', 6, 60, 90, '#06B6D4', '🍊',
    '["Mantener riego", "Aplicación de K", "Monitoreo de madurez"]'::jsonb,
    '["Fruto tamaño completo", "Piel más delgada"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'MAD', 'Maduración', 'Cambio de color y acumulación de azúcares', 7, 30, 60, '#F97316', '🟠',
    '["Reducir riego", "Aplicación de K", "Preparar cosecha"]'::jsonb,
    '["Cambio de color verde a naranja/amarillo", "Aumento de °Brix"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND')
UNION ALL
SELECT c.id, 'COS', 'Cosecha', 'Recolección de frutos', 8, 20, 40, '#EC4899', '🧺',
    '["Cosecha cuidadosa", "Clasificación por calibre", "Manejo post-cosecha"]'::jsonb,
    '["Color característico alcanzado", "Firmeza y sabor óptimos"]'::jsonb
FROM catalogo_cultivos c WHERE c.codigo IN ('NAR', 'LIM', 'MND');

-- Etapas fenológicas: CAFÉ (7 etapas)
INSERT INTO catalogo_fenologia (cultivo_id, codigo, nombre, descripcion, orden, duracion_dias_min, duracion_dias_max, color, emoji, cuidados, indicadores)
SELECT id, 'ZOC', 'Zoca / Poda Renovación', 'Renovación del cafetal', 1, 90, 120, '#9CA3AF', '✂️',
    '["Poda drástica", "Fertilización post-poda", "Control de malezas"]'::jsonb,
    '["Tallos cortados", "Nuevos brotes basales"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF'
UNION ALL
SELECT id, 'VEG', 'Crecimiento Vegetativo', 'Desarrollo de ramas y follaje', 2, 120, 180, '#22C55E', '🌿',
    '["Fertilización nitrogenada", "Sombra regulada", "Control de malezas"]'::jsonb,
    '["Crecimiento activo", "Hojas verde intenso"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF'
UNION ALL
SELECT id, 'FLO', 'Floración', 'Producción de flores blancas', 3, 10, 20, '#F8F8FF', '🌸',
    '["Evitar aplicaciones durante floración", "Control de roya preventivo"]'::jsonb,
    '["Flores blancas abundantes", "Aroma intenso"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF'
UNION ALL
SELECT id, 'CUA', 'Cuajado', 'Formación inicial de cerezas', 4, 20, 30, '#10B981', '🟢',
    '["Riego si es época seca", "Fertilización completa"]'::jsonb,
    '["Cerezas pequeñas verdes"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF'
UNION ALL
SELECT id, 'LLE', 'Llenado de Grano', 'Crecimiento de la cereza', 5, 120, 150, '#3B82F6', '��',
    '["Riego regular", "Control de broca", "Fertilización potásica"]'::jsonb,
    '["Cerezas verdes aumentando tamaño"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF'
UNION ALL
SELECT id, 'MAD', 'Maduración', 'Cereza cambia a color rojo', 6, 30, 45, '#DC2626', '🔴',
    '["Monitoreo diario", "Control de broca", "Preparar cosecha"]'::jsonb,
    '["Cerezas rojas (maduras) o amarillas según variedad"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF'
UNION ALL
SELECT id, 'COS', 'Cosecha', 'Recolección de cerezas maduras', 7, 60, 90, '#EC4899', '🧺',
    '["Cosecha selectiva (solo maduros)", "Beneficio rápido", "Secado apropiado"]'::jsonb,
    '["Recolección manual o mecánica", "Clasificación de cerezas"]'::jsonb
FROM catalogo_cultivos WHERE codigo = 'CAF';
```


---

### 19.4 Catálogo de Plagas

Registro de plagas comunes por cultivo:

```sql
-- Tabla ya definida en schema (ver doc 04)
-- Aquí solo los INSERTs

-- PLAGAS GENERALES (afectan múltiples cultivos)
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('GEN-PL-001', 'Araña Roja', 'Tetranychus urticae', 'plaga', 'acaro', 
 'Ácaro polífago que ataca gran variedad de cultivos', 
 'Amarillamiento de hojas, punteado clorótico, telarañas finas en el envés',
 'Clima seco y cálido, baja humedad relativa, polvo en follaje',
 '{"culturales": ["Riego por aspersión", "Eliminar malezas hospederas"], "biologicos": ["Depredadores: Phytoseiulus persimilis", "Hongos entomopatógenos"], "quimicos": ["Abamectina", "Spiromesifen", "Azufre micronizado"]}'::jsonb, true),

('GEN-PL-002', 'Mosca Blanca', 'Bemisia tabaci', 'plaga', 'insecto',
 'Insecto chupador vector de virus', 
 'Fumagina, amarillamiento, debilitamiento general, transmisión de virus',
 'Temperaturas cálidas, alta humedad, cultivos densos',
 '{"culturales": ["Trampas amarillas", "Eliminar malezas"], "biologicos": ["Encarsia formosa", "Beauveria bassiana"], "quimicos": ["Imidacloprid", "Spiromesifen", "Aceites agrícolas"]}'::jsonb, true),

('GEN-PL-003', 'Trips', 'Frankliniella spp.', 'plaga', 'insecto',
 'Insectos pequeños que raspan tejidos vegetales',
 'Plateado de hojas, deformaciones, cicatrices en frutos',
 'Clima seco, floración abundante',
 '{"culturales": ["Trampas azules", "Control de malezas"], "biologicos": ["Orius spp.", "Amblyseius swirskii"], "quimicos": ["Spinosad", "Abamectina", "Formetanato"]}'::jsonb, true),

('GEN-PL-004', 'Pulgones', 'Aphis spp.', 'plaga', 'insecto',
 'Insectos chupadores que deforman brotes',
 'Hojas enrolladas, melaza, fumagina, transmisión de virus',
 'Brotes tiernos, clima templado, exceso de nitrógeno',
 '{"culturales": ["Evitar exceso de N", "Eliminar colonias manualmente"], "biologicos": ["Chrysoperla", "Coccinélidos", "Aphidius"], "quimicos": ["Imidacloprid", "Pirimicarb", "Jabón potásico"]}'::jsonb, true),

('GEN-PL-005', 'Cochinillas', 'Coccidae', 'plaga', 'insecto',
 'Insectos chupadores protegidos por escudo ceroso',
 'Melaza abundante, fumagina, debilitamiento del árbol',
 'Condiciones de baja ventilación, alta humedad',
 '{"culturales": ["Poda de aireación", "Lavados con agua a presión"], "biologicos": ["Metaphycus helvolus", "Criptolaemus"], "quimicos": ["Aceite mineral", "Spirotetramat", "Buprofezin"]}'::jsonb, true);

-- PLAGAS ESPECÍFICAS DE AGUACATE
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('AGU-PL-001', 'Trips del Aguacate', 'Scirtothrips perseae', 'plaga', 'insecto',
 'Plaga específica del aguacate que daña frutos',
 'Cicatrices bronceadas en frutos, deformaciones, daño cosmético severo',
 'Floración y desarrollo de fruto, clima seco',
 '{"culturales": ["Monitoreo en floración", "Eliminar frutos dañados"], "biologicos": ["Depredadores naturales Orius"], "quimicos": ["Spinosad", "Abamectina", "Formetanato"]}'::jsonb, true),

('AGU-PL-002', 'Barrenador del Hueso', 'Heilipus lauri', 'plaga', 'insecto',
 'Escarabajo que perfora frutos y deposita huevos',
 'Galerías en pulpa y semilla, pudrición del fruto, caída prematura',
 'Presencia de frutos, épocas de lluvia',
 '{"culturales": ["Recolección de frutos caídos", "Trampas con frutos"], "biologicos": ["Beauveria bassiana"], "quimicos": ["Clorpirifos en tronco", "Cipermetrinas"]}'::jsonb, true),

('AGU-PL-003', 'Barrenador de Ramas', 'Copturus aguacatae', 'plaga', 'insecto',
 'Escarabajo que perfora ramas tiernas causando muerte regresiva',
 'Ramas secas, aserrín en perforaciones, muerte de puntas',
 'Ramas tiernas, árboles debilitados',
 '{"culturales": ["Poda de ramas afectadas", "Destrucción de material"], "biologicos": ["Hongos entomopatógenos"], "quimicos": ["Aplicaciones dirigidas de Clorpirifos"]}'::jsonb, true),

('AGU-PL-004', 'Mosca del Fruto', 'Anastrepha spp.', 'plaga', 'insecto',
 'Mosca que deposita huevos en frutos maduros',
 'Picaduras en piel, larvas en pulpa, pudrición interna',
 'Frutos en maduración, temperaturas cálidas',
 '{"culturales": ["Trampeo masivo", "Recolección sanitaria", "Proteína hidrolizada"], "biologicos": ["Liberación de machos estériles"], "quimicos": ["Spinosad + cebo proteico", "Malatión"]}'::jsonb, true);

-- PLAGAS ESPECÍFICAS DE CÍTRICOS
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('CIT-PL-001', 'Minador de la Hoja', 'Phyllocnistis citrella', 'plaga', 'insecto',
 'Larva que mina hojas jóvenes dejando galerías',
 'Galerías serpenteantes plateadas, hojas enrolladas, deformadas',
 'Brotes tiernos abundantes, clima cálido',
 '{"culturales": ["Evitar exceso de nitrógeno", "Sincronización de podas"], "biologicos": ["Ageniaspis citricola"], "quimicos": ["Abamectina", "Imidacloprid sistémico"]}'::jsonb, true),

('CIT-PL-002', 'Psílido Asiático de los Cítricos', 'Diaphorina citri', 'plaga', 'insecto',
 'Vector del HLB (Huanglongbing), enfermedad mortal',
 'Brotes deformados, ninfas con secreción cerosa, transmisión de bacteria',
 'Brotes tiernos, clima subtropical',
 '{"culturales": ["Eliminación de plantas enfermas", "Monitoreo intensivo"], "biologicos": ["Tamarixia radiata"], "quimicos": ["Imidacloprid", "Thiamethoxam", "Dimetoato"]}'::jsonb, true),

('CIT-PL-003', 'Ácaro del Tostado', 'Phyllocoptruta oleivora', 'plaga', 'acaro',
 'Ácaro que daña frutos causando oxidación de la piel',
 'Bronceado de frutos, piel áspera, pérdida de valor comercial',
 'Clima cálido y seco, frutos en desarrollo',
 '{"culturales": ["Monitoreo regular"], "biologicos": ["Ácaros depredadores"], "quimicos": ["Azufre mojable", "Abamectina", "Fenazaquin"]}'::jsonb, true),

('CIT-PL-004', 'Escama Roja de California', 'Aonidiella aurantii', 'plaga', 'insecto',
 'Cochinilla que daña frutos y ramas',
 'Escamas rojas circulares, decoloración, debilitamiento',
 'Alta densidad de plantación, falta de aireación',
 '{"culturales": ["Poda de ventilación"], "biologicos": ["Aphytis melinus"], "quimicos": ["Aceite mineral + Buprofezin", "Spirotetramat"]}'::jsonb, true);

-- PLAGAS ESPECÍFICAS DE MANGO
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('MAN-PL-001', 'Mosca de la Fruta', 'Anastrepha obliqua', 'plaga', 'insecto',
 'Mosca específica del mango',
 'Picaduras en fruto, larvas en pulpa, pudrición',
 'Frutos en desarrollo y maduración',
 '{"culturales": ["Trampeo masivo", "Recolección sanitaria"], "biologicos": ["Técnica del insecto estéril"], "quimicos": ["Spinosad + proteína", "Malatión"]}'::jsonb, true),

('MAN-PL-002', 'Trips del Mango', 'Scirtothrips dorsalis', 'plaga', 'insecto',
 'Trips que daña flores y frutos pequeños',
 'Cicatrices en frutos, deformaciones, reducción de cuajado',
 'Floración, clima seco',
 '{"culturales": ["Monitoreo en floración"], "biologicos": ["Depredadores naturales"], "quimicos": ["Spinosad", "Acetamiprid"]}'::jsonb, true),

('MAN-PL-003', 'Escama Blanca', 'Aulacaspis tubercularis', 'plaga', 'insecto',
 'Cochinilla que ataca hojas, ramas y frutos',
 'Escamas blancas en hojas y frutos, debilitamiento',
 'Baja ventilación, alta humedad',
 '{"culturales": ["Poda de ventilación"], "biologicos": ["Chilocorus spp."], "quimicos": ["Aceite mineral", "Spirotetramat"]}'::jsonb, true);

-- PLAGAS ESPECÍFICAS DE CAFÉ
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('CAF-PL-001', 'Broca del Café', 'Hypothenemus hampei', 'plaga', 'insecto',
 'Escarabajo que perfora granos de café',
 'Perforaciones en cerezas, granos dañados, pérdida de calidad',
 'Cerezas en maduración, alta humedad',
 '{"culturales": ["Re-re (repase y repique)", "Pepena de suelo"], "biologicos": ["Beauveria bassiana"], "quimicos": ["Endosulfán (en algunos países)"]}'::jsonb, true),

('CAF-PL-002', 'Minador de la Hoja del Café', 'Leucoptera coffeella', 'plaga', 'insecto',
 'Larva que mina hojas causando defoliación',
 'Minas en hojas, manchas necróticas, caída de hojas',
 'Época seca, alta insolación',
 '{"culturales": ["Regulación de sombra", "Nutrición adecuada"], "biologicos": ["Avispas parasitoides"], "quimicos": ["Control generalmente no necesario"]}'::jsonb, true);
```


---

### 19.5 Catálogo de Enfermedades

Registro de enfermedades comunes (tipo='enfermedad' en catalogo_plagas):

```sql
-- ENFERMEDADES GENERALES
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('GEN-EN-001', 'Antracnosis', 'Colletotrichum spp.', 'enfermedad', 'hongo',
 'Enfermedad fúngica que causa lesiones necróticas',
 'Manchas negras hundidas en frutos, hojas y ramas',
 'Alta humedad, temperaturas medias (20-25°C), heridas en tejidos',
 '{"culturales": ["Poda de ventilación", "Eliminar restos vegetales"], "biologicos": ["Trichoderma spp."], "quimicos": ["Mancozeb", "Cobre", "Azoxystrobin"]}'::jsonb, true),

('GEN-EN-002', 'Botrytis (Moho Gris)', 'Botrytis cinerea', 'enfermedad', 'hongo',
 'Hongo que pudre flores, frutos y tejidos tiernos',
 'Moho grisáceo en flores y frutos, pudrición blanda',
 'Alta humedad, temperaturas frescas, tejidos senescentes',
 '{"culturales": ["Ventilación", "Eliminar tejidos infectados"], "biologicos": ["Bacillus subtilis"], "quimicos": ["Iprodione", "Pirimetanil", "Fenhexamid"]}'::jsonb, true),

('GEN-EN-003', 'Oídio (Cenicilla)', 'Oidium spp.', 'enfermedad', 'hongo',
 'Hongo que forma polvo blanco en hojas',
 'Polvo blanco harinoso en hojas y brotes, deformaciones',
 'Clima seco, sombrío, temperaturas moderadas',
 '{"culturales": ["Poda de aireación", "Evitar exceso de sombra"], "biologicos": ["Azufre elemental"], "quimicos": ["Azufre mojable", "Trifloxystrobin", "Penconazol"]}'::jsonb, true),

('GEN-EN-004', 'Fusarium (Marchitez)', 'Fusarium spp.', 'enfermedad', 'hongo',
 'Hongo del suelo que causa marchitez vascular',
 'Amarillamiento, marchitez, oscurecimiento vascular',
 'Suelos mal drenados, heridas en raíces, estrés',
 '{"culturales": ["Drenaje adecuado", "Rotación", "Variedades resistentes"], "biologicos": ["Trichoderma harzianum"], "quimicos": ["Sin control químico eficaz una vez establecido"]}'::jsonb, true);

-- ENFERMEDADES ESPECÍFICAS DE AGUACATE
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('AGU-EN-001', 'Phytophthora (Pudrición Radicular)', 'Phytophthora cinnamomi', 'enfermedad', 'hongo',
 'Oomiceto que causa pudrición de raíces y cuello',
 'Marchitez, amarillamiento, decaimiento general, muerte del árbol',
 'Suelos pesados, mal drenaje, encharcamiento, pH bajo',
 '{"culturales": ["Drenaje excelente", "Portainjertos tolerantes", "Evitar encharcamiento"], "biologicos": ["Trichoderma", "Bacterias antagonistas"], "quimicos": ["Fosetil Aluminio", "Metalaxil", "Oxathiapiprolin"]}'::jsonb, true),

('AGU-EN-002', 'Antracnosis del Aguacate', 'Colletotrichum gloeosporioides', 'enfermedad', 'hongo',
 'Mancha negra en frutos post-cosecha',
 'Manchas negras circulares hundidas que se expanden en fruto maduro',
 'Alta humedad durante floración y desarrollo, heridas',
 '{"culturales": ["Cosecha oportuna", "Manejo post-cosecha"], "biologicos": ["Trichoderma"], "quimicos": ["Cobre", "Mancozeb", "Azoxystrobin"]}'::jsonb, true),

('AGU-EN-003', 'Roña del Aguacate', 'Sphaceloma perseae', 'enfermedad', 'hongo',
 'Costras en frutos, hojas y ramas jóvenes',
 'Lesiones corchosas elevadas, deformaciones de fruto',
 'Lluvia en desarrollo del fruto, tejidos jóvenes',
 '{"culturales": ["Variedades resistentes", "Poda sanitaria"], "biologicos": ["Control biológico limitado"], "quimicos": ["Cobre", "Mancozeb en preventivo"]}'::jsonb, true),

('AGU-EN-004', 'Verticillium', 'Verticillium dahliae', 'enfermedad', 'hongo',
 'Marchitez vascular progresiva',
 'Marchitez de un lado del árbol, oscurecimiento vascular',
 'Suelos infectados, estrés hídrico',
 '{"culturales": ["Sin cultivos susceptibles previos", "Buena nutrición"], "biologicos": ["Trichoderma preventivo"], "quimicos": ["Sin control químico eficaz"]}'::jsonb, true);

-- ENFERMEDADES ESPECÍFICAS DE CÍTRICOS
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('CIT-EN-001', 'HLB (Huanglongbing)', 'Candidatus Liberibacter asiaticus', 'enfermedad', 'bacteria',
 'Enfermedad bacteriana mortal transmitida por psílido',
 'Brotes amarillos asimétricos, frutos deformes y amargos, muerte del árbol',
 'Presencia del psílido vector, clima subtropical',
 '{"culturales": ["Erradicación de árboles infectados", "Material certificado", "Control del vector"], "biologicos": ["No existe"], "quimicos": ["No existe cura - solo manejo del vector"]}'::jsonb, true),

('CIT-EN-002', 'Gomosis (Phytophthora)', 'Phytophthora spp.', 'enfermedad', 'hongo',
 'Pudrición del pie y raíces con exudado gomoso',
 'Exudado gomoso en tronco, clorosis, muerte de ramas',
 'Mal drenaje, encharcamiento, heridas en tronco',
 '{"culturales": ["Drenaje", "Portainjertos tolerantes", "Pintar tronco con cal"], "biologicos": ["Trichoderma"], "quimicos": ["Fosetil Aluminio", "Metalaxil"]}'::jsonb, true),

('CIT-EN-003', 'Cancrosis Cítrica', 'Xanthomonas citri', 'enfermedad', 'bacteria',
 'Lesiones elevadas corchosas en hojas, ramas y frutos',
 'Lesiones circulares elevadas con halo amarillo',
 'Lluvia, viento, temperaturas cálidas, heridas',
 '{"culturales": ["Material certificado", "Cortinas rompevientos", "Eliminar material infectado"], "biologicos": ["No disponible"], "quimicos": ["Cobre preventivo"]}'::jsonb, true),

('CIT-EN-004', 'Melanosis', 'Diaporthe citri', 'enfermedad', 'hongo',
 'Manchas rugosas en frutos y hojas',
 'Pequeñas protuberancias marrones, piel rugosa',
 'Lluvia en primavera, madera muerta en árbol',
 '{"culturales": ["Poda de madera muerta"], "biologicos": ["Control biológico limitado"], "quimicos": ["Cobre en floración"]}'::jsonb, true);

-- ENFERMEDADES ESPECÍFICAS DE MANGO
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('MAN-EN-001', 'Antracnosis del Mango', 'Colletotrichum gloeosporioides', 'enfermedad', 'hongo',
 'Mancha negra en flores, hojas y frutos',
 'Manchas negras en flores, lesiones en frutos, gomosis',
 'Alta humedad, lluvia durante floración',
 '{"culturales": ["Poda de ventilación", "Eliminar restos"], "biologicos": ["Trichoderma"], "quimicos": ["Mancozeb", "Azoxystrobin", "Benomyl"]}'::jsonb, true),

('MAN-EN-002', 'Oídio del Mango', 'Oidium mangiferae', 'enfermedad', 'hongo',
 'Polvo blanco en inflorescencias y brotes',
 'Polvo blanco en panículas, aborto floral, deformaciones',
 'Clima seco, temperaturas moderadas, sombrío',
 '{"culturales": ["Poda de aireación"], "biologicos": ["Azufre elemental"], "quimicos": ["Azufre mojable", "Trifloxystrobin"]}'::jsonb, true),

('MAN-EN-003', 'Muerte Regresiva', 'Lasiodiplodia theobromae', 'enfermedad', 'hongo',
 'Muerte descendente de ramas desde las puntas',
 'Secamiento de puntas, gomosis, muerte de ramas',
 'Estrés hídrico, heridas de poda, alta temperatura',
 '{"culturales": ["Evitar estrés", "Poda sanitaria", "Desinfección de herramientas"], "biologicos": ["Trichoderma"], "quimicos": ["Tiabendazol", "Benomyl"]}'::jsonb, true);

-- ENFERMEDADES ESPECÍFICAS DE CAFÉ
INSERT INTO catalogo_plagas (codigo, nombre_comun, nombre_cientifico, tipo, categoria, descripcion, sintomas, condiciones_favorables, metodos_control, activo) VALUES
('CAF-EN-001', 'Roya del Café', 'Hemileia vastatrix', 'enfermedad', 'hongo',
 'Pústulas anaranjadas en envés de hojas',
 'Manchas amarillas con pústulas naranjas, defoliación severa',
 'Alta humedad, temperaturas 20-25°C, sombra excesiva',
 '{"culturales": ["Variedades resistentes", "Regulación de sombra", "Nutrición balanceada"], "biologicos": ["Bacillus subtilis"], "quimicos": ["Oxicloruro de cobre", "Triazoles", "Estrobilurinas"]}'::jsonb, true),

('CAF-EN-002', 'Ojo de Gallo', 'Mycena citricolor', 'enfermedad', 'hongo',
 'Manchas circulares con centro claro en hojas',
 'Lesiones concéntricas tipo "ojo", defoliación',
 'Alta humedad, temperaturas frescas, altitudes altas',
 '{"culturales": ["Regulación de sombra", "Poda"], "biologicos": ["Control biológico limitado"], "quimicos": ["Cobre", "Mancozeb"]}'::jsonb, true),

('CAF-EN-003', 'Mal de Hilachas', 'Pellicularia koleroga', 'enfermedad', 'hongo',
 'Micelio blanco que cubre hojas y ramas',
 'Telaraña blanca en ramas, muerte de tejidos',
 'Alta humedad, sombra excesiva, falta de ventilación',
 '{"culturales": ["Poda de ventilación", "Reducir sombra"], "biologicos": ["Trichoderma"], "quimicos": ["Cobre"]}'::jsonb, true);
```


---

### 19.6 Tipos de Tareas y Labores

Catálogo de labores agrícolas del sistema:

```sql
-- Tabla: catalogo_labores
CREATE TABLE IF NOT EXISTS catalogo_labores (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    categoria VARCHAR(50), -- 'inspeccion', 'aplicacion', 'poda', 'fertilizacion', 'riego', 'cosecha', 'mantenimiento', 'malezas', 'otro'
    descripcion TEXT,
    duracion_estimada_hrs DECIMAL(5,2),
    requiere_insumos BOOLEAN DEFAULT false,
    frecuencia_tipica VARCHAR(50), -- 'diaria', 'semanal', 'quincenal', 'mensual', 'bimestral', 'trimestral', 'semestral', 'anual', 'segun_necesidad'
    prioridad_default VARCHAR(20), -- 'baja', 'media', 'alta', 'urgente'
    aplica_a JSONB, -- Array de tipos de cultivo o 'todos'
    activo BOOLEAN DEFAULT true
);

-- INSPECCIONES (5)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('INS-GEN', 'Inspección General', 'inspeccion', 'Revisión visual del estado general de árboles y lote', 2.0, false, 'semanal', 'media', '["todos"]'::jsonb, true),
('INS-FIT', 'Inspección Fitosanitaria', 'inspeccion', 'Detección de plagas y enfermedades', 3.0, false, 'semanal', 'alta', '["todos"]'::jsonb, true),
('INS-FEN', 'Inspección Fenológica', 'inspeccion', 'Evaluación de etapa fenológica y desarrollo', 2.5, false, 'quincenal', 'media', '["todos"]'::jsonb, true),
('INS-RIE', 'Inspección de Riego', 'inspeccion', 'Verificación del sistema de riego y humedad del suelo', 1.5, false, 'semanal', 'alta', '["todos"]'::jsonb, true),
('INS-COS', 'Inspección Pre-Cosecha', 'inspeccion', 'Evaluación de madurez y estimación de cosecha', 2.0, false, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true);

-- APLICACIONES (5)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('APL-FOL', 'Aplicación Foliar', 'aplicacion', 'Aplicación de productos fitosanitarios o nutricionales vía foliar', 4.0, true, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true),
('APL-SUE', 'Aplicación al Suelo', 'aplicacion', 'Aplicación de productos al suelo (drench)', 3.0, true, 'segun_necesidad', 'media', '["todos"]'::jsonb, true),
('APL-DRE', 'Aplicación por Drench', 'aplicacion', 'Aplicación de productos líquidos al cuello del árbol', 2.5, true, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true),
('APL-FER', 'Aplicación de Fertirrigación', 'aplicacion', 'Aplicación de fertilizantes a través del sistema de riego', 1.5, true, 'semanal', 'media', '["todos"]'::jsonb, true),
('APL-INY', 'Inyección al Tronco', 'aplicacion', 'Inyección de productos directamente al sistema vascular', 5.0, true, 'anual', 'baja', '["aguacate", "citricos", "mango"]'::jsonb, true);

-- PODAS (5)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('POD-FOR', 'Poda de Formación', 'poda', 'Poda para dar estructura al árbol joven', 1.5, false, 'anual', 'media', '["todos"]'::jsonb, true),
('POD-SAN', 'Poda Sanitaria', 'poda', 'Eliminación de ramas enfermas, secas o dañadas', 2.0, false, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true),
('POD-PRO', 'Poda de Producción', 'poda', 'Poda para mejorar producción y calidad de fruto', 2.5, false, 'anual', 'media', '["todos"]'::jsonb, true),
('POD-REJ', 'Poda de Rejuvenecimiento', 'poda', 'Poda drástica para renovar árboles viejos', 4.0, false, 'segun_necesidad', 'baja', '["todos"]'::jsonb, true),
('POD-ACL', 'Poda de Aclareo', 'poda', 'Remoción selectiva de ramas para mejorar ventilación y luz', 2.0, false, 'anual', 'media', '["todos"]'::jsonb, true);

-- FERTILIZACIÓN (3)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('FRT-SUE', 'Fertilización al Suelo', 'fertilizacion', 'Aplicación de fertilizantes granulados al suelo', 3.0, true, 'mensual', 'media', '["todos"]'::jsonb, true),
('FRT-FOL', 'Fertilización Foliar', 'fertilizacion', 'Aplicación de nutrientes vía foliar', 3.5, true, 'quincenal', 'media', '["todos"]'::jsonb, true),
('FRT-ORG', 'Fertilización Orgánica', 'fertilizacion', 'Aplicación de abonos orgánicos o compost', 4.0, true, 'trimestral', 'media', '["todos"]'::jsonb, true);

-- RIEGO (3)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('RIE-NOR', 'Riego Normal', 'riego', 'Riego programado según requerimiento hídrico', 0.5, false, 'diaria', 'alta', '["todos"]'::jsonb, true),
('RIE-AUX', 'Riego Auxiliar', 'riego', 'Riego adicional por condiciones de sequía o estrés', 1.0, false, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true),
('RIE-LAV', 'Lavado de Sales', 'riego', 'Riego abundante para lixiviar sales del suelo', 2.0, false, 'segun_necesidad', 'media', '["todos"]'::jsonb, true);

-- COSECHA (3)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('COS-REC', 'Recolección de Frutos', 'cosecha', 'Cosecha de frutos maduros', 6.0, false, 'segun_necesidad', 'urgente', '["todos"]'::jsonb, true),
('COS-SEL', 'Selección y Clasificación', 'cosecha', 'Clasificación de frutos por calibre y calidad', 4.0, false, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true),
('COS-EMP', 'Empaque', 'cosecha', 'Empaque de frutos para transporte', 3.0, true, 'segun_necesidad', 'alta', '["todos"]'::jsonb, true);

-- MANTENIMIENTO (5)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('MNT-BOM', 'Mantenimiento de Bombas', 'mantenimiento', 'Revisión y mantenimiento de bombas de riego', 2.0, false, 'mensual', 'media', '["todos"]'::jsonb, true),
('MNT-FIL', 'Limpieza de Filtros', 'mantenimiento', 'Limpieza de filtros del sistema de riego', 1.5, false, 'semanal', 'media', '["todos"]'::jsonb, true),
('MNT-GOT', 'Revisión de Goteros', 'mantenimiento', 'Verificación y limpieza de goteros', 3.0, false, 'quincenal', 'media', '["todos"]'::jsonb, true),
('MNT-CER', 'Mantenimiento de Cercos', 'mantenimiento', 'Reparación y mantenimiento de cercos perimetrales', 4.0, true, 'trimestral', 'baja', '["todos"]'::jsonb, true),
('MNT-CAM', 'Mantenimiento de Caminos', 'mantenimiento', 'Arreglo de caminos internos de la finca', 6.0, true, 'semestral', 'baja', '["todos"]'::jsonb, true);

-- CONTROL DE MALEZAS (3)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('MAL-MAN', 'Control Manual de Malezas', 'malezas', 'Deshierba manual alrededor de árboles', 5.0, false, 'mensual', 'media', '["todos"]'::jsonb, true),
('MAL-MEC', 'Control Mecánico de Malezas', 'malezas', 'Control con guadaña o desbrozadora', 3.0, false, 'mensual', 'media', '["todos"]'::jsonb, true),
('MAL-QUI', 'Control Químico de Malezas', 'malezas', 'Aplicación de herbicidas selectivos', 2.5, true, 'bimestral', 'baja', '["todos"]'::jsonb, true);

-- OTRAS LABORES (3)
INSERT INTO catalogo_labores (codigo, nombre, categoria, descripcion, duracion_estimada_hrs, requiere_insumos, frecuencia_tipica, prioridad_default, aplica_a, activo) VALUES
('OTR-MON', 'Monitoreo de Trampas', 'otro', 'Revisión y reposición de trampas de plagas', 2.0, true, 'semanal', 'media', '["todos"]'::jsonb, true),
('OTR-MUE', 'Toma de Muestras', 'otro', 'Recolección de muestras para análisis (suelo, tejido, agua)', 1.5, false, 'segun_necesidad', 'media', '["todos"]'::jsonb, true),
('OTR-ETI', 'Etiquetado de Árboles', 'otro', 'Colocación o actualización de etiquetas de identificación', 3.0, true, 'segun_necesidad', 'baja', '["todos"]'::jsonb, true);
```


---

### 19.7 Estados de Salud

Catálogo de estados de salud para seguimiento árbol por árbol:

```sql
-- Tabla: catalogo_estados_salud
CREATE TABLE IF NOT EXISTS catalogo_estados_salud (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    color VARCHAR(7) NOT NULL, -- Código hex color
    emoji VARCHAR(10),
    orden_severidad INTEGER, -- 1=mejor, 9=peor
    acciones_requeridas JSONB, -- Array de acciones
    activo BOOLEAN DEFAULT true
);

-- INSERT de estados de salud
INSERT INTO catalogo_estados_salud (codigo, nombre, descripcion, color, emoji, orden_severidad, acciones_requeridas, activo) VALUES
('SAL', 'Saludable', 'Árbol sin problemas detectados, en condiciones óptimas', '#22C55E', '🟢', 1, 
 '["Continuar monitoreo rutinario", "Mantener programa de nutrición", "Seguir calendario de riego"]'::jsonb, true),

('OBS', 'En Observación', 'Síntomas leves o inespecíficos detectados, requiere seguimiento cercano', '#EAB308', '🟡', 2,
 '["Inspección detallada en próxima visita", "Tomar fotografías de referencia", "Anotar síntomas específicos", "Monitorear evolución"]'::jsonb, true),

('RIE', 'En Riesgo', 'Problema confirmado, condición puede empeorar sin intervención', '#F97316', '🟠', 3,
 '["Programar tratamiento esta semana", "Identificar causa específica", "Evaluar necesidad de análisis", "Aislar si es contagioso"]'::jsonb, true),

('CRI', 'Crítico', 'Situación grave, requiere intervención inmediata', '#EF4444', '🔴', 4,
 '["Tratamiento urgente hoy", "Evaluar si eliminar para evitar contagio", "Consultar agrónomo", "Documentar extensivamente"]'::jsonb, true),

('TRA', 'En Tratamiento', 'Tratamiento activo en curso, bajo observación', '#3B82F6', '🔵', 2,
 '["Continuar tratamiento según prescripción", "Monitorear efectividad", "Registrar respuesta", "Ajustar si no hay mejoría"]'::jsonb, true),

('REC', 'En Recuperación', 'Post-tratamiento, mostrando mejoría, requiere seguimiento', '#8B5CF6', '🟣', 2,
 '["Verificar recuperación completa", "Mantener monitoreo frecuente", "Evaluar retorno a estado saludable", "Prevenir recaídas"]'::jsonb, true),

('MUE', 'Muerto', 'Árbol muerto o sin viabilidad', '#1F2937', '⚫', 5,
 '["Evaluar causa de muerte", "Decidir si eliminar", "Planificar replante si aplica", "Prevenir propagación si enfermedad"]'::jsonb, true),

('REM', 'Removido', 'Árbol eliminado del lote', '#9CA3AF', '⬜', 5,
 '["Registrar fecha y causa de eliminación", "Preparar sitio para replante", "Desinfectar área si fue por enfermedad"]'::jsonb, true),

('NUE', 'Nuevo / Replante', 'Árbol recién plantado, en establecimiento', '#06B6D4', '🩵', 1,
 '["Riego frecuente", "Protección contra plagas iniciales", "Monitoreo de prendimiento", "Fertilización de establecimiento"]'::jsonb, true);
```


---

### 19.8 Catálogo de Productos Fitosanitarios

Productos agroquímicos y biológicos para el control de plagas y nutrición:

```sql
-- Tabla: catalogo_productos
CREATE TABLE IF NOT EXISTS catalogo_productos (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(50) UNIQUE NOT NULL,
    nombre_comercial VARCHAR(150) NOT NULL,
    ingrediente_activo VARCHAR(200),
    tipo VARCHAR(50), -- 'insecticida', 'acaricida', 'fungicida', 'bactericida', 'herbicida', 'fertilizante', 'coadyuvante', 'biologico'
    categoria VARCHAR(50),
    concentracion VARCHAR(50),
    presentacion VARCHAR(100),
    fabricante VARCHAR(150),
    categoria_toxicologica VARCHAR(20), -- 'I-Extremadamente peligroso', 'II-Altamente peligroso', 'III-Moderadamente peligroso', 'IV-Ligeramente peligroso', 'U-No probable'
    periodo_carencia_dias INTEGER, -- Días antes de cosecha
    periodo_reingreso_horas INTEGER, -- Horas antes de reingresar al lote
    modo_accion TEXT,
    activo BOOLEAN DEFAULT true
);

-- INSECTICIDAS (7)
INSERT INTO catalogo_productos (codigo, nombre_comercial, ingrediente_activo, tipo, categoria, concentracion, presentacion, fabricante, categoria_toxicologica, periodo_carencia_dias, periodo_reingreso_horas, modo_accion, activo) VALUES
('INS-001', 'Engeo', 'Lambda-cyhalothrin + Thiamethoxam', 'insecticida', 'piretroide + neonicotinoide', '106 + 141 g/L', 'SC 1L', 'Syngenta', 'II', 7, 24, 'Contacto e ingestión, sistémico parcial', true),
('INS-002', 'Movento', 'Spirotetramat', 'insecticida', 'regulador de crecimiento', '150 g/L', 'OD 1L', 'Bayer', 'III', 14, 12, 'Sistémico bidireccional, inhibidor lipogénesis', true),
('INS-003', 'Tracer', 'Spinosad', 'insecticida', 'espinosina', '120 g/L', 'SC 1L', 'Corteva', 'III', 3, 12, 'Ingestión y contacto, activador nicotínico', true),
('INS-004', 'Vertimec', 'Abamectina', 'insecticida/acaricida', 'avermectina', '18 g/L', 'EC 1L', 'Syngenta', 'II', 7, 24, 'Contacto e ingestión, activador canal Cl', true),
('INS-005', 'Confidor', 'Imidacloprid', 'insecticida', 'neonicotinoide', '350 g/L', 'SC 1L', 'Bayer', 'III', 21, 12, 'Sistémico, agonista receptor nicotínico', true),
('INS-006', 'Lorsban', 'Chlorpyrifos', 'insecticida', 'organofosforado', '480 g/L', 'EC 1L', 'Corteva', 'II', 21, 48, 'Contacto e ingestión, inhibidor colinesterasa', true),
('INS-007', 'Success GF', 'Spinosad', 'insecticida', 'espinosina granulado', '25 g/kg', 'GR 10kg', 'Corteva', 'III', 3, 12, 'Ingestión, cebo atrayente', true);

-- ACARICIDAS (3)
INSERT INTO catalogo_productos (codigo, nombre_comercial, ingrediente_activo, tipo, categoria, concentracion, presentacion, fabricante, categoria_toxicologica, periodo_carencia_dias, periodo_reingreso_horas, modo_accion, activo) VALUES
('ACA-001', 'Envidor', 'Spirodiclofen', 'acaricida', 'cetoenol', '240 g/L', 'SC 1L', 'Bayer', 'III', 14, 12, 'Inhibidor síntesis lípidos, efecto huevos y ninfas', true),
('ACA-002', 'Oberon', 'Spiromesifen', 'acaricida', 'cetoenol', '240 g/L', 'SC 1L', 'Bayer', 'III', 14, 12, 'Inhibidor síntesis lípidos', true),
('ACA-003', 'Azufre Micronizado 80%', 'Azufre elemental', 'acaricida/fungicida', 'inorgánico', '800 g/kg', 'WG 25kg', 'Varios', 'U', 0, 4, 'Contacto, acción multisitio', true);

-- FUNGICIDAS (8)
INSERT INTO catalogo_productos (codigo, nombre_comercial, ingrediente_activo, tipo, categoria, concentracion, presentacion, fabricante, categoria_toxicologica, periodo_carencia_dias, periodo_reingreso_horas, modo_accion, activo) VALUES
('FUN-001', 'Amistar', 'Azoxystrobin', 'fungicida', 'estrobilurina', '250 g/L', 'SC 1L', 'Syngenta', 'III', 14, 12, 'Sistémico, inhibidor complejo III mitocondrial', true),
('FUN-002', 'Ridomil Gold', 'Metalaxyl-M + Mancozeb', 'fungicida', 'fenilamida + ditiocarbamato', '40 + 640 g/kg', 'WG 5kg', 'Syngenta', 'III', 21, 24, 'Sistémico + contacto', true),
('FUN-003', 'Aliette', 'Fosetil Aluminio', 'fungicida', 'fosfonato', '800 g/kg', 'WP 5kg', 'Bayer', 'U', 30, 12, 'Sistémico bidireccional, activador defensas', true),
('FUN-004', 'Mancozeb 80%', 'Mancozeb', 'fungicida', 'ditiocarbamato', '800 g/kg', 'WP 25kg', 'Varios', 'III', 15, 24, 'Contacto, multisitio protectante', true),
('FUN-005', 'Cobre Nordox 75%', 'Oxido cuproso', 'fungicida/bactericida', 'cobre', '750 g/kg', 'WG 25kg', 'Nordox', 'III', 0, 12, 'Contacto, ión cúprico', true),
('FUN-006', 'Score', 'Difenoconazole', 'fungicida', 'triazol', '250 g/L', 'EC 1L', 'Syngenta', 'III', 21, 12, 'Sistémico, inhibidor biosíntesis ergosterol', true),
('FUN-007', 'Folicur', 'Tebuconazole', 'fungicida', 'triazol', '250 g/L', 'EW 1L', 'Bayer', 'III', 14, 12, 'Sistémico, inhibidor desmetilación', true),
('FUN-008', 'Tilt', 'Propiconazole', 'fungicida', 'triazol', '250 g/L', 'EC 1L', 'Syngenta', 'III', 21, 12, 'Sistémico, inhibidor biosíntesis ergosterol', true);

-- BIOLÓGICOS (4)
INSERT INTO catalogo_productos (codigo, nombre_comercial, ingrediente_activo, tipo, categoria, concentracion, presentacion, fabricante, categoria_toxicologica, periodo_carencia_dias, periodo_reingreso_horas, modo_accion, activo) VALUES
('BIO-001', 'Trichoderma TrichoPlus', 'Trichoderma harzianum', 'biologico', 'hongo antagonista', '1x10^8 UFC/g', 'WP 1kg', 'Varios', 'U', 0, 0, 'Competencia, parasitismo, antibiosis', true),
('BIO-002', 'Beauveria BoveMax', 'Beauveria bassiana', 'biologico', 'hongo entomopatógeno', '1x10^8 conidias/g', 'WP 500g', 'Varios', 'U', 0, 0, 'Infecta y mata insectos por penetración cuticular', true),
('BIO-003', 'Metarhizium MetaMax', 'Metarhizium anisopliae', 'biologico', 'hongo entomopatógeno', '1x10^8 conidias/g', 'WP 500g', 'Varios', 'U', 0, 0, 'Infecta insectos del suelo y follaje', true),
('BIO-004', 'Bacillus BactoPlus', 'Bacillus subtilis', 'biologico', 'bacteria antagonista', '1x10^9 UFC/g', 'WP 1kg', 'Varios', 'U', 0, 0, 'Antibiosis, competencia, inducción resistencia', true);

-- FERTILIZANTES (5)
INSERT INTO catalogo_productos (codigo, nombre_comercial, ingrediente_activo, tipo, categoria, concentracion, presentacion, fabricante, categoria_toxicologica, periodo_carencia_dias, periodo_reingreso_horas, modo_accion, activo) VALUES
('FER-001', 'Nutrimins Ca-B', 'Calcio + Boro', 'fertilizante', 'foliar', '10% CaO + 0.5% B', 'SL 20L', 'Yara', 'U', 0, 0, 'Nutrición de Ca y B para cuajado de fruto', true),
('FER-002', 'Nutrimins Zn', 'Zinc quelatado', 'fertilizante', 'foliar', '12% Zn EDTA', 'SL 20L', 'Yara', 'U', 0, 0, 'Nutrición zinc, síntesis auxinas', true),
('FER-003', 'Kelatex Fe', 'Hierro quelatado', 'fertilizante', 'foliar/suelo', '6% Fe EDDHA', 'SG 25kg', 'Haifa', 'U', 0, 0, 'Corrección de clorosis férrica', true),
('FER-004', 'Triple 15', 'NPK 15-15-15', 'fertilizante', 'granulado', '15-15-15 + 2MgO + ME', 'Granular 50kg', 'Varios', 'U', 0, 0, 'Fertilización balanceada NPK', true),
('FER-005', 'Cloruro de Potasio', 'KCl', 'fertilizante', 'granulado', '60% K2O', 'Granular 50kg', 'Varios', 'U', 0, 0, 'Fuente de potasio para llenado y calidad', true);

-- COADYUVANTES (2)
INSERT INTO catalogo_productos (codigo, nombre_comercial, ingrediente_activo, tipo, categoria, concentracion, presentacion, fabricante, categoria_toxicologica, periodo_carencia_dias, periodo_reingreso_horas, modo_accion, activo) VALUES
('COA-001', 'Inex-A', 'Surfactante no iónico', 'coadyuvante', 'adherente', 'Alquilfenol etoxilado', 'SL 20L', 'Varios', 'U', 0, 0, 'Mejora cobertura y adherencia', true),
('COA-002', 'Aceite Agrícola 83%', 'Aceite mineral parafínico', 'coadyuvante', 'aceite', '830 g/L', 'EC 20L', 'Varios', 'U', 0, 4, 'Insecticida de contacto, asfixia', true);
```


---

### 19.9 Dosis Recomendadas

Relaciones plaga-producto con dosis específicas:

```sql
-- Tabla: dosis_recomendadas
CREATE TABLE IF NOT EXISTS dosis_recomendadas (
    id SERIAL PRIMARY KEY,
    plaga_id INTEGER REFERENCES catalogo_plagas(id),
    producto_id INTEGER REFERENCES catalogo_productos(id),
    cultivo_codigo VARCHAR(20), -- Puede ser específico o 'general'
    dosis_min DECIMAL(10,3),
    dosis_max DECIMAL(10,3),
    unidad VARCHAR(20), -- 'cc/L', 'g/L', 'kg/ha', 'L/ha', 'cc/arbol'
    frecuencia_dias INTEGER,
    numero_aplicaciones INTEGER,
    observaciones TEXT,
    eficacia VARCHAR(20), -- 'alta', 'media', 'baja'
    activo BOOLEAN DEFAULT true
);

-- Ejemplos de dosis recomendadas (muestra)
INSERT INTO dosis_recomendadas (plaga_id, producto_id, cultivo_codigo, dosis_min, dosis_max, unidad, frecuencia_dias, numero_aplicaciones, observaciones, eficacia, activo)
SELECT 
    (SELECT id FROM catalogo_plagas WHERE codigo = 'GEN-PL-001') as plaga_id,
    (SELECT id FROM catalogo_productos WHERE codigo = 'INS-004') as producto_id,
    'general', 0.5, 1.0, 'cc/L', 7, 3, 'Aplicar en envés de hojas. No aplicar con temperaturas >30°C', 'alta', true
UNION ALL SELECT
    (SELECT id FROM catalogo_plagas WHERE codigo = 'AGU-PL-001'),
    (SELECT id FROM catalogo_productos WHERE codigo = 'INS-003'),
    'AGU', 0.3, 0.5, 'cc/L', 7, 2, 'Aplicar al inicio de floración y cuajado', 'alta', true
UNION ALL SELECT
    (SELECT id FROM catalogo_plagas WHERE codigo = 'AGU-EN-001'),
    (SELECT id FROM catalogo_productos WHERE codigo = 'FUN-003'),
    'AGU', 3.0, 4.0, 'g/L', 30, 4, 'Aplicar drench al cuello del árbol. Mejor con suelo húmedo', 'alta', true
UNION ALL SELECT
    (SELECT id FROM catalogo_plagas WHERE codigo = 'CIT-PL-001'),
    (SELECT id FROM catalogo_productos WHERE codigo = 'INS-004'),
    'general', 0.3, 0.5, 'cc/L', 7, 2, 'Aplicar en brotes tiernos', 'alta', true
UNION ALL SELECT
    (SELECT id FROM catalogo_plagas WHERE codigo = 'CIT-EN-001'),
    (SELECT id FROM catalogo_productos WHERE codigo = 'INS-005'),
    'general', 0.5, 0.7, 'cc/L', 15, NULL, 'Manejo del vector. No cura HLB', 'media', true
UNION ALL SELECT
    (SELECT id FROM catalogo_plagas WHERE codigo = 'MAN-EN-001'),
    (SELECT id FROM catalogo_productos WHERE codigo = 'FUN-001'),
    'MAN', 0.5, 0.75, 'cc/L', 7, 3, 'Aplicar desde inicio floración', 'alta', true
UNION ALL SELECT
    (SELECT id FROM catalogo_plagas WHERE codigo = 'CAF-EN-001'),
    (SELECT id FROM catalogo_productos WHERE codigo = 'FUN-005'),
    'CAF', 3.0, 5.0, 'g/L', 21, 3, 'Preventivo. Aplicar antes de lluvias', 'alta', true;
```

---

### 19.10 Usuarios Admin de Plataforma

Usuarios del equipo AgroGrid con acceso administrativo:

```sql
-- Tabla: usuarios (estructura simplificada)
-- Se asume que existe una tabla usuarios con campos: email, password_hash, nombre, rol, tenant_id (null para admin plataforma), activo

-- INSERT de usuarios administradores de la plataforma
INSERT INTO usuarios (email, password_hash, nombre, apellido, rol, tenant_id, activo, created_at) VALUES
('admin@agrogrid.com', '$2b$10$...hash...', 'Administrador', 'Sistema', 'ROLE_SUPER_ADMIN', NULL, true, CURRENT_TIMESTAMP),
('soporte@agrogrid.com', '$2b$10$...hash...', 'Equipo', 'Soporte', 'ROLE_ADMIN_SOPORTE', NULL, true, CURRENT_TIMESTAMP),
('ventas@agrogrid.com', '$2b$10$...hash...', 'Equipo', 'Ventas', 'ROLE_ADMIN_VENTAS', NULL, true, CURRENT_TIMESTAMP);

-- Nota: Los password_hash deben generarse usando bcrypt con el password real
-- Ejemplo para desarrollo/demo: todos pueden usar password "AgroGrid2025!"
```

---

### 19.11 Tenant de Demostración

Configuración del tenant demo para pruebas y demostraciones:

```sql
-- Tabla: tenants
CREATE TABLE IF NOT EXISTS tenants (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(200) NOT NULL,
    slug VARCHAR(100) UNIQUE NOT NULL,
    plan_id INTEGER REFERENCES planes(id),
    activo BOOLEAN DEFAULT true,
    fecha_inicio DATE DEFAULT CURRENT_DATE,
    configuracion JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabla: suscripciones
CREATE TABLE IF NOT EXISTS suscripciones (
    id SERIAL PRIMARY KEY,
    tenant_id INTEGER REFERENCES tenants(id),
    plan_id INTEGER REFERENCES planes(id),
    estado VARCHAR(20), -- 'activa', 'suspendida', 'cancelada', 'prueba'
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE,
    proximo_pago DATE,
    monto_mensual DECIMAL(10,2),
    activo BOOLEAN DEFAULT true
);

-- INSERT del tenant de demostración
INSERT INTO tenants (nombre, slug, plan_id, activo, configuracion) VALUES
('Finca Demo AgroGrid', 'demo', 
 (SELECT id FROM planes WHERE nombre = 'Professional' LIMIT 1), 
 true,
 '{
   "cultivos_habilitados": ["AGU", "MAN", "NAR", "LIM", "MND", "CAF"],
   "idioma": "es",
   "moneda": "COP",
   "zona_horaria": "America/Bogota",
   "unidades": {
     "distancia": "metros",
     "area": "hectareas",
     "peso": "kilogramos",
     "temperatura": "celsius"
   },
   "limites": {
     "max_fincas": 5,
     "max_usuarios": 20,
     "max_arboles": 10000
   }
 }'::jsonb
);

-- Suscripción activa para el tenant demo
INSERT INTO suscripciones (tenant_id, plan_id, estado, fecha_inicio, fecha_fin, proximo_pago, monto_mensual, activo)
SELECT 
    id as tenant_id,
    plan_id,
    'activa',
    CURRENT_DATE - INTERVAL '60 days',
    CURRENT_DATE + INTERVAL '305 days',
    CURRENT_DATE + INTERVAL '30 days',
    149.00,
    true
FROM tenants WHERE slug = 'demo';
```

---

### 19.12 Usuarios de Prueba del Tenant Demo

Usuarios con diferentes roles para el tenant de demostración:

```sql
-- INSERT de usuarios del tenant demo (8 usuarios)
INSERT INTO usuarios (email, password_hash, nombre, apellido, rol, tenant_id, activo, created_at)
SELECT 
    'propietario@fincademo.com', '$2b$10$...hash...', 'Carlos', 'Rodríguez', 'ROLE_TENANT_OWNER',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'gerente@fincademo.com', '$2b$10$...hash...', 'Ana', 'Martínez', 'ROLE_TENANT_MANAGER',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'agronomo@fincademo.com', '$2b$10$...hash...', 'Luis', 'Gómez', 'ROLE_TENANT_AGRONOMO',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'supervisor@fincademo.com', '$2b$10$...hash...', 'María', 'López', 'ROLE_TENANT_SUPERVISOR',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'operario1@fincademo.com', '$2b$10$...hash...', 'Pedro', 'Sánchez', 'ROLE_TENANT_OPERARIO',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'operario2@fincademo.com', '$2b$10$...hash...', 'José', 'Ramírez', 'ROLE_TENANT_OPERARIO',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'operario3@fincademo.com', '$2b$10$...hash...', 'Miguel', 'Torres', 'ROLE_TENANT_OPERARIO',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP
UNION ALL SELECT
    'inversionista@fincademo.com', '$2b$10$...hash...', 'Roberto', 'Vargas', 'ROLE_TENANT_VIEWER',
    (SELECT id FROM tenants WHERE slug = 'demo'), true, CURRENT_TIMESTAMP;

-- Nota: Todos usan el mismo password en demo: "Demo2025!"
```

---

### 19.13 Finca de Demostración

Estructura completa de la finca demo "La Esperanza":

```sql
-- Tabla: fincas
CREATE TABLE IF NOT EXISTS fincas (
    id SERIAL PRIMARY KEY,
    tenant_id INTEGER REFERENCES tenants(id),
    nombre VARCHAR(200) NOT NULL,
    ubicacion_municipio VARCHAR(100),
    ubicacion_departamento VARCHAR(100),
    ubicacion_pais VARCHAR(100),
    coordenadas_centro GEOGRAPHY(POINT),
    area_total_ha DECIMAL(10,2),
    area_cultivada_ha DECIMAL(10,2),
    altitud_msnm INTEGER,
    activo BOOLEAN DEFAULT true
);

-- INSERT de la finca demo
INSERT INTO fincas (tenant_id, nombre, ubicacion_municipio, ubicacion_departamento, ubicacion_pais, 
                    coordenadas_centro, area_total_ha, area_cultivada_ha, altitud_msnm, activo)
SELECT 
    id as tenant_id,
    'La Esperanza',
    'Fresno',
    'Tolima',
    'Colombia',
    ST_SetSRID(ST_MakePoint(-75.0333, 5.1500), 4326)::geography,
    50.00,
    42.00,
    1650,
    true
FROM tenants WHERE slug = 'demo';

-- Tabla: sectores
CREATE TABLE IF NOT EXISTS sectores (
    id SERIAL PRIMARY KEY,
    finca_id INTEGER REFERENCES fincas(id),
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    area_ha DECIMAL(10,2),
    poligono GEOGRAPHY(POLYGON),
    activo BOOLEAN DEFAULT true
);

-- INSERT de sectores
INSERT INTO sectores (finca_id, nombre, descripcion, area_ha, activo)
SELECT 
    id as finca_id,
    'Sector Norte',
    'Aguacate Hass establecido, árboles de 8-12 años en producción plena',
    12.50,
    true
FROM fincas WHERE nombre = 'La Esperanza'
UNION ALL SELECT
    id, 'Sector Sur', 'Mango Tommy Atkins, árboles de 10 años', 10.00, true
FROM fincas WHERE nombre = 'La Esperanza'
UNION ALL SELECT
    id, 'Sector Este', 'Cítricos mixtos (naranja, limón, mandarina)', 9.50, true
FROM fincas WHERE nombre = 'La Esperanza'
UNION ALL SELECT
    id, 'Sector Oeste', 'Aguacate Hass recién plantado (2 años)', 10.00, true
FROM fincas WHERE nombre = 'La Esperanza';

-- Tabla: lotes
CREATE TABLE IF NOT EXISTS lotes (
    id SERIAL PRIMARY KEY,
    sector_id INTEGER REFERENCES sectores(id),
    nombre VARCHAR(100) NOT NULL,
    codigo VARCHAR(50),
    cultivo_id INTEGER REFERENCES catalogo_cultivos(id),
    variedad VARCHAR(100),
    fecha_siembra DATE,
    distancia_siembra_m DECIMAL(5,2),
    distancia_entre_surcos_m DECIMAL(5,2),
    numero_arboles INTEGER,
    area_ha DECIMAL(8,3),
    activo BOOLEAN DEFAULT true
);

-- INSERT de lotes
INSERT INTO lotes (sector_id, nombre, codigo, cultivo_id, variedad, fecha_siembra, 
                   distancia_siembra_m, distancia_entre_surcos_m, numero_arboles, area_ha, activo)
SELECT 
    s.id as sector_id,
    'Lote Norte A',
    'LN-A',
    (SELECT id FROM catalogo_cultivos WHERE codigo = 'AGU'),
    'Hass',
    '2015-06-15',
    7.0,
    6.0,
    500,
    2.10,
    true
FROM sectores s WHERE s.nombre = 'Sector Norte'
UNION ALL SELECT
    s.id, 'Lote Norte B', 'LN-B', (SELECT id FROM catalogo_cultivos WHERE codigo = 'AGU'),
    'Hass', '2016-03-20', 7.0, 6.0, 480, 2.02, true
FROM sectores s WHERE s.nombre = 'Sector Norte'
UNION ALL SELECT
    s.id, 'Lote Norte C', 'LN-C', (SELECT id FROM catalogo_cultivos WHERE codigo = 'AGU'),
    'Hass', '2014-11-10', 7.0, 6.0, 520, 2.18, true
FROM sectores s WHERE s.nombre = 'Sector Norte'
UNION ALL SELECT
    s.id, 'Lote Sur A', 'LS-A', (SELECT id FROM catalogo_cultivos WHERE codigo = 'MAN'),
    'Tommy Atkins', '2014-05-01', 8.0, 8.0, 320, 2.05, true
FROM sectores s WHERE s.nombre = 'Sector Sur'
UNION ALL SELECT
    s.id, 'Lote Sur B', 'LS-B', (SELECT id FROM catalogo_cultivos WHERE codigo = 'MAN'),
    'Tommy Atkins', '2015-08-15', 8.0, 8.0, 300, 1.92, true
FROM sectores s WHERE s.nombre = 'Sector Sur'
UNION ALL SELECT
    s.id, 'Lote Este A', 'LE-A', (SELECT id FROM catalogo_cultivos WHERE codigo = 'NAR'),
    'Valencia', '2016-02-10', 6.0, 5.0, 450, 1.35, true
FROM sectores s WHERE s.nombre = 'Sector Este'
UNION ALL SELECT
    s.id, 'Lote Este B', 'LE-B', (SELECT id FROM catalogo_cultivos WHERE codigo = 'LIM'),
    'Tahití', '2017-01-20', 6.0, 5.0, 420, 1.26, true
FROM sectores s WHERE s.nombre = 'Sector Este'
UNION ALL SELECT
    s.id, 'Lote Este C', 'LE-C', (SELECT id FROM catalogo_cultivos WHERE codigo = 'MND'),
    'Arrayana', '2016-09-05', 5.0, 4.0, 380, 0.76, true
FROM sectores s WHERE s.nombre = 'Sector Este'
UNION ALL SELECT
    s.id, 'Lote Oeste A', 'LO-A', (SELECT id FROM catalogo_cultivos WHERE codigo = 'AGU'),
    'Hass', '2023-04-10', 7.0, 6.0, 450, 1.89, true
FROM sectores s WHERE s.nombre = 'Sector Oeste'
UNION ALL SELECT
    s.id, 'Lote Oeste B', 'LO-B', (SELECT id FROM catalogo_cultivos WHERE codigo = 'AGU'),
    'Hass', '2023-11-05', 7.0, 6.0, 480, 2.02, true
FROM sectores s WHERE s.nombre = 'Sector Oeste';
```


---

### 19.14 Datos de Ejemplo

Script PL/pgSQL para generar 500 árboles del Lote Norte A con datos realistas:

```sql
-- Tabla: arboles
CREATE TABLE IF NOT EXISTS arboles (
    id SERIAL PRIMARY KEY,
    lote_id INTEGER REFERENCES lotes(id),
    codigo VARCHAR(50) NOT NULL,
    fila INTEGER,
    columna INTEGER,
    coordenadas GEOGRAPHY(POINT),
    fecha_siembra DATE,
    edad_anos DECIMAL(4,1),
    activo BOOLEAN DEFAULT true,
    UNIQUE(lote_id, codigo)
);

-- Tabla: estados_actuales
CREATE TABLE IF NOT EXISTS estados_actuales (
    id SERIAL PRIMARY KEY,
    arbol_id INTEGER REFERENCES arboles(id) UNIQUE,
    estado_salud_id INTEGER REFERENCES catalogo_estados_salud(id),
    etapa_fenologica_id INTEGER REFERENCES catalogo_fenologia(id),
    ndvi DECIMAL(4,3), -- Índice vegetación (0-1)
    fecha_ultima_inspeccion DATE,
    observaciones TEXT,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Función para generar árboles del Lote Norte A
CREATE OR REPLACE FUNCTION generar_arboles_demo() RETURNS void AS $$
DECLARE
    v_lote_id INTEGER;
    v_cultivo_id INTEGER;
    v_fecha_siembra DATE;
    v_lat_base DECIMAL(10,7);
    v_lon_base DECIMAL(10,7);
    v_fila INTEGER;
    v_columna INTEGER;
    v_arbol_id INTEGER;
    v_estado_id INTEGER;
    v_fenologia_id INTEGER;
    v_random DECIMAL(3,2);
BEGIN
    -- Obtener datos del lote
    SELECT l.id, l.cultivo_id, l.fecha_siembra, 
           ST_Y(f.coordenadas_centro::geometry), 
           ST_X(f.coordenadas_centro::geometry)
    INTO v_lote_id, v_cultivo_id, v_fecha_siembra, v_lat_base, v_lon_base
    FROM lotes l
    JOIN sectores s ON l.sector_id = s.id
    JOIN fincas f ON s.finca_id = f.id
    WHERE l.codigo = 'LN-A';

    -- Generar 500 árboles en cuadrícula
    FOR v_fila IN 1..25 LOOP
        FOR v_columna IN 1..20 LOOP
            -- Insertar árbol
            INSERT INTO arboles (lote_id, codigo, fila, columna, coordenadas, fecha_siembra, edad_anos, activo)
            VALUES (
                v_lote_id,
                'LN-A-' || LPAD(v_fila::text, 2, '0') || '-' || LPAD(v_columna::text, 2, '0'),
                v_fila,
                v_columna,
                ST_SetSRID(
                    ST_MakePoint(
                        v_lon_base + (v_columna * 0.00007), -- ~7m entre árboles
                        v_lat_base + (v_fila * 0.00006)     -- ~6m entre filas
                    ), 
                    4326
                )::geography,
                v_fecha_siembra,
                EXTRACT(YEAR FROM AGE(CURRENT_DATE, v_fecha_siembra)) + 
                    EXTRACT(MONTH FROM AGE(CURRENT_DATE, v_fecha_siembra)) / 12.0,
                true
            ) RETURNING id INTO v_arbol_id;

            -- Generar estado actual con distribución realista
            v_random := RANDOM();
            
            -- 70% Saludable, 20% En Observación, 8% En Riesgo, 2% otros
            IF v_random < 0.70 THEN
                SELECT id INTO v_estado_id FROM catalogo_estados_salud WHERE codigo = 'SAL';
            ELSIF v_random < 0.90 THEN
                SELECT id INTO v_estado_id FROM catalogo_estados_salud WHERE codigo = 'OBS';
            ELSIF v_random < 0.98 THEN
                SELECT id INTO v_estado_id FROM catalogo_estados_salud WHERE codigo = 'RIE';
            ELSE
                SELECT id INTO v_estado_id FROM catalogo_estados_salud WHERE codigo = 'TRA';
            END IF;

            -- Etapa fenológica: mayoría en Desarrollo de Fruto (DES)
            IF RANDOM() < 0.85 THEN
                SELECT id INTO v_fenologia_id FROM catalogo_fenologia 
                WHERE cultivo_id = v_cultivo_id AND codigo = 'DES' LIMIT 1;
            ELSE
                SELECT id INTO v_fenologia_id FROM catalogo_fenologia 
                WHERE cultivo_id = v_cultivo_id AND codigo = 'MAD' LIMIT 1;
            END IF;

            -- Insertar estado actual
            INSERT INTO estados_actuales (arbol_id, estado_salud_id, etapa_fenologica_id, ndvi, fecha_ultima_inspeccion)
            VALUES (
                v_arbol_id,
                v_estado_id,
                v_fenologia_id,
                0.65 + (RANDOM() * 0.25), -- NDVI entre 0.65 y 0.90
                CURRENT_DATE - (RANDOM() * 14)::INTEGER -- Última inspección últimos 14 días
            );
        END LOOP;
    END LOOP;

    RAISE NOTICE 'Se generaron 500 árboles exitosamente para el Lote Norte A';
END;
$$ LANGUAGE plpgsql;

-- Ejecutar la función
SELECT generar_arboles_demo();
```

---

### 19.15 Resumen de Catálogos

Tabla resumen de todos los catálogos poblados:

| Catálogo | Registros | Descripción |
|----------|-----------|-------------|
| **Cultivos** | 22 | Aguacate, Mango, Durazno, Ciruela, Cereza, Manzana, Pera, Naranja, Limón, Mandarina, Toronja, Lima, Papaya, Guayaba, Maracuyá, Pitahaya, Banano, Uva, Olivo, Nuez, Almendra, Café, Cacao |
| **Fenología** | ~70 | Etapas fenológicas específicas por cultivo: Aguacate (9), Mango (9), Cítricos (8), Café (7) |
| **Plagas** | ~25 | Plagas generales (5) + específicas de Aguacate (4), Cítricos (4), Mango (3), Café (2) |
| **Enfermedades** | ~20 | Enfermedades generales (4) + específicas de Aguacate (4), Cítricos (4), Mango (3), Café (3) |
| **Labores** | 35 | Inspecciones (5), Aplicaciones (5), Podas (5), Fertilización (3), Riego (3), Cosecha (3), Mantenimiento (5), Malezas (3), Otras (3) |
| **Estados Salud** | 9 | Saludable, En Observación, En Riesgo, Crítico, En Tratamiento, En Recuperación, Muerto, Removido, Nuevo/Replante |
| **Productos** | ~30 | Insecticidas (7), Acaricidas (3), Fungicidas (8), Biológicos (4), Fertilizantes (5), Coadyuvantes (2) |
| **Dosis Recomendadas** | ~50 | Relaciones plaga-producto con dosis específicas y observaciones |
| **Usuarios Admin** | 3 | Super Admin, Soporte, Ventas |
| **Tenant Demo** | 1 | Finca Demo AgroGrid con plan Professional activo |
| **Usuarios Demo** | 8 | Owner (1), Manager (1), Agrónomo (1), Supervisor (1), Operario (3), Viewer (1) |
| **Finca Demo** | 1 finca | La Esperanza - Fresno, Tolima (50 ha, 42 ha cultivada) |
| **Sectores** | 4 | Norte, Sur, Este, Oeste |
| **Lotes** | 10 | Distribuidos en los 4 sectores con diferentes cultivos |
| **Árboles Ejemplo** | 500 | Lote Norte A - Aguacate Hass con estados y fenología |

**Total de registros de datos semilla**: ~750+ registros

---

### 19.16 Script de Ejecución

Orden recomendado para ejecutar los scripts de inicialización:

```sql
-- ============================================
-- SCRIPT MAESTRO DE INICIALIZACIÓN
-- AgroGrid SaaS - Catálogos y Datos Semilla
-- ============================================

-- PASO 1: Crear tablas (ejecutar schema completo)
-- Ver documentos de arquitectura para DDL completo

-- PASO 2: Insertar roles y permisos del sistema
-- (Depende de la implementación del sistema de autenticación)

-- PASO 3: Insertar planes de suscripción
-- Ver documento de Modelo de Negocio SaaS (doc 02)

-- PASO 4: Insertar usuarios administradores de plataforma
-- Ejecutar scripts de sección 19.10

-- PASO 5: Insertar catálogos globales (compartidos entre tenants)
BEGIN;
    -- 5.1 Cultivos
    -- Ejecutar scripts de sección 19.2
    
    -- 5.2 Fenología por cultivo
    -- Ejecutar scripts de sección 19.3
    
    -- 5.3 Plagas
    -- Ejecutar scripts de sección 19.4
    
    -- 5.4 Enfermedades
    -- Ejecutar scripts de sección 19.5
    
    -- 5.5 Labores
    -- Ejecutar scripts de sección 19.6
    
    -- 5.6 Estados de Salud
    -- Ejecutar scripts de sección 19.7
    
    -- 5.7 Productos Fitosanitarios
    -- Ejecutar scripts de sección 19.8
    
    -- 5.8 Dosis Recomendadas
    -- Ejecutar scripts de sección 19.9
COMMIT;

-- PASO 6: Insertar tenant de demostración
BEGIN;
    -- 6.1 Tenant y suscripción
    -- Ejecutar scripts de sección 19.11
    
    -- 6.2 Usuarios del tenant demo
    -- Ejecutar scripts de sección 19.12
COMMIT;

-- PASO 7: Insertar estructura de finca demo
BEGIN;
    -- 7.1 Finca, sectores y lotes
    -- Ejecutar scripts de sección 19.13
COMMIT;

-- PASO 8: Generar árboles de ejemplo
BEGIN;
    -- 8.1 Crear función y ejecutar
    -- Ejecutar scripts de sección 19.14
COMMIT;

-- PASO 9: Verificar integridad
SELECT 'Cultivos' as tabla, COUNT(*) as registros FROM catalogo_cultivos
UNION ALL
SELECT 'Fenología', COUNT(*) FROM catalogo_fenologia
UNION ALL
SELECT 'Plagas', COUNT(*) FROM catalogo_plagas WHERE tipo = 'plaga'
UNION ALL
SELECT 'Enfermedades', COUNT(*) FROM catalogo_plagas WHERE tipo = 'enfermedad'
UNION ALL
SELECT 'Labores', COUNT(*) FROM catalogo_labores
UNION ALL
SELECT 'Estados Salud', COUNT(*) FROM catalogo_estados_salud
UNION ALL
SELECT 'Productos', COUNT(*) FROM catalogo_productos
UNION ALL
SELECT 'Dosis Recomendadas', COUNT(*) FROM dosis_recomendadas
UNION ALL
SELECT 'Tenants', COUNT(*) FROM tenants
UNION ALL
SELECT 'Usuarios', COUNT(*) FROM usuarios
UNION ALL
SELECT 'Fincas', COUNT(*) FROM fincas
UNION ALL
SELECT 'Sectores', COUNT(*) FROM sectores
UNION ALL
SELECT 'Lotes', COUNT(*) FROM lotes
UNION ALL
SELECT 'Árboles', COUNT(*) FROM arboles
UNION ALL
SELECT 'Estados Actuales', COUNT(*) FROM estados_actuales;

-- PASO 10: Opcional - Generar datos históricos adicionales
-- (Inspecciones pasadas, aplicaciones, cosechas, etc.)
-- Según necesidades de demostración

RAISE NOTICE 'Inicialización completa exitosa!';
```

### Notas Importantes

1. **Passwords**: Los hashes de password mostrados son placeholders. En producción, generar con bcrypt usando un salt factor de 10 o superior.

2. **IDs en producción**: Los scripts usan referencias dinámicas con SELECT para obtener IDs. Esto asegura compatibilidad si las secuencias cambian.

3. **Transacciones**: Usar BEGIN/COMMIT para asegurar consistencia. Si algo falla, se hace rollback automático.

4. **Extensiones requeridas**: 
   - PostGIS para tipos GEOGRAPHY
   - pgcrypto para funciones de hashing

5. **Performance**: Para grandes volúmenes de árboles (>10,000), considerar usar COPY o INSERT masivo en lugar de la función PL/pgSQL.

6. **Mantenimiento**: Actualizar campo `updated_at` usando triggers:
   ```sql
   CREATE OR REPLACE FUNCTION update_timestamp()
   RETURNS TRIGGER AS $$
   BEGIN
       NEW.updated_at = CURRENT_TIMESTAMP;
       RETURN NEW;
   END;
   $$ LANGUAGE plpgsql;
   ```

7. **Índices**: Crear índices en campos de búsqueda frecuente:
   - `catalogo_cultivos(codigo)`
   - `catalogo_plagas(tipo, codigo)`
   - `arboles(lote_id, activo)`
   - `estados_actuales(arbol_id, estado_salud_id)`

---

> Navegación: [← Anterior](12-proximos-pasos.md) | [📑 Índice](README.md)
