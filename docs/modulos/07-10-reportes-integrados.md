### 7.10 📊 Reportes Integrados

> **Reportes que conectan todos los módulos para visión completa del negocio**

## 📑 Índice de Reportes

| # | Reporte | Descripción |
|---|---------|-------------|
| 1 | [📊 Estado de Resultados](#1--estado-de-resultados-por-temporada) | Estado financiero completo de la temporada |
| 2 | [🌿 Salud del Cultivo](#2--reporte-de-salud-del-cultivo) | Estado sanitario por finca/lote/sector con tendencias |
| 3 | [🍎 Producción y Cosecha](#3--reporte-de-producción-y-cosecha) | Kg cosechados, rendimiento y calidad |
| 4 | [💉 Aplicaciones Fitosanitarias](#4--reporte-de-aplicaciones-fitosanitarias) | Historial de tratamientos y eficacia |
| 5 | [💰 Rentabilidad por Árbol](#5--reporte-de-rentabilidad-por-árbol) | ROI individual y distribución de rentabilidad |
| 6 | [📈 Comparativo Temporal](#6--reporte-comparativo-temporal) | Análisis año vs año con tendencias |
| 7 | [🌱 Fenológico](#7--reporte-fenológico) | Distribución por etapa y calendario fenológico |
| 8 | [📦 Inventario y Consumos](#8--reporte-de-inventario-y-consumos) | Stock actual y proyecciones |
| 9 | [✅ Tareas y Productividad](#9--reporte-de-tareas-y-productividad) | Eficiencia operativa y productividad |
| 10 | [⚠️ Alertas y Problemas](#10--reporte-de-alertas-y-problemas) | Incidencias y tiempo de resolución |

---

## 1. 📊 Estado de Resultados por Temporada

**Propósito:** Proporcionar una visión financiera completa de la temporada, incluyendo ingresos por ventas, desglose de costos de producción y utilidad operativa.

**Filtros disponibles:**
- Temporada/Período
- Finca
- Comparar con temporada anterior

```
┌─────────────────────────────────────────────────────────────────┐
│  📊 ESTADO DE RESULTADOS - Temporada 2025                       │
│  Finca Los Alamos | Enero - Diciembre 2025                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  INGRESOS                                                       │
│  ───────────────────────────────────────────────────────────── │
│  Ventas brutas                                    $485,200.00  │
│    Premium (12,500 kg × $15.20)      $190,000.00              │
│    Primera (18,200 kg × $12.10)      $220,220.00              │
│    Segunda (6,800 kg × $8.50)         $57,800.00              │
│    Industrial (1,000 kg × $5.18)       $5,180.00              │
│                                                                 │
│  (-) Gastos de comercialización                   -$24,150.00  │
│    Fletes                            -$12,400.00              │
│    Empaque                            -$5,850.00              │
│    Comisiones                         -$5,900.00              │
│  ───────────────────────────────────────────────────────────── │
│  INGRESO NETO                                     $461,050.00  │
│                                                                 │
│  COSTOS DE PRODUCCIÓN                                          │
│  ───────────────────────────────────────────────────────────── │
│  Mano de obra                                     $142,800.00  │
│    Labores de cultivo                 $82,400.00              │
│    Cosecha                            $48,200.00              │
│    Mantenimiento                      $12,200.00              │
│                                                                 │
│  Insumos                                           $78,200.00  │
│    Agroquímicos                       $42,500.00              │
│    Fertilizantes                      $28,700.00              │
│    Combustibles                        $4,200.00              │
│    Otros materiales                    $2,800.00              │
│                                                                 │
│  Servicios externos                                $52,450.00  │
│    Fumigación aérea                   $28,000.00              │
│    Análisis laboratorio                $3,450.00              │
│    Asesoría técnica                   $12,000.00              │
│    Otros servicios                     $9,000.00              │
│                                                                 │
│  Costos fijos                                      $39,000.00  │
│    Electricidad/Agua                  $18,000.00              │
│    Mantenimiento infraestructura       $8,500.00              │
│    Seguros                             $6,500.00              │
│    Administrativos                     $6,000.00              │
│  ───────────────────────────────────────────────────────────── │
│  TOTAL COSTOS                                    -$312,450.00  │
│                                                                 │
│  ═══════════════════════════════════════════════════════════════│
│  UTILIDAD OPERATIVA                               $148,600.00  │
│  Margen operativo                                      32.2%   │
│  ═══════════════════════════════════════════════════════════════│
│                                                                 │
│  MÉTRICAS CLAVE                                                 │
│  ───────────────────────────────────────────────────────────── │
│  Producción total:        38,500 kg                            │
│  Árboles productivos:     1,130                                │
│  Producción/árbol:        34.1 kg                              │
│  Costo/kg:                $8.11                                │
│  Precio venta prom/kg:    $12.60                               │
│  Utilidad/árbol:          $131.50                              │
│  Utilidad/hectárea:       $14,860                              │
│                                                                 │
│  [📤 Exportar PDF] [📧 Enviar] [📊 Comparar años]              │
└─────────────────────────────────────────────────────────────────┘
```

---

## 2. 🌿 Reporte de Salud del Cultivo

**Propósito:** Monitorear el estado sanitario de los árboles por finca, lote o sector, identificar tendencias de salud y detectar problemas tempranamente.

**Filtros disponibles:**
- Finca / Lote / Sector
- Período temporal
- Estado de salud específico

```
┌─────────────────────────────────────────────────────────────────┐
│  🌿 REPORTE DE SALUD DEL CULTIVO                                │
│  Finca Los Alamos | Actualizado: 08/Dic/2025                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN GENERAL                                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Total árboles: 1,130 | Período: Últimos 30 días            ││
│  │                                                             ││
│  │ Estado          │ Cantidad │  %   │ Tendencia              ││
│  │ ────────────────────────────────────────────────────────────││
│  │ 🟢 Saludable    │   882    │ 78.1%│ ↗ +2.3%               ││
│  │ 🟡 Observación  │   135    │ 11.9%│ ↘ -1.5%               ││
│  │ 🟠 Riesgo       │    79    │  7.0%│ → Estable             ││
│  │ 🔴 Crítico      │    22    │  1.9%│ ↗ +0.8%               ││
│  │ 🔵 Tratamiento  │     8    │  0.7%│ ↘ -0.4%               ││
│  │ 🟣 Recuperación │     4    │  0.4%│ ↘ -0.2%               ││
│  │ ⚫ Muerto        │     0    │  0.0%│ → Sin cambios         ││
│  │ 🟢 Nuevo        │     0    │  0.0%│ → Sin cambios         ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🗺️ DISTRIBUCIÓN POR LOTE                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Lote │ Total │ 🟢SAL│ 🟡OBS│ 🟠RIE│ 🔴CRI│ Salud Prom│     ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │  A   │  500  │ 410  │  52  │  28  │  10  │   82.0%   │ ✅  ││
│  │  B   │  350  │ 275  │  45  │  24  │   6  │   78.6%   │ ✅  ││
│  │  C   │  280  │ 197  │  38  │  27  │   6  │   70.4%   │ ⚠️  ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Total│ 1,130 │ 882  │ 135  │  79  │  22  │   78.1%   │     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⚠️ PROBLEMAS DETECTADOS                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Problema              │ Árboles │ Lotes    │ Severidad      ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Trips del aguacate    │   45    │ A, B     │ 🟠 Media       ││
│  │ Antracnosis           │   28    │ C        │ 🔴 Alta        ││
│  │ Déficit nutricional   │   18    │ C        │ 🟡 Baja        ││
│  │ Estrés hídrico        │   12    │ B        │ 🟠 Media       ││
│  │ Phytophthora (raíz)   │    6    │ A        │ 🔴 Alta        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🚨 ALERTAS ACTIVAS (3)                                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ ⚠️ ALTA - Foco de Antracnosis en Lote C, Sector 2          ││
│  │    12 árboles afectados | Requiere tratamiento urgente     ││
│  │    Plazo: 2 días                                            ││
│  │                                                             ││
│  │ ⚠️ ALTA - Phytophthora detectada en 6 árboles Lote A       ││
│  │    Aplicar fungicida y mejorar drenaje                     ││
│  │    Plazo: 3 días                                            ││
│  │                                                             ││
│  │ ⚠️ MEDIA - Estrés hídrico en Sector 3, Lote B              ││
│  │    Revisar sistema de riego                                ││
│  │    Plazo: 7 días                                            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📈 GRÁFICO DE TENDENCIA (Últimos 6 meses)                     │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ 100%│                                                       ││
│  │  90%│ ████████████████████████████████████████▓▓▓▓▓        ││
│  │  80%│ ████████████████████████████████████████████▓▓       ││
│  │  70%│ ▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░        ││
│  │  60%│ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░        ││
│  │   0%│___│___│___│___│___│___│___│___│___│___│___│___│    ││
│  │     │Jul│Ago│Sep│Oct│Nov│Dic│                             ││
│  │     │ 🟢 Saludable  🟡 Observación  🟠 Riesgo/Crítico     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [🗺️ Ver Mapa] [📊 Detalles] [📧 Enviar]    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 3. 🍎 Reporte de Producción y Cosecha

**Propósito:** Analizar la producción total y distribución por calidad, identificar árboles de alto rendimiento y planificar cosecha.

**Filtros disponibles:**
- Temporada
- Finca / Lote
- Rango de fechas
- Calidad del fruto

```
┌─────────────────────────────────────────────────────────────────┐
│  🍎 REPORTE DE PRODUCCIÓN Y COSECHA - Temporada 2025            │
│  Finca Los Alamos | Período: Ene-Dic 2025                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN DE PRODUCCIÓN                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │  Total cosechado:        38,500 kg                         ││
│  │  Árboles cosechados:      1,130                            ││
│  │  Rendimiento promedio:     34.1 kg/árbol                   ││
│  │  Rendimiento por ha:      3,850 kg/ha                      ││
│  │                                                             ││
│  │  Período de cosecha:     15/Oct - 20/Dic                   ││
│  │  Días de cosecha:         67 días                          ││
│  │                                                             ││
│  │  vs Temporada 2024:      +8.5% ⬆️                          ││
│  │  vs Meta planificada:    +2.1% ✅                          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📦 DISTRIBUCIÓN POR CALIDAD                                    │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Calidad      │   Kg    │   %   │ Precio/kg│  Valor Total   ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ 🥇 Premium   │ 12,500  │ 32.5% │  $15.20  │  $190,000.00  ││
│  │ 🥈 Primera   │ 18,200  │ 47.3% │  $12.10  │  $220,220.00  ││
│  │ 🥉 Segunda   │  6,800  │ 17.7% │   $8.50  │   $57,800.00  ││
│  │ 🏭 Industrial│  1,000  │  2.6% │   $5.18  │    $5,180.00  ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ TOTAL        │ 38,500  │ 100%  │  $12.60  │  $485,200.00  ││
│  │                                                             ││
│  │  ████████░░░░░░░░░░░  Premium 32.5%                        ││
│  │  ███████████████░░░░░  Primera 47.3%                       ││
│  │  ██████░░░░░░░░░░░░░░  Segunda 17.7%                       ││
│  │  █░░░░░░░░░░░░░░░░░░░  Industrial 2.6%                     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🏆 TOP 10 ÁRBOLES MÁS PRODUCTIVOS                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ #  │ Árbol   │  Kg  │ Calidad Prom│ Valor    │ Lote        ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ 1  │ A-5-12  │  95  │ 78% Premium │ $1,204  │ A            ││
│  │ 2  │ A-3-8   │  88  │ 72% Premium │ $1,142  │ A            ││
│  │ 3  │ A-7-15  │  82  │ 68% Premium │ $1,058  │ A            ││
│  │ 4  │ B-2-18  │  79  │ 65% Primera │  $925   │ B            ││
│  │ 5  │ A-9-5   │  76  │ 70% Premium │  $982   │ A            ││
│  │ 6  │ B-6-12  │  74  │ 60% Primera │  $872   │ B            ││
│  │ 7  │ A-4-22  │  72  │ 75% Premium │  $945   │ A            ││
│  │ 8  │ B-8-9   │  71  │ 55% Primera │  $820   │ B            ││
│  │ 9  │ C-1-14  │  68  │ 50% Primera │  $785   │ C            ││
│  │ 10 │ A-6-18  │  67  │ 65% Premium │  $865   │ A            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📅 CALENDARIO DE COSECHA                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │         Oct      │      Nov      │      Dic      │          ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Semana 1│        │ ████████░░    │ ████████████  │          ││
│  │ Semana 2│        │ ████████████  │ ████████████  │          ││
│  │ Semana 3│ ██░░   │ ████████████  │ ████████░░░░  │          ││
│  │ Semana 4│ ██████ │ ████████████  │ ████░░░░░░░░  │          ││
│  │                                                             ││
│  │ Pico de cosecha: 15-30 Noviembre (18,500 kg)               ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 RENDIMIENTO POR LOTE                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Lote │ Árboles │ Total kg │ kg/árbol │ Calidad │ Valor      ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │  A   │   500   │  18,200  │   36.4   │ 68% P+1ª│ $235,420  ││
│  │  B   │   350   │  11,800  │   33.7   │ 58% P+1ª│ $145,680  ││
│  │  C   │   280   │   8,500  │   30.4   │ 45% P+1ª│ $104,100  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📊 Gráficas] [🗺️ Ver Mapa] [📧 Enviar]    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4. 💉 Reporte de Aplicaciones Fitosanitarias

**Propósito:** Documentar el historial completo de tratamientos fitosanitarios, evaluar su eficacia y asegurar el cumplimiento de intervalos de seguridad.

**Filtros disponibles:**
- Período
- Tipo de aplicación
- Producto utilizado
- Lote / Sector
- Estado de cumplimiento

```
┌─────────────────────────────────────────────────────────────────┐
│  💉 REPORTE DE APLICACIONES FITOSANITARIAS                      │
│  Finca Los Alamos | Período: Nov-Dic 2025                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN DEL PERÍODO                                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Total aplicaciones:           18                            ││
│  │ Árboles tratados:             1,130 (100%)                  ││
│  │ Productos utilizados:         12                            ││
│  │ Costo total aplicaciones:     $28,450                       ││
│  │                                                             ││
│  │ Por tipo:                                                   ││
│  │ • Fitosanitarias (plagas):     8 aplicaciones              ││
│  │ • Fungicidas (enfermedades):   6 aplicaciones              ││
│  │ • Nutricionales:               4 aplicaciones              ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📋 HISTORIAL DE APLICACIONES                                   │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Fecha    │ Tipo      │ Producto       │ Obj│ Lotes│Eficacia ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ 05/Dic   │ Fungicida │ Aliette 80WG   │Phy│ A,B  │ ✅ 92%  ││
│  │ 02/Dic   │ Insect.   │ Success 480SC  │Trp│ A,C  │ ✅ 88%  ││
│  │ 28/Nov   │ Foliar    │ Nutrifoliar    │Nut│ A,B,C│ ✅ 85%  ││
│  │ 25/Nov   │ Fungicida │ Cupravit       │Ant│  C   │ ⚠️ 68%  ││
│  │ 20/Nov   │ Insect.   │ Abamectina     │Áca│  B   │ ✅ 90%  ││
│  │ 18/Nov   │ Fungicida │ Aliette 80WG   │Phy│  A   │ ✅ 95%  ││
│  │ 15/Nov   │ Foliar    │ Aminoácidos    │Nut│ A,B,C│ ✅ 82%  ││
│  │ 12/Nov   │ Insect.   │ Success 480SC  │Trp│ A,B  │ ✅ 86%  ││
│  │ 08/Nov   │ Fungicida │ Score 250EC    │Ant│  C   │ ⚠️ 72%  ││
│  │ 05/Nov   │ Herbicida │ Glifosato      │Mal│ A,B,C│ ✅ 94%  ││
│  │ ...                                           [Ver todas]   ││
│  │                                                             ││
│  │ Objetivo: Phy=Phytophthora, Trp=Trips, Ant=Antracnosis,    ││
│  │           Áca=Ácaros, Nut=Nutricional, Mal=Malezas         ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💊 PRODUCTOS MÁS UTILIZADOS                                    │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Producto          │ Aplicac│ Cantidad │ Costo   │Efic. Prom││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ Success 480 SC    │   4    │  800 ml  │ $8,200  │   88%   ││
│  │ Aliette 80 WG     │   3    │  4.5 kg  │ $6,800  │   94%   ││
│  │ Nutrifoliar Multi │   3    │   18 L   │ $4,200  │   84%   ││
│  │ Cupravit Blue     │   2    │  3.0 kg  │ $2,400  │   70%   ││
│  │ Abamectina 1.8 EC │   2    │  1.2 L   │ $3,600  │   90%   ││
│  │ Score 250 EC      │   2    │  800 ml  │ $2,100  │   73%   ││
│  │ Otros             │   2    │    -     │ $1,150  │   80%   ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⏰ CUMPLIMIENTO DE INTERVALOS                                  │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Producto          │ Intervalo │ Última  │ Próxima │Estado  ││
│  │                   │ Seguridad │ Aplic.  │ Cosecha │        ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ Success 480 SC    │  14 días  │ 02/Dic  │ 15/Ene  │ ✅ OK  ││
│  │ Aliette 80 WG     │  30 días  │ 05/Dic  │ 15/Ene  │ ✅ OK  ││
│  │ Abamectina        │  21 días  │ 20/Nov  │ 15/Ene  │ ✅ OK  ││
│  │ Cupravit Blue     │  45 días  │ 25/Nov  │ 15/Ene  │ ✅ OK  ││
│  │ Score 250 EC      │  28 días  │ 08/Nov  │ 15/Ene  │ ✅ OK  ││
│  │                                                             ││
│  │ ✅ TODOS LOS PRODUCTOS CUMPLEN INTERVALOS DE CARENCIA       ││
│  │    Cosecha programada: 15 Enero 2026                       ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📈 ANÁLISIS DE EFICACIA                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Problema: Trips del aguacate (Lotes A, B)                  ││
│  │ ┌───────────────────────────────────────────────────────┐  ││
│  │ │ Tratamientos:  3 aplicaciones de Success 480 SC      │  ││
│  │ │ Fechas:        12/Nov, 02/Dic, 20/Dic                │  ││
│  │ │                                                       │  ││
│  │ │ Árboles afectados:  45 → 28 → 12 → 5                │  ││
│  │ │ Reducción:          89% ✅                            │  ││
│  │ │ Estado actual:      🟢 Controlado                     │  ││
│  │ └───────────────────────────────────────────────────────┘  ││
│  │                                                             ││
│  │ Problema: Antracnosis (Lote C)                             ││
│  │ ┌───────────────────────────────────────────────────────┐  ││
│  │ │ Tratamientos:  2 aplicaciones fungicidas             │  ││
│  │ │ Fechas:        08/Nov (Score), 25/Nov (Cupravit)     │  ││
│  │ │                                                       │  ││
│  │ │ Árboles afectados:  35 → 28 → 28                     │  ││
│  │ │ Reducción:          20% ⚠️                            │  ││
│  │ │ Estado actual:      🟡 Requiere tratamiento adicional │  ││
│  │ │                                                       │  ││
│  │ │ 💡 Recomendación: Cambiar fungicida. Evaluar         │  ││
│  │ │    Fitoraz o Mancozeb + Azoxystrobin                 │  ││
│  │ └───────────────────────────────────────────────────────┘  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📊 Gráfica Eficacia] [📋 Bitácora Completa]│
└─────────────────────────────────────────────────────────────────┘
```

---

## 5. 💰 Reporte de Rentabilidad por Árbol

**Propósito:** Analizar el ROI de cada árbol individual, identificar los más y menos rentables, y tomar decisiones sobre árboles con pérdidas sostenidas.

**Filtros disponibles:**
- Temporada
- Lote
- Rango de rentabilidad
- Ordenar por: Utilidad, Margen, Producción


```
┌─────────────────────────────────────────────────────────────────┐
│  💰 REPORTE DE RENTABILIDAD POR ÁRBOL - Temporada 2025          │
│  Finca Los Alamos | 1,130 árboles analizados                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 DISTRIBUCIÓN DE RENTABILIDAD                                │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Rango de Margen     │ Árboles │   %   │ Utilidad Promedio  ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ 🟢 Excelente (>45%) │   215   │ 19.0% │    $185            ││
│  │ 🟢 Bueno (35-45%)   │   485   │ 42.9% │    $142            ││
│  │ 🟡 Regular (25-35%) │   328   │ 29.0% │     $98            ││
│  │ 🟠 Bajo (15-25%)    │    85   │  7.5% │     $52            ││
│  │ 🔴 Negativo (<15%)  │    17   │  1.5% │    -$85            ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Promedio general:                          $131.50          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🏆 TOP 10 ÁRBOLES MÁS RENTABLES                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ # │ Árbol  │  Prod  │ Ingreso │ Costo  │ Utilidad│ Margen  ││
│  │ ────────────────────────────────────────────────────────────││
│  │ 1 │ A-5-12 │  95kg  │ $1,152  │  $593  │  $559   │ 48.6%  ││
│  │ 2 │ A-3-8  │  88kg  │ $1,085  │  $620  │  $465   │ 42.9%  ││
│  │ 3 │ A-7-15 │  82kg  │  $998   │  $615  │  $383   │ 38.4%  ││
│  │ 4 │ B-2-18 │  79kg  │  $912   │  $585  │  $327   │ 35.9%  ││
│  │ 5 │ A-9-5  │  76kg  │  $952   │  $608  │  $344   │ 36.1%  ││
│  │ 6 │ B-6-12 │  74kg  │  $858   │  $592  │  $266   │ 31.0%  ││
│  │ 7 │ A-4-22 │  72kg  │  $920   │  $612  │  $308   │ 33.5%  ││
│  │ 8 │ B-8-9  │  71kg  │  $810   │  $580  │  $230   │ 28.4%  ││
│  │ 9 │ C-1-14 │  68kg  │  $765   │  $548  │  $217   │ 28.4%  ││
│  │ 10│ A-6-18 │  67kg  │  $842   │  $598  │  $244   │ 29.0%  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⚠️ ÁRBOLES CON RENTABILIDAD NEGATIVA (Requieren atención)     │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Árbol  │ Prod │ Ingreso │ Costo  │ Pérdida │ Problema      ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ C-2-5  │ 12kg │  $148   │  $346  │  -$198  │ Phytophthora  ││
│  │ B-8-3  │ 18kg │  $215   │  $371  │  -$156  │ Bajo rend.    ││
│  │ C-1-9  │ 15kg │  $178   │  $320  │  -$142  │ Déficit nutr. ││
│  │ A-3-25 │ 22kg │  $268   │  $385  │  -$117  │ Antracnosis   ││
│  │ C-5-8  │ 19kg │  $228   │  $340  │  -$112  │ Estrés hídr.  ││
│  │ ...                                         [Ver 17 árboles]││
│  │                                                             ││
│  │ 💡 Recomendación: 12 árboles con pérdida >$100             ││
│  │    - 8 árboles: Tratamiento intensivo                      ││
│  │    - 4 árboles: Evaluar reemplazo                          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 ANÁLISIS POR COMPONENTE DE COSTO                            │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Componente          │ Costo Prom/Árbol │  % del Total      ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Mano de obra        │     $126.37      │     45.7%         ││
│  │ Insumos             │      $69.20      │     25.0%         ││
│  │ Servicios externos  │      $46.42      │     16.8%         ││
│  │ Costos fijos        │      $34.51      │     12.5%         ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ TOTAL               │     $276.50      │    100.0%         ││
│  │                                                             ││
│  │  ████████████████████░░░░░░░░░░░░░  Mano de obra 45.7%    ││
│  │  ████████████░░░░░░░░░░░░░░░░░░░░░  Insumos 25.0%         ││
│  │  ████████░░░░░░░░░░░░░░░░░░░░░░░░░  Servicios 16.8%       ││
│  │  ██████░░░░░░░░░░░░░░░░░░░░░░░░░░░  Fijos 12.5%           ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📈 COMPARATIVA POR LOTE                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Lote │ Árboles │ Util/Árbol │ Margen │ ROI Anual│  Estado  ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │  A   │   500   │   $168.50  │ 40.8%  │   45.2%  │  ✅ Exc ││
│  │  B   │   350   │   $115.20  │ 29.2%  │   32.8%  │  🟢 Bue ││
│  │  C   │   280   │    $85.40  │ 27.4%  │   24.1%  │  🟡 Reg ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📊 Gráficas] [📋 Ver Detalles] [🗺️ Mapa]  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6. 📈 Reporte Comparativo Temporal

**Propósito:** Comparar métricas clave entre temporadas para identificar tendencias, mejoras y áreas de oportunidad.

**Filtros disponibles:**
- Años a comparar (2-5 años)
- Finca / Lote
- Métricas específicas

```
┌─────────────────────────────────────────────────────────────────┐
│  📈 REPORTE COMPARATIVO TEMPORAL                                │
│  Finca Los Alamos | Comparativa 2024 vs 2025                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 MÉTRICAS PRINCIPALES                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Métrica              │  2024   │  2025   │ Variación│Trend  ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ Producción total     │35,500kg │38,500kg │  +8.5%  │ ⬆️ Exc││
│  │ Rendimiento/árbol    │ 31.6kg  │ 34.1kg  │  +7.9%  │ ⬆️ Exc││
│  │ Calidad Premium+1ª   │  75.8%  │  79.8%  │  +5.3%  │ ⬆️ Bue││
│  │                      │         │         │         │       ││
│  │ Ventas brutas        │$432,800 │$485,200 │ +12.1%  │ ⬆️ Exc││
│  │ Precio prom/kg       │ $12.19  │ $12.60  │  +3.4%  │ ⬆️ Bue││
│  │                      │         │         │         │       ││
│  │ Costos totales       │$298,400 │$312,450 │  +4.7%  │ ⬇️ Reg││
│  │ Costo/kg             │  $8.41  │  $8.11  │  -3.6%  │ ⬆️ Exc││
│  │                      │         │         │         │       ││
│  │ Utilidad operativa   │$134,400 │$148,600 │ +10.6%  │ ⬆️ Exc││
│  │ Margen operativo     │  31.0%  │  32.2%  │  +3.9%  │ ⬆️ Bue││
│  │ Utilidad/árbol       │ $119.50 │ $131.50 │ +10.0%  │ ⬆️ Exc││
│  │                      │         │         │         │       ││
│  │ Árboles saludables   │  74.5%  │  78.1%  │  +4.8%  │ ⬆️ Bue││
│  │ Problemas críticos   │   2.8%  │   1.9%  │ -32.1%  │ ⬆️ Exc││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📈 GRÁFICO COMPARATIVO (5 años)                                │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    PRODUCCIÓN TOTAL (Toneladas)             ││
│  │                                                             ││
│  │  40│                                               ●──38.5  ││
│  │  38│                                       ●──36.8          ││
│  │  36│                               ●──35.5                 ││
│  │  34│                                                        ││
│  │  32│                       ●──32.1                         ││
│  │  30│               ●──29.8                                 ││
│  │  28│                                                        ││
│  │   0│───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼         ││
│  │     2021  2022  2023  2024  2025                           ││
│  │                                                             ││
│  │                    MARGEN OPERATIVO (%)                     ││
│  │                                                             ││
│  │  35│                                               ●──32.2  ││
│  │  32│                               ●──●──31.0               ││
│  │  29│                       ●──29.5                         ││
│  │  26│               ●──27.2                                 ││
│  │  23│       ●──24.8                                         ││
│  │   0│───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼───┼         ││
│  │     2021  2022  2023  2024  2025                           ││
│  │                                                             ││
│  │ Tendencia: ⬆️ Crecimiento sostenido en todos los indicadores││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💡 FACTORES DE MEJORA IDENTIFICADOS                            │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ ✅ Positivos (que explican el crecimiento):                ││
│  │                                                             ││
│  │  1. Mejor manejo fitosanitario                             ││
│  │     • Reducción de árboles críticos: -32.1%                ││
│  │     • Control más efectivo de Trips y Phytophthora         ││
│  │                                                             ││
│  │  2. Optimización de cosecha                                ││
│  │     • Incremento de calidad Premium+Primera: +5.3%         ││
│  │     • Mejor timing de cosecha (madurez óptima)             ││
│  │                                                             ││
│  │  3. Mejores precios de venta                               ││
│  │     • Precio promedio/kg: +3.4%                            ││
│  │     • Negociación con nuevos compradores                   ││
│  │                                                             ││
│  │  4. Eficiencia en costos                                   ││
│  │     • Costo/kg reducido: -3.6%                             ││
│  │     • Optimización de insumos y mano de obra               ││
│  │                                                             ││
│  │ ⚠️ Áreas de atención:                                       ││
│  │                                                             ││
│  │  1. Incremento en costos totales (+4.7%)                   ││
│  │     • Revisar eficiencia operativa                         ││
│  │     • Evaluar tercerización de servicios                   ││
│  │                                                             ││
│  │  2. Lote C con rendimiento bajo                            ││
│  │     • Producción/árbol: 30.4 kg (vs 36.4 en Lote A)        ││
│  │     • Implementar plan de recuperación                     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 COMPARATIVA DETALLADA POR CATEGORÍA                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                        2024     2025     Δ      Tendencia   ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ PRODUCCIÓN                                                  ││
│  │ • kg Premium        11,200   12,500   +11.6%   ⬆️ Excelente││
│  │ • kg Primera        15,700   18,200   +15.9%   ⬆️ Excelente││
│  │ • kg Segunda         7,500    6,800    -9.3%   ⬆️ Bueno    ││
│  │ • kg Industrial      1,100    1,000    -9.1%   ⬆️ Bueno    ││
│  │                                                             ││
│  │ COSTOS                                                      ││
│  │ • Mano de obra    $136,200 $142,800    +4.8%   ⬇️ Regular  ││
│  │ • Insumos          $74,800  $78,200    +4.5%   ⬇️ Regular  ││
│  │ • Servicios ext    $49,400  $52,450    +6.2%   ⬇️ Regular  ││
│  │ • Costos fijos     $38,000  $39,000    +2.6%   → Estable   ││
│  │                                                             ││
│  │ SALUD                                                       ││
│  │ • Saludable         74.5%    78.1%    +4.8%   ⬆️ Bueno    ││
│  │ • Observación       14.2%    11.9%   -16.2%   ⬆️ Bueno    ││
│  │ • Riesgo             8.5%     7.0%   -17.6%   ⬆️ Bueno    ││
│  │ • Crítico            2.8%     1.9%   -32.1%   ⬆️ Excelente││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📊 Ver Más Años] [📈 Proyecciones]         │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. 🌱 Reporte Fenológico

**Propósito:** Visualizar la distribución de árboles por etapa fenológica, predecir fechas de cosecha y detectar árboles con desarrollo atípico.

**Filtros disponibles:**
- Finca / Lote
- Cultivo / Variedad
- Etapa fenológica específica

```
┌─────────────────────────────────────────────────────────────────┐
│  🌱 REPORTE FENOLÓGICO - Aguacate Hass                          │
│  Finca Los Alamos | Actualizado: 08/Dic/2025                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 DISTRIBUCIÓN POR ETAPA FENOLÓGICA                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Etapa              │ Árboles│  %   │ Duración Prom│ Estado ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ 🌾 REP - Reposo    │    0   │ 0.0% │    -         │   -    ││
│  │ 🌿 BRO - Brotación │    0   │ 0.0% │    -         │   -    ││
│  │ 🌳 VEG - Vegetat.  │    0   │ 0.0% │    -         │   -    ││
│  │ 🌸 IFL - Inducción │    0   │ 0.0% │    -         │   -    ││
│  │ 🌺 FLO - Floración │    0   │ 0.0% │    -         │   -    ││
│  │ 🥚 CUA - Cuajado   │   45   │ 4.0% │  28 días     │ ✅ OK  ││
│  │ 🍏 DES - Desarrollo│  890   │78.8% │  65 días     │ ✅ OK  ││
│  │ 🥑 MAD - Maduración│  185   │16.4% │  25 días     │ ✅ OK  ││
│  │ ✅ COS - Cosecha   │   10   │ 0.9% │   -          │ ✅ OK  ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │ TOTAL              │ 1,130  │ 100% │              │        ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📅 CALENDARIO FENOLÓGICO VISUAL                                │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │          Oct    Nov    Dic    Ene    Feb    Mar    Abr     ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Cuajado  │▓▓▓│      │      │      │      │      │          ││
│  │          │ 4%│      │      │      │      │      │          ││
│  │          │   │      │      │      │      │      │          ││
│  │ Desarr.  │   │▓▓▓▓▓▓│▓▓▓▓▓▓│▓▓▓▓  │      │      │          ││
│  │          │   │ 78.8%│      │      │      │      │          ││
│  │          │   │      │      │      │      │      │          ││
│  │ Madurac. │   │      │      │░░░░░░│▓▓▓▓▓▓│▓▓▓▓  │          ││
│  │          │   │      │      │      │ 16.4%│      │          ││
│  │          │   │      │      │      │      │      │          ││
│  │ Cosecha  │   │      │  ░░  │▓▓▓▓▓▓│▓▓▓▓▓▓│▓▓▓▓▓▓│▓▓▓▓░░    ││
│  │          │   │      │      │PICO COSECHA│      │          ││
│  │          │   │      │      │ 15 Ene - 28 Feb              ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔮 PREDICCIÓN DE COSECHA                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Próximas fechas de cosecha (estimadas):                    ││
│  │                                                             ││
│  │ • Semana 15-21 Ene:    185 árboles listos (~6,300 kg)      ││
│  │ • Semana 22-28 Ene:    420 árboles listos (~14,300 kg)     ││
│  │ • Semana 29 Ene-04 Feb: 310 árboles listos (~10,500 kg)    ││
│  │ • Semana 05-11 Feb:    170 árboles listos (~5,800 kg)      ││
│  │ • Semana 12-18 Feb:     45 árboles listos (~1,500 kg)      ││
│  │                                                             ││
│  │ ✅ Total estimado:     1,130 árboles    (~38,400 kg)       ││
│  │                                                             ││
│  │ 💡 Recomendación: Preparar cuadrillas para pico de cosecha ││
│  │    (22-28 Enero). Necesario personal adicional.            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⚠️ ALERTAS FENOLÓGICAS                                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ 🟡 ATENCIÓN - 8 árboles con desarrollo retrasado           ││
│  │    Árboles: C-2-5, C-3-8, C-5-12, B-8-15, ...              ││
│  │    En etapa DES por >95 días (esperado: 90 días)           ││
│  │    Acción: Evaluar nutrición y salud                       ││
│  │                                                             ││
│  │ 🟢 OK - Desarrollo general normal                          ││
│  │    98% de árboles dentro de plazos esperados               ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 DISTRIBUCIÓN POR LOTE                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Lote │ CUA │  DES │  MAD │ COS │ Etapa Dominante│ Proyecc ││
│  │ ──────────────────────────────────────────────────────────  ││
│  │  A   │  20 │  395 │   80 │  5  │ DES (79%)      │ 18 Ene  ││
│  │  B   │  15 │  280 │   52 │  3  │ DES (80%)      │ 20 Ene  ││
│  │  C   │  10 │  215 │   53 │  2  │ DES (77%)      │ 22 Ene  ││
│  │                                                             ││
│  │ Proyección: Fecha estimada inicio cosecha masiva por lote  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📅 Calendario Completo] [🔔 Config Alertas]│
└─────────────────────────────────────────────────────────────────┘
```

---

## 8. 📦 Reporte de Inventario y Consumos

**Propósito:** Monitorear el stock actual de insumos, identificar productos con stock bajo y proyectar necesidades futuras.

**Filtros disponibles:**
- Categoría de producto
- Almacén / Bodega
- Estado de stock (Crítico, Bajo, Normal, Alto)
- Período de análisis



```
┌─────────────────────────────────────────────────────────────────┐
│  📦 REPORTE DE INVENTARIO Y CONSUMOS                            │
│  Finca Los Alamos | Mes: Diciembre 2025                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN GENERAL                                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Total productos:          85                                ││
│  │ Valor total stock:        $52,400                           ││
│  │ Consumo del mes:          $8,950                            ││
│  │ Productos en crítico:     5   🔴                            ││
│  │ Productos en stock bajo:  12  🟡                            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔴 PRODUCTOS CON STOCK CRÍTICO (Requieren compra urgente)     │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Producto           │ Stock │ Mín │ Consumo│ Días  │ Acción ││
│  │                    │Actual │     │ Mens.  │Rest.  │        ││
│  │ ─────────────────────────────────────────────────────────── ││
│  │ Success 480 SC     │ 100ml │250ml│ 400ml  │  7d   │🔴 URG  ││
│  │ Aliette 80 WG      │ 0.5kg │ 2kg │ 1.5kg  │  10d  │🔴 URG  ││
│  │ Fertilizante 10-30 │  8kg  │15kg │  25kg  │  9d   │🔴 URG  ││
│  │ Gasolina (bomba)   │  18L  │30L  │  45L   │  12d  │🔴 URG  ││
│  │ Cajas de empaque   │  150  │300  │  850   │  5d   │🔴 URG  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  💰 CONSUMO VS PRESUPUESTO                                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                                                             ││
│  │ Categoría        │ Presup.│ Consumo│ Variación│ % Usado    ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Agroquímicos     │$7,500  │$6,850  │  -$650   │ 91% ✅    ││
│  │ Fertilizantes    │$4,200  │$4,680  │  +$480   │111% ⚠️    ││
│  │ Combustibles     │$1,800  │$1,420  │  -$380   │ 79% ✅    ││
│  │ ────────────────────────────────────────────────────────────││
│  │ TOTAL            │$13,500 │$12,950 │  -$550   │ 96% ✅    ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔮 PROYECCIÓN PRÓXIMO MES (Enero 2026)                         │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Categoría              │ Consumo Est.│ Costo Est.           ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Agroquímicos           │   $5,200    │   $5,500            ││
│  │ Fertilizantes          │   $3,800    │   $4,100            ││
│  │ Combustibles           │   $1,200    │   $1,300            ││
│  │ Materiales diversos    │     $850    │     $900            ││
│  │ ────────────────────────────────────────────────────────────││
│  │ TOTAL ESTIMADO         │  $11,050    │  $11,800            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📋 Lista Compras] [📊 Historial]            │
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. ✅ Reporte de Tareas y Productividad

**Propósito:** Evaluar la eficiencia operativa, analizar productividad del personal y optimizar la planificación de labores.

**Filtros disponibles:**
- Período
- Tipo de tarea
- Trabajador / Cuadrilla
- Estado de tarea
- Lote / Sector

```
┌─────────────────────────────────────────────────────────────────┐
│  ✅ REPORTE DE TAREAS Y PRODUCTIVIDAD                           │
│  Finca Los Alamos | Período: Noviembre 2025                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN EJECUTIVO                                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Total tareas programadas:    185                            ││
│  │ Tareas completadas:          168  (90.8%)  ✅               ││
│  │ Tareas en progreso:           12  (6.5%)   🔵               ││
│  │ Tareas pendientes:             5  (2.7%)   🟡               ││
│  │                                                             ││
│  │ Horas trabajadas:            1,248 horas                    ││
│  │ Costo mano de obra:          $42,680                        ││
│  │ Eficiencia general:          92.4%  ✅                       ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🏷️ TAREAS POR CATEGORÍA                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Categoría            │ Tareas│ Horas │ Costo   │ % Total   ││
│  │ ────────────────────────────────────────────────────────────││
│  │ 💉 Aplicaciones      │   42  │  285h │$9,750   │   22.8%  ││
│  │ ✂️ Podas/Manten.     │   35  │  312h │$10,680  │   25.0%  ││
│  │ 🚿 Riego/Fertirr.    │   28  │  198h │$6,780   │   15.9%  ││
│  │ 🔍 Inspecciones      │   25  │  142h │$4,860   │   11.4%  ││
│  │ 🍎 Cosecha           │   22  │  245h │$8,380   │   19.6%  ││
│  │ 🔧 Mantenimiento     │   18  │   48h │$1,640   │    3.8%  ││
│  │ 🧹 Limpieza          │   15  │   18h │  $590   │    1.4%  ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  👷 PRODUCTIVIDAD POR TRABAJADOR (Top 5)                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Trabajador        │Tareas│ Horas│Efic.│Rendimiento         ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Juan Pérez        │  28  │ 185h │ 95% │ ✅ Excelente      ││
│  │ Carlos Ramírez    │  25  │ 168h │ 94% │ ✅ Excelente      ││
│  │ Miguel Torres     │  24  │ 172h │ 92% │ ✅ Bueno          ││
│  │ Pedro González    │  22  │ 158h │ 91% │ ✅ Bueno          ││
│  │ José Martínez     │  21  │ 165h │ 88% │ 🟢 Bueno          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⏱️ TIEMPO PROMEDIO POR TIPO DE TAREA                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tipo de Tarea           │ Tiempo │ Estándar│ Variación    ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Aplicación fitosanit.   │  6.8h  │   7.0h  │ -2.9% ✅     ││
│  │ Poda de formación       │  8.9h  │   8.5h  │ +4.7% 🟡     ││
│  │ Cosecha (por cuadrilla) │ 11.1h  │  12.0h  │ -7.5% ✅     ││
│  │ Inspección sanitaria    │  5.7h  │   6.0h  │ -5.0% ✅     ││
│  │ Riego/Fertirriego       │  7.1h  │   7.5h  │ -5.3% ✅     ││
│  │ Mantenimiento equipo    │  2.7h  │   3.0h  │-10.0% ✅     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📊 Gráficas] [📋 Planificación]             │
└─────────────────────────────────────────────────────────────────┘
```

---

## 10. ⚠️ Reporte de Alertas y Problemas

**Propósito:** Centralizar todas las incidencias, evaluar tiempos de resolución y medir la eficacia de los tratamientos aplicados.

**Filtros disponibles:**
- Período
- Severidad (Baja, Media, Alta, Crítica)
- Tipo de problema
- Estado (Activa, En atención, Resuelta)
- Lote / Sector

```
┌─────────────────────────────────────────────────────────────────┐
│  ⚠️ REPORTE DE ALERTAS Y PROBLEMAS                              │
│  Finca Los Alamos | Período: Últimos 30 días                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  📊 RESUMEN GENERAL                                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Total alertas generadas:      42                            ││
│  │ Alertas resueltas:            35  (83.3%)  ✅               ││
│  │ Alertas en atención:           4  (9.5%)   🔵               ││
│  │ Alertas activas pendientes:    3  (7.1%)   🔴               ││
│  │                                                             ││
│  │ Tiempo prom. de resolución:   4.2 días                      ││
│  │ Eficacia prom. tratamientos:  87.5%  ✅                     ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🚨 ALERTAS POR SEVERIDAD                                       │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Severidad    │ Total │ Activas│ Resuel│   %Resol           ││
│  │ ────────────────────────────────────────────────────────────││
│  │ 🔴 Crítica   │   8   │    1   │   5   │   62.5%            ││
│  │ 🟠 Alta      │  15   │    2   │  11   │   73.3%            ││
│  │ 🟡 Media     │  12   │    0   │  12   │  100.0%            ││
│  │ 🟢 Baja      │   7   │    0   │   7   │  100.0%            ││
│  │ ────────────────────────────────────────────────────────────││
│  │ TOTAL        │  42   │    3   │  35   │   83.3%            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🏷️ ALERTAS POR TIPO DE PROBLEMA                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Tipo                    │ Cantidad│Resueltas│ % Eficacia   ││
│  │ ────────────────────────────────────────────────────────────││
│  │ 🐛 Foco de plaga        │   15    │   12    │    88.2%    ││
│  │ 🦠 Foco de enfermedad   │   12    │   10    │    85.4%    ││
│  │ 🌳 Árbol crítico        │    8    │    7    │    91.5%    ││
│  │ 💧 Estrés hídrico       │    4    │    4    │   100.0%    ││
│  │ 🍃 Déficit nutricional  │    3    │    2    │    82.0%    ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  ⏱️ TIEMPO DE RESOLUCIÓN                                        │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Rango de días   │ Alertas │  %   │ Evaluación             ││
│  │ ────────────────────────────────────────────────────────────││
│  │ < 2 días        │   12    │34.3% │ ✅ Excelente           ││
│  │ 2-5 días        │   18    │51.4% │ ✅ Bueno               ││
│  │ 6-10 días       │    4    │11.4% │ 🟡 Regular             ││
│  │ > 10 días       │    1    │ 2.9% │ 🔴 Requiere mejora     ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Promedio general: 4.2 días  (Meta: ≤ 5 días) ✅            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  🔍 PROBLEMAS MÁS FRECUENTES                                    │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ # │ Problema            │ Frecuenc│ Estatus                ││
│  │ ────────────────────────────────────────────────────────────││
│  │ 1 │ Trips del aguacate  │   15×   │ 🟢 Controlado          ││
│  │ 2 │ Antracnosis         │   12×   │ 🟡 En control          ││
│  │ 3 │ Phytophthora raíz   │    6×   │ 🔴 Activo              ││
│  │ 4 │ Déficit nutricional │    3×   │ 🟢 Resuelto            ││
│  │ 5 │ Estrés hídrico      │    4×   │ 🟢 Resuelto            ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  📊 EFICACIA DE TRATAMIENTOS                                    │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │ Problema          │Tratamiento      │Aplic.│Eficacia       ││
│  │ ────────────────────────────────────────────────────────────││
│  │ Trips             │Success 480 SC   │  3×  │  88% ✅      ││
│  │ Phytophthora      │Aliette 80 WG    │  2×  │  94% ✅      ││
│  │ Antracnosis       │Cupravit + Score │  2×  │  68% 🟡      ││
│  │ Ácaros            │Abamectina       │  1×  │  90% ✅      ││
│  │ Déficit K         │Fertiliz. foliar │  1×  │  82% ✅      ││
│  │ Estrés hídrico    │Ajuste riego     │  -   │ 100% ✅      ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
│  [📤 Exportar PDF] [📊 Detalles] [🔔 Configurar]                │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📚 Documentos relacionados

- [Datos de costos y ventas](07-09-costos-ventas-rentabilidad.md)
- [Indicadores de salud del cultivo](07-03-salud-fenologia.md)
- [Gestión de tareas y planificación](07-06-planificacion-semanal.md)
- [Inventario y compras](07-08-compras-inventario.md)
- [Producción y cosecha](07-09-costos-ventas-rentabilidad.md)
- [Aplicaciones fitosanitarias](07-05-aplicaciones-calculo-dosis.md)
- [Arquitectura de generación de reportes](../08-arquitectura-tecnica.md)

---

> Navegación: [← Anterior](07-09-costos-ventas-rentabilidad.md) | [📑 Índice](../README.md) | [Siguiente →](../08-arquitectura-tecnica.md)
