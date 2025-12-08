### 7.1 📍 Módulo de Mapeo y Geolocalización

| Funcionalidad | Descripción |
|---------------|-------------|
| **Registro de Árboles** | Alta individual con coordenadas GPS precisas |
| **Mapas Interactivos** | Visualización de la finca con capas de información |
| **Sectores y Lotes** | Organización jerárquica: Finca → Sector → Lote → Árbol |
| **Integración Satelital** | Importación de imágenes satelitales/drones |
| **QR/NFC por Árbol** | Etiquetado físico para escaneo en campo |

#### Datos por Árbol:
```json
{
  "arbol_id": "AGC-001-A-0234",
  "tenant_id": "finca_los_alamos",
  "coordenadas": {
    "latitud": 4.7110,
    "longitud": -74.0721
  },
  "cultivo": "aguacate",
  "variedad": "Hass",
  "fecha_siembra": "2019-03-15",
  "patron": "Criollo",
  "sector": "Norte",
  "lote": "A",
  "fila": 12,
  "columna": 34,
  "estado_actual": "SAL",
  "ultimo_ndvi": 0.78,
  "edad_anos": 6
}
```

---

## 📚 Documentos relacionados

- [Representación visual de árboles en el mapa](07-02-vista-cuadricula.md)
- [Seguimiento de salud por ubicación](07-03-salud-fenologia.md)

---

> Navegación: [← Anterior](../06-objetivos-sistema.md) | [📑 Índice](../README.md) | [Siguiente →](07-02-vista-cuadricula.md)
