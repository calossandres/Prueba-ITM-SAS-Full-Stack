# Mesa · Gestión de soporte interno

**Español** | [English](README.en.md)

Aplicación web para registrar, organizar y dar seguimiento a solicitudes de soporte tecnológico.

## Funciones

- Creación y edición de solicitudes abiertas.
- Búsqueda, filtros por estado, prioridad y categoría, orden y paginación.
- Historial de cambios con responsable, fecha y observaciones.
- Imagen del problema al crear y evidencia de solución al resolver.
- Interfaz adaptable a escritorio, tablet y móvil.

```text
Pendiente → En progreso → Resuelta
Pendiente → Cancelada
```

Las solicitudes resueltas y canceladas se conservan para consulta. Resolver una solicitud crítica requiere una observación. Las imágenes guardadas no se reemplazan ni se eliminan.

## Tecnologías

Next.js, React, TypeScript y Tailwind CSS; API REST con NestJS; PostgreSQL y Storage en Supabase. Las operaciones y su historial se guardan mediante una función SQL transaccional.

## Instalación

Requisitos: Node.js 24, npm y un proyecto Supabase.

Clona este repositorio y ejecuta desde su carpeta:

```sh
npm ci
```

Crea `.env.local` en la raíz con estas variables:

```dotenv
SUPABASE_URL=https://YOUR_PROJECT.supabase.co
SUPABASE_SECRET_KEY=sb_secret_YOUR_KEY
PORT=3001
FRONTEND_ORIGIN=http://localhost:3000
```

Obtén la URL en Supabase → Settings → Data API y la clave en Settings → API Keys → Secret keys. La clave se usa exclusivamente en el backend.

Ejecuta los archivos de [supabase/migrations](supabase/migrations) en el SQL Editor, en orden del `001` al `006`.

```sh
npm run dev
```

- Aplicación: [localhost:3000](http://localhost:3000).
- API: [localhost:3001/api/requests](http://localhost:3001/api/requests).
- Salud: [localhost:3001/api/health](http://localhost:3001/api/health).

## Verificación

```sh
npm test
npm run typecheck
npm run build
```

Las pruebas usan PostgreSQL embebido y servicios simulados; no requieren credenciales de Supabase.

## Despliegue

El monorepo se publica como dos proyectos Vercel:

| Proyecto | Directorio | Variables |
|---|---|---|
| API NestJS | `backend` | `SUPABASE_URL`, `SUPABASE_SECRET_KEY`, `FRONTEND_ORIGIN` |
| Aplicación Next.js | `frontend` | `API_URL` |

`API_URL` debe contener el origen HTTPS del backend, sin `/api`. Consulta la [guía de despliegue](docs/VERCEL.md).

## Documentación

- [Arquitectura](docs/ARQUITECTURA.md)
- [API REST](docs/API.md)
- [Imágenes](docs/IMAGENES.md)
- [Verificación manual](docs/VERIFICACION.md)

## Acceso

La versión actual no incluye autenticación ni roles; el responsable se introduce manualmente. La instancia pública debe utilizar datos ficticios. El uso con información real requiere autenticación y autorización.
