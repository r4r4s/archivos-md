---
name: landing-cliente-local
description: Genera una landing completa profesional para un negocio local hispano (panadería, restaurante, gimnasio, despacho, peluquería, etc.). Incluye hero, secciones de productos/servicios, formulario de contacto que envía pedidos al WhatsApp del cliente, mapa de Google Maps, SEO local optimizado y stack moderno (Next.js + Tailwind). Activar cuando el usuario pida una landing para un cliente real con datos concretos.
---

# Skill: landing-cliente-local

## Cuándo activarse

Esta skill se activa automáticamente cuando el usuario:

- Pide montar una landing/web para un negocio local
- Menciona un negocio físico hispano (panadería, restaurante, peluquería, despacho de abogados, clínica dental, gimnasio, taller mecánico, academia, etc.)
- Da datos concretos del cliente (nombre, ciudad, especialidad, WhatsApp)

Frases típicas que la disparan:
- "Hazme una landing para [tipo de negocio]"
- "Necesito una web para mi cliente la panadería X"
- "Monta una landing para una peluquería en Barcelona"

## Inputs que la skill pedirá si faltan

Antes de empezar a generar, asegúrate de tener estos datos. Si alguno falta, **pregúntalo al usuario** ANTES de programar:

| Input | Ejemplo | Obligatorio |
|---|---|---|
| `nombre_negocio` | "Panadería La Rosa" | ✅ |
| `tipo_negocio` | "panadería ecológica" | ✅ |
| `ciudad` | "Sevilla" | ✅ |
| `whatsapp_negocio` | "+34 600 000 000" | ✅ |
| `direccion_completa` | "Calle Sierpes 12, 41004 Sevilla" | ✅ |
| `horarios` | "Lunes a sábado 7:00-14:00" | ✅ |
| `productos_servicios` | "pan ecológico, bollería artesana, cafetería" | ✅ |
| `colores_marca` | "tonos cálidos, tierra, blanco roto" | ⚠️ Si falta, deduce por el sector |
| `instagram` | "@panaderialarosa" | ⚠️ Opcional |

**Pregunta de uno en uno los que falten — no lances un cuestionario gigante.**

## Stack que vas a usar

```
- Framework: Next.js 15 (App Router)
- Estilos: Tailwind CSS 4
- Componentes UI: shadcn/ui
- Iconos: lucide-react
- Mapas: Google Maps embed (iframe sin API key, suficiente para esto)
- Formulario WhatsApp: link wa.me con plantilla pre-rellenada
- SEO: schema.org JSON-LD para LocalBusiness
- Deploy: pensado para Vercel (también vale Netlify, Hostinger)
```

## Estructura de archivos a generar

```
[nombre-cliente-slug]/
├── package.json
├── next.config.js
├── tailwind.config.ts
├── app/
│   ├── layout.tsx        ← metadata, fonts, schema.org
│   ├── page.tsx          ← landing principal
│   ├── globals.css       ← estilos base
│   └── components/
│       ├── Hero.tsx
│       ├── Productos.tsx
│       ├── Horarios.tsx
│       ├── Mapa.tsx
│       ├── Contacto.tsx     ← incluye botón WhatsApp con mensaje pre-rellenado
│       └── Footer.tsx
├── public/
│   ├── og-image.jpg      ← imagen para compartir en redes (placeholder por ahora)
│   └── favicon.ico       ← placeholder
├── README.md             ← cómo arrancar y deployar
└── .env.example          ← solo si hace falta API key (Google Maps API si decide upgrade)
```

## Secciones obligatorias de la landing

1. **Hero**
   - Nombre del negocio en grande con tipografía premium
   - Tagline corto (1 línea) que conecte con el dolor del cliente final
   - Imagen/foto del negocio (placeholder con instrucción para sustituir)
   - 2 CTAs: "Pídelo ahora por WhatsApp" (primario) y "Cómo llegar" (secundario, salta a la sección de mapa)

2. **Productos/Servicios** (grid 3 columnas en desktop, 1 en mobile)
   - Cards con foto, nombre, precio (si aplica), descripción corta
   - Si el sector es restauración/comercio: incluye precios visibles
   - Si es servicios: incluye CTA "Solicita más info" → WhatsApp con producto pre-rellenado

3. **Sobre nosotros** (sección breve, 2 columnas: foto + texto)
   - Historia o misión del negocio en 3-4 líneas
   - Highlight de lo que diferencia (años de experiencia, especialidad, certificaciones)

