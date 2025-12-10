# 🌳 AgroGrid SaaS - Documentación del Sistema

## Plataforma Multi-Cultivo de Control Árbol por Árbol

Bienvenido a la documentación modular del sistema **AgroGrid**, una plataforma SaaS multi-tenant para la gestión integral de fincas frutícolas con seguimiento árbol por árbol.

> 📁 **Nota**: Esta es la versión modular de la especificación del sistema. El archivo consolidado original está disponible en [`ESPECIFICACION_SISTEMA_LEGACY.md`](../ESPECIFICACION_SISTEMA_LEGACY.md) como referencia histórica.

---

## 📖 Tabla de Contenidos

### 📋 Documentación General

| # | Documento | Descripción |
|---|-----------|-------------|
| 1 | [Resumen Ejecutivo](01-resumen-ejecutivo.md) | Visión general del sistema, características distintivas y benchmarks |
| 2 | [Modelo de Negocio SaaS](02-modelo-negocio-saas.md) | Arquitectura multi-tenant, planes de suscripción y modelo de reventa |
| 3 | [Catálogo Multi-Cultivo](03-catalogo-multi-cultivo.md) | Soporte para múltiples especies frutícolas y sus características |
| 4 | [Catálogo de Plagas y Enfermedades](04-catalogo-plagas-enfermedades.md) | Base de datos de plagas, enfermedades y tratamientos por cultivo |
| 5 | [API de Agronomía de Precisión](05-api-agronomia-precision.md) | Integración con drones, satélites y análisis multiespectrales |
| 6 | [Objetivos del Sistema](06-objetivos-sistema.md) | Metas y principios guía del desarrollo |

### 🔧 Módulos Funcionales

| # | Módulo | Descripción |
|---|--------|-------------|
| 7.1 | [📍 Mapeo y Geolocalización](modulos/07-01-mapeo-geolocalizacion.md) | Registro de ubicación GPS de cada árbol y visualización en mapa |
| 7.2 | [🔲 Vista de Cuadrícula](modulos/07-02-vista-cuadricula.md) | Representación visual estilo hoja cuadriculada (CORE) |
| 7.3 | [🌱 Salud y Fenología](modulos/07-03-salud-fenologia.md) | Seguimiento de estados fenológicos y condiciones de salud |
| 7.4 | [💧 Infraestructura Hídrica y Riego](modulos/07-04-infraestructura-hidrica-riego.md) | Gestión de sistemas de riego, válvulas y programación |
| 7.5 | [🧪 Aplicaciones con Cálculo de Dosis](modulos/07-05-aplicaciones-calculo-dosis.md) | Registro de aplicaciones fitosanitarias con dosificación automática |
| 7.6 | [📋 Planificación Semanal](modulos/07-06-planificacion-semanal.md) | Programación de tareas y mantenimiento preventivo |
| 7.7 | [📱 App Móvil de Campo](modulos/07-07-app-movil-campo.md) | Aplicación ligera para trabajadores en campo |
| 7.8 | [📦 Compras e Inventario](modulos/07-08-compras-inventario.md) | Control de stock, órdenes de compra y gestión de proveedores |
| 7.9 | [💰 Costos, Ventas y Rentabilidad](modulos/07-09-costos-ventas-rentabilidad.md) | Análisis financiero y rentabilidad por árbol/lote |
| 7.10 | [📊 Reportes Integrados](modulos/07-10-reportes-integrados.md) | Dashboards y reportes consolidados del sistema |

### 🏗️ Arquitectura y Planificación

| # | Documento | Descripción |
|---|-----------|-------------|
| 8 | [Arquitectura Técnica](08-arquitectura-tecnica.md) | Stack tecnológico, base de datos y arquitectura del sistema |
| 9 | [Plan de Implementación](09-plan-implementacion.md) | Fases de desarrollo y entregables por sprint |
| 10 | [Métricas de Éxito](10-metricas-exito.md) | KPIs y criterios de evaluación del sistema |
| 11 | [Resumen de Especificación](11-resumen-especificacion.md) | Síntesis de características y capacidades |
| 12 | [Próximos Pasos](12-proximos-pasos.md) | Roadmap y siguientes acciones |
| 17 | [🔌 API REST](17-api-rest.md) | Documentación completa de endpoints REST por dominio |
| 18 | [Plan MVP y Roadmap](18-plan-mvp.md) | Planificación de desarrollo por fases |
| 19 | [Catálogos y Datos Semilla](19-catalogos-datos-semilla.md) | Scripts SQL de catálogos y datos de prueba |

### 👥 UX y Experiencia de Usuario

| # | Documento | Descripción |
|---|-----------|-------------|
| 13 | [Módulo Administrador](13-modulo-administrador.md) | Panel interno del equipo AgroGrid, gestión de plataforma |
| 14 | [Módulo Usuarios](14-modulo-usuarios.md) | Roles, permisos y funcionalidades para clientes |
| 15 | [Customer Journeys](15-customer-journeys.md) | Flujos de usuario: onboarding, setup, operación |
| 16 | [Wireframes](16-wireframes.md) | Diseños de pantallas principales |

---

## 🗺️ Mapa de Dependencias entre Módulos

