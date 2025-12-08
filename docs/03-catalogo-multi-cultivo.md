## 3. Catálogo Multi-Cultivo

### 3.1 Cultivos Soportados

| Cultivo | Nombre Científico | Variedades Comunes | Estado |
|---------|-------------------|-------------------|--------|
| 🥑 **Aguacate** | *Persea americana* | Hass, Fuerte, Criollo, Reed | ✅ Completo |
| 🍑 **Durazno** | *Prunus persica* | Elberta, O'Henry, Redhaven | ✅ Completo |
| 🍎 **Manzana** | *Malus domestica* | Gala, Fuji, Granny Smith | ✅ Completo |
| 🍋 **Limón** | *Citrus limon* | Eureka, Lisboa, Meyer | ✅ Completo |
| 🍊 **Naranja** | *Citrus sinensis* | Valencia, Navel, Sanguina | ✅ Completo |
| 🍐 **Pera** | *Pyrus communis* | Bartlett, Anjou, Bosc | 🔄 En desarrollo |
| 🍒 **Cereza** | *Prunus avium* | Bing, Rainier, Sweetheart | 🔄 En desarrollo |
| 🫒 **Olivo** | *Olea europaea* | Arbequina, Picual, Hojiblanca | 📋 Planificado |

### 3.2 Estructura del Catálogo de Cultivos

```json
{
  "cultivo_id": "aguacate",
  "nombre_comun": "Aguacate",
  "nombre_cientifico": "Persea americana",
  "familia": "Lauraceae",
  "variedades": [
    {
      "id": "hass",
      "nombre": "Hass",
      "caracteristicas": {
        "piel": "rugosa, oscura",
        "peso_promedio_g": "200-300",
        "tiempo_madurez_meses": "12-18"
      }
    }
  ],
  "etapas_fenologicas": ["reposo", "brotacion", "floracion", "cuajado", "desarrollo_fruto", "madurez", "cosecha"],
  "plagas_comunes": ["trips", "arana_roja", "barrenador"],
  "enfermedades_comunes": ["phytophthora", "antracnosis", "roña"],
  "calibres": [...],
  "requerimientos": {
    "temperatura_optima_c": "20-25",
    "precipitacion_mm_ano": "1200-1800",
    "ph_suelo": "5.5-7.0"
  }
}
```

---

> Navegación: [← Anterior](02-modelo-negocio-saas.md)[📑 Índice](README.md) | [Siguiente →](04-catalogo-plagas-enfermedades.md)
