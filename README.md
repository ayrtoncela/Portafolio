# 🤖 WhatsApp + Instagram AI Bot — Multi-Industry Framework

> Agente conversacional multicanal construido con Node.js + OpenAI GPT-3.5 + Meta Cloud API.  
> Un solo backend soporta múltiples industrias, canales y flujos de negocio — con base de datos real, rate limiting, dashboard y notificaciones en tiempo real.

---

## 🚀 Demo en vivo

🎥 **Video demo:** [Ver en YouTube](https://www.youtube.com/watch?v=_C-984BwlBQ)  
🌐 **Frontend:** [web-page-saa-s.vercel.app](https://web-page-saa-s.vercel.app)  
💬 **Chatbot widget:** disponible en la esquina inferior derecha del sitio

---

## 🏗️ Arquitectura

```
┌──────────────┐     ┌─────────────────────────┐     ┌──────────────────┐
│  WhatsApp    │────▶│                         │────▶│  OpenAI GPT-3.5  │
│  (Meta API)  │     │   Node.js + Express     │     │  (system prompt  │
└──────────────┘     │      en Railway         │     │   por vertical)  │
                     │                         │     └──────────────────┘
┌──────────────┐     │  • Webhook handler      │
│  Instagram   │────▶│  • Deduplicación        │────▶┌──────────────────┐
│  DMs (Meta)  │     │  • Memoria de sesión    │     │     Supabase     │
└──────────────┘     │  • Rate limiting        │     │  (PostgreSQL)    │
                     │  • Multi-botType        │     └──────────────────┘
┌──────────────┐     │  • Session timeout      │
│  Web Chat    │────▶│  • Dashboard auth       │────▶┌──────────────────┐
│  (Frontend)  │     │                         │     │  Google Sheets   │
└──────────────┘     └─────────────────────────┘     │  (notificaciones)│
                                                     └──────────────────┘
```

---

## 🏢 Verticales implementados

| Industria | Canal | Funcionalidades |
|---|---|---|
| 🧪 **Laboratorio Clínico** | WhatsApp + Web | Agendamiento 5 pasos, 3 sucursales, instrucciones pre-análisis |
| 💻 **Agencia de Software / IA** | WhatsApp + Instagram + Web | Atención a leads, servicios de automatización, captura de contacto |
| 🍽️ **Restaurante** | Web | Reservaciones, menú, horarios, pedidos |

Un solo backend — el vertical se selecciona vía `botType` en el request.

---

## ✨ Features del sistema

### 🤖 Conversación
- Memoria de sesión por usuario (últimos 10 mensajes)
- Session timeout configurable (30 min de inactividad → reset automático)
- Flujos estructurados paso a paso (agendamiento sin salirse del orden)
- Detección de fin de conversación (goodbye words) → reset automático
- Reglas de negocio embebidas: horarios, sucursales, restricciones, edge cases

### 📊 Data & Logging
- **Supabase (PostgreSQL)** como base de datos principal
  - Tablas: `leads`, `conversations`, `processed_messages`, `rate_limits`
  - RPC para limpieza automática de mensajes procesados
- **Google Sheets** como capa de notificaciones y vista humana (vía Apps Script)
- Campos registrados: `Timestamp · Teléfono/ID · Primer Mensaje · Fuente · Estado · Conversación`
- Estado del lead actualizado automáticamente

### 🛡️ Confiabilidad
- **Deduplicación** de mensajes en Supabase (evita respuestas duplicadas de Meta API)
- **Rate limiting** por usuario (20 msgs/hr) y global (500 msgs/hr)
- **Filtro de mensajes stale** — ignora reintentos de Meta con >30s de antigüedad
- **Echo filter** — ignora mensajes enviados por la propia app (is_echo)
- Manejo de errores con respuesta de fallback al usuario

### 📧 Notificaciones
- Email automático al capturar cada nuevo lead
- Notificación instantánea vía Google Apps Script

### 🌐 Multicanal
- **WhatsApp** vía Meta Cloud API (webhook verificado, producción)
- **Instagram DMs** vía Instagram Graph API (`graph.instagram.com`)
- **Web chat** vía endpoint `/api/chat` — mismo backend, mismo AI

### 📈 Dashboard
- Panel protegido con autenticación Basic Auth
- Métricas en tiempo real: leads totales, leads hoy, mensajes hoy, usuarios activos
- Vista de conversaciones por usuario
- Monitor de rate limits

---

## 🛠️ Stack

| Capa | Tecnología |
|---|---|
| Backend | Node.js + Express |
| Hosting | Railway |
| IA | OpenAI GPT-3.5-turbo |
| Base de datos | Supabase (PostgreSQL) |
| Mensajería WhatsApp | Meta WhatsApp Cloud API v22.0 |
| Mensajería Instagram | Meta Instagram Graph API v22.0 |
| Notificaciones | Google Sheets + Apps Script |
| Frontend | HTML/CSS/JS — desplegado en Vercel |
| Control de versiones | GitHub |

---

## 📋 Flujo del agente — Agencia de IA (Instagram/WhatsApp)

```
Usuario envía DM (Instagram o WhatsApp)
        │
        ▼
Webhook recibe → deduplicación → rate limit check
        │
        ▼
Lead capturado en Supabase → Notificación Google Sheets + Email
        │
        ▼
1. ¿En qué industria o tipo de negocio estás?
        ↓
2. Identifica el problema principal
        ↓
3. Presenta el servicio más relevante con ejemplo concreto
        ↓
4. Ofrece agendar llamada o continuar por WhatsApp
        │
        ▼
Conversación guardada en Supabase + Google Sheets
        │
        ▼
Usuario dice "gracias/adiós" → Estado: "Conversación finalizada" → reset sesión
```

---

## 📁 Estructura del proyecto

```
whatsapp-bot/
├── server.js          # Backend principal (Express + webhook + AI + dashboard)
├── dashboard.html     # Panel de administración
├── .env               # Variables de entorno (tokens, keys)
├── package.json
└── README.md
```

---

## 🔧 Variables de entorno requeridas

```env
WHATSAPP_TOKEN=          # Meta WhatsApp Business token
PHONE_NUMBER_ID=         # ID del número de WhatsApp
INSTAGRAM_TOKEN=         # Token IGAAC de Instagram Graph API
INSTAGRAM_ACCOUNT_ID=    # Instagram Business Account ID
OPENAI_API_KEY=          # OpenAI API key
SUPABASE_URL=            # URL del proyecto Supabase
SUPABASE_SECRET_KEY=     # Service role key de Supabase
VERIFY_TOKEN=            # Token de verificación del webhook
DASHBOARD_PASSWORD=      # Password para el dashboard
PORT=3000
```

---

## ✅ Estado del proyecto

| Feature | Estado |
|---|---|
| WhatsApp Bot (producción) | ✅ Live |
| Instagram DMs Bot | ✅ Live (pendiente App Review) |
| Web Chat widget | ✅ Live |
| Supabase como DB principal | ✅ Implementado |
| Rate limiting | ✅ Implementado |
| Session timeout | ✅ Implementado |
| Dashboard de métricas | ✅ Implementado |
| Deploy en Railway | ✅ Live |
| App Review Meta (instagram_business_manage_messages) | ⏳ En proceso |
| Google Calendar API | 🔜 Próximo |
| Telegram channel | 🔜 Próximo |

---

## 👨‍💻 Autor

**Ayrton Cela** — Consulting Engineering Manager & AI Builder  
Ciudad de México 🇲🇽

> *Construido con ayuda de Claude*

