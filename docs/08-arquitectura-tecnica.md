## 8. Arquitectura Técnica

### 8.1 Stack Tecnológico

```
Frontend Web:
├── Angular 17+ (Standalone Components)
├── Angular Material / PrimeNG (UI Components)
├── NgRx (State Management)
├── Leaflet / Mapbox GL JS (Mapas interactivos)
├── Chart.js / ngx-charts (Gráficos)
├── Angular PWA (Progressive Web App)
└── RxJS (Reactive Programming)

App Móvil:
├── Ionic 7+ con Angular 17+
├── Capacitor 5+ (Bridge nativo)
├── Capacitor Plugins:
│   ├── @capacitor/camera (captura de fotos)
│   ├── @capacitor/geolocation (GPS)
│   ├── @capawesome/capacitor-mlkit-barcode-scanning (QR/Barcode)
│   ├── @capacitor/filesystem (almacenamiento local)
│   ├── @capacitor/network (estado de conectividad)
│   └── @capacitor/splash-screen
├── Ionic Storage + SQLite (offline-first)
└── Builds: iOS (App Store) + Android (Play Store) + PWA

Backend:
├── Java 17 + Spring Boot 3.2
├── Spring Security 6 + JWT
├── Spring Data JPA + Hibernate
├── PostgreSQL 15 + PostGIS + TimescaleDB
├── Redis (Cache)
├── RabbitMQ (Cola de mensajes)
├── MinIO / AWS S3 (Storage)
└── SpringDoc OpenAPI (Swagger)

ML/AI (Microservicio separado):
├── Python + FastAPI
├── PyTorch / TensorFlow
├── GDAL, Rasterio (procesamiento geoespacial)
├── YOLO (detección de objetos)
├── CNN (clasificación de imágenes)
└── MLflow (MLOps)

Infraestructura:
├── Docker + Kubernetes
├── GitHub Actions (CI/CD)
├── AWS / GCP / Azure
├── CloudFlare (CDN)
└── Grafana + Prometheus (Monitoreo)
```

### 8.2 Modelo de Datos Multi-Tenant

```sql
-- Tenant (Cliente/Organización)
CREATE TABLE tenants (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nombre VARCHAR(100) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    plan VARCHAR(20) DEFAULT 'starter',
    configuracion JSONB,
    activo BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Cultivos habilitados por tenant
CREATE TABLE tenant_cultivos (
    tenant_id UUID REFERENCES tenants(id),
    cultivo_id VARCHAR(50) NOT NULL,
    configuracion_especifica JSONB,
    PRIMARY KEY (tenant_id, cultivo_id)
);

-- Todas las tablas principales tienen tenant_id
CREATE TABLE fincas (
    id SERIAL PRIMARY KEY,
    tenant_id UUID REFERENCES tenants(id),
    nombre VARCHAR(100) NOT NULL,
    ...
);

-- Row Level Security para aislamiento
ALTER TABLE fincas ENABLE ROW LEVEL SECURITY;
CREATE POLICY tenant_isolation ON fincas
    USING (tenant_id = current_setting('app.current_tenant')::UUID);
```

### 8.7 Estructura del Proyecto

