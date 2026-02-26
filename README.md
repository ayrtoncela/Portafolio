🤖 WhatsApp AI Bot — Multi-Industry Framework
> Agente conversacional multicanal construido con Node.js + OpenAI GPT-3.5 + Meta Cloud API.  
> Un solo backend soporta múltiples industrias, canales y flujos de negocio — con logging automático y notificaciones en tiempo real.
***🚀 Demo en vivo
🌐 Frontend: web-page-saa-s.vercel.app  
💬 Chatbot widget: disponible en la esquina inferior derecha del sitio
***🏗️ Arquitectura
┌──────────────┐     ┌─────────────────────┐     ┌──────────────────┐
│  WhatsApp    │────▶│                     │────▶│  OpenAI GPT-3.5  │
│  (Meta API)  │     │   Node.js + Express │     │  (system prompt  │
└──────────────┘     │      Backend        │     │   por vertical)  │
                     │                     │     └──────────────────┘
┌──────────────┐     │  • Webhook handler  │
│  Web Chat    │────▶│  • Deduplicación    │────▶┌──────────────────┐
│  (Frontend)  │     │  • Memoria sesión   │     │  Google Sheets   │
└──────────────┘     │  • Multi-botType    │     │  (via Apps       │
                     │                     │     │   Script)        │
                     └─────────────────────┘     └──────────────────┘
                                │
                                ▼
                     ┌─────────────────────┐
                     │  Email Notification │
                     │  (nuevo lead →      │
                     │   notificación      │
                     │   instantánea)      │
                     └─────────────────────┘
***🏢 Verticales implementados
| Industria | Canal | Funcionalidades |
|-----------|-------|----------------|
| 🧪 Laboratorio Clínico | WhatsApp + Web | Agendamiento 5 pasos, 3 sucursales, instrucciones pre-análisis |
| 💻 Agencia de Software | Web | Atención a leads, servicios, captura de contacto |
| 🍽️ Restaurante | Web | Reservaciones, menú, horarios, pedidos |
Un solo backend — el vertical se selecciona vía botType en el request.
***✨ Features del sistema
🤖 Conversación
Memoria de sesión por usuario (últimos 10 mensajes)
Flujos estructurados paso a paso (agendamiento sin salirse del orden)
Detección de fin de conversación (goodbye words) → reset automático
Reglas de negocio embebidas: horarios, sucursales, restricciones, edge cases
📊 Data & Logging
Google Sheets como DB — cada conversación se guarda automáticamente vía Google Apps Script
Campos registrados: Timestamp · Teléfono · Primer Mensaje · Fuente · Estado · Conversación · Message ID
Estado del lead actualizado automáticamente (Conversación finalizada al detectar goodbye)
📧 Notificaciones
Email automático al capturar cada nuevo lead
Notificación instantánea con datos del contacto
🔒 Confiabilidad
Deduplicación de mensajes con cache (evita respuestas duplicadas de Meta API)
Limpieza automática del cache cada 5 minutos
Manejo de errores con respuesta de fallback al usuario
🌐 Multicanal
WhatsApp vía Meta Cloud API (webhook verificado)
Web chat vía endpoint /api/chat — mismo backend, mismo AI
***🛠️ Stack
| Capa | Tecnología |
|------|-----------|
| Backend | Node.js + Express |
| IA | OpenAI GPT-3.5-turbo |
| Mensajería | Meta WhatsApp Cloud API v22.0 |
| Base de datos | Google Sheets + Google Apps Script |
| Notificaciones | Email via Apps Script |
| Frontend | HTML/CSS/JS — desplegado en Vercel |
| Control de versiones | GitHub |
***📋 Flujo del agente — Laboratorio clínico
Usuario inicia conversación
        │
        ▼
Lead capturado → Google Sheets + Email notification
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
Conversación guardada en Google Sheets
        │
        ▼
Usuario dice "gracias/adiós" → Estado: "Conversación finalizada"
***📁 Estructura del proyecto
whatsapp-bot/
├── server.js          # Backend principal (Express + webhook + AI)
├── .env               # Variables de entorno (tokens, keys)
├── package.json
└── README.md
***🔜 Próximos pasos
Migrar persistencia de sesión a Supabase (PostgreSQL)
Agregar canal Telegram con el mismo backend
Dashboard de citas en tiempo real
Pasar a producción con número de WhatsApp Business real
Integración con Google Calendar API
Deploy del backend en Railway (actualmente local)
***👨‍💻 Autor
Ayrton Cela — Consulting Engineering Manager & AI Builder  
Ciudad de México 🇲🡽
> Construido con vibe coding usando Claude Code
