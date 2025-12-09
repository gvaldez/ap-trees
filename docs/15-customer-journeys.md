## 15. 🗺️ Customer Journeys - Flujos de Usuario

> **📊 Documentación detallada de los principales flujos de usuario** en AgroGrid, desde el onboarding hasta las operaciones diarias.

Este documento describe los customer journeys completos, incluyendo pasos, actores involucrados, puntos de decisión y métricas de éxito para cada flujo crítico del sistema.

---

## 15.1 Journey 1: Onboarding de Nuevo Cliente

**Objetivo:** Convertir un prospecto en cliente activo del sistema con su cuenta configurada.

**Duración estimada:** 15-30 minutos

**Actor principal:** Nuevo cliente (Propietario/Admin del Tenant)

**Punto de inicio:** Landing page o invitación comercial

**Punto final:** Primera sesión en el dashboard con perfil configurado

### 15.1.1 Diagrama de Flujo

```
┌──────────────────────────────────────────────────────────────┐
│                 JOURNEY 1: ONBOARDING                        │
└──────────────────────────────────────────────────────────────┘

[Landing Page] 
     │
     ├─► Origen: Marketing
     ├─► Origen: Referido
     └─► Origen: Prueba gratuita
     │
     ▼
┌─────────────────┐
│ Registro Inicial│
├─────────────────┤
│ • Email         │
│ • Contraseña    │
│ • Nombre        │
│ • Empresa       │
│ [Registrar]     │
└────────┬────────┘
         │
         ▼
    ┌────────┐
    │Verificar│ ────► Email enviado
    │ Email  │       "Confirma tu email..."
    └────┬───┘
         │
         ├──NO──► ⏳ Esperar verificación
         │        └─► Reenviar email
         │
         ▼ SÍ
┌──────────────────┐
│ Selección de Plan│
├──────────────────┤
│ ○ Starter        │
│   $49/mes        │
│                  │
│ ● Professional   │◄── Recomendado
│   $199/mes       │
│                  │
│ ○ Enterprise     │
│   Contactar      │
│                  │
│ Trial: 14 días ✓ │
│ [Continuar]      │
└────────┬─────────┘
         │
         ├──Plan Pago──► Método de pago
         │               └─► Validar tarjeta
         │
         ├──Trial──────► Omitir pago
         │
         ▼
┌────────────────────┐
│ Configuración      │
│ Inicial del Perfil│
├────────────────────┤
│ • País: [COL ▼]    │
│ • Timezone         │
│ • Moneda: COP      │
│ • Idioma: ES       │
│ • Tipo cultivo:    │
│   ☑ Aguacate       │
│   ☐ Cítricos       │
│   ☐ Durazno        │
│ [Guardar]          │
└─────────┬──────────┘
          │
          ▼
┌──────────────────┐
│   Primer Login   │
├──────────────────┤
│ Bienvenido!      │
│                  │
│ ¿Deseas un tour? │
│                  │
│ [Sí, mostrar]    │
│ [No, explorar]   │
└─────────┬────────┘
          │
          ├──Tour──► Wizard interactivo (3 min)
          │
          ▼
┌───────────────────┐
│ Dashboard Inicial │◄── ✅ JOURNEY COMPLETO
│ (Estado: Vacío)   │
│                   │
│ Siguiente paso:   │
│ "Crear tu primera │
│  finca" →         │
└───────────────────┘
```

### 15.1.2 Pantallas/Módulos Utilizados

1. **Landing page** (marketing)
2. **Formulario de registro**
3. **Confirmación de email**
4. **Selector de plan**
5. **Checkout/pago** (si aplica)
6. **Configuración de perfil**
7. **Tour guiado** (opcional)
8. **Dashboard principal**

### 15.1.3 Puntos de Decisión

| Punto | Decisión | Camino A | Camino B |
|-------|----------|----------|----------|
| **Verificación** | ¿Email verificado? | SÍ → Continuar | NO → Bloquear hasta verificar |
| **Plan** | ¿Qué plan elegir? | Trial 14 días | Pago inmediato |
| **Tour** | ¿Mostrar tour? | SÍ → Wizard | NO → Dashboard directo |

### 15.1.4 Posibles Errores y Recuperación

| Error | Causa | Solución |
|-------|-------|----------|
| Email ya existe | Usuario intenta registrarse dos veces | Mostrar "Ya tienes cuenta, ¿olvidaste contraseña?" |
| Email no llega | Spam, email incorrecto | Botón "Reenviar email" + verificar email correcto |
| Pago rechazado | Tarjeta inválida, fondos | Permitir cambiar método o iniciar trial |
| Sesión expira | Usuario tarda mucho | Guardar progreso, reanudar donde quedó |

### 15.1.5 Métricas de Éxito

- **Time to Value (TTV):** < 30 minutos desde registro hasta primer login
- **Completion Rate:** > 70% de registros completan onboarding
- **Email verification rate:** > 90%
- **Trial to paid conversion:** > 25%
- **Drop-off principal:** Pago (optimizar este paso)

---

## 15.2 Journey 2: Setup del Campo Operativo ⭐ CRÍTICO

**Objetivo:** Configurar la estructura completa de la operación agrícola en el sistema.

**Duración estimada:** 2-4 horas (dependiendo del tamaño)

**Actor principal:** Propietario o Gerente de Finca

**Actor secundario:** Agrónomo (asesoría)

**Punto de inicio:** Dashboard vacío con mensaje "Crear tu primera finca"