```
agrogrid/
├── apps/
│   ├── web/                    # Angular Web App
│   │   ├── src/app/
│   │   │   ├── core/           # Servicios singleton, guards, interceptors
│   │   │   ├── shared/         # Componentes, pipes, directives compartidos
│   │   │   ├── features/       # Módulos lazy-loaded por funcionalidad
│   │   │   │   ├── auth/
│   │   │   │   ├── dashboard/
│   │   │   │   ├── fincas/
│   │   │   │   ├── arboles/
│   │   │   │   ├── cuadricula/
│   │   │   │   ├── inspecciones/
│   │   │   │   ├── aplicaciones/
│   │   │   │   ├── tareas/
│   │   │   │   ├── inventario/
│   │   │   │   ├── reportes/
│   │   │   │   └── configuracion/
│   │   │   ├── app.component.ts
│   │   │   ├── app.config.ts
│   │   │   └── app.routes.ts
│   │   └── angular.json
│   │
│   ├── mobile/                 # Ionic + Angular Mobile App
│   │   ├── src/app/
│   │   │   ├── core/
│   │   │   │   ├── services/
│   │   │   │   │   ├── offline.service.ts
│   │   │   │   │   ├── sync.service.ts
│   │   │   │   │   ├── camera.service.ts
│   │   │   │   │   └── geolocation.service.ts
│   │   │   │   └── guards/
│   │   │   ├── shared/
│   │   │   ├── pages/
│   │   │   │   ├── login/
│   │   │   │   ├── home/
│   │   │   │   ├── tareas/
│   │   │   │   ├── inspeccion/
│   │   │   │   ├── qr-scan/
│   │   │   │   ├── arbol-detalle/
│   │   │   │   └── sync-status/
│   │   │   └── tabs/
│   │   ├── capacitor.config.ts
│   │   ├── ionic.config.json
│   │   └── angular.json
│   │
│   └── admin/                  # Angular Admin Panel (interno AgroGrid)
│       └── src/app/
│           ├── features/
│           │   ├── tenants/
│           │   ├── catalogos/
│           │   ├── planes/
│           │   ├── soporte/
│           │   └── monitoreo/
│           └── ...
│
├── libs/                       # Librerías compartidas (Nx opcional)
│   ├── shared-models/          # Interfaces TypeScript
│   │   ├── src/lib/
│   │   │   ├── arbol.model.ts
│   │   │   ├── finca.model.ts
│   │   │   ├── inspeccion.model.ts
│   │   │   └── index.ts
│   ├── shared-utils/           # Utilidades comunes
│   └── api-client/             # Cliente HTTP (generado desde OpenAPI)
│
├── backend/                    # Spring Boot API
│   ├── src/main/java/com/agrogrid/api/
│   │   ├── config/
│   │   ├── security/
│   │   ├── common/
│   │   └── modules/
│   ├── src/main/resources/
│   └── pom.xml
│
├── ml-service/                 # Python FastAPI
│   ├── app/
│   ├── models/
│   └── requirements.txt
│
├── database/
│   ├── migrations/             # Flyway/Liquibase
│   └── seeds/                  # Datos iniciales
│
├── docker/
│   ├── docker-compose.yml
│   ├── docker-compose.dev.yml
│   └── Dockerfiles/
│
├── docs/                       # Documentación (actual)
│
├── nx.json                     # Nx workspace (opcional)
├── package.json
└── README.md
```

### 8.8 Configuración Angular

#### 8.8.1 Módulos Core de Angular

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding } from '@angular/router';
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { provideAnimations } from '@angular/platform-browser/animations';

import { routes } from './app.routes';
import { authInterceptor } from './core/interceptors/auth.interceptor';
import { tenantInterceptor } from './core/interceptors/tenant.interceptor';
import { errorInterceptor } from './core/interceptors/error.interceptor';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withComponentInputBinding()),
    provideHttpClient(
      withInterceptors([authInterceptor, tenantInterceptor, errorInterceptor])
    ),
    provideAnimations(),
  ],
};
```

#### 8.8.2 Interceptor de Autenticación

```typescript
// auth.interceptor.ts
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { AuthService } from '../services/auth.service';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authService = inject(AuthService);
  const token = authService.getAccessToken();

  if (token) {
    req = req.clone({
      setHeaders: {
        Authorization: `Bearer ${token}`,
      },
    });
  }

  return next(req);
};
```

#### 8.8.3 Servicio de API Base

```typescript
// api.service.ts
import { Injectable, inject } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable } from 'rxjs';
import { environment } from '@env/environment';

@Injectable({ providedIn: 'root' })
export class ApiService {
  private http = inject(HttpClient);
  private baseUrl = environment.apiUrl;

  get<T>(endpoint: string, params?: Record<string, any>): Observable<T> {
    const httpParams = new HttpParams({ fromObject: params || {} });
    return this.http.get<T>(`${this.baseUrl}${endpoint}`, { params: httpParams });
  }

