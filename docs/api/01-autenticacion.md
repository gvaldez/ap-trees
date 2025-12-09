# API - Autenticación

> Endpoints para autenticación y gestión de sesiones con JSON Web Tokens (JWT)

## Resumen

La autenticación en AgroGrid utiliza JWT (JSON Web Tokens) con dos tipos de tokens:
- **Access Token:** Válido por 15 minutos, incluye claims del usuario y tenant
- **Refresh Token:** Válido por 7 días, permite renovar el access token

## Endpoints

### `POST /api/v1/auth/login`

Inicia sesión y obtiene tokens de acceso.

**Autenticación:** Pública

**Request Body:**
```json
{
  "email": "juan.perez@finca.com",
  "password": "SecurePassword123!"
}
```

**Response 200:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "dGhpc2lz...",
  "tokenType": "Bearer",
  "expiresIn": 900,
  "user": {
    "id": 42,
    "email": "juan.perez@finca.com",
    "nombre": "Juan",
    "apellido": "Pérez",
    "tenant": {
      "id": "uuid-tenant",
      "nombre": "Finca La Esperanza",
      "slug": "finca-esperanza"
    },
    "roles": ["ROLE_TENANT_MANAGER"],
    "permisos": ["fincas:leer", "arboles:actualizar", ...],
    "finca_id": 5
  }
}
```

**Códigos de error:**
- `400` - Email o password faltante
- `401` - Credenciales inválidas
- `403` - Cuenta bloqueada, desactivada o tenant suspendido
- `429` - Demasiados intentos fallidos (rate limiting)

**Notas:**
- Después de 5 intentos fallidos, la cuenta se bloquea por 15 minutos
- El access token debe incluirse en todas las peticiones subsecuentes
- El tenant_id se extrae del usuario y se incluye en el JWT

---

### `POST /api/v1/auth/logout`

Cierra la sesión actual e invalida los tokens.

**Autenticación:** Requerida

**Permisos:** Ninguno (cualquier usuario autenticado)

**Request Body:**
```json
{
  "refreshToken": "dGhpc2lz..."
}
```

**Response 200:**
```json
{
  "message": "Sesión cerrada exitosamente"
}
```

**Códigos de error:**
- `401` - No autenticado o token inválido

**Notas:**
- Invalida la sesión en la base de datos
- Los tokens no podrán ser usados después del logout
- Es importante llamar este endpoint al cerrar sesión

---

### `POST /api/v1/auth/refresh`

Renueva el access token usando el refresh token.

**Autenticación:** Pública (usa refresh token)

**Request Body:**
```json
{
  "refreshToken": "dGhpc2lz..."
}
```

**Response 200:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "bmV3LXJlZnJlc2g...",
  "tokenType": "Bearer",
  "expiresIn": 900
}
```

**Códigos de error:**
- `400` - Refresh token faltante
- `401` - Refresh token inválido, expirado o revocado
- `403` - Usuario o tenant desactivado

**Notas:**
- El refresh token también se renueva (rotación)
- El viejo refresh token se invalida automáticamente
- Llamar este endpoint cuando el access token expire

---

### `POST /api/v1/auth/forgot-password`

Solicita el restablecimiento de contraseña. Envía un email con un token de reset.

**Autenticación:** Pública

**Request Body:**
```json
{
  "email": "juan.perez@finca.com"
}
```

**Response 200:**
```json
{
  "message": "Si el email existe, recibirás un enlace para restablecer tu contraseña"
}
```

**Códigos de error:**
- `400` - Email faltante o inválido
- `429` - Demasiadas solicitudes

**Notas:**
- Por seguridad, siempre retorna 200 aunque el email no exista
- El token de reset expira en 1 hora
- Solo puede haber un token activo por usuario
- El email incluye un link: `https://app.agrogrid.io/reset-password?token={token}`

---

### `POST /api/v1/auth/reset-password`

Restablece la contraseña usando el token recibido por email.

**Autenticación:** Pública (usa reset token)

**Request Body:**
```json
{
  "token": "abc123def456",
  "newPassword": "NewSecurePassword456!",
  "confirmPassword": "NewSecurePassword456!"
}
```

**Response 200:**
```json
{
  "message": "Contraseña restablecida exitosamente"
}
```

**Códigos de error:**
- `400` - Token, password faltante o passwords no coinciden
- `401` - Token inválido o expirado
- `422` - Password no cumple requisitos de seguridad

**Requisitos de contraseña:**
- Mínimo 8 caracteres
- Al menos una mayúscula
- Al menos una minúscula
- Al menos un número
- Al menos un caracter especial

**Notas:**
- El token se invalida después de usar
- Todas las sesiones activas se cierran
- El usuario debe hacer login nuevamente

---

### `GET /api/v1/auth/me`

Obtiene información del usuario autenticado actual.

**Autenticación:** Requerida

**Permisos:** Ninguno (cualquier usuario autenticado)

**Response 200:**
```json
{
  "id": 42,
  "email": "juan.perez@finca.com",
  "nombre": "Juan",
  "apellido": "Pérez",
  "telefono": "+52 123 456 7890",
  "avatar_url": "https://storage.agrogrid.io/avatars/42.jpg",
  "tenant": {
    "id": "uuid-tenant",
    "nombre": "Finca La Esperanza",
    "slug": "finca-esperanza",
    "plan": "Professional",
    "activo": true
  },
  "roles": [
    {
      "id": 5,
      "nombre": "ROLE_TENANT_MANAGER",
      "descripcion": "Gerente con acceso completo",
      "finca_id": null
    }
  ],
  "permisos": [
    "fincas:leer",
    "fincas:actualizar",
    "arboles:crear",
    "arboles:actualizar",
    "inspecciones:crear",
    "aplicaciones:aprobar",
    "reportes:avanzados"
  ],
  "finca_id": 5,
  "activo": true,
  "email_verificado": true,
  "ultimo_login": "2024-01-15T10:30:00Z",
  "requiere_cambio_password": false
}
```