**Punto final:** Finca completamente configurada con árboles listos para inspección

> ⚠️ **Este es el journey MÁS CRÍTICO del sistema.** El éxito del cliente depende de completar este setup correctamente. Un mal setup inicial causa frustración y abandono.

### 15.2.1 Diagrama de Flujo Detallado

```
┌────────────────────────────────────────────────────────────────┐
│         JOURNEY 2: SETUP DEL CAMPO OPERATIVO                   │
│              (El más importante del sistema)                   │
└────────────────────────────────────────────────────────────────┘

[Dashboard Vacío]
     │
     ▼
┌──────────────────────────────────────────┐
│ PASO 1: Definir Cultivo Principal        │
├──────────────────────────────────────────┤
│ ¿Qué cultivas?                           │
│                                          │
│ ┌────────┐ ┌────────┐ ┌────────┐       │
│ │   🥑   │ │   🍊   │ │   🍑   │       │
│ │Aguacate│ │Cítricos│ │Durazno │       │
│ └────────┘ └────────┘ └────────┘       │
│ ┌────────┐ ┌────────┐ ┌────────┐       │
│ │   🍎   │ │   🥭   │ │  Otro  │       │
│ │Manzana │ │ Mango  │ │[Buscar]│       │
│ └────────┘ └────────┘ └────────┘       │
│                                          │
│ Cultivo seleccionado: Aguacate 🥑       │
│ [Siguiente]                              │
└───────────────────┬──────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────┐
│ PASO 2: Crear la Finca                   │
├──────────────────────────────────────────┤
│ Nombre: [Finca El Paraíso          ]    │
│                                          │
│ 📍 Ubicación:                            │
│ ├─ [Buscar en mapa...]                  │
│ └─ O ingresar coordenadas:              │
│    Lat: [4.6097         ]               │
│    Lon: [-74.0817       ]               │
│                                          │
│ 📏 Área total:                           │
│ ├─ Valor: [25           ]               │
│ └─ Unidad: [Hectáreas ▼]                │
│                                          │
│ 🌡️ Clima típico: [Templado ▼]          │
│ 🌧️ Precipitación anual: [1200 mm]      │
│                                          │
│ [Atrás] [Siguiente]                      │
└───────────────────┬──────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────┐
│ PASO 3: Definir Sectores                 │
├──────────────────────────────────────────┤
│ Los sectores dividen tu finca en zonas   │
│                                          │
│ Sectores actuales (2):                   │
│ ┌──────────────────────────────────────┐ │
│ │ ✏️ Sector Norte                      │ │
│ │    10 ha | Plano | Riego por goteo   │ │
│ │    [Editar] [Eliminar]                │ │
│ ├──────────────────────────────────────┤ │
│ │ ✏️ Sector Sur                        │ │
│ │    15 ha | Pendiente | Aspersión     │ │
│ │    [Editar] [Eliminar]                │ │
│ └──────────────────────────────────────┘ │
│                                          │
│ [+ Agregar Sector]                       │
│ [Atrás] [Siguiente]                      │
└───────────────────┬──────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────┐
│ PASO 4: Crear Lotes dentro de Sectores           │
├──────────────────────────────────────────────────┤
│ Los lotes son subdivisiones con el mismo cultivo │
│                                                   │
│ Sector: Sector Norte (10 ha)                     │
│                                                   │
│ Lotes en este sector (3):                        │
│ ┌────────────────────────────────────────────┐   │
│ │ Lote A-1                                   │   │
│ │ ├─ Cultivo: Aguacate Hass                 │   │
│ │ ├─ Área: 3.5 ha                            │   │
│ │ ├─ Plantación: 2018                        │   │
│ │ ├─ Distancias: 6m x 6m                     │   │
│ │ ├─ Árboles estimados: 972                  │   │
│ │ └─ [Editar] [Eliminar]                     │   │
│ ├────────────────────────────────────────────┤   │
│ │ Lote A-2                                   │   │
│ │ ├─ Cultivo: Aguacate Hass                 │   │
│ │ ├─ Área: 3.2 ha                            │   │
│ │ ├─ Distancias: 6m x 6m                     │   │
│ │ └─ Árboles estimados: 889                  │   │
│ ├────────────────────────────────────────────┤   │
│ │ Lote A-3                                   │   │
│ │ ├─ Cultivo: Aguacate Hass                 │   │
│ │ ├─ Área: 3.3 ha                            │   │
│ │ └─ Árboles estimados: 917                  │   │
│ └────────────────────────────────────────────┘   │
│                                                   │
│ [+ Agregar Lote]                                  │
│ [Atrás] [Siguiente]                               │
└────────────────────┬──────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────┐
│ PASO 4.1: Configurar Lote A-1                       │
├──────────────────────────────────────────────────────┤
│ Nombre: [Lote A-1                           ]       │
│                                                      │
│ 🥑 Cultivo y Variedad:                              │
│ ├─ Cultivo: [Aguacate ▼]                           │
│ └─ Variedad: [Hass ▼]                              │
│                                                      │
│ 📐 Dimensiones del lote:                            │
│ ├─ Filas: [27              ]                       │
│ ├─ Columnas: [36           ]                       │
│ ├─ Total árboles: 972 (calculado)                  │
│ │                                                    │
│ └─ O especificar área:                              │
│    └─ 3.5 ha (auto-calcula filas/columnas)         │
│                                                      │
│ 📏 Distancias entre árboles:                        │
│ ├─ Entre filas: [6.0 ] metros                      │
│ └─ Entre columnas: [6.0 ] metros                    │
│                                                      │
│ 📅 Fecha de plantación: [2018-03-15]                │
│                                                      │
│ 💧 Sistema de riego:                                │
│ └─ Tipo: [Goteo por gotero ▼]                      │
│                                                      │
│ [Guardar Lote] [Cancelar]                           │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ PASO 5: Registrar Árboles                            │
├──────────────────────────────────────────────────────┤
│ ¿Cómo quieres agregar los árboles del Lote A-1?     │
│                                                      │
│ Método 1: ⚡ GENERACIÓN AUTOMÁTICA (Recomendado)    │
│ ┌──────────────────────────────────────────────┐   │
│ │ • Genera cuadrícula perfecta                  │   │
│ │ • Usa dimensiones ya configuradas (27x36)     │   │
│ │ • Asigna códigos secuenciales (A1-001...)     │   │
│ │ • Calcula coordenadas GPS automáticas         │   │
│ │ • Genera códigos QR para cada árbol           │   │
│ │                                                │   │
│ │ Total a generar: 972 árboles                  │   │
│ │ Tiempo estimado: ~2 minutos                   │   │
│ │                                                │   │
│ │ [✨ Generar Automáticamente]                  │   │
│ └──────────────────────────────────────────────┘   │
│                                                      │
│ Método 2: 📋 IMPORTACIÓN MASIVA                     │
│ ┌──────────────────────────────────────────────┐   │
│ │ • Sube un archivo CSV o Excel                 │   │
│ │ • Debe incluir: fila, columna, código         │   │
│ │ • Opcional: lat, lon, fecha_plantacion        │   │
│ │                                                │   │
│ │ [📥 Descargar plantilla]                      │   │
│ │ [📤 Subir archivo]                            │   │
│ └──────────────────────────────────────────────┘   │
│                                                      │
│ Método 3: ✍️ REGISTRO INDIVIDUAL                    │
│ ┌──────────────────────────────────────────────┐   │
│ │ • Agregar árbol por árbol manualmente         │   │
│ │ • Útil para lotes irregulares o pequeños      │   │
│ │ • Puedes usar app móvil en campo              │   │
│ │                                                │   │
│ │ [+ Agregar Árbol Individual]                  │   │
│ └──────────────────────────────────────────────┘   │
│                                                      │
│ [Atrás] [Continuar]                                  │
└───────────────────┬──────────────────────────────────┘
                    │
                    ├──Método 1─► Generación automática
                    │              └─► ⏳ Procesando...
                    │                  └─► ✅ 972 árboles creados
                    │
                    ├──Método 2─► Upload CSV
                    │              └─► Validar formato
                    │                  ├─► ❌ Errores → Corregir
                    │                  └─► ✅ OK → Importar
                    │
                    └──Método 3─► Form individual
                                   └─► Repetir para cada árbol
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ PASO 5.1: Vista Previa de Árboles Generados          │
├──────────────────────────────────────────────────────┤
│ ✅ Se generaron 972 árboles en Lote A-1             │
│                                                      │
│ Vista de Cuadrícula:                                 │
│ ┌──────────────────────────────────────────────┐   │
│ │  Col→ 1   2   3   4  ... 34  35  36          │   │
│ │ F1│  ⚪  ⚪  ⚪  ⚪ ... ⚪  ⚪  ⚪              │   │
│ │ F2│  ⚪  ⚪  ⚪  ⚪ ... ⚪  ⚪  ⚪              │   │
│ │ F3│  ⚪  ⚪  ⚪  ⚪ ... ⚪  ⚪  ⚪              │   │
│ │...│  ...  ...  ...  ...  ...  ...  ...        │   │
│ │F27│  ⚪  ⚪  ⚪  ⚪ ... ⚪  ⚪  ⚪              │   │
│ └──────────────────────────────────────────────┘   │
│                                                      │
│ Códigos asignados:                                   │
│ ├─ Formato: LOTE-FILA-COL                          │
│ ├─ Primer árbol: A1-F01-C01                         │
│ ├─ Último árbol: A1-F27-C36                         │
│ └─ QR generados: 972/972 ✅                         │
│                                                      │
│ [Ver Detalle de Árbol] [Editar Códigos]             │
│ [Atrás] [Siguiente]                                  │
└───────────────────┬──────────────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ PASO 6: Configurar Infraestructura Hídrica (Opcional)│
├──────────────────────────────────────────────────────┤
│ ¿Tu finca tiene sistema de riego automatizado?      │
│                                                      │
│ ○ Sí, tengo riego automatizado                      │
│   └─► Continuar con configuración                   │
│                                                      │
│ ● No, riego manual o sin riego                      │
│   └─► Omitir este paso                              │
│                                                      │
│ [Si marcaste SÍ, configurar:]                        │
│                                                      │
│ Válvulas/Sectores de riego en Lote A-1:             │
│ ┌──────────────────────────────────────────────┐   │
│ │ Válvula 1: Zona A (Filas 1-9)                 │   │
│ │ Válvula 2: Zona B (Filas 10-18)               │   │
│ │ Válvula 3: Zona C (Filas 19-27)               │   │
│ └──────────────────────────────────────────────┘   │
│                                                      │
│ [+ Agregar Válvula] [Configurar Horarios]            │
│ [Omitir] [Siguiente]                                 │
└───────────────────┬──────────────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ PASO 7: Invitar Usuarios del Equipo                  │
├──────────────────────────────────────────────────────┤
│ Invita a tu equipo para empezar a colaborar         │
│                                                      │
│ Invitaciones:                                        │
│ ┌──────────────────────────────────────────────┐   │
│ │ Email: [agronoma@finca.com        ]           │   │
│ │ Rol: [Agrónomo ▼]                             │   │
│ │ [+ Agregar]                                    │   │
│ ├──────────────────────────────────────────────┤   │
│ │ Email: [supervisor@finca.com      ]           │   │
│ │ Rol: [Supervisor ▼]                           │   │
│ │ [+ Agregar]                                    │   │
│ └──────────────────────────────────────────────┘   │
│                                                      │
│ [Enviar Invitaciones] [Omitir, invitar después]     │
│ [Atrás] [Siguiente]                                  │
└───────────────────┬──────────────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ PASO 8: Configurar Alertas y Notificaciones          │
├──────────────────────────────────────────────────────┤
│ ¿Qué alertas quieres recibir?                       │
│                                                      │
│ ☑ Problemas críticos de salud (Recomendado)         │
│ ☑ Análisis de drone completado                      │
│ ☑ Tareas pendientes de aprobación                   │
│ ☐ Resumen diario de actividad                       │
│ ☐ Alertas de riego                                  │
│                                                      │
│ Canal preferido:                                     │
│ ☑ Email: propietario@finca.com                      │
│ ☑ Push (app móvil)                                  │
│ ☐ SMS: +57 300 123 4567                             │
│                                                      │
│ [Guardar Configuración] [Omitir]                     │
│ [Atrás] [Finalizar Setup]                            │
└───────────────────┬──────────────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ PASO 9 (OPCIONAL): Primera Inspección de Prueba      │
├──────────────────────────────────────────────────────┤
│ ¡Genial! Tu finca está configurada.                 │
│                                                      │
│ ¿Quieres hacer una inspección de prueba?            │
│                                                      │
│ Te guiaremos para inspeccionar 5 árboles como       │
│ ejemplo y familiarizarte con el sistema.             │
│                                                      │
│ [📱 Sí, hacer inspección de prueba]                 │
│ [⏭️ No, ir directo al dashboard]                    │
└───────────────────┬──────────────────────────────────┘
                    │
                    ├──Prueba─► Wizard inspección guiada
                    │            └─► Inspeccionar 5 árboles
                    │                └─► ✅ Prueba completa
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ ✅ SETUP COMPLETO - Dashboard Operativo              │
├──────────────────────────────────────────────────────┤
│ 🎉 ¡Felicitaciones! Tu finca está lista             │
│                                                      │
│ Resumen de lo configurado:                           │
│ ✓ 1 Finca: Finca El Paraíso (25 ha)                 │
│ ✓ 2 Sectores: Norte (10 ha), Sur (15 ha)            │
│ ✓ 3 Lotes: A-1, A-2, A-3                            │
│ ✓ 2,778 Árboles registrados                         │
│ ✓ 3 Usuarios invitados                               │
│ ✓ Alertas configuradas                               │
│                                                      │
│ Próximos pasos sugeridos:                            │
│ 1. [Descargar e imprimir códigos QR]                │
│ 2. [Realizar primera inspección de campo]           │
│ 3. [Configurar primera tarea]                        │
│                                                      │
│ [Ir al Dashboard] [Ver Tour del Sistema]             │
└──────────────────────────────────────────────────────┘
```

