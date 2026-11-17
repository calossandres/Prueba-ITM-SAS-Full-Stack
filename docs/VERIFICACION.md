# Verificación manual

## Preparación

1. Configurar `.env.local` y ejecutar las migraciones `001` a `006`.
2. Iniciar `npm run dev`.
3. Comprobar `/api/health`: `status: "ok"`, `image_creation_atomic: true` y `evidence_images: true`.

## Casos

| Caso | Comprobación |
|---|---|
| Crear | Datos guardados, estado Pendiente y un evento de creación |
| Validar | Rechazo de campos vacíos o longitudes inválidas |
| Editar | Datos actualizados e historial con responsable y campos modificados |
| Iniciar atención | Pendiente → En progreso |
| Resolver | En progreso → Resuelta; observación obligatoria para críticas |
| Cancelar | Pendiente → Cancelada, conservando datos e historial |
| Cerrar | Resueltas y canceladas sin edición ni reapertura |
| Buscar | Coincidencias en título, descripción o solicitante |
| Filtrar | Resultados que cumplan estado, prioridad y categoría |
| Ordenar | Orden correcto por fecha, título o prioridad |
| Paginar | Con nueve registros, segunda página sin duplicados |
| Persistir | Datos conservados después de recargar |
| Adaptar pantalla | Formularios, filtros y listado utilizables en escritorio, tablet y móvil |
| Imagen inicial | Un evento de creación; evidencia inmutable |
| Imagen de solución | Ambas imágenes conservadas y evento de evidencia con responsable y fecha |
| Archivo inválido | Rechazo sin solicitud ni cambio de estado parcial |

## Comprobaciones automáticas

```sh
npm test
npm run typecheck
npm run build
```

Las pruebas automatizadas no verifican las credenciales ni el despliegue público. Repetir los casos principales en el entorno publicado.

## Diagnóstico

| Error | Revisar |
|---|---|
| `EADDRINUSE` | Procesos duplicados en los puertos 3000 o 3001 |
| `Cannot find module` | Dependencias instaladas con `npm ci` |
| `PGRST202` | Migraciones y función `support_api` en el proyecto configurado |
| `401` o `42501` | Clave secreta vigente, proyecto y permisos |
| Error de imagen | Migraciones `004` a `006` y bucket privado `request-images` |
