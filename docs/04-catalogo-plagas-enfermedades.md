## 4. Catálogo de Plagas y Enfermedades

### 4.1 Modelo de Datos

```sql
-- Catálogo maestro de plagas (compartido entre tenants)
CREATE TABLE catalogo_plagas (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(50) UNIQUE NOT NULL,
    nombre_comun VARCHAR(100) NOT NULL,
    nombre_cientifico VARCHAR(150),
    tipo VARCHAR(20) CHECK (tipo IN ('plaga', 'enfermedad', 'deficiencia', 'fisiopatia')),
    categoria VARCHAR(50), -- 'insecto', 'acaro', 'hongo', 'bacteria', 'virus', 'nematodo'
    descripcion TEXT,
    sintomas TEXT,
    imagen_url VARCHAR(500),
    activo BOOLEAN DEFAULT true
);

-- Relación plaga-cultivo con información específica
CREATE TABLE plaga_cultivo (
    id SERIAL PRIMARY KEY,
    plaga_id INTEGER REFERENCES catalogo_plagas(id),
    cultivo_id VARCHAR(50) NOT NULL,
    severidad_tipica VARCHAR(20), -- 'leve', 'moderada', 'severa', 'critica'
    frecuencia VARCHAR(20), -- 'rara', 'ocasional', 'frecuente', 'muy_frecuente'
    epoca_riesgo VARCHAR(100), -- 'verano', 'todo_año', 'epoca_lluvias'
    productos_recomendados JSONB,
    umbral_economico TEXT,
    notas_especificas TEXT
);

-- Estados de severidad por árbol
CREATE TABLE estados_arbol (
    id SERIAL PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE NOT NULL,
    nombre VARCHAR(50) NOT NULL,
    color_hex VARCHAR(7) NOT NULL,
    icono VARCHAR(10),
    descripcion TEXT,
    accion_requerida TEXT,
    orden_severidad INTEGER -- 1=mejor, 10=peor
);
```

### 4.2 Catálogo de Plagas por Cultivo

#### 🥑 Aguacate

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `AGU-PL-001` | Trips (*Scirtothrips perseae*) | Plaga | Cicatrices en frutos, deformación | Moderada | Spinosad, Abamectina |
| `AGU-PL-002` | Araña roja (*Oligonychus punicae*) | Plaga | Amarillamiento hojas, telarañas | Moderada | Abamectina, Azufre |
| `AGU-PL-003` | Barrenador del hueso | Plaga | Galerías en semilla y pulpa | Severa | Clorpirifos, trampas |
| `AGU-PL-004` | Mosca del fruto | Plaga | Larvas en pulpa, pudrición | Severa | Spinosad, trampeo |
| `AGU-EN-001` | Phytophthora (*P. cinnamomi*) | Enfermedad | Marchitez, pudrición raíz | Crítica | Fosetil Al, Metalaxil |
| `AGU-EN-002` | Antracnosis (*Colletotrichum*) | Enfermedad | Manchas negras en frutos | Moderada | Cobre, Mancozeb |
| `AGU-EN-003` | Roña (*Sphaceloma perseae*) | Enfermedad | Costras en frutos y hojas | Leve | Cobre, Oxicloruro |
| `AGU-EN-004` | Fusariosis | Enfermedad | Marchitez vascular | Severa | Sin control químico efectivo |
| `AGU-DF-001` | Deficiencia de Zinc | Deficiencia | Hojas pequeñas, entrenudos cortos | Leve | Sulfato de Zinc foliar |
| `AGU-DF-002` | Deficiencia de Hierro | Deficiencia | Clorosis intervenal | Moderada | Quelatos de Fe |

#### 🍑 Durazno

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `DUR-PL-001` | Polilla oriental (*Grapholita molesta*) | Plaga | Galerías en brotes y frutos | Severa | Confusión sexual, Clorpirifos |
| `DUR-PL-002` | Pulgón verde | Plaga | Hojas enrolladas, melaza | Moderada | Imidacloprid, jabón potásico |
| `DUR-PL-003` | Mosca de la fruta | Plaga | Larvas en frutos | Severa | Spinosad, trampeo masivo |
| `DUR-EN-001` | Torque (*Taphrina deformans*) | Enfermedad | Hojas deformadas, rojizas | Moderada | Cobre en dormancia |
| `DUR-EN-002` | Monilia (*Monilinia fructicola*) | Enfermedad | Pudrición parda en frutos | Severa | Iprodione, Ciprodinil |
| `DUR-EN-003` | Oídio | Enfermedad | Polvo blanco en hojas | Leve | Azufre, Trifloxistrobin |
| `DUR-EN-004` | Gomosis bacteriana | Enfermedad | Exudado gomoso en tronco | Moderada | Cobre, poda sanitaria |