### 15.2.2 Actores Involucrados

| Actor | Rol en el Journey | Participación |
|-------|-------------------|---------------|
| **Propietario** | Líder del setup | 100% - Toma decisiones principales |
| **Gerente** | Apoyo operativo | 70% - Ayuda con detalles técnicos |
| **Agrónomo** | Asesoría técnica | 40% - Valida configuración de cultivos |
| **Equipo de Ventas** (AgroGrid) | Soporte inicial | 20% - Onboarding call opcional |
| **Equipo de Soporte** (AgroGrid) | Asistencia técnica | 10% - Solo si hay problemas |

### 15.2.3 Pantallas/Módulos Utilizados

1. **Wizard de setup** (8 pasos)
2. **Módulo de Fincas** (crear/editar)
3. **Módulo de Lotes** (configuración)
4. **Generador de cuadrícula**
5. **Importador CSV**
6. **Módulo de infraestructura hídrica**
7. **Gestión de usuarios** (invitaciones)
8. **Configuración de alertas**
9. **Dashboard principal**

### 15.2.4 Puntos de Decisión Críticos

| Paso | Decisión | Impacto | Recomendación |
|------|----------|---------|---------------|
| **Paso 5** | ¿Método de registro de árboles? | Alto - Afecta tiempo de setup | Generación automática para lotes regulares |
| **Paso 5** | ¿Asignar códigos QR? | Medio - Necesario para inspección móvil | SÍ, siempre generar QR |
| **Paso 6** | ¿Configurar riego ahora? | Bajo - Se puede agregar después | Opcional, no bloquear avance |
| **Paso 7** | ¿Invitar usuarios ahora? | Medio - Afecta colaboración | Invitar al menos 1 agrónomo |
| **Paso 9** | ¿Hacer inspección de prueba? | Alto - Reduce curva de aprendizaje | SÍ, recomendado fuertemente |

