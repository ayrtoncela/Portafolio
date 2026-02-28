# 🤖 WhatsApp AI Bot — Multi-Industry Framework

> Agente conversacional multicanal construido con Node.js + OpenAI GPT-3.5 + Meta Cloud API.  
> Un solo backend soporta múltiples industrias, canales y flujos de negocio — con persistencia en Supabase, logging en Google Sheets y notificaciones en tiempo real.

---

## 🚀 Demo en vivo

🎥 **Video demo:** [Ver en YouTube](https://www.youtube.com/watch?v=_C-984BwlBQ)  
🌐 **Frontend:** [web-page-saa-s.vercel.app](https://web-page-saa-s.vercel.app)  
💬 **Chatbot widget:** disponible en la esquina inferior derecha del sitio

---

## 🏗️ Arquitectura

```
┌──────────────┐     ┌─────────────────────┐     ┌──────────────────┐
│  WhatsApp    │────▶│                     │────▶│  OpenAI GPT-3.5  │
│  (Meta API)  │     │   Node.js + Express │     │  (system prompt  │
└──────────────┘     │      Backend        │     │   por vertical)  │
                     │   (Railway 24/7)    │     └──────────────────┘
┌──────────────┐     │                     │
│  Web Chat    │────▶│  • Webhook handler  │────▶┌──────────────────┐
│  (Frontend)  │     │  • Deduplicación    │     │    Supabase      │
└──────────────┘     │  • Historial sesión │     │  (DB principal)  │
                     │  • Multi-botType    │     │  PostgreSQL      │
                     │  • Rate limiting    │     └──────────────────┘
                     └─────────────────────┘              │
                                │                         ▼
                                │              ┌──────────────────┐
                                └─────────────▶│  Google Sheets   │
                                               │  (notificaciones │
                                               │  + vista humana) │
                                               └──────────────────┘
```

---

## 🏢 Verticales implementados

| Industria | Canal | Funcionalidades |
|-----------|-------|----------------|
| 🧪 **Laboratorio Clínico** | WhatsApp + Web | Agendamiento 5 pasos, 3 sucursales, instrucciones pre-análisis |
| 💻 **Agencia de Software** | Web | Atención a leads, servicios, captura de contacto |
| 🍽️ **Restaurante** | Web | Reservaciones, menú, horarios, pedidos |

Un solo backend — el vertical se selecciona vía `botType` en el request.

---

## ✨ Features del sistema

### 🤖 Conversación
- Historial de sesión persistente por usuario (Supabase — sobrevive reinicios)
- Flujos estructurados paso a paso (agendamiento sin salirse del orden)
- Detección de fin de conversación (goodbye words) → actualización de estado automática
- Reglas de negocio embebidas: horarios, sucursales, restricciones, edge cases

### 🗄️ Persistencia (Supabase)
- **`leads`** — captura de cada nuevo contacto con fuente, primer mensaje y estado
- **`conversations`** — historial completo por usuario y canal
- **`processed_messages`** — deduplicación persistente con limpieza automática cada 5 min
- Estado del lead actualizado automáticamente en tiempo real

### 📊 Logging & Notificaciones (Google Sheets)
- Cada conversación se guarda vía Google Apps Script como vista humana
- Email automático al capturar cada nuevo lead
- Campos: `Timestamp · Teléfono · Primer Mensaje · Fuente · Estado · Conversación`

### 🔒 Confiabilidad
- Deduplicación de mensajes persistente en DB (evita respuestas duplicadas de Meta API)
- Manejo de errores con respuesta de fallback al usuario
- Deploy en Railway con redeploy automático en cada push a `main`

### 🌐 Multicanal
- **WhatsApp** vía Meta Cloud API (webhook verificado)
- **Web chat** vía endpoint `/api/chat` — mismo backend, mismo AI

---

## 🛠️ Stack

| Capa | Tecnología |
|------|-----------|
| Backend | Node.js v20 + Express |
| IA | OpenAI GPT-3.5-turbo |
| Mensajería | Meta WhatsApp Cloud API v22.0 |
| Base de datos | Supabase (PostgreSQL) |
| Logging | Google Sheets + Google Apps Script |
| Notificaciones | Email via Apps Script |
| Deploy | Railway (24/7, auto-deploy desde GitHub) |
| Frontend | HTML/CSS/JS — desplegado en Vercel |
| Control de versiones | GitHub |

---

## 📋 Flujo del agente — Laboratorio clínico

```
Usuario inicia conversación
        │
        ▼
Lead capturado → Supabase (leads) + Google Sheets + Email
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
        ▼
Usuario dice "gracias/adiós" → Estado: "Conversación finalizada"
```

---

## 📁 Estructura del proyecto

```
whatsapp-bot/
├── server.js          # Backend principal (Express + webhook + AI + Supabase)
├── .env               # Variables de entorno (no incluido en repo)
├── .gitignore         # .env y node_modules excluidos
├── package.json
└── README.md
```

---

## ⚙️ Variables de entorno requeridas

```env
PORT=3000
VERIFY_TOKEN=tu_verify_token
WHATSAPP_TOKEN=tu_whatsapp_token
PHONE_NUMBER_ID=tu_phone_number_id
OPENAI_API_KEY=tu_openai_key
SUPABASE_URL=https://tu-proyecto.supabase.co
SUPABASE_SECRET_KEY=tu_supabase_secret_key
```

---

## 🗺️ Roadmap

- [x] ~~Migrar persistencia de sesión a Supabase (PostgreSQL)~~ Completado 27 Feb 2026 ✅
- [x] ~~Deploy del backend en Railway (24/7, auto-deploy)~~ Completado 27 Feb 2026 ✅
- [x] ~~Deduplicación de mensajes persistente en DB~~ Completado 27 Feb 2026 ✅
- [x] ~~Video demo grabado y publicado~~ (V1) Completado 2 Feb 2026 ✅
- [ ] Video demo v2 (Supabase y Railway)
- [ ] Rate limiting por usuario (protección de tokens OpenAI)
- [ ] Número permanente de WhatsApp Business (token que no expira)
- [ ] Dashboard admin para ver leads y conversaciones desde Supabase
- [ ] Integración con Google Calendar API
- [ ] Canal Telegram con el mismo backend

---

## 👨‍💻 Autor

**Ayrton Cela** — Consulting Engineering Manager & AI Builder  
Ciudad de México 🇲🇽

> *Construido con vibe coding usando Claude*