**Códigos de error:**
- `401` - No autenticado o token inválido

**Notas:**
- Este endpoint es útil para validar el token y obtener información actualizada
- Los permisos pueden cambiar si un admin modifica los roles
- La información del tenant incluye si está activo

---

### `PUT /api/v1/auth/change-password`

Cambia la contraseña del usuario autenticado.

**Autenticación:** Requerida

**Permisos:** Ninguno (cualquier usuario autenticado)

**Request Body:**
```json
{
  "currentPassword": "OldPassword123!",
  "newPassword": "NewSecurePassword456!",
  "confirmPassword": "NewSecurePassword456!"
}
```

**Response 200:**
```json
{
  "message": "Contraseña actualizada exitosamente"
}
```

**Códigos de error:**
- `400` - Datos faltantes o passwords no coinciden
- `401` - Password actual incorrecta
- `422` - Nueva password no cumple requisitos

**Notas:**
- Todas las sesiones activas (excepto la actual) se invalidan
- El usuario debe usar la nueva contraseña en el próximo login
- Se recomienda cambiar password cada 90 días

---

### `POST /api/v1/auth/verify-email`

Verifica el email del usuario usando el token enviado por correo.

**Autenticación:** Pública

**Request Body:**
```json
{
  "token": "email-verification-token-123"
}
```

**Response 200:**
```json
{
  "message": "Email verificado exitosamente"
}
```

**Códigos de error:**
- `400` - Token faltante
- `401` - Token inválido o expirado
- `409` - Email ya verificado

---

### `POST /api/v1/auth/resend-verification`

Reenvía el email de verificación.

**Autenticación:** Requerida

**Permisos:** Ninguno (solo si email no verificado)

**Response 200:**
```json
{
  "message": "Email de verificación enviado"
}
```

**Códigos de error:**
- `401` - No autenticado
- `409` - Email ya verificado
- `429` - Demasiadas solicitudes

---

## Estructura del Access Token (JWT)

```json
{
  "sub": "42",
  "email": "juan.perez@finca.com",
  "tenant_id": "uuid-tenant",
  "roles": ["ROLE_TENANT_MANAGER"],
  "finca_id": 5,
  "iat": 1705320000,
  "exp": 1705320900,
  "jti": "unique-token-id"
}
```

### Claims del Token

| Claim | Tipo | Descripción |
|-------|------|-------------|
| `sub` | string | ID del usuario |
| `email` | string | Email del usuario |
| `tenant_id` | string | UUID del tenant |
| `roles` | array | Lista de roles del usuario |
| `finca_id` | integer | ID de la finca principal (opcional) |
| `iat` | integer | Timestamp de emisión |
| `exp` | integer | Timestamp de expiración |
| `jti` | string | ID único del token (para revocación) |

---

## Seguridad

### Rate Limiting por Endpoint

| Endpoint | Límite |
|----------|--------|
| `/auth/login` | 5 intentos cada 15 minutos por IP |
| `/auth/forgot-password` | 3 intentos cada hora por email |
| `/auth/reset-password` | 5 intentos cada hora por IP |
| `/auth/refresh` | 10 intentos cada minuto |
| Otros | Sin límite específico |

### Buenas Prácticas

1. **Almacenar tokens de forma segura:**
   - Web: HttpOnly cookies o memoria (no localStorage)
   - Móvil: Secure storage nativo

2. **Renovar access token proactivamente:**
   - Renovar 1-2 minutos antes de expirar
   - No esperar al error 401

3. **Implementar logout en el cliente:**
   - Limpiar tokens del storage
   - Llamar endpoint de logout
   - Redirigir a login

4. **Manejar sesiones inválidas:**
   - Capturar 401 globalmente
   - Intentar refresh automático
   - Si falla, redirigir a login

---

## Ejemplos de Implementación

### JavaScript/TypeScript

```typescript
class AuthService {
  private accessToken: string | null = null;
  private refreshToken: string | null = null;

  async login(email: string, password: string) {
    const response = await fetch('https://api.agrogrid.io/api/v1/auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email, password })
    });

    if (!response.ok) throw new Error('Login failed');

    const data = await response.json();
    this.accessToken = data.accessToken;
    this.refreshToken = data.refreshToken;
    
    return data.user;
  }

  async refreshAccessToken() {
    const response = await fetch('https://api.agrogrid.io/api/v1/auth/refresh', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ refreshToken: this.refreshToken })
    });

    if (!response.ok) {
      this.logout();
      throw new Error('Refresh failed');
    }

    const data = await response.json();
    this.accessToken = data.accessToken;
    this.refreshToken = data.refreshToken;
  }

  getAuthHeaders() {
    return {
      'Authorization': `Bearer ${this.accessToken}`,
      'Content-Type': 'application/json'
    };
  }

  async logout() {
    try {
      await fetch('https://api.agrogrid.io/api/v1/auth/logout', {
        method: 'POST',
        headers: this.getAuthHeaders(),
        body: JSON.stringify({ refreshToken: this.refreshToken })
      });
    } finally {
      this.accessToken = null;
      this.refreshToken = null;
    }
  }
}
```
