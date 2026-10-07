# Enro's Vet App

Frontend público de Enro's Vet construido con React y Vite. La web consume productos de Kafka Core desde Supabase en modo solo lectura.

## Variables de entorno

Crear un archivo `.env` local basado en `.env.example`:

```bash
VITE_SUPABASE_URL=
VITE_SUPABASE_PUBLISHABLE_KEY=
```

No usar `service_role`, contraseña de base de datos ni secretos privados en este frontend. La app solo debe utilizar la publishable key de Supabase.

## Ejecutar el proyecto

```bash
npm install
npm run dev
```

Para validar producción:

```bash
npm run lint
npm run build
```

## Tienda actual

El catálogo usa temporalmente:

```js
export const CURRENT_STORE_ID = 1
```

La configuración vive en `src/config/store.js`. Más adelante puede reemplazarse por resolución por slug, dominio o configuración remota.

## Lectura de productos

La app reutiliza un cliente centralizado en `src/lib/supabase.js` y consulta productos desde `src/services/products.js`.

La función `getPublicProducts()` lee la tabla `products` filtrando:

- `store_id = 1`
- `active = true`

Los productos se ordenan por `created_at desc`. Los productos con `available = false` se muestran como “Agotado”, sin ocultarse.

## Imágenes

Las imágenes se leen desde Supabase Storage en el bucket público:

```text
product-images
```

El helper `src/services/productImages.js` resuelve cada `image_path` con:

```js
supabase.storage.from('product-images').getPublicUrl(imagePath)
```

El frontend solo lee imágenes. No sube, edita ni elimina archivos.

## Seguridad y RLS

Este proyecto actúa como usuario `anon`. Supabase debe permitir lectura pública de productos visibles, por ejemplo productos con `active = true`, y lectura pública del bucket `product-images`.

Este repositorio no implementa:

- login
- roles
- CRUD
- dashboard
- subida de imágenes
- pagos
- carrito
- órdenes
- backend propio
