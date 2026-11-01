# Configuración de Supabase

MotoTraker conserva una copia local y sincroniza una copia persistente en Supabase al iniciar sesión. Para usar la misma información en varios dispositivos, inicia sesión en todos con la misma cuenta.

## Preparar el proyecto

1. **Obligatorio: crear la tabla.** La comprobación del proyecto confirmó que `public.mototracker_data` todavía no está creada. En Supabase, abre **SQL Editor**, pega el contenido de [`supabase.sql`](./supabase.sql) y ejecútalo. La clave pública del navegador no tiene permisos para crear tablas ni políticas. La tabla guarda el estado del prototipo como JSONB y limita el acceso a la fila del usuario autenticado mediante RLS.
2. En **Authentication → Providers**, habilita Email. Crea o confirma en Supabase Auth el usuario autorizado que usará la aplicación. La pantalla de MotoTraker solo permite iniciar sesión con la cuenta autorizada; no ofrece registro público. Si la confirmación de correo está activa, confirma la cuenta antes del primer acceso.
3. La Project URL y la clave **publishable** del proyecto están configuradas como valores públicos predeterminados en MotoTraker. Si cambias de proyecto, reemplázalas desde **Configuración del proyecto** en la tarjeta de cuenta. No uses la clave `service_role`.
4. Inicia sesión con la cuenta autorizada en cada dispositivo. La primera sesión sube los datos que ya estuvieran guardados localmente si aún no existe una fila remota. Si ya hay datos en Supabase, se combinan con los registros locales para evitar perder historiales; el odómetro conserva el valor mayor y los campos locales completados tienen prioridad.

La aplicación sincroniza los cambios automáticamente y permite solicitar una sincronización manual. La tarjeta superior informa si hay cambios pendientes o errores. **Exportar copia (JSON)** sigue disponible como respaldo independiente.

## Seguridad y alcance

La clave anon/publishable se entrega al navegador y no es una contraseña del proyecto. La política RLS de `supabase.sql` exige una sesión y limita cada cuenta a su propia fila. La cuenta compartida permite acceder a los datos de esta moto desde los dispositivos en los que se use esa cuenta; no compartas sus credenciales con personas no autorizadas.

Los datos están disponibles mientras el proyecto Supabase exista y la cuenta conserve acceso. Para comprobarlos desde el panel, consulta `public.mototracker_data` en **Table Editor**. La aplicación requiere HTTPS para conectarse desde un sitio publicado.
