# Encuesta de Satisfacción del Cliente · Transhicol

Proyecto **independiente** de `transhicol-manager`. Es un solo `index.html` que los
clientes diligencian desde un **link externo** y que guarda los resultados en la
**misma base de datos Supabase** que usa la app (proyecto `fogwynnmbnipsvuxacxw`).

- La encuesta solo puede **INSERTAR** encuestas (rol `anon` + RLS). No puede leer ni
  editar datos de nadie.
- `transhicol-manager` (módulo **Comercial**) solo **LEE** estos resultados.

## 1. Requisito previo (una sola vez)

En Supabase → **SQL Editor**, ejecutar la migración:

```
transhicol-manager/supabase/migrations/0013_encuestas_satisfaccion.sql
```

Crea la tabla `encuestas_satisfaccion` con sus políticas RLS.

## 2. Publicar la encuesta (elige una)

Es un sitio estático de un solo archivo. Opciones:

- **Vercel**: arrastra esta carpeta en https://vercel.com/new, o `vercel --prod`.
- **Netlify**: arrastra la carpeta en https://app.netlify.com/drop.
- **Cloudflare Pages / GitHub Pages**: sube la carpeta como sitio estático.

No requiere build ni variables de entorno: la URL y la *anon key* (pública por diseño)
ya están dentro del `index.html`.

## 3. Conectar el link en la app

Cuando tengas la URL pública (ej. `https://encuesta-transhicol.vercel.app`), pégala en:

```
transhicol-manager/src/views/ComercialView.jsx  →  const ENCUESTA_URL = '';
```

Así, en el módulo Comercial aparecerán los botones **Copiar link** y **Abrir** la encuesta.

## Notas

- La *anon key* es pública por naturaleza (se usa en el navegador). La seguridad real
  la dan las políticas RLS de la migración 0013.
- Si en el futuro quieres **notificación por correo** al recibir una encuesta, hazlo
  del lado servidor con un *Database Webhook* o *Edge Function* de Supabase (no desde
  el navegador del cliente).
