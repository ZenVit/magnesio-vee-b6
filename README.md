# Magnesio + Vitamina B6 VEE

Landing estática hecha con la misma plantilla que ZenVit/articumina: checkout en ventana, departamentos y ciudades de Paraguay, mapa opcional, combos, descuento "antes de irte", pago al recibir y botón de WhatsApp.

## Precios
- 1 frasco: Gs. 195.000
- 2 frascos: Gs. 295.000 (ahorro Gs. 95.000)
- 3 frascos: Gs. 395.000 (ahorro Gs. 190.000)
- Descuento "antes de irte": Gs. 10.000 (1 frasco) y Gs. 20.000 (combos), igual que Articumina.

## Pedidos
El formulario envía el pedido a la edge function `create-magnesio-vee-order` (Supabase PROD). La función guarda en `pedidos_web` (producto `MAGNESIO VEE`, origen `landing_magnesio_vee`) con los precios fijados en el servidor. El INSERT avisa por Telegram. Después, el cliente confirma por WhatsApp con el número de pedido. Si la función falla, se ofrece mandar el pedido por WhatsApp.

Archivos del servidor en `supabase/`:
- `functions/create-magnesio-vee-order/index.ts`: copia de `create-articumina-order` con producto, origen y precios de Magnesio VEE. Se despliega con `verify_jwt = false`.
- `migrations/20261112180000_landing_magnesio_vee_origin.sql`: agrega `landing_magnesio_vee` a `private.validate_order_creation_mode` (sin esto el panel rechaza el pedido) y a `private.queue_new_order_meta_purchase` (Purchase por CAPI).
- En `meta-purchase-worker` se sumó `landing_magnesio_vee: { id: 'MAGNESIO-VEE-90', name: 'Magnesio + B6 VEE - 90 cápsulas', sourceUrl: 'https://zenvit.github.io/magnesio-vee-b6/' }` al mapa `PRODUCTS`.

Estado (12/11/2026): las tres cosas están aplicadas en PROD (función v1, migración y worker v28). Publicada en https://zenvit.github.io/magnesio-vee-b6/

## Meta y visitas
Pixel VENTA CAPI: PageView, ViewContent, InitiateCheckout, Contact y Purchase con eventID `<pedido>:purchase`. `zv-visitas.js` alimenta la Vista en tiempo real del panel (`data-landing="landing_magnesio_vee"`).

## Contenido
Los datos salen de la etiqueta (ver `Anuncios/Magnesio VEE/analisis.md`): 90 cápsulas, porción de 3 cápsulas con 350 mg de magnesio y 1,7 mg de vitamina B6, bisglicinato de magnesio, mayores de 19 años. Las únicas funciones mencionadas son las tres del envase (neuromuscular, huesos y dientes, división celular). Sin reseñas, contadores ni "personas viendo" inventados. Falta cotejar las propiedades con la inscripción ante DINAVISA.

## Imágenes
`img/` tiene las 8 piezas de `Anuncios/Magnesio VEE` en WebP (1080 y 560 px) y las fotos reales del frasco (frente, tabla y advertencias) en 1080×1350. `combo-1/2/3.webp` se armaron con la foto real del frasco. Todo suma unos 1,4 MB.

## Desarrollo y publicación
Sin dependencias: `python -m http.server 4173` en esta carpeta. Todo lo del producto está en `CONFIG` dentro de `index.html`. Pensada para GitHub Pages (repo `ZenVit/magnesio-vee-b6`, rama `main`, raíz `/`).