### 15.2.5 Posibles Errores y Recuperación

| Error | Causa | Solución | Prevención |
|-------|-------|----------|------------|
| **Coordenadas GPS erróneas** | Usuario ingresa mal ubicación | Validar con mapa interactivo | Mostrar preview en mapa |
| **Dimensiones inconsistentes** | Filas x columnas no coincide con área | Alertar discrepancia, sugerir ajuste | Calculadora automática |
| **Códigos duplicados** | Importación CSV con IDs repetidos | Validar unicidad antes de importar | Validación pre-import |
| **CSV mal formateado** | Columnas incorrectas | Mostrar errores específicos por línea | Plantilla descargable |
| **Generación de árboles toma mucho** | Lote muy grande (>5000 árboles) | Procesar en background + notificar | Advertir si > 3000 árboles |
| **Usuario abandona a mitad** | Proceso muy largo | Auto-guardar progreso cada paso | "Guardar y continuar después" |
| **No tiene datos GPS** | Finca sin levantamiento topográfico | Permitir omitir GPS, agregar después | Marcar como "Pendiente GPS" |

### 15.2.6 Validaciones por Paso

**Paso 2 - Crear Finca:**
- ✓ Nombre único dentro del tenant
- ✓ Coordenadas GPS válidas (-90 a 90, -180 a 180)
- ✓ Área > 0

