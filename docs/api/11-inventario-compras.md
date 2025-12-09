# API - Inventario y Compras

> Endpoints para gestión de inventario de insumos y órdenes de compra

## Resumen

Gestión completa de productos agrícolas, movimientos de inventario y proceso de compras.

---

## Endpoints de Inventario

### `GET /api/v1/inventario/productos`

Lista productos/insumos del inventario.

**Autenticación:** Requerida

**Permisos:** `inventario:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| tipo | string | No | insecticida, fungicida, fertilizante, etc. |
| stock_bajo | boolean | No | Filtrar productos con stock bajo |
| buscar | string | No | Búsqueda por nombre o ingrediente activo |

**Response 200:**
```json
{
  "data": [
    {
      "id": 101,
      "tipo": "insecticida",
      "nombre_comercial": "Insecticida XYZ",
      "ingrediente_activo": "Imidacloprid",
      "concentracion": "350 SC",
      "stock_actual": 5.5,
      "stock_minimo": 2.0,
      "unidad_medida": "L",
      "precio_unitario": 45.50,
      "alerta_stock_bajo": true
    }
  ]
}
```

---

### `POST /api/v1/inventario/productos`

Registra un nuevo producto en inventario.

**Autenticación:** Requerida

**Permisos:** `inventario:crear`

**Request Body:**
```json
{
  "tipo": "fungicida",
  "nombre_comercial": "Fungicida ABC",
  "ingrediente_activo": "Mancozeb",
  "concentracion": "80% WP",
  "presentacion": "polvo",
  "unidad_medida": "kg",
  "fabricante": "AgroTech S.A.",
  "registro_sanitario": "RS-12345",
  "categoria_toxicologica": "III",
  "periodo_carencia_dias": 14,
  "precio_unitario": 25.00,
  "stock_minimo": 5.0
}
```

**Response 201:**
```json
{
  "id": 102,
  "nombre_comercial": "Fungicida ABC",
  "created_at": "2024-01-15T20:00:00Z"
}
```

---

### `POST /api/v1/inventario/movimientos`

Registra un movimiento de inventario (entrada/salida).

**Autenticación:** Requerida

**Permisos:** `inventario:actualizar`

**Request Body:**
```json
{
  "producto_id": 101,
  "tipo_movimiento": "salida",
  "cantidad": 1.5,
  "motivo": "Aplicación en Lote Norte A",
  "aplicacion_id": 456,
  "fecha": "2024-01-18T08:00:00Z",
  "observaciones": "Consumo en aplicación fitosanitaria"
}
```

**Response 201:**
```json
{
  "id": 567,
  "producto_id": 101,
  "tipo_movimiento": "salida",
  "cantidad": 1.5,
  "stock_anterior": 5.5,
  "stock_nuevo": 4.0,
  "created_at": "2024-01-18T08:15:00Z"
}
```

---

## Endpoints de Compras

### `GET /api/v1/compras/ordenes`

Lista órdenes de compra.

**Autenticación:** Requerida

**Permisos:** `compras:leer`

**Query Parameters:**
| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|-----------|-------------|
| estado | string | No | borrador, enviada, recibida, cancelada |
| fecha_desde | date | No | Fecha inicial |
| fecha_hasta | date | No | Fecha final |

**Response 200:**
```json
{
  "data": [
    {
      "id": 89,
      "numero_orden": "OC-2024-0089",
      "proveedor": "Distribuidora Agrícola S.A.",
      "fecha_orden": "2024-01-15",
      "fecha_entrega_estimada": "2024-01-22",
      "items_count": 5,
      "total": 1250.50,
      "moneda": "USD",
      "estado": "enviada"
    }
  ]
}
```

---

### `POST /api/v1/compras/ordenes`

Crea una nueva orden de compra.

**Autenticación:** Requerida

**Permisos:** `compras:crear`

**Request Body:**
```json
{
  "proveedor": "Distribuidora Agrícola S.A.",
  "fecha_entrega_estimada": "2024-01-22",
  "items": [
    {
      "producto_id": 101,
      "cantidad": 10,
      "precio_unitario": 45.50
    },
    {
      "producto_id": 102,
      "cantidad": 20,
      "precio_unitario": 25.00
    }
  ],
  "observaciones": "Entregar en bodega principal"
}
```

**Response 201:**
```json
{
  "id": 89,
  "numero_orden": "OC-2024-0089",
  "total": 955.00,
  "estado": "borrador",
  "created_at": "2024-01-15T20:30:00Z"
}
```

---

### `POST /api/v1/compras/ordenes/{id}/recibir`

Registra la recepción de una orden de compra.

**Autenticación:** Requerida

**Permisos:** `compras:recibir`

**Request Body:**
```json
{
  "fecha_recepcion": "2024-01-22T10:00:00Z",
  "items_recibidos": [
    {
      "item_id": 123,
      "cantidad_recibida": 10,
      "lote_proveedor": "LOTE-2024-A",
      "fecha_vencimiento": "2025-12-31"
    }
  ],
  "observaciones": "Mercancía recibida en buen estado",
  "recibido_por_id": 45
}
```

**Response 200:**
```json
{
  "id": 89,
  "estado": "recibida",
  "fecha_recepcion": "2024-01-22T10:00:00Z",
  "movimientos_inventario": [567, 568],
  "updated_at": "2024-01-22T10:15:00Z"
}
```

---
