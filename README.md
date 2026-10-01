# Supabase Local Environment

Entorno de desarrollo local de Supabase. Provee la base de datos PostgreSQL, autenticación (Supabase Auth con OAuth), Storage S3, servidor de correos (Mailpit) y la interfaz gráfica de administración (Supabase Studio).

---

## Servicios y Puertos Locales

Cuando el entorno de Supabase está activo (`supabase start`), los siguientes servicios están disponibles:

| Servicio | URL / Endpoint | Descripción |
| :--- | :--- | :--- |
| **Supabase Studio (Dashboard)** | `http://127.0.0.1:54323` | Panel web para explorar tablas, autenticación, storage y logs. |
| **API Gateway (Kong)** | `http://127.0.0.1:54321` | REST API / GraphQL / Auth / Storage endpoints. |
| **PostgreSQL DB** | `127.0.0.1:54322` | Conexión directa a PostgreSQL (`postgres:postgres`). |
| **Mailpit (Inbucket / SMTP)** | `http://127.0.0.1:54324` | Bandeja web de pruebas para emails (OTP, confirmaciones). |
| **Edge Functions / Inspector**| `127.0.0.1:8083` | Puerto para depuración de Edge Functions. |

---

## Requisitos Previos

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (debe estar en ejecución).
- [Supabase CLI](https://supabase.com/docs/guides/local-development/cli/getting-started):
  ```bash
  # En Windows vía Scoop:
  scoop bucket add supabase https://github.com/supabase/scoop-bucket.git
  scoop install supabase

  # O vía npm:
  npm install -g supabase
  ```

---

## Comandos Principales

Ejecuta estos comandos dentro del directorio `supadev/`:

### 1. Iniciar los servicios
```bash
supabase start
```
> **Nota:** La primera vez descargará las imágenes Docker necesarias y mostrará las claves públicas (`anon key`) y secretas (`service_role key`).

### 2. Detener los servicios
```bash
supabase stop
```

### 3. Resetear la base de datos local
Reinicia la base de datos a un estado limpio sin datos residuales:
```bash
supabase db reset
```

### 4. Ver el estado de los servicios y credenciales
```bash
supabase status
```

---

## Configuración de Variables de Entorno

El archivo [`supabase/config.toml`](./supabase/config.toml) utiliza variables de entorno para servicios externos (como Google OAuth y llaves API). Puedes definir estas variables en tu entorno o en un archivo `.env` en la raíz de `supadev/`:

```env
# Google OAuth (opcional para login social local)
GCLIENT=tu_google_client_id
GSECRET=tu_google_client_secret

# Supabase AI (opcional para asistentes en Studio)
OPENAI_API_KEY=tu_openai_key
```

---

## Flujo de Trabajo con Backend

En este proyecto, la herramienta que utilices para migración será la fuenta de la estructura de las tablas del esquema de la aplicación.

1. **Iniciar Supabase Local:**
   ```bash
   cd supadev
   supabase start
   ```

2. **Explorar datos en Supabase Studio:**
   Abre [`http://127.0.0.1:54323`](http://127.0.0.1:54323) para inspeccionar usuarios (`auth.users`)

---

## Pruebas de Correo Electrónico

Los correos de confirmación, restablecimiento de contraseña y alertas generadas en local no se envían a internet. Se capturan en **Mailpit**:
👉 Abre [`http://127.0.0.1:54324`](http://127.0.0.1:54324) para ver todos los correos entrantes de prueba en tiempo real.
