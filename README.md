# ✦ ASTRA — Guía de Arquitectura y Monetización

## Archivos incluidos en este paquete

| Archivo | Descripción |
|---|---|
| `index.html` | App mística completa (frontend PWA) |
| `admin.html` | Panel de administración (protegido por login) |
| `manifest.json` | Config PWA (instalable como app nativa) |
| `sw.js` | Service Worker (modo offline + caché) |

---

## Stack recomendado para producción

### Frontend
- **React + Vite** (migrar desde este HTML para escalar)
- Deploy en **Vercel** (gratis, CDN global, HTTPS automático)
- O en **Hostinger** subiendo la carpeta `/dist` después del build

### Backend / Base de datos
- **Supabase** (recomendado):
  - Auth para el panel admin
  - PostgreSQL para artículos y usuarios
  - Edge Functions para llamar a la API de IA
  - Storage para imágenes
  - GRATIS hasta 50MB / 50K requests/mes

### IA
- **Google Gemini 1.5 Flash** (más económico para escalar):
  - Gratis hasta 15 RPM / 1M tokens/día
  - Costo a escala: ~$0.075 por 1M tokens input
  - API key en: https://aistudio.google.com/apikey
- **OpenAI GPT-4o-mini** (alternativa):
  - $0.15 por 1M tokens input

---

## Plan de monetización por capas

### Capa 1 — Google AdSense (desde el día 1)
```html
<!-- Reemplazá los placeholders en index.html con: -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-XXXXXXXX" crossorigin="anonymous"></script>
<ins class="adsbygoogle" data-ad-client="ca-pub-XXXXXXXX" data-ad-slot="XXXXXXXXXX"></ins>
```
**Revenue esperado**: $2–$8 RPM con audiencia de terror/misterio

### Capa 2 — Suscripción Premium ($4.99/mes)
Integrar con **Stripe** o **MercadoPago**:
```javascript
// Stripe Checkout (1 línea de código)
const session = await stripe.checkout.sessions.create({
  line_items: [{ price: 'price_XXXXXXXX', quantity: 1 }],
  mode: 'subscription',
  success_url: 'https://tuapp.com/premium',
  cancel_url: 'https://tuapp.com/',
});
```
**Revenue esperado**: 1% conversión = 23 premium con 2,300 usuarios activos = $115/mes

### Capa 3 — Afiliados Amazon
Reemplazar los `href="#"` en la sección de tienda con links de afiliado:
- https://www.amazon.com/dp/XXXXXXXX?tag=TU-TAG-20
**Commission**: 3–8% por venta

### Capa 4 — Cursos / Ebooks
Integrar con **Gumroad** o **Hotmart** para vender:
- Curso "Leé el Tarot en 21 días" — $29
- Ebook "Numerología Práctica" — $9.99

---

## Variables de entorno (.env) para producción

```
VITE_GEMINI_API_KEY=tu_key_aqui
VITE_SUPABASE_URL=https://xxx.supabase.co
VITE_SUPABASE_ANON_KEY=tu_key_aqui
VITE_STRIPE_PUBLIC_KEY=pk_live_xxx
VITE_ADSENSE_ID=ca-pub-XXXXXXXX
```

---

## Configuración de IA (ejemplo con Gemini)

```javascript
const response = await fetch(
  `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`,
  {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      contents: [{
        parts: [{
          text: `Eres un oráculo místico que habla en español argentino...
          Usuario: ${nombre}, Número de vida: ${lifeNumber}
          Cartas elegidas: ${cards.map(c => c.name).join(', ')}
          
          Generá una lectura de 150 palabras, mística y personal.`
        }]
      }]
    })
  }
);
```

---

## SEO — Estrategia para terror/misterio

### Keywords de alto tráfico para tu nicho:
- "lectura de tarot gratis" (90K/mes)
- "horóscopo semanal" (40K/mes)
- "numerología nombre" (22K/mes)
- "casas embrujadas argentina" (8K/mes)
- "qué me dicen las cartas" (6K/mes)

### Sitemap automático (agregar a Supabase Edge Function):
```javascript
// Genera /sitemap.xml dinámicamente con cada artículo nuevo
const articles = await supabase.from('articles').select('slug, updated_at').eq('status', 'published');
const urls = articles.map(a => `
  <url>
    <loc>https://astra.com/blog/${a.slug}</loc>
    <lastmod>${a.updated_at}</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>`).join('');
```

---

## Credenciales del Panel Admin (demo)
- **Usuario**: admin
- **Contraseña**: astra2025
- ⚠️ Cambiar antes de publicar usando Supabase Auth

---

## Próximos pasos (roadmap)

1. ✅ Frontend PWA (este archivo)
2. ✅ Panel Admin con editor SEO
3. 🔲 Conectar Supabase (artículos reales en DB)
4. 🔲 Integrar API Gemini/OpenAI para lecturas reales
5. 🔲 Integrar Stripe/MercadoPago
6. 🔲 Configurar Google AdSense
7. 🔲 Publicar en Vercel/Hostinger
8. 🔲 Conectar dominio propio
9. 🔲 Registrar en Google Search Console
10. 🔲 Campaña de tráfico desde Facebook (230K seguidores 🚀)
