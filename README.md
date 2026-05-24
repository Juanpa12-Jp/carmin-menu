# 🍕 Carmín — Menú Digital

Sitio web estático para la pizzería **Carmín**. Un solo archivo HTML con menú interactivo y pedidos por WhatsApp. Sin backend, sin base de datos, sin costo de hosting.

---

## Stack

| Capa | Tecnología | Costo |
|------|-----------|-------|
| Frontend | HTML + CSS + JS vanilla | Gratis |
| Fonts | Google Fonts (Cormorant Garamond + Raleway) | Gratis |
| Hosting | Vercel | Gratis |
| Dominio | `.vercel.app` incluido | Gratis |
| Pedidos | WhatsApp `wa.me` link | Gratis |

---

## Estructura del proyecto

```
carmin/
└── index.html      ← todo el sitio vive aquí
```

Un solo archivo. No hay build, no hay dependencias, no hay `npm install`.

---

## Configuración inicial (lo único que hay que cambiar)

Abre `index.html` y busca esta línea:

```js
const phone = '50612345678';
```

Reemplázala con el número de WhatsApp real de la pizzería, con código de país y sin espacios ni guiones:

```js
// Costa Rica (+506), número 8888-8888
const phone = '50688888888';
```

---

## Cómo modificar el menú

### Agregar una pizza

Busca la sección donde quieras agregarla (`id="clasicas"` o `id="especiales"`) y copia este bloque:

```html
<div class="menu-item" onclick="addItem('NOMBRE', PRECIO)">
  <div class="item-name">NOMBRE</div>
  <div class="item-desc">Ingrediente 1, ingrediente 2, ingrediente 3</div>
  <div class="item-price">₡PRECIO</div>
</div>
```

Ejemplo real:

```html
<div class="menu-item" onclick="addItem('Cuatro Quesos', 6000)">
  <div class="item-name">Cuatro Quesos</div>
  <div class="item-desc">Mozzarella, parmigiano, gorgonzola y ricotta</div>
  <div class="item-price">₡6,000</div>
</div>
```

> ⚠️ El precio en `addItem('Nombre', PRECIO)` debe ser número entero sin puntos ni comas (ej: `6000`, no `6,000`).

### Eliminar una pizza

Borra el bloque completo desde `<div class="menu-item"` hasta su `</div>` de cierre.

### Cambiar el precio de una pizza

Hay que cambiarlo en **dos lugares** del mismo bloque:

```html
<!-- 1. En el onclick -->
<div class="menu-item" onclick="addItem('Pepperoni', 5500)">
<!--                                               ^^^^  -->

<!-- 2. En el texto visible -->
  <div class="item-price">₡5,500</div>
```

### Agregar una categoría nueva (ej: "Bebidas")

**Paso 1** — Agrega un botón en el `<nav>`:

```html
<button class="nav-tab" onclick="showSection('bebidas', this)">Bebidas</button>
```

**Paso 2** — Agrega la sección con ese mismo `id`:

```html
<section class="menu-section" id="bebidas">
  <p class="section-label">Para acompañar</p>
  <h2 class="section-title">Bebidas</h2>
  <div class="section-rule"></div>

  <div class="menu-item" onclick="addItem('Agua mineral', 1000)">
    <div class="item-name">Agua Mineral</div>
    <div class="item-desc">500ml</div>
    <div class="item-price">₡1,000</div>
  </div>
</section>
```

---

## Cambiar colores

Los colores están definidos como variables CSS al inicio del `<style>`. Búscalos así:

```css
:root {
  --cream: #F2EDE4;   /* fondo principal */
  --dark:  #1C1A17;   /* texto y hero */
  --wine:  #8B1A2F;   /* acento rojo/vino */
  --gold:  #B8935A;   /* acento dorado */
  --mid:   #6B6259;   /* texto secundario */
}
```

Cambia cualquier valor hex para ajustar la paleta completa del sitio.

---

## Cambiar nombre o tipografía del hero

El nombre grande en la pantalla inicial:

```html
<h1 class="hero-title">CAR<span>MÍN</span></h1>
```

La parte dentro de `<span>` se muestra en color vino. Puedes ajustar qué parte va en ese acento.

La tipografía se importa de Google Fonts:

```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400;1,600&family=Raleway:wght@300;400;500;600&display=swap" rel="stylesheet"/>
```

Para cambiar la fuente del título, modifica la propiedad en `.hero-title`:

```css
.hero-title {
  font-family: 'Cormorant Garamond', serif; /* ← cambiar aquí */
}
```

---

## Subir a Vercel (deploy gratuito)

### Opción A — Desde el navegador (sin CLI)

1. Crea cuenta en [vercel.com](https://vercel.com) (gratis, con GitHub o correo)
2. En el dashboard, clic en **"Add New Project"**
3. Elige **"Deploy from template"** → busca "HTML" o simplemente arrastra la carpeta del proyecto
4. Vercel detecta que es un sitio estático y lo publica automáticamente
5. Tu sitio queda en `https://carmin.vercel.app` (o similar)

### Opción B — Desde Claude Code (CLI)

```bash
# Instalar Vercel CLI
npm install -g vercel

# Entrar a la carpeta del proyecto
cd carmin/

# Deploy
vercel

# Seguir las instrucciones del CLI:
# - ¿Set up and deploy? → Y
# - ¿Which scope? → tu cuenta
# - ¿Link to existing project? → N
# - Project name: carmin
# - Directory: ./
# - Override settings? → N
```

El CLI devuelve una URL pública lista para usar.

### Actualizar el sitio después de cambios

```bash
vercel --prod
```

---

## Generar el código QR

Una vez que tengas la URL del sitio (ej: `https://carmin.vercel.app`), genera el QR gratis en:

- [qr-code-generator.com](https://www.qr-code-generator.com)
- [qrcode-monkey.com](https://www.qrcode-monkey.com) (permite personalizar colores)

Descarga en PNG o SVG e imprímelo para las mesas.

---

## Cómo funciona el pedido por WhatsApp

Cuando el cliente toca **"Enviar pedido"**, el sitio construye este mensaje automáticamente:

```
🍕 Pedido — Carmín

• 1x Burrata y Arúgula — ₡10,000
• 2x Pepperoni — ₡10,000

*Total: ₡20,000*

📝 Nota: sin cebolla por favor
```

Y abre WhatsApp con ese mensaje pre-cargado al número configurado. El negocio solo tiene que confirmar.

---

## Próximos pasos opcionales (todos gratuitos)

| Feature | Cómo hacerlo |
|---------|-------------|
| Dominio propio (ej: `carminpizzeria.com`) | Comprar en Namecheap (~$12/año) y apuntarlo a Vercel |
| Sistema de puntos / fidelización | Agregar Firebase Firestore (tier gratuito) |
| Analytics de visitas | Agregar Vercel Analytics (gratuito) o Google Analytics |
| Fotos reales de pizzas | Subir imágenes a Cloudinary (gratuito) y referenciarlas en el HTML |
| Formulario de contacto | Usar [Formspree](https://formspree.io) (gratuito hasta 50 respuestas/mes) |