#### 🍎 Manzana

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `MAN-PL-001` | Carpocapsa (*Cydia pomonella*) | Plaga | Galerías hacia semillas | Severa | Confusión sexual, Clorpirifos |
| `MAN-PL-002` | Áfidos | Plaga | Hojas enrolladas, melaza | Moderada | Imidacloprid, depredadores |
| `MAN-PL-003` | Araña roja europea | Plaga | Bronceado de hojas | Moderada | Abamectina, Hexitiazox |
| `MAN-EN-001` | Sarna (*Venturia inaequalis*) | Enfermedad | Manchas oliváceas en hojas/frutos | Severa | Captan, Difenoconazol |
| `MAN-EN-002` | Oídio | Enfermedad | Polvo blanco en brotes | Moderada | Azufre, Miclobutanil |
| `MAN-EN-003` | Fuego bacteriano (*Erwinia*) | Enfermedad | Brotes quemados, exudado | Crítica | Cobre, eliminar tejido infectado |
| `MAN-EN-004` | Podredumbre amarga | Enfermedad | Manchas hundidas en frutos | Moderada | Captan, manejo postcosecha |

#### 🍋 Limón / Cítricos

| Código | Plaga/Enfermedad | Tipo | Síntomas | Severidad | Tratamiento |
|--------|------------------|------|----------|-----------|-------------|
| `CIT-PL-001` | Minador de hojas (*Phyllocnistis citrella*) | Plaga | Galerías serpenteantes en hojas | Moderada | Abamectina, Imidacloprid |
| `CIT-PL-002` | Psílido asiático (*Diaphorina citri*) | Plaga | Vector de HLB, brotes amarillos | Crítica | Imidacloprid, monitoreo intensivo |
| `CIT-PL-003` | Mosca blanca | Plaga | Fumagina, debilitamiento | Moderada | Aceites, Buprofezin |
| `CIT-PL-004` | Cochinilla acanalada | Plaga | Colonias en ramas, melaza | Moderada | Aceite, Buprofezin |
| `CIT-EN-001` | HLB/Huanglongbing | Enfermedad | Brotes amarillos, frutos deformes | Crítica | Sin cura - erradicación |
| `CIT-EN-002` | Gomosis (*Phytophthora*) | Enfermedad | Exudado en tronco, muerte | Severa | Fosetil Al, drenaje |
| `CIT-EN-003` | Mancha grasienta | Enfermedad | Manchas amarillas aceitosas | Leve | Cobre preventivo |
| `CIT-EN-004` | Cancrosis | Enfermedad | Lesiones elevadas en hojas/frutos | Moderada | Cobre, material certificado |

### 4.3 Estados del Árbol (Universales)

```yaml
estados_arbol:
  - codigo: "SAL"
    nombre: "Saludable"
    color: "#22C55E"  # Verde
    icono: "🟢"
    descripcion: "Árbol sin problemas detectados"
    accion: "Continuar monitoreo rutinario"
    severidad: 1

  - codigo: "ATE"
    nombre: "Atención"
    color: "#EAB308"  # Amarillo
    icono: "🟡"
    descripcion: "Síntomas leves detectados, requiere seguimiento"
    accion: "Inspección detallada en próxima visita"
    severidad: 2

  - codigo: "ADV"
    nombre: "Advertencia"
    color: "#F97316"  # Naranja
    icono: "🟠"
    descripcion: "Problema confirmado, intervención próxima"
    accion: "Programar tratamiento esta semana"
    severidad: 3

  - codigo: "CRI"
    nombre: "Crítico"
    color: "#EF4444"  # Rojo
    icono: "🔴"
    descripcion: "Requiere intervención inmediata"
    accion: "Tratamiento urgente hoy"
    severidad: 4

  - codigo: "TRA"
    nombre: "En Tratamiento"
    color: "#3B82F6"  # Azul
    icono: "🔵"
    descripcion: "Tratamiento activo en curso"
    accion: "Monitorear efectividad del tratamiento"
    severidad: 2

  - codigo: "REC"
    nombre: "En Recuperación"
    color: "#06B6D4"  # Cyan
    icono: "🩵"
    descripcion: "Post-tratamiento, en observación"
    accion: "Verificar recuperación completa"
    severidad: 2

  - codigo: "JOV"
    nombre: "Juvenil/Desarrollo"
    color: "#A855F7"  # Morado
    icono: "🟣"
    descripcion: "Árbol joven, aún no productivo"
    accion: "Cuidados de establecimiento"
    severidad: 1

  - codigo: "MUE"
    nombre: "Muerto/Removido"
    color: "#1F2937"  # Negro/Gris oscuro
    icono: "⚫"
    descripcion: "Árbol muerto o eliminado"
    accion: "Planificar replante si aplica"
    severidad: 5

  - codigo: "SIN"
    nombre: "Sin Inspeccionar"
    color: "#D1D5DB"  # Gris claro
    icono: "⬜"
    descripcion: "Pendiente de primera inspección"
    accion: "Incluir en próxima ronda"
    severidad: 0
```

---

> Navegación: [← Anterior](03-catalogo-multi-cultivo.md)[📑 Índice](README.md) | [Siguiente →](05-api-agronomia-precision.md)
