# Imágenes

Cada solicitud admite dos evidencias opcionales:

- **Problema:** se adjunta al crear la solicitud.
- **Solución:** se adjunta al pasar de En progreso a Resuelta, con responsable y fecha.

Las evidencias guardadas no se reemplazan ni se eliminan. La imagen del problema no se añade después de la creación. Antes de enviar el formulario se puede cambiar la selección.

## Archivos y almacenamiento

Se aceptan JPG, PNG y WebP de hasta 500 MB por original. El navegador valida dimensiones y optimiza a un máximo de 2400 píxeles de lado y 2 MiB. Si el dispositivo no puede procesar el archivo, se rechaza la selección.

NestJS valida el contenido con Sharp y lo recodifica como WebP. Backend y bucket admiten hasta 3 MiB por archivo optimizado. El original no se envía ni se almacena completo.

El bucket `request-images` es privado. El detalle entrega enlaces firmados durante diez minutos; volver a abrirlo renueva los enlaces. La API no autentica usuarios, por lo que este almacenamiento privado no restringe quién puede consultar una solicitud mediante la API.

## Historial

Crear con imagen genera un único evento Creación. Resolver con imagen guarda el estado, la referencia y los eventos de estado y evidencia en una transacción SQL. Una crítica requiere observación incluso si incluye imagen.

El archivo se sube antes de confirmar la referencia. Si falla la transacción, el backend intenta eliminar ese archivo nuevo; un fallo de limpieza requiere revisión del almacenamiento.

## Configuración y API

Aplicar las migraciones `001` a `006`. La `004` configura Storage y la `006` protege las evidencias.

| Ruta | Contenido |
|---|---|
| `POST /api/requests` | JSON o multipart con los campos de creación e `image` opcional |
| `PATCH /api/requests/:id/status` | JSON o multipart con `status`, `actor`, `observation` e `image` opcional al resolver |
| `GET /api/requests/:id` | Referencias, enlaces firmados y responsable/fecha de solución |

Las rutas antiguas `POST` y `DELETE /api/requests/:id/image` responden 409 para impedir cambios sobre la imagen original.
