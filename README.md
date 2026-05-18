# SDLC Agent Framework

> Framework de desarrollo de software guiado por agente IA.
> Versión 4.0 — Pipeline secuencial inmutable por prompt. Sin saltos. Sin atajos.
> Agnóstico de LLM. Funciona con cualquier modelo via CLI, API o interfaz.

---

## ⭐ Archivos Maestros

**`agent/core/agent.md`** — El sistema completo. Copia todo su contenido como system prompt.

**`agent/core/prompt-pipeline.md`** — El recordatorio del pipeline. Incluirlo en cada llamada refuerza que el agente ejecute los 8 pasos en cada prompt.

---

## 🧠 Principio fundamental v4.0

**Cada prompt = 8 pasos en orden. Sin excepción.**

El agente no puede responder "agrega este botón" y simplemente agregar el botón. Primero declara el estado del proyecto, clasifica el prompt, verifica precondiciones, revisa memoria, ejecuta el sub-pipeline correspondiente, corre el bucle de calidad, registra todo, y valida que no saltó nada. Solo entonces la respuesta está completa.

Aplica a TODOS los prompts: simples, complejos, técnicos, de consulta, de deploy. No existe prompt que no pase por el pipeline.

---

## 🆕 Novedades en v4.0

| Mejora | Descripción |
|--------|-------------|
| **Pipeline de 8 pasos por prompt** | Cada prompt dispara el pipeline completo en orden — ningún paso es saltable |
| **Clasificación obligatoria del prompt** | 7 tipos (A-G), cada uno con su sub-pipeline específico |
| **Precondiciones verificables** | Antes de ejecutar, el agente verifica explícitamente que las condiciones están dadas |
| **`prompt-pipeline.md`** | Archivo incluible en cada llamada de API/CLI para forzar el pipeline |
| **Autovalidación final** | El agente verifica que no saltó pasos antes de enviar su respuesta |
| **Señales de alerta documentadas** | Cómo detectar cuando el agente está saltándose pasos |

*(v3.0 incluyó: Ley de Inmutabilidad, failure-recovery.md, subfase-tracker.md, registro granular automático)*

---

## 🚀 Inicio rápido

### Paso 1 — Copia el sistema
Ve a `agent/core/agent.md` y copia todo su contenido.

### Paso 2 — Pégalo como system prompt

| Plataforma | Dónde pegarlo |
|------------|----|
| **Claude** (claude.ai) | Projects → Project Instructions |
| **ChatGPT** | Custom Instructions o GPT System Prompt |
| **Gemini** | Gems → Instrucciones del Gem |
| **API / CLI** | System message — ver ejemplo abajo |

### Paso 3 — Incluye el pipeline en cada llamada (para API/CLI)

```python
system_prompt = open("agent/core/agent.md").read()
pipeline      = open("agent/core/prompt-pipeline.md").read()
project_state = open("mi-proyecto/context/project-context.md").read()

messages = [
    {"role": "system", "content": system_prompt + "\n\n---\n\n" + pipeline},
    {"role": "user",   "content": project_state + "\n\n" + user_prompt}
]
```

### Paso 4 — Inicia con el contexto

Di: *"Aquí está el contexto de mi proyecto: [pega project-context.md]"*

El agente ejecutará el pipeline desde el PASO 0 automáticamente.

---

## Estructura del proyecto

```
/
├── agent/
│   ├── core/
│   │   ├── agent.md              ⭐ System prompt maestro (v4.0)
│   │   └── prompt-pipeline.md    ⭐ Pipeline de 8 pasos por prompt (v4.0)
│   ├── loops/
│   │   ├── quality-loop.md       Bucle de calidad universal
│   │   ├── change-loop.md        Bucle para cambios
│   │   ├── sync-loop.md          Protocolo de sincronización total
│   │   ├── failure-recovery.md   Recuperación autónoma ante fallos (v3)
│   │   └── subfase-tracker.md    Registro granular de subfases (v3)
│   ├── memory/
│   │   ├── project-context.md    Archivo vivo del proyecto
│   │   └── config.md             Configuración del framework
│   ├── phases/
│   │   └── phase-7-delivery.md
│   ├── tools/
│   │   ├── doc-generator.md
│   │   ├── github-manager.md
│   │   ├── sprint-manager.md
│   │   └── doc-generator-commands.md
│   └── validators/
│       ├── code-validator.md
│       └── documentation-validator.md
├── config/
│   └── framework.yml
├── docs/
│   ├── guides/
│   └── templates/
│       ├── sprint/
│       ├── progress/
│       └── delivery/
├── project-template/             Copia esto para cada proyecto nuevo
├── examples/
└── scripts/
    └── new-project.sh
```

---

## Principios del framework

1. **Pipeline primero** — cada prompt pasa por los 8 pasos, sin excepción
2. **Primero entender, luego construir** — nunca código sin contexto
3. **Bucle de calidad obligatorio** — cada fase y subfase se cierra o no avanza
4. **Registro automático** — el agente registra todo sin que el usuario lo pida
5. **Recuperación autónoma** — ante fallos, el agente replanifica y actúa (hasta 3 intentos)
6. **Aprendizaje continuo** — cada error produce una lección con prevención verificable
7. **Agnóstico de tecnología y LLM** — funciona con cualquier stack y modelo
8. **Documentación como ciudadano de primera clase** — sincronizada siempre, bloqueante si no

---

## Cómo contribuir

Revisa la estructura y envía un pull request con tus mejoras.
