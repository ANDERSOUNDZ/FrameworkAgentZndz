# SDLC Agent Framework

> Framework de desarrollo de software guiado por agente IA.
> Agnóstico de LLM. Aprende, revisa, piensa y mejora en cada iteración.

---

## ⭐ Archivo Maestro

**`agent/core/agent.md`** — Copia **todo el contenido** de este archivo como system prompt en cualquier LLM (Claude, ChatGPT, Gemini, etc.).

Sin este archivo el framework no funciona. Es el **corazón del sistema**.

---

## 🚀 Inicio rápido (3 pasos)

### Paso 1 — Abre el archivo maestro

Ve a `agent/core/agent.md` y **copia todo su contenido** (749 líneas).

### Paso 2 — Pégalo como system prompt

| Plataforma | Dónde pegarlo |
|------------|--------------|
| **Claude** (claude.ai) | Projects → Project Instructions |
| **ChatGPT** | Custom Instructions o GPT System Prompt |
| **Gemini** | Gems → Instrucciones del Gem |
| **Mastra / LangChain** | System message de tu agente |

### Paso 3 — Inicia el proyecto

Solo di **"hola"** o describe tu proyecto. El agente comenzará automáticamente la **entrevista inicial (Fase 0)** y te guiará paso a paso hasta la entrega final.

---

## ¿Qué es esto?

Un framework completo que convierte cualquier LLM en un **agente senior de desarrollo de software**. No es solo un chatbot — es un sistema que:

- Entrevista al desarrollador antes de escribir código
- Genera el workflow completo personalizado del proyecto
- Ejecuta un bucle de calidad por cada fase, subfase y cambio
- Aprende del historial del proyecto para no repetir errores
- Bloquea el avance si la calidad no está cerrada
- Documenta cada decisión arquitectónica automáticamente
- Genera la documentación de entrega para el cliente

## Estructura del proyecto

```
/
├── README.md                           # Este archivo
│
├── agent/                              # Núcleo del agente
│   ├── core/
│   │   └── agent.md                    # ⭐ ARCHIVO MAESTRO (system prompt)
│   ├── memory/
│   │   └── project-context.md          # Plantilla de contexto del proyecto
│   ├── loops/
│   │   ├── quality-loop.md             # Bucle de calidad universal
│   │   ├── change-loop.md              # Bucle para cambios y refactorizaciones
│   │   └── sync-loop.md                # Bucle de sincronización total
│   ├── phases/
│   │   └── phase-7-delivery.md         # Fase 7: Documentación de entrega
│   ├── tools/
│   │   └── doc-generator.md            # Generador de documentación de entrega
│   └── validators/
│       ├── documentation-validator.md  # Validador de documentación
│       └── code-validator.md           # Validador de código
│
├── config/
│   └── framework.yml                   # Configuración global del framework
│
├── docs/
│   ├── guides/
│   │   ├── how-to-use.md               # Guía de uso del framework
│   │   └── delivery-process.md         # Proceso de entrega de documentación
│   ├── templates/
│   │   ├── adr-template.md             # Plantilla de ADR
│   │   └── delivery/                   # Plantillas de documentos de entrega
│   │       ├── technical-architecture-template.md
│   │       ├── user-manual-template.md
│   │       ├── api-reference-template.md
│   │       ├── deployment-guide-template.md
│   │       ├── operations-guide-template.md
│   │       ├── release-notes-template.md
│   │       ├── admin-guide-template.md
│   │       └── security-compliance-template.md
│   └── architecture/
│
├── examples/
│   └── example-project-context.md      # Ejemplo de contexto completado
│
├── scripts/
│   └── new-project.sh                  # Script para iniciar nuevo proyecto
│
└── tests/
    ├── unit/
    └── integration/
```

## Principios del framework

1. **Primero entender, luego construir** — nunca código sin contexto
2. **Bucle de calidad obligatorio** — cada fase se cierra o no avanza
3. **Aprendizaje continuo** — el agente aprende de cada error y decisión
4. **Agnóstico de tecnología** — funciona con cualquier stack
5. **Agnóstico de LLM** — funciona con cualquier modelo
6. **Documentación como ciudadano de primera clase** — no es opcional
7. **Documentación de entrega sincronizada siempre** — cada cambio en el proyecto actualiza automáticamente los documentos que se entregan al cliente

## Cómo contribuir

Para contribuir, revisa la estructura del proyecto y envía un pull request con tus mejoras.