4. **Horarios + Ubicación** (2 columnas)
   - Lista de horarios con tipografía clara
   - Iframe de Google Maps centrado en la dirección
   - Botón "Cómo llegar" que abre Google Maps con la ruta

5. **Contacto** (sección destacada con fondo distinto)
   - Botón WhatsApp grande con icono y mensaje pre-rellenado:
     `https://wa.me/[NUMERO]?text=Hola%20${nombre_negocio}%2C%20me%20gustaría...`
   - Email (opcional, si lo da)
   - Redes sociales (Instagram principal)

6. **Footer**
   - Repetición de horarios
   - Dirección completa
   - Schema.org JSON-LD para LocalBusiness aquí (NO visible)
   - Copyright + "Web hecha por Divisual" (opcional, deja link al final)

## SEO local — checklist obligatorio

Cada landing generada DEBE incluir:

- [ ] `<title>` con formato: "{nombre_negocio} — {tipo_negocio} en {ciudad}"
- [ ] `<meta name="description">` con CTA y keywords locales (~150 caracteres)
- [ ] Schema.org JSON-LD `LocalBusiness` (subtipo correcto: `Bakery`, `Restaurant`, `HairSalon`, `LegalService`, etc.)
- [ ] `lang="es"` en `<html>`
- [ ] Open Graph tags (og:title, og:description, og:image)
- [ ] `robots.txt` permitiendo indexación
- [ ] `sitemap.xml` con la home
- [ ] Imágenes con `alt` text descriptivo en español

## Cómo enrutar las tareas a Claude vs DeepSeek

Esta skill funciona mejor cuando:

- **Claude (V4-Pro vía proxy)** se encarga de:
  - Componente Hero (estética premium)
  - Paleta de colores y tipografía
  - Microcopy de tagline y CTAs
  - Decisiones de diseño y composición

- **DeepSeek-Pro** se encarga de:
  - Lógica del formulario WhatsApp (URL encoding, mensajes pre-rellenados)
  - Schema.org JSON-LD
  - SEO técnico (meta tags, OG, sitemap)
  - Configuración de Next.js, Tailwind
  - README de deployment

- **DeepSeek-Flash** se encarga de:
  - Rellenar contenido demo (productos, descripciones placeholder)
  - Componentes repetitivos (cards, footers)
  - Convertir tipos / interfaces TypeScript

## Plan de trabajo paso a paso

Cuando se active la skill, ejecuta este flow:

```
1. Recopilar inputs faltantes (preguntar al usuario uno a uno)
2. Crear estructura de carpetas y package.json
3. Generar layout.tsx con metadata + schema.org
4. Generar Hero (pedir a Claude/Pro por estética)
5. Generar Productos/Servicios (Flash para repetitivo)
6. Generar sección Sobre nosotros
7. Generar Horarios + Mapa
8. Generar Contacto con botón WhatsApp
9. Generar Footer + schema.org final
10. Generar README.md con instrucciones de arranque
11. Mostrar resumen al usuario:
    - Cómo arrancar en local: pnpm install && pnpm dev
    - Cómo deployar en Vercel: 3 pasos
    - Qué placeholders sustituir antes de entregar al cliente
```

## Avisos finales que la skill DEBE dar al terminar

Cuando termines de generar todo, muestra al usuario:

1. **Imágenes**: "Las imágenes son placeholder. Sustituye `public/hero-bg.jpg` y las fotos de productos con las del cliente real antes de entregar."
2. **Coste real de tokens**: Reporta los tokens consumidos en esta generación con DeepSeek y el ahorro vs Claude Opus.
3. **Precio sugerido al cliente**: 800-1.500€ una vez (web simple). Mantenimiento 30-50€/mes (opcional).
4. **Próximos pasos**: arrancar en local, hacer ajustes finales, deployar en Vercel, configurar dominio del cliente.

## Ejemplo de invocación completa

```
Usuario: "Usa la skill landing-cliente-local. Cliente: Panadería La Rosa.
Sevilla, panadería ecológica. WhatsApp +34 600 000 000.
Calle Sierpes 12. Lunes a sábado 7:00-14:00.
Vendemos pan ecológico, bollería y café para llevar."

Skill:
[reconoce inputs, completa lo que falte preguntando]
[genera estructura completa]
[reporta resumen final con coste tokens y siguiente paso]
```

---

**Skill creada por Juanpe Divisual · Distribuida gratis con la sesión del domingo de Divisual Project**
