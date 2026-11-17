# Despliegue en Vercel

El monorepo requiere dos proyectos Vercel y un proyecto Supabase.

## Preparación

Ejecutar las migraciones `001` a `006` en Supabase y comprobar:

```sh
npm test
npm run typecheck
npm run build
```

## Backend

1. Importar el repositorio en Vercel con Root Directory `backend`.
2. Usar NestJS y Node.js 24 con la configuración automática.
3. Configurar `SUPABASE_URL` y `SUPABASE_SECRET_KEY`.
4. Desplegar y comprobar `/api/health` en el dominio asignado.

Vercel administra el puerto; no es necesario configurar `PORT`.

## Frontend

1. Importar el mismo repositorio en otro proyecto con Root Directory `frontend`.
2. Usar Next.js y Node.js 24.
3. Configurar `API_URL` con el origen HTTPS del backend, sin `/api`.
4. Desplegar.
5. Configurar `FRONTEND_ORIGIN` en el backend con el origen HTTPS del frontend y volver a desplegar el backend.

`API_URL` se evalúa durante la compilación: cualquier cambio requiere un nuevo despliegue del frontend. La URL del backend debe ser accesible al proxy; una vista previa protegida puede responder 401 o 403.

## Comprobación

Verificar conexión, creación, persistencia, cambios de estado e imágenes siguiendo [VERIFICACION.md](VERIFICACION.md).

El navegador optimiza las imágenes a un máximo de 2 MiB antes de enviarlas. Backend y Storage admiten hasta 3 MiB por archivo optimizado. Los originales de hasta 500 MB se procesan en el dispositivo y no se envían completos.

La clave secreta se configura únicamente en el backend. La instancia pública debe usar datos ficticios mientras no exista autenticación y autorización.

## Referencias

- [Monorepos](https://vercel.com/docs/monorepos)
- [NestJS](https://vercel.com/docs/frameworks/backend/nestjs)
- [Límites de funciones](https://vercel.com/docs/functions/limitations)