  post<T>(endpoint: string, body: any): Observable<T> {
    return this.http.post<T>(`${this.baseUrl}${endpoint}`, body);
  }

  put<T>(endpoint: string, body: any): Observable<T> {
    return this.http.put<T>(`${this.baseUrl}${endpoint}`, body);
  }

  delete<T>(endpoint: string): Observable<T> {
    return this.http.delete<T>(`${this.baseUrl}${endpoint}`);
  }
}
```

### 8.9 Configuración Ionic/Capacitor

#### 8.9.1 Configuración de Capacitor

```typescript
// capacitor.config.ts
import { CapacitorConfig } from '@capacitor/cli';

const config: CapacitorConfig = {
  appId: 'com.agrogrid.app',
  appName: 'AgroGrid',
  webDir: 'www',
  server: {
    androidScheme: 'https',
  },
  plugins: {
    SplashScreen: {
      launchShowDuration: 2000,
      backgroundColor: '#22c55e',
      showSpinner: true,
      spinnerColor: '#ffffff',
    },
    Camera: {
      presentationStyle: 'fullscreen',
    },
    Geolocation: {
      enableHighAccuracy: true,
    },
  },
};

export default config;
```

#### 8.9.2 Servicio Offline

```typescript
// offline.service.ts
import { Injectable, inject } from '@angular/core';
import { Storage } from '@ionic/storage-angular';
import { Network } from '@capacitor/network';
import { BehaviorSubject, Observable } from 'rxjs';

export interface PendingSync {
  id: string;
  type: 'inspeccion' | 'tarea' | 'aplicacion';
  data: any;
  timestamp: Date;
  retries: number;
}

@Injectable({ providedIn: 'root' })
export class OfflineService {
  private storage = inject(Storage);
  private isOnline$ = new BehaviorSubject<boolean>(true);
  private pendingSync$ = new BehaviorSubject<PendingSync[]>([]);

  async init(): Promise<void> {
    await this.storage.create();
    
    // Monitorear estado de red
    Network.addListener('networkStatusChange', (status) => {
      this.isOnline$.next(status.connected);
      if (status.connected) {
        this.syncPendingData();
      }
    });

    // Cargar pendientes
    const pending = await this.storage.get('pending_sync') || [];
    this.pendingSync$.next(pending);
  }

  get online$(): Observable<boolean> {
    return this.isOnline$.asObservable();
  }

  async queueForSync(type: PendingSync['type'], data: any): Promise<void> {
    const pending = this.pendingSync$.value;
    const newItem: PendingSync = {
      id: crypto.randomUUID(),
      type,
      data,
      timestamp: new Date(),
      retries: 0,
    };
    pending.push(newItem);
    await this.storage.set('pending_sync', pending);
    this.pendingSync$.next(pending);
  }

  private async syncPendingData(): Promise<void> {
    // Implementar lógica de sincronización
  }
}
```

#### 8.9.3 Servicio de Cámara/QR

```typescript
// camera.service.ts
import { Injectable } from '@angular/core';
import { Camera, CameraResultType, CameraSource } from '@capacitor/camera';
import { BarcodeScanner, BarcodeFormat } from '@capawesome/capacitor-mlkit-barcode-scanning';

@Injectable({ providedIn: 'root' })
export class CameraService {
  
  async takePhoto(): Promise<string | null> {
    try {
      const photo = await Camera.getPhoto({
        quality: 80,
        allowEditing: false,
        resultType: CameraResultType.Base64,
        source: CameraSource.Camera,
      });
      return photo.base64String || null;
    } catch (error) {
      console.error('Error taking photo:', error);
      return null;
    }
  }

  async scanQRCode(): Promise<string | null> {
    try {
      const { barcodes } = await BarcodeScanner.scan({
        formats: [BarcodeFormat.QrCode],
      });
      return barcodes[0]?.rawValue || null;
    } catch (error) {
      console.error('Error scanning QR:', error);
      return null;
    }
  }
}
```

---

> Navegación: [← Anterior](modulos/07-10-reportes-integrados.md)[📑 Índice](README.md) | [Siguiente →](09-plan-implementacion.md)
