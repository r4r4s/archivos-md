---
name: propuesta-cliente
description: Genera una propuesta comercial profesional completa para un cliente nuevo. Output: PDF con desglose de servicios, precios, timeline y términos + email listo para enviar + recordatorio de follow-up. Activar cuando el usuario quiera mandar una propuesta a un cliente prospecto y le pase los datos básicos del proyecto.
---

# Skill: propuesta-cliente

## Cuándo activarse

Esta skill se activa automáticamente cuando el usuario:

- Pide preparar una propuesta para un cliente
- Va a cerrar un proyecto y necesita formalizar precios
- Quiere generar el "paquete de venta" para un prospecto
- Frases típicas:
  - "Hazme una propuesta para el cliente X"
  - "Necesito mandar una cotización por la web de la panadería"
  - "Prepara el documento comercial para [cliente]"

## Inputs requeridos

Antes de generar nada, asegúrate de tener estos datos. Pregúntalos uno a uno:

| Input | Ejemplo | Obligatorio |
|---|---|---|
| `cliente_nombre` | "Panadería La Rosa" | ✅ |
| `cliente_contacto` | "María Fernández" | ✅ |
| `cliente_email` | "info@panaderialarosa.es" | ✅ |
| `proyecto_nombre` | "Landing + Bot WhatsApp" | ✅ |
| `proyecto_descripcion` | "Web profesional con pedidos automáticos por WhatsApp" | ✅ |
| `precio_total` | 1500 (en euros) | ✅ |
| `timeline_dias` | 14 (días laborables) | ✅ |
| `entregables` | Lista de qué incluye exactamente | ✅ |
| `forma_pago` | "50% al inicio, 50% a la entrega" | ⚠️ Si falta, asume 50/50 |
| `incluye_mantenimiento` | true/false + precio mensual | ⚠️ Opcional |

## Output que genera la skill

Cuando termines, debes haber producido **3 archivos** en una carpeta nueva `propuestas/[cliente-slug]/`:

```
propuestas/
└── panaderia-la-rosa/
    ├── propuesta-comercial.pdf      ← documento principal para enviar al cliente
    ├── email-envio.md               ← asunto + cuerpo del email listos para Gmail
    └── followup-calendar.md         ← recordatorio para crear evento en Google Calendar
```

## Estructura del PDF de propuesta

Genera un PDF profesional con esta estructura (usa `reportlab` o `weasyprint` en Python — tu elección según lo que tenga el usuario instalado):

### Página 1 — Portada

- Logo de Divisual Project (o de la agencia del usuario, si lo configura)
- Título grande: "Propuesta Comercial"
- Nombre del cliente
- Fecha (hoy)
- Validez de la propuesta (15 días desde hoy)
- Footer pequeño: nombre y datos de la agencia

### Página 2 — Contexto y entendimiento

Sección "Sobre tu proyecto":
- Resumen de lo que el cliente ha pedido (1 párrafo)
- Por qué importa esto para su negocio (2-3 líneas)
- Qué problema resuelve concretamente

Tono: empático, no técnico. La audiencia del PDF es el cliente final, no un programador.

### Página 3 — Solución propuesta

Sección "Lo que vamos a construir":
- Lista bullet con todos los entregables concretos
- Para cada entregable: una línea de descripción + el valor que aporta
- Si aplica, mockup/diagrama simple

Mantén un lenguaje simple. Cero jerga técnica innecesaria. El cliente NO sabe qué es Next.js — di "web rápida y moderna" en vez de "web con framework Next.js".

### Página 4 — Inversión y timeline

**Tabla de servicios y precios** (clara y simple):

| Servicio | Precio |
|---|---|
| Diseño y desarrollo web | X€ |
| Bot WhatsApp con pedidos | X€ |
| SEO local + indexación Google | X€ |
| **Total proyecto** | **X€** |

Si hay mantenimiento mensual, ponlo APARTE en otra tabla:

| Servicio mensual | Precio |
|---|---|
| Hosting y mantenimiento | X€/mes |
| Soporte y cambios menores | incluido |

**Forma de pago**:
- 50% al firmar el contrato
- 50% a la entrega final

**Timeline**: timeline en formato visual horizontal con 3-4 hitos:
1. Kickoff y diseño (días 1-3)
2. Desarrollo (días 4-10)
3. Revisión cliente (días 11-12)
4. Entrega y deploy (días 13-14)

### Página 5 — Términos y siguientes pasos