```
┌─────────────────────────────────────────────────────────────────┐
│                    FLUJO DE INFORMACIÓN                         │
└─────────────────────────────────────────────────────────────────┘

    ┌──────────────────────────────────────────────────┐
    │  📍 Mapeo y          🔲 Vista de                  │
    │  Geolocalización     Cuadrícula                   │
    │  (Base espacial)     (Visualización)              │
    └──────────┬───────────────┬───────────────────────┘
               │               │
               ▼               ▼
    ┌──────────────────────────────────────────────────┐
    │  🌱 Salud y Fenología                            │
    │  (Estado de árboles)                             │
    └──────────┬───────────────────────────────────────┘
               │
               ├──────────────────┬─────────────────────┐
               ▼                  ▼                     ▼
    ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
    │  💧 Riego e       │  │  🧪 Aplicaciones │  │  📋 Planificación│
    │  Infraestructura  │  │  y Dosis         │  │  Semanal         │
    └──────────┬────────┘  └──────────┬───────┘  └──────────┬───────┘
               │                      │                     │
               └──────────┬───────────┴─────────────────────┘
                          ▼
               ┌──────────────────────────┐
               │  📱 App Móvil Campo      │
               │  (Ejecución tareas)      │
               └──────────┬───────────────┘
                          │
                ┌─────────┴──────────┐
                ▼                    ▼
    ┌──────────────────────┐  ┌──────────────────────┐
    │  📦 Compras e        │  │  💰 Costos, Ventas   │
    │  Inventario          │  │  y Rentabilidad      │
    └──────────┬───────────┘  └──────────┬───────────┘
               │                         │
               └──────────┬──────────────┘
                          ▼
               ┌──────────────────────────┐
               │  📊 Reportes Integrados  │
               │  (Análisis y decisiones) │
               └──────────────────────────┘
```

---

## 🧭 Cómo Navegar la Documentación

### Para Desarrolladores
1. Comienza con el [Resumen Ejecutivo](01-resumen-ejecutivo.md) para entender la visión general
2. Revisa la [Arquitectura Técnica](08-arquitectura-tecnica.md) para comprender el stack tecnológico
3. Consulta la [API REST](17-api-rest.md) para conocer los endpoints disponibles
4. Explora los módulos individuales según el área en la que trabajarás
5. Consulta el [Plan de Implementación](09-plan-implementacion.md) para conocer las prioridades

### Para Product Managers
1. Lee el [Modelo de Negocio SaaS](02-modelo-negocio-saas.md) para entender el modelo comercial
2. Revisa los [Objetivos del Sistema](06-objetivos-sistema.md) para alinear expectativas
3. Consulta cada módulo funcional para especificaciones detalladas
4. Verifica las [Métricas de Éxito](10-metricas-exito.md) para definir KPIs

### Para Stakeholders
1. Inicia con el [Resumen Ejecutivo](01-resumen-ejecutivo.md)
2. Consulta el [Resumen de Especificación](11-resumen-especificacion.md) para una vista consolidada
3. Revisa los [Próximos Pasos](12-proximos-pasos.md) para conocer el roadmap

---

## 🔄 Relaciones entre Módulos Clave

### Módulos Centrales (CORE)
- **Vista de Cuadrícula** + **Mapeo**: Base visual y espacial del sistema
- **Salud y Fenología**: Estado y seguimiento de cada árbol

### Módulos de Operación
- **Riego**: Depende de salud, alimenta a planificación
- **Aplicaciones**: Usa catálogo de plagas, consume inventario
- **Planificación**: Coordina riego, aplicaciones y tareas de campo

### Módulos de Gestión
- **Inventario**: Controla insumos consumidos por aplicaciones y riego
- **Costos y Ventas**: Integra datos de inventario, planificación y cosecha
- **Reportes**: Consolida información de todos los módulos

---

## 📚 Recursos Adicionales

- **Archivo Legacy**: [ESPECIFICACION_SISTEMA_LEGACY.md](../ESPECIFICACION_SISTEMA_LEGACY.md) - Especificación consolidada completa
- **Repositorio**: [github.com/gvaldez/ap-trees](https://github.com/gvaldez/ap-trees)

---

## 📝 Notas de Versión

- **Versión 2.5**: Documentación API REST (Diciembre 2025)
  - 🔌 Documento índice de API REST (17-api-rest.md)
  - 📁 16 documentos de endpoints organizados por dominio en `/api/`
  - 🔐 Autenticación JWT con refresh tokens
  - 📄 Estándar RFC 7807 para errores
  - 📊 Paginación HATEOAS y rate limiting por plan
- **Versión 2.4**: Documentación UX y Experience (Diciembre 2025)
  - Módulo Administrador (backoffice interno)
  - Módulo Usuarios (roles y permisos de clientes)
  - Customer Journeys (flujos de usuario detallados)
  - Wireframes (diseños ASCII de pantallas)
- **Versión 2.4**: Actualización del Stack Tecnológico (Diciembre 2025)
  - ✨ **Frontend Web**: Migración a Angular 17+ con Standalone Components
  - 📱 **App Móvil**: Cambio a Ionic 7+ con Angular 17+ y Capacitor 5+
  - ☕ **Backend**: Actualización a Spring Boot 3.2 con Java 17
  - 📁 **Estructura del Proyecto**: Definición de arquitectura de monorepo
  - ⚙️ **Configuración**: Ejemplos de configuración Angular e Ionic/Capacitor
  - 📡 **Servicios**: Implementación de servicios offline-first, cámara y QR

- **Versión 2.3**: Modularización de la documentación (Diciembre 2025)
  - Separación en archivos individuales por módulo
  - Adición de navegación y referencias cruzadas
  - Archivo legacy creado como respaldo

- **Versión 2.2**: Módulos 7.8-7.10 añadidos
  - Compras e Inventario
  - Costos, Ventas y Rentabilidad
  - Reportes Integrados

- **Versión 2.1**: Expansión de módulos 7.4-7.7
  - Infraestructura Hídrica detallada
  - Cálculo automático de dosis
  - Planificación semanal completa
  - App móvil especificada

- **Versión 2.0**: Base multi-tenant y multi-cultivo establecida

---

> 🚀 **¿Listo para comenzar?** Empieza con el [Resumen Ejecutivo](01-resumen-ejecutivo.md)
