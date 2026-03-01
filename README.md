# 🤖 WhatsApp AI Bot — Multi-Industry Framework

> Agente conversacional multicanal construido con Node.js + OpenAI GPT-3.5 + Meta Cloud API.  
> Un solo backend soporta múltiples industrias, canales y flujos de negocio — con persistencia en PostgreSQL, logging automático y notificaciones en tiempo real.

---

## 🚀 Demo en vivo

🎥 **Video demo:** [Ver en YouTube](https://www.youtube.com/watch?v=_C-984BwlBQ)  
🌐 **Portfolio:** [ayrtoncela.vercel.app](https://ayrtoncela.vercel.app)  
💬 **Chatbot widget:** disponible en la esquina inferior derecha del sitio

---

## 🏗️ Arquitectura

```
┌──────────────┐     ┌─────────────────────┐     ┌──────────────────┐
│  WhatsApp    │────▶│                     │────▶│  OpenAI GPT-3.5  │
│  (Meta API)  │     │   Node.js + Express │     │  (system prompt  │
└──────────────┘     │   Railway (24/7)    │     │   por vertical)  │
                     │                     │     └──────────────────┘
┌──────────────┐     │  • Webhook handler  │
│  Instagram   │────▶│  • Deduplicación    │────▶┌──────────────────┐
│  DMs (Meta)  │     │  • Sesión + timeout │     │    Supabase      │
└──────────────┘     │  • Rate limiting    │     │  (PostgreSQL)    │
                     │  • Multi-botType    │     └──────────────────┘
┌──────────────┐     │  • Admin dashboard  │              │
│  Web Chat    │────▶│                     │              ▼
│  (Frontend)  │     └─────────────────────┘   ┌──────────────────┐
└──────────────┘                │               │  Google Sheets   │
                                └──────────────▶│  (log humano +   │
                                                │   notificaciones)│
                                                └──────────────────┘
```

---

## 🏢 Verticales implementados

| Industria | Canal | Funcionalidades |
| --- | --- | --- |
| 🧪 **Laboratorio Clínico** | WhatsApp + Web | Agendamiento 5 pasos, 3 sucursales, instrucciones pre-análisis |
| 💻 **Agencia de Software** | WhatsApp + Instagram + Web | Atención a leads, servicios, captura de contacto |
| 🍽️ **Restaurante** | Web | Reservaciones, menú, horarios, pedidos |

Un solo backend — el vertical se selecciona vía `botType` en el request.

---

## ✨ Features del sistema

### 🤖 Conversación

* Memoria de sesión persistente en Supabase (últimos 10 mensajes)
* Timeout de sesión automático — historial se borra tras 30 min de inactividad
* Flujos estructurados paso a paso (agendamiento sin salirse del orden)
* Detección de fin de conversación (goodbye words) → reset automático
* Reglas de negocio embebidas: horarios, sucursales, restricciones, edge cases

### 🗄️ Base de datos (Supabase / PostgreSQL)

* `leads` — captura automática de cada nuevo contacto con metadata
* `conversations` — historial completo de mensajes por usuario
* `processed_messages` — deduplicación persistente (sobrevive reinicios)
* `rate_limits` — control de uso por usuario y global
* Cleanup automático de mensajes procesados viejos vía función SQL

### 📊 Admin Dashboard

* Accesible en `/dashboard` con autenticación básica por password
* Métricas en tiempo real: total leads, leads hoy, mensajes hoy, usuarios activos
* Tabla de leads con teléfono, canal, primer mensaje, estado y última actividad
* Historial de conversaciones expandible por usuario inline
* Visualización de rate limits por usuario con barra de progreso
* Auto-refresh cada 30 segundos

### 📊 Logging

* **Google Sheets** como vista humana — cada conversación guardada vía Apps Script
* Campos registrados: `Timestamp · Teléfono · Primer Mensaje · Fuente · Estado · Conversación`
* Estado del lead actualizado automáticamente

### 📧 Notificaciones

* Email automático al capturar cada nuevo lead
* Notificación instantánea con datos del contacto

### 🛡️ Confiabilidad & Seguridad

* Deduplicación de mensajes persistente en DB (evita respuestas duplicadas)
* Filtro de mensajes stale — ignora reintentos de Meta (>30s de antigüedad)
* Rate limiting: 20 msgs/hr por usuario · 500 msgs/hr global
* Timeout de sesión: 30 min de inactividad limpia el historial silenciosamente
* Dashboard protegido con autenticación HTTP Basic
* Manejo de errores con respuesta de fallback al usuario

### 🌐 Multicanal

* **WhatsApp** vía Meta Cloud API — número real de WhatsApp Business (`+52 993 234 0850`)
* **Instagram DMs** vía Meta Graph API — webhook integrado, pendiente App Review
* **Web chat** vía endpoint `/api/chat` — mismo backend, mismo AI

---

## 🛠️ Stack

| Capa | Tecnología |
| --- | --- |
| Backend | Node.js + Express |
| IA | OpenAI GPT-3.5-turbo |
| Mensajería | Meta WhatsApp Cloud API v22.0 + Instagram Graph API |
| Base de datos | Supabase (PostgreSQL) |
| Hosting | Railway (24/7, auto-deploy desde GitHub) |
| Logging humano | Google Sheets + Google Apps Script |
| Frontend | HTML/CSS/JS — desplegado en Vercel |
| Control de versiones | GitHub |

---

## 📋 Flujo del agente — Laboratorio clínico

```
Usuario inicia conversación
        │
        ▼
Lead capturado → Supabase + Google Sheets + Email notification
        │
        ▼
1. ¿Qué tipo de estudio necesitas?
        ↓
2. ¿Cuál sucursal? (Centro / Naucalpan / Roma)
        ↓
3. ¿Qué día prefieres? (Lun-Vie 7AM-2PM · Sáb 7AM-12PM)
        ↓
4. ¿A qué hora?
        ↓
5. Nombre completo + teléfono
        │
        ▼
Conversación guardada en Supabase + Google Sheets
        │
        ├── Usuario dice "gracias/adiós" → Estado: "Conversación finalizada"
        └── 30 min sin actividad → historial limpiado automáticamente
```

---

## 📁 Estructura del proyecto

```
whatsapp-bot/
├── server.js          # Backend principal (Express + webhook + AI + dashboard API)
├── dashboard.html     # Admin dashboard UI
├── .env               # Variables de entorno (tokens, keys)
├── package.json
└── README.md
```

---

## ✅ Roadmap

- [x] ~~Bot funcional con WhatsApp + OpenAI~~ ✅
- [x] ~~Soporte multicanal (WhatsApp + Web chat)~~ ✅
- [x] ~~Múltiples verticales (lab, restaurante, agencia)~~ ✅
- [x] ~~Video demo grabado y publicado~~ ✅
- [x] ~~Migrar persistencia de sesión a Supabase (PostgreSQL)~~ ✅
- [x] ~~Deploy del backend en Railway (24/7, auto-deploy)~~ ✅
- [x] ~~Deduplicación de mensajes persistente en DB~~ ✅
- [x] ~~Rate limiting por usuario y global~~ ✅
- [x] ~~Timeout de sesión automático (30 min)~~ ✅
- [x] ~~Número permanente de WhatsApp Business real~~ ✅
- [x] ~~Token permanente (System User de Meta)~~ ✅
- [x] ~~Admin dashboard con métricas, leads y conversaciones~~ ✅
- [x] ~~Portfolio personal — ayrtoncela.vercel.app~~ ✅
- [x] ~~Instagram DMs — webhook integrado y leads capturados~~ ✅
- [ ] Instagram DMs — respuestas activas (pendiente App Review de Meta)
- [ ] Integración con Google Calendar API
- [ ] Canal Telegram con el mismo backend

---

## 👨‍💻 Autor

**Ayrton Cela** — Consulting Engineering Manager & AI Builder  
Ciudad de México 🇲🇽

> *Construido con ayuda de Claude*
