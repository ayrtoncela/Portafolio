# 🤖 WhatsApp & Instagram AI Bot Framework

> Agente conversacional multicanal construido con Node.js + OpenAI + Meta API.  
> Arquitectura multi-cliente: un backend base adaptado a cada industria con flujos, precios y lógica propios.

---

## 🚀 Demo en vivo

🎥 **Video demo:** [Ver en YouTube](https://www.youtube.com/watch?v=_C-984BwlBQ)  
🌐 **Frontend:** [web-page-saa-s.vercel.app](https://web-page-saa-s.vercel.app)  
💬 **Chatbot widget:** disponible en la esquina inferior derecha del sitio

---

## 🏗️ Arquitectura base

```
┌──────────────────┐     ┌─────────────────────┐     ┌──────────────────┐
│  Instagram DMs   │────▶│                     │────▶│  OpenAI GPT-4o   │
│  WhatsApp        │     │   Node.js + Express │     │  (system prompt  │
│  Web Chat        │────▶│      Backend        │     │   por cliente)   │
└──────────────────┘     │                     │     └──────────────────┘
                         │  • Webhook handler  │
                         │  • State machine    │────▶┌──────────────────┐
                         │  • Deduplicación    │     │    Supabase      │
                         │  • Sesión + idioma  │     │  (PostgreSQL)    │
                         │  • Human takeover   │     └──────────────────┘
                         │  • Email notif.     │
                         └─────────────────────┘
                                    │
                         ┌──────────┴──────────┐
                         │   Dashboard HTML    │
                         │   (por cliente)     │
                         └─────────────────────┘
```

---

## 📦 Proyectos implementados

### 🎞️ Film Lab — Instagram Bot (CDMX)
**Repo:** [client-foto](https://github.com/ayrtoncela/client-foto)

Laboratorio de fotografía analógica en CDMX. Bot de Instagram que maneja el flujo completo de pedidos de revelado y scan de rollos.

| Canal | Stack | Estado |
|---|---|---|
| Instagram DMs | Node.js + Supabase + Railway + Resend | ✅ Producción |

**Funcionalidades:**
- Máquina de estados completa: cotización → pedido → pago → entrega
- Detección automática de idioma (ES/EN)
- 3 sucursales + recolección a domicilio por colonias de CDMX
- Generación de órdenes (ORD-YYYYMM-XXXX) + notificación por email
- Recepción de comprobantes de pago (imagen) por DM
- Dashboard de operaciones con pipeline visual, validación de pagos y handoff humano
- Soporte para formatos 35mm y 120 con precios diferenciados

---

### 🧪 Laboratorio Clínico — WhatsApp Bot
**Canal:** WhatsApp + Web Chat

Bot de agendamiento de estudios clínicos con flujo estructurado de 5 pasos.

| Canal | Stack | Estado |
|---|---|---|
| WhatsApp + Web | Node.js + Google Sheets + Railway | ✅ Demo live |

**Funcionalidades:**
- Flujo de agendamiento paso a paso (tipo de estudio → sucursal → día → hora → datos)
- 3 sucursales con horarios embebidos
- Instrucciones pre-análisis por tipo de estudio
- Logging automático a Google Sheets + notificación email por lead
- Deduplicación de mensajes (caché en memoria)

---

### 💻 Agencia de Software — Web Chat Bot
**Canal:** Web

Bot de atención a leads para agencia de desarrollo.

**Funcionalidades:**
- Presentación de servicios y casos de uso
- Captura de contacto y calificación de leads
- Mismo backend que WhatsApp, canal web vía `/api/chat`

---

### 🍽️ Restaurante — Web Chat Bot
**Canal:** Web

Bot de reservaciones y atención de comensales.

**Funcionalidades:**
- Consulta de menú y horarios
- Flujo de reservaciones
- Manejo de pedidos

---

## ✨ Features del framework

**Conversación**
- Máquina de estados por cliente (flujos estructurados sin salirse del orden)
- Memoria de sesión con timeout configurable
- Detección de idioma automática (ES/EN)
- Palabras clave globales para reset de menú (`menú`, `inicio`, `start`)
- Fallback a OpenAI para mensajes fuera de flujo

**Operaciones**
- Dashboard HTML por cliente con autenticación
- Human takeover — pausa el bot y permite respuesta manual del asesor
- Pipeline visual de estado del pedido
- Notificaciones push al cliente por Instagram/WhatsApp
- Soporte para imágenes entrantes (comprobantes, fotos)

**Infraestructura**
- Deduplicación de mensajes via Supabase (`processed_messages`)
- Session timeout configurable
- Email transaccional via Resend
- Deploy en Railway con variables de entorno

---

## 🛠️ Stack completo

| Capa | Tecnología |
|---|---|
| Backend | Node.js 18 + Express |
| IA | OpenAI GPT-4o-mini |
| Mensajería | Meta Graph API (Instagram + WhatsApp) |
| Base de datos | Supabase (PostgreSQL) |
| Email | Resend |
| Deploy | Railway |
| Frontend dashboard | HTML/CSS/JS vanilla |
| Frontend web | Vercel |

---

## 👨‍💻 Autor

**Ayrton Cela** — Consulting Engineering Manager & AI Builder  
Ciudad de México 🇲🇽

> *Construido con ayuda de Claude*