**Paso 4 - Crear Lote:**
- ✓ Nombre único dentro del sector
- ✓ Cultivo seleccionado (requerido)
- ✓ Distancias > 0
- ✓ Filas y columnas > 0
- ✓ Fecha plantación no futura

**Paso 5 - Registrar Árboles:**
- ✓ Códigos únicos por lote
- ✓ Si CSV: validar todas las columnas requeridas
- ✓ Coordenadas dentro de los límites de la finca
- ✓ Máximo 10,000 árboles por lote

### 15.2.7 Métricas de Éxito

| Métrica | Target | Crítico |
|---------|--------|---------|
| **Completion Rate** | > 85% | ✅ Más importante |
| **Time to Complete** | < 3 horas | ✅ |
| **Abandonment Rate en Paso 5** | < 10% | ✅ Paso más crítico |
| **Usuarios que usan gen. automática** | > 70% | ⚠️ |
| **Usuarios que hacen inspección prueba** | > 60% | ⚠️ |
| **Usuarios que invitan al equipo** | > 50% | ⚠️ |
| **% que regresan después de setup** | > 90% | ✅ Indicador de éxito |

### 15.2.8 Tips para el Equipo de Producto

> 💡 **Este journey es el más importante de todo el sistema.** Un setup exitoso predice fuertemente la retención del cliente.

- Ofrecer **onboarding call** personalizado para clientes Enterprise
- **Salvar progreso automáticamente** en cada paso
- Permitir **"setup incompleto"** y reanudar después
- Mostrar **barra de progreso clara** (Paso 3 de 9)
- Incluir **help tooltips** en cada campo
- Ofrecer **videos tutoriales** de 1-2 min por paso
- Crear **plantillas de finca** pre-configuradas por cultivo
- Implementar **validación en tiempo real** (evitar errores al final)

---

## 15.3 Journey 3: Operación Diaria - Inspección

**Objetivo:** Realizar inspecciones rutinarias de árboles en campo y registrar hallazgos.

**Duración estimada:** Jornada completa (6-8 horas)

**Actor principal:** Operario de campo

**Actores secundarios:** Supervisor (asigna tareas), Agrónomo (revisa hallazgos)

**Punto de inicio:** Operario llega al campo con app móvil

**Punto final:** Tareas completadas y sincronizadas al servidor

### 15.3.1 Diagrama de Flujo