- Validez de la propuesta: 15 días
- Términos básicos: propiedad intelectual del código pasa al cliente al pago final, garantía de 30 días post-entrega, qué cambios entran en el alcance
- Siguientes pasos: "Para empezar, responde a este email confirmando y te mando contrato + factura del 50% inicial"

## Estructura del email-envio.md

```markdown
**Para:** [cliente_email]
**Asunto:** Tu propuesta para [proyecto_nombre] — Divisual Project

Hola [cliente_contacto],

[Saludo personalizado en 1 línea, mencionando algo concreto de la conversación previa]

Te paso adjunta la propuesta para [proyecto_nombre]. He intentado que sea
clara y directa — sin tecnicismos innecesarios.

**Resumen rápido:**
- Total proyecto: **[precio_total]€**
- Timeline: **[timeline_dias] días laborables**
- Entrega: **[fecha estimada]**

Si todo te encaja, respóndeme a este email confirmando y te mando contrato
+ factura del 50% inicial. Si algo no te cuadra o quieres modificar el
alcance, también respóndeme y lo ajustamos juntos.

La propuesta tiene validez de **15 días** desde hoy.

Cualquier duda, aquí me tienes.

Un abrazo,
Juanpe Navarro
CEO · Divisual Project
[firma con datos de contacto]
```

## Estructura del followup-calendar.md

Instrucciones para crear evento en Google Calendar (puede automatizarse con MCP de Calendar si está disponible):

```markdown
**Crear evento en Google Calendar:**

- **Título**: Follow-up [cliente_nombre] — propuesta [proyecto_nombre]
- **Fecha**: [hoy + 3 días laborables]
- **Hora**: 10:00 (recordatorio)
- **Duración**: 15 min
- **Descripción**:
  Hacer seguimiento si el cliente no ha respondido al email del [fecha hoy].
  Plantilla follow-up:
  - Hola [cliente_contacto], te escribo para asegurarme de que la propuesta
    te llegó bien y ver si tienes alguna duda. Quedo atento.
- **Recordatorio**: 1 día antes
```

## Cómo enrutar las tareas a Claude vs DeepSeek

Esta skill **NO necesita Claude**. Es 100% backend (generación de archivos, lógica de plantillas).

- **DeepSeek-Pro**: redacción de los textos de la propuesta (necesita matiz para el cliente final)
- **DeepSeek-Flash**: estructura del PDF, generación de los archivos auxiliares, código de generación

Esto es un caso ideal para ahorro máximo de coste — no hay diseño visual real, todo es texto y plantilla.

## Plan de trabajo paso a paso

```
1. Recopilar inputs faltantes (preguntar uno a uno)
2. Crear carpeta propuestas/[cliente-slug]/
3. Generar contenido del PDF con DeepSeek-Pro:
   - Portada
   - Contexto y entendimiento
   - Solución propuesta
   - Inversión y timeline
   - Términos
4. Compilar el PDF (con reportlab o weasyprint)
5. Generar email-envio.md
6. Generar followup-calendar.md
7. Resumir al usuario:
   - Ruta de los 3 archivos
   - Cómo enviar el email (copy-paste a Gmail o usar MCP)
   - Coste tokens consumidos
```

## Avisos finales que la skill DEBE dar al terminar

1. **Revisión humana obligatoria**: "Antes de enviar el PDF al cliente, revísalo. La IA puede haber redactado algo que no encaja con tu tono o con detalles del proyecto que no le pasaste."
2. **Términos legales**: "La sección de términos es genérica. Si el proyecto es grande (>5K€), reemplaza por un contrato real revisado por abogado."
3. **Coste real**: Reporta tokens consumidos y el ahorro vs Claude Opus. Usar este caso como ejemplo de cuándo NO necesitas Claude.
4. **Plantilla reutilizable**: "Esta propuesta queda como plantilla en la carpeta `propuestas/`. Para el siguiente cliente, simplemente cambia los inputs — la estructura ya está afinada."

## Ejemplo de invocación

```
Usuario: "Usa la skill propuesta-cliente. Cliente: Panadería La Rosa,
contacto María Fernández (info@panaderialarosa.es). Proyecto: landing
+ bot WhatsApp con pedidos. Precio total: 1500€. Timeline: 14 días."

Skill:
[pregunta entregables exactos: cuántas páginas, integraciones, mantenimiento]
[genera los 3 archivos]
[reporta resumen final]
```

---

**Skill creada por Juanpe Divisual · Distribuida gratis con la sesión del domingo de Divisual Project**
