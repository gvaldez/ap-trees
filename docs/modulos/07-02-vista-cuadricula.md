### 7.2 🔲 Módulo de Vista de Cuadrícula (CORE)

> **💡 Funcionalidad inspirada en el método tradicional de hoja cuadriculada**, digitalizada para visualización rápida del estado de toda la finca.

Este módulo es el **corazón visual del sistema**, permitiendo ver el estado de cada árbol en una cuadrícula interactiva de filas y columnas.

#### Ejemplo de Vista de Cuadrícula - Lote A

```
                    LOTE A - Sector Norte - Aguacate Hass
                    Fecha: 2025-12-08 | NDVI promedio: 0.72
    
        Col→  1    2    3    4    5    6    7    8    9   10
      ┌─────────────────────────────────────────────────────────┐
Fila 1│  🟢   🟢   🟢   🟢   🟡   🟡   🔴   🔴   🟢   🟢  │
Fila 2│  🟢   🟢   🟢   🟡   🟡   🔴   🔴   🟠   🟢   🟢  │
Fila 3│  🟢   🟢   🟡   🟡   🔴   🔴   🟠   🟠   🟢   🟢  │
Fila 4│  🟢   🟢   🟢   🟡   🟠   🟠   🟢   🟢   🟢   🟢  │
Fila 5│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢  │
Fila 6│  🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢   🟢  │
Fila 7│  🟢   🟢   🔵   🔵   🟢   🟢   🟢   🟢   🟣   🟣  │
Fila 8│  🟢   🟢   🔵   🔵   🟢   🟢   🟢   🟢   🟣   🟣  │
      └─────────────────────────────────────────────────────────┘
      
      📊 Resumen: 80 árboles | 🟢 58 | 🟡 6 | 🟠 5 | 🔴 5 | 🔵 4 | 🟣 4
      ⚠️ ALERTA: Posible foco de Phytophthora en zona [F1-3, C5-8]
      🛰️ Última actualización drone: 2025-12-07
```

#### Funcionalidades de Cuadrícula
- Código de colores configurable por estado
- Capas de visualización (salud, fenología, NDVI, producción)
- Actualización manual desde app móvil
- Actualización automática desde análisis de drone
- Histórico y comparación temporal
- Análisis de propagación de plagas
- Exportación PDF, Excel, PNG, GIF animado

---

## 📚 Documentos relacionados

- [Coordenadas geográficas de cada árbol](07-01-mapeo-geolocalizacion.md)
- [Inspección de árboles en campo](07-07-app-movil-campo.md)
- [Visualización del estado de salud](07-03-salud-fenologia.md)

---

> Navegación: [← Anterior](07-01-mapeo-geolocalizacion.md) | [📑 Índice](../README.md) | [Siguiente →](07-03-salud-fenologia.md)