```
┌──────────────────────────────────────────────────────────┐
│          JOURNEY 3: OPERACIÓN DIARIA - INSPECCIÓN        │
└──────────────────────────────────────────────────────────┘

[Operario llega al campo]
     │
     ▼
┌────────────────────┐
│ Inicio de Jornada  │
├────────────────────┤
│ App móvil AgroGrid │
│                    │
│ Login:             │
│ • Email/Usuario    │
│ • Contraseña       │
│ • [Entrar]         │
└─────────┬──────────┘
          │
          ▼
┌─────────────────────┐
│ Home - Tareas del Día│
├─────────────────────┤
│ Hola José! 👷        │
│                     │
│ Tareas hoy:         │
│ ┌─────────────────┐ │
│ │ ☐ Inspeccionar  │ │
│ │   Lote A-3      │ │
│ │   150 árboles   │ │
│ │   ⏱️ 4h est.    │ │
│ │   [Iniciar]     │ │
│ └─────────────────┘ │
│ ┌─────────────────┐ │
│ │ ☐ Aplicar       │ │
│ │   Fungicida     │ │
│ │   Sector Norte  │ │
│ │   ⏱️ 3h est.    │ │
│ └─────────────────┘ │
│                     │
│ [Ver Todas]         │
└─────────┬───────────┘
          │
          ▼ [Iniciar tarea]
┌──────────────────────┐
│ Tarea: Inspeccionar  │
│ Lote A-3             │
├──────────────────────┤
│ Árboles: 0 / 150     │
│ Progreso: [▱▱▱▱▱]0%  │
│                      │
│ Métodos:             │
│ • [📷 Escanear QR]   │
│ • [🗺️ Ver en Mapa]  │
│ • [📋 Lista]         │
│                      │
│ [Pausar] [Finalizar] │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────────────┐
│ Modo: Escanear QR            │
├──────────────────────────────┤
│ ┌──────────────────────────┐ │
│ │                          │ │
│ │  📷 CÁMARA ACTIVA        │ │
│ │                          │ │
│ │     [  QR FRAME  ]       │ │
│ │                          │ │
│ │  Apunta al código QR     │ │
│ │  del árbol               │ │
│ │                          │ │
│ └──────────────────────────┘ │
│                              │
│ O ingresar código:           │
│ [A3-F05-C12________]         │
└──────────────┬───────────────┘
               │
               ▼ QR escaneado: A3-F05-C12
┌────────────────────────────────────────┐
│ Árbol: A3-F05-C12                      │
│ Fila 5, Columna 12                     │
├────────────────────────────────────────┤
│ Última inspección: Hace 7 días         │
│ Estado anterior: 🟢 Bueno              │
│                                        │
│ ¿Cómo se ve este árbol hoy?            │
│                                        │
│ ┌────────┐ ┌────────┐ ┌────────┐     │
│ │   🟢   │ │   🟡   │ │   🔴   │     │
│ │ Bueno  │ │ Regular│ │  Malo  │     │
│ └────────┘ └────────┘ └────────┘     │
│                                        │
│ [Tomar Foto] (opcional)                │
│ [🎙️ Nota de voz] (opcional)           │
│                                        │
│ [Guardar y Siguiente]                  │
└────────────┬───────────────────────────┘
             │
             ├─ Estado: Bueno ─► Guardar simple
             │                   └─► Siguiente árbol
             │
             └─ Estado: Malo/Regular ─► ¿Tiene problema?
                                        │
                                        ▼
┌────────────────────────────────────────────────┐
│ Registrar Hallazgo/Problema                    │
├────────────────────────────────────────────────┤
│ Árbol: A3-F05-C12 en estado 🔴 Malo            │
│                                                │
│ Tipo de problema:                              │
│ ☐ Hojas amarillas                              │
│ ☑ Hojas manchadas                              │
│ ☐ Caída de hojas                               │
│ ☐ Frutos dañados                               │
│ ☐ Plaga visible                                │
│ ☐ Ramas secas                                  │
│ ☐ Otro                                         │
│                                                │
│ Severidad:                                     │
│ ○ Leve  ● Moderada  ○ Grave                    │
│                                                │
│ 📷 Fotos (requerido):                          │
│ [Tomar Foto 1] [Tomar Foto 2] [Tomar Foto 3]  │
│                                                │
│ 🎙️ Descripción (opcional):                    │
│ [Grabar nota de voz]                           │
│ O escribir: [__________________________]       │
│                                                │
│ [Guardar Hallazgo]                             │
└────────────────────┬───────────────────────────┘
                     │
                     ▼
┌──────────────────────────────┐
│ ✅ Hallazgo registrado       │
├──────────────────────────────┤
│ Se notificará al supervisor  │
│ y al agrónomo.               │
│                              │
│ [OK - Continuar inspección]  │
└──────────────┬───────────────┘
               │
               ▼
    [Repetir para cada árbol]
               │
               ▼
┌──────────────────────────────────┐
│ Progreso: 150 / 150 ✅           │
├──────────────────────────────────┤
│ ¡Tarea completada!               │
│                                  │
│ Resumen:                         │
│ ├─ Árboles inspeccionados: 150  │
│ ├─ Buenos: 142 (94.7%)           │
│ ├─ Regulares: 5 (3.3%)           │
│ ├─ Malos: 3 (2.0%)               │
│ └─ Problemas reportados: 8       │
│                                  │
│ Tiempo: 3h 47min                 │
│                                  │
│ [Finalizar Tarea]                │
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│ Sincronización                   │
├──────────────────────────────────┤
│ ⏳ Subiendo datos...             │
│ • 150 inspecciones               │
│ • 24 fotos                       │
│ • 3 notas de voz                 │
│                                  │
│ [▓▓▓▓▓▓▓▓▓▓] 87%                 │
└──────────────┬───────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│ ✅ Sincronizado                  │
├──────────────────────────────────┤
│ Todos los datos se guardaron     │
│ en el servidor.                  │
│                                  │
│ Supervisor validará tu trabajo.  │
│                                  │
│ [Volver a Inicio]                │
│ [Ver Siguiente Tarea]            │
└──────────────────────────────────┘
```

### 15.3.2 Actores y Responsabilidades

| Actor | Inicio | Durante | Fin |
|-------|--------|---------|-----|
| **Operario** | Recibe tarea | Ejecuta inspección | Marca como completada |
| **Supervisor** | Asigna tarea | Monitorea progreso | Valida trabajo |
| **Agrónomo** | - | Revisa hallazgos críticos | Prescribe tratamiento si necesario |
| **Sistema** | Envía notificación | Guarda datos offline | Sincroniza al servidor |

### 15.3.3 Métricas de Éxito

- **Completion Rate:** > 95% de tareas asignadas se completan
- **Tiempo promedio por árbol:** 1-2 minutos
- **% de inspecciones con fotos:** > 80% (para problemas detectados)
- **Tasa de sincronización exitosa:** > 99%
- **% de operarios usando app móvil:** 100%

---

## 15.4 Journey 4: Gestión de Alerta/Problema

**Objetivo:** Desde la detección de un problema hasta su resolución completa.

**Duración estimada:** 5-14 días (depende de severidad)

**Actores:** Sistema, Agrónomo, Supervisor, Operario

### 15.4.1 Diagrama de Flujo

