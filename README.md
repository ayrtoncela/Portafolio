# 🤖 WhatsApp AI Bot — Multi-Industry Framework

> Agente conversacional para WhatsApp construido con Node.js + OpenAI GPT-4 + Meta Cloud API.  
> Arquitectura configurable para distintos verticales de negocio con un solo backend.

---

## 🚀 Demo en vivo

🌐 **Frontend:** [web-page-saa-s.vercel.app](https://web-page-saa-s.vercel.app)  
💬 **Chatbot widget:** disponible en la esquina inferior derecha del sitio  

---

## 🏗️ Arquitectura

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────┐
│  WhatsApp User  │────▶│  Meta Cloud API   │────▶│  Node.js    │
└─────────────────┘     └──────────────────┘     │  Backend    │
                                                  │  (Express)  │
                                                  └──────┬──────┘
                                                         │
                                              ┌──────────▼──────────┐
                                              │   OpenAI GPT-4      │
                                              │   (system prompt    │
                                              │    por vertical)    │
                                              └─────────────────────┘
```

---

## 🏢 Verticales implementados

| Industria | Funcionalidades |
|-----------|----------------|
| 🧪 **Laboratorio Clínico** | Agendamiento de citas (flujo 5 pasos), 3 sucursales, horarios, instrucciones pre-análisis |
| 💻 **Agencia de Software** | Atención a leads, presentación de servicios, captura de contacto |
| 🍽️ **Restaurante** | Reservaciones, menú, horarios, pedidos por WhatsApp |

Cada vertical usa el mismo backend — solo cambia el `system prompt` de OpenAI.

---

## ✨ Features

- ✅ Conversación contextual con memoria de sesión
- ✅ Flujos estructurados (ej: agendamiento paso a paso sin salirse del orden)
- ✅ Reglas de negocio embebidas en el prompt (horarios, restricciones, sucursales)
- ✅ Respuestas concisas optimizadas para móvil (≤500 caracteres)
- ✅ Manejo de casos edge (horarios fuera de rango, ubicaciones no disponibles)
- ✅ Instrucciones especiales por tipo de estudio (ayuno, primera orina, etc.)
- ✅ Frontend web con widget de chatbot integrado (desplegado en Vercel)
- ✅ Webhook verificado con Meta

---

## 🛠️ Stack

| Capa | Tecnología |
|------|-----------|
| Backend | Node.js + Express |
| IA | OpenAI GPT-4 (Chat Completions) |
| Mensajería | Meta WhatsApp Cloud API |
| Frontend | HTML/CSS/JS vanilla |
| Deploy frontend | Vercel |
| Control de versiones | GitHub |
| LLM runtime | Groq (velocidad de inferencia) |

---

## 📋 Cómo funciona el flujo del laboratorio

El agente sigue un orden estricto al agendar citas:

```
1. ¿Qué tipo de estudio necesitas?
        ↓
2. ¿Cuál sucursal te queda más cerca? (Centro / Naucalpan / Roma)
        ↓
3. ¿Qué día prefieres? (Lun-Vie 7AM-2PM · Sáb 7AM-12PM)
        ↓
4. ¿A qué hora te acomoda?
        ↓
5. Nombre completo + teléfono para confirmar
```

El bot no avanza al siguiente paso hasta completar el anterior, y maneja edge cases (horarios fuera de rango, sucursales no listadas) sin romper el flujo.

---

## 🔜 Próximos pasos

- [ ] Migrar persistencia de sesión a Supabase (PostgreSQL)
- [ ] Agregar canal Telegram con el mismo backend
- [ ] Dashboard de citas en tiempo real
- [ ] Pasar a producción con número de WhatsApp Business real
- [ ] Integración con calendario (Google Calendar API)

---

## 👨‍💻 Autor

**Ayrton Cela** — Consulting Engineering Manager & AI Builder  
Ciudad de México 🇲🇽

> *Construido con vibe coding usando Claude Code*
