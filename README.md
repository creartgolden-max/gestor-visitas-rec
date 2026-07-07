# Gestor de Visitas REC

Módulo comercial del Gestor Corporativo Recouromex. HTML único, conectado a
Supabase (proyecto principal, prefijo de tablas `gvis_`). Publicado en GitHub Pages.

## Puesta en marcha

1. **Base de datos** (SQL Editor de Supabase, proyecto principal):
   - Correr el script de **Fase 1** (tablas `gvis_`).
   - Correr `gvis_login_seguro.sql` (RPC `gvis_check_pin`, vista sin PIN, cierre de lectura de PINs a la anon key).
2. **Credenciales**: en `index.html`, bloque `CONFIG` (arriba del `<script>`),
   pegar `SUPABASE_URL` y `SUPABASE_ANON_KEY`. Nadie más toca ese bloque.
3. **PINs**: se administran directo en la tabla `gvis_usuarios` en Supabase
   (la app NO los edita). Cambiar los placeholder `0000` antes de producción.
4. **Publicar**: subir `index.html` a la raíz del repo `gestor-visitas-rec` y
   activar GitHub Pages (branch `main`, carpeta raíz).

## Roles y pestañas

- **Vendedor** (Antonio, José): Mi Día (captura de visitas), Mi Plan, Mi Cartera, Prospectos, Más Buscados.
- **Coordinador** (Jair): Tablero, Plan (mensual/semanal por vendedor), Clientes, Prospectos, Más Buscados, Cortes.
- **Admin** (Alma): Compras del mes, Clientes, Catálogos (tipos/marcas), Umbrales, Usuarios.
- **Dirección** (Rodrigo): Consolidado, Clientes, Cortes trimestrales, Más Buscados.

## Cortes trimestrales

Candidatos calculados con las compras del trimestre y los umbrales de `gvis_config`:
- `sin_compra`: sin compra en los 3 meses.
- `jair_a_jose`: cliente de Jair con los 3 meses < umbral_reasignación.
- `jose_a_mostrador`: cliente de José con los 3 meses < umbral_mostrador.

Al aplicar "Mover"/"Inactivar" se actualiza el cliente y se registra en
`gvis_movimientos_cartera` (bitácora).

## Notas

- Seguridad: control real por PIN vía RPC; RLS abierta en el resto de tablas
  (mismo patrón que los demás gestores). Los PINs no quedan expuestos a la anon key.
- HTML crudo (sin minificar) para edición directa.