```
┌────────────────────────────────────────────────────┐
│       JOURNEY 4: GESTIÓN DE ALERTA/PROBLEMA       │
└────────────────────────────────────────────────────┘

[Detección]
     │
     ├─── Origen 1: Automática (Drone, ML)
     ├─── Origen 2: Manual (Operario inspección)
     └─── Origen 3: Sistema (Umbral fenología)
     │
     ▼
┌─────────────────────────────┐
│ ALERTA GENERADA             │
├─────────────────────────────┤
│ ID: #ALT-2847               │
│ Tipo: Phytophthora (sospecha)│
│ Severidad: 🔴 Alta          │
│ Área afectada:              │
│ └─ Lote A-3, F5-8, C12-15   │
│ Árboles: 15                 │
│ Fecha: 2025-12-08 10:30     │
└─────────────┬───────────────┘
              │
              ▼
┌────────────────────────────────────────┐
│ Notificación a Responsables            │
├────────────────────────────────────────┤
│ 📧 Email → Agrónoma Ana                │
│ 📱 Push  → Supervisor Pedro            │
│ 📱 Push  → Gerente María               │
│ 📧 Email → Propietario Juan (copia)    │
└────────────────┬───────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────┐
│ Agrónoma REVISA Alerta                 │
├────────────────────────────────────────┤
│ 1. Ve detalles en dashboard            │
│ 2. Revisa cuadrícula afectada          │
│ 3. Ve fotos subidas por operario       │
│ 4. Analiza imágenes de drone (si hay)  │
│ 5. Revisa historial de área            │
└────────────────┬───────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────┐
│ DIAGNÓSTICO                            │
├────────────────────────────────────────┤
│ Agrónoma confirma:                     │
│ "Phytophthora cinnamomi - Alta"        │
│                                        │
│ Diagnóstico registrado:                │
│ • Plaga/enfermedad: Phytophthora       │
│ • Severidad confirmada: Alta           │
│ • Nivel de propagación: Focal          │
│ • Riesgo si no se trata: Muy Alto     │
│ • Árboles adicionales en riesgo: 45    │
│                                        │
│ [Guardar Diagnóstico]                  │
│ [Crear Tratamiento]                    │
└────────────────┬───────────────────────┘
                 │
                 ▼
┌──────────────────────────────────────────┐
│ PLANIFICACIÓN DE TRATAMIENTO             │
├──────────────────────────────────────────┤
│ Producto: Fosetil-Al 80% WP              │
│ Dosis: 3 kg/ha                           │
│                                          │
│ Aplicación en:                           │
│ ├─ 15 árboles afectados (curativa)       │
│ └─ 45 árboles perímetro (preventiva)     │
│ Total árboles: 60                        │
│                                          │
│ Dosis por árbol: 450g                    │
│ Total producto: 27 kg                    │
│                                          │
│ Método: Drench (suelo)                   │
│ Volumen agua: 10 L/árbol                 │
│ Total agua: 600 L                        │
│                                          │
│ Fecha programada: 2025-12-10             │
│ Responsable: Supervisor Pedro            │
│                                          │
│ [Solicitar Aprobación Gerente]           │
└──────────────────┬───────────────────────┘
                   │
                   ▼
┌────────────────────────────────────────┐
│ Gerente APRUEBA Tratamiento            │
├────────────────────────────────────────┤
│ Costo estimado: $450 USD               │
│ Justificación: Crítico                 │
│ Impacto si no se trata: -$15,000       │
│                                        │
│ Estado: ✅ APROBADO                    │
│ Autorizado por: María González         │
└────────────────┬───────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────┐
│ EJECUCIÓN DE APLICACIÓN                │
├────────────────────────────────────────┤
│ Tarea asignada a: Operarios            │
│ Supervisado por: Pedro                 │
│                                        │
│ [Operarios aplican tratamiento]        │
│ • Registran aplicación árbol x árbol   │
│ • Confirman dosis aplicada             │
│ • Toman fotos post-aplicación          │
│ • Registran hora inicio/fin            │
│                                        │
│ Estado: ✅ COMPLETADO                  │
│ Fecha: 2025-12-10 14:30                │
│ Duración: 3h 15min                     │
└────────────────┬───────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────┐
│ SEGUIMIENTO (día 7)                    │
├────────────────────────────────────────┤
│ Sistema crea tarea automática:         │
│ "Inspeccionar árboles tratados"        │
│                                        │
│ Agrónoma revisa:                       │
│ • ¿Mejoró el estado?                   │
│ • ¿Se detuvo propagación?              │
│ • ¿Necesita segunda aplicación?        │
└────────────────┬───────────────────────┘
                 │
                 ├─ Mejoró ──► Cerrar alerta ✅
                 │
                 └─ No mejoró ──► Repetir tratamiento
                                  └─► Escalar a experto
                 │
                 ▼
┌────────────────────────────────────────┐
│ VERIFICACIÓN FINAL (día 14)            │
├────────────────────────────────────────┤
│ Inspección de verificación             │
│ • Estado de árboles: Recuperados       │
│ • Propagación: Controlada              │
│ • Nuevos casos: 0                      │
│                                        │
│ [Cerrar Alerta]                        │
└────────────────┬───────────────────────┘
                 │
                 ▼
┌────────────────────────────────────────┐
│ ✅ ALERTA CERRADA                      │
├────────────────────────────────────────┤
│ ID: #ALT-2847                          │
│ Estado: Resuelta                       │
│ Duración total: 12 días                │
│ Costo tratamiento: $437                │
│ Árboles recuperados: 15/15             │
│                                        │
│ Lecciones aprendidas:                  │
│ • Detectado temprano (éxito)           │
│ • Tratamiento efectivo                 │
│ • Prevención evitó propagación         │
│                                        │
│ [Archivar]                             │
└────────────────────────────────────────┘
```

### 15.4.2 Métricas de Éxito

- **Tiempo de primera respuesta:** < 2 horas
- **Tiempo total de resolución:** < 14 días
- **Tasa de éxito de tratamiento:** > 85%
- **% de alertas que no se propagan:** > 90%
- **Costo promedio por alerta:** Tracked pero variable

---

## 15.5 Journey 5: Ciclo de Cosecha

**Objetivo:** Planificar, ejecutar y analizar la cosecha de un lote.

**Duración:** 1-2 semanas (por lote)

**Actores:** Gerente, Supervisor, Operarios

### 15.5.1 Flujo Simplificado

```
Planificación → Asignación Cuadrillas → Ejecución Cosecha
     ↓               ↓                         ↓
Determinar       Asignar áreas           Registrar por árbol
madurez         y horarios              cantidad y calidad
     ↓               ↓                         ↓
Coordinar      Briefing diario          Control de calidad
logística      con operarios            en campo
     ↓               ↓                         ↓
Preparar      Monitoreo de           Transporte y pesaje
contenedores  progreso real-time     en centro acopio
     ↓               ↓                         ↓
[Cosecha finalizada] → Análisis de Rendimiento → Reportes
```

**Métricas clave:**
- Kg cosechados por árbol
- Kg por hectárea
- % por categoría de calidad
- Eficiencia de cuadrilla (kg/persona/hora)
- Comparación vs años anteriores

---

## 15.6 Journey 6: Análisis con Drone

**Objetivo:** Capturar, procesar y actuar sobre imágenes aéreas multiespectrales.

**Duración:** 2-3 días (captura a resultados)

**Actores:** Operador de Drone, Agrónomo, Sistema (IA)

### 15.6.1 Flujo Simplificado

```
Programación de Vuelo → Ejecución en Campo → Upload Imágenes
        ↓                      ↓                    ↓
   Definir área          Volar con drone      Subir 800+ fotos
   y parámetros          capturar imágenes    al servidor
        ↓                      ↓                    ↓
  Seleccionar           GPS tracking         Validación formato
  resolución            de cobertura         (JPG, TIFF, DNG)
        ↓                      ↓                    ↓
[Upload completo] → Procesamiento Automático → Generación Mapas
                           ↓                          ↓
                    Crear ortomosaico           NDVI, GNDVI
                    Calcular índices            Clasificación
                           ↓                          ↓
                    [Análisis ML/IA] ──► Detección Anomalías
                           ↓
                    Generación de Alertas → Revisión Agrónomo
                           ↓                          ↓
                    23 árboles con NDVI<0.6    Confirmar/Descartar
                           ↓                          ↓
                    [Alertas confirmadas] → Crear Tareas Inspección
```

**Métricas clave:**
- Tiempo procesamiento: < 2 horas para 800 imágenes
- Precisión detección: > 90%
- Alertas verdaderas vs falsos positivos: Ratio > 4:1
- Árboles monitoreados por vuelo: 1,000-5,000

---

## 📊 Resumen de Journeys - Vista Comparativa

| Journey | Frecuencia | Duración | Criticidad | Actores principales |
|---------|------------|----------|------------|---------------------|
| **1. Onboarding** | Una vez | 30 min | 🔴 Crítico | Propietario |
| **2. Setup Campo** | Una vez | 2-4 h | 🔴🔴🔴 MUY Crítico | Propietario, Gerente |
| **3. Inspección** | Diaria | 6-8 h | 🟡 Alta | Operario, Supervisor |
| **4. Gestión Alerta** | Según necesidad | 5-14 días | 🔴 Crítico | Agrónomo, Equipo |
| **5. Cosecha** | 1-3x/año | 1-2 semanas | 🟡 Alta | Gerente, Operarios |
| **6. Análisis Drone** | Mensual | 2-3 días | 🟢 Media | Operador, Agrónomo |

---

## 🎯 KPIs Globales de Customer Journeys

### Onboarding y Activación
- **Time to First Value:** < 4 horas (desde registro hasta setup completo)
- **Activation Rate:** > 80% (completan setup campo)
- **7-day Retention:** > 60%
- **30-day Retention:** > 70%

### Uso del Sistema
- **DAU/MAU Ratio:** > 40% (usuarios activos diarios/mensuales)
- **Feature Adoption:**
  - Vista cuadrícula: 100%
  - App móvil: > 80%
  - Análisis drone: > 30%
  - Reportes: > 60%

### Satisfacción
- **NPS (Net Promoter Score):** > 50
- **CSAT (Customer Satisfaction):** > 4.5/5
- **Support Tickets por Cliente/Mes:** < 2

---

## 📚 Documentos Relacionados

- [Módulo Usuarios](14-modulo-usuarios.md) - Roles y permisos
- [App Móvil de Campo](modulos/07-07-app-movil-campo.md) - Funcionalidades móviles
- [Wireframes](16-wireframes.md) - Diseño de pantallas

---

> Navegación: [← Anterior](14-modulo-usuarios.md) | [📑 Índice](README.md) | [Siguiente →](16-wireframes.md)
