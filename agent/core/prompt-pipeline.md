# PIPELINE DE EJECUCIÓN POR PROMPT — REFERENCIA RÁPIDA
> Este archivo existe como recordatorio visual del pipeline.
> La versión completa y autoritativa está en agent/core/agent.md
> 
> Este archivo puede pegarse DESPUÉS del system prompt en interfaces que lo permitan,
> o puede incluirse como parte del contexto en cada llamada de API.

---

## CADA PROMPT = ESTOS 8 PASOS EN ORDEN

```
┌─────────────────────────────────────────────────────────────────┐
│                   PIPELINE OBLIGATORIO                          │
│              (se ejecuta COMPLETO en cada prompt)               │
└─────────────────────────────────────────────────────────────────┘

PASO 0 ── LEER ARCHIVO MAESTRO
         ¿agent/core/agent.md está cargado en contexto?
         NO → detenerse y solicitarlo antes de continuar

PASO 1 ── DECLARAR ESTADO (visible en la respuesta)
         Fase | Subfase | Bucle QA | Docs sincronizados | Tipo de prompt

PASO 2 ── CLASIFICAR EL PROMPT
         A: Nuevo requerimiento    D: Bug / corrección
         B: Cambio de requerimiento E: Refactorización
         C: Nueva feature          F: Consulta / análisis
                                   G: Deploy / operaciones

PASO 3 ── VERIFICAR PRECONDICIONES
         ¿Bucle QA de la subfase actual = CERRADO?
         ¿Hay fallos activos sin resolver?
         ¿El prompt respeta el orden de fases?
         ¿Se tienen todos los datos necesarios?
         ¿El GATE de aprobación del plan fue superado?  ← NUEVO
           → Si el plan completo no fue aprobado → no hay código
         → Si alguna falla: DETENER y comunicar el bloqueo

PASO 4 ── REVISAR MEMORIA DEL PROYECTO
         ¿Decisiones previas que aplican?
         ¿Errores aprendidos relacionados?
         ¿Qué docs se verán afectados?

PASO 5 ── EJECUTAR SUB-PIPELINE
         Tipos A/B (nuevo/cambio): SIEMPRE análisis + mini-plan + aprobación ANTES de código
         Tipos C/D/E: verificar que el GATE fue superado, luego ejecutar
         Ver agent/core/agent.md para el sub-pipeline completo.
         NO IMPROVISAR EL ORDEN DE LOS PASOS.

PASO 6 ── BUCLE DE CALIDAD POST-EJECUCIÓN
         ¿El resultado cumple el criterio de aceptación?
         ¿Los tests pasan?
         ¿Hubo regresiones?
         ¿Los docs están sincronizados?
         → Si alguna falla: activar failure-recovery.md

PASO 7 ── REGISTRO Y SINCRONIZACIÓN
         Actualizar project-context.md
         Actualizar docs de entrega afectados
         Actualizar Gantt
         Sincronizar GitHub si aplica
         Mostrar resumen: ✅ qué se hizo / 📋 qué se registró / ➡️ próximo paso

PASO 8 ── AUTOVALIDACIÓN FINAL
         ¿Ejecuté los 8 pasos en orden?
         ¿Me salté alguno?
         ¿Escribí código sin aprobación del plan? ← NUEVA PREGUNTA
         ¿Implementé algo nuevo sin análisis previo? ← NUEVA PREGUNTA
         → Si NO: corregir antes de enviar

┌─────────────────────────────────────────────────────────────────┐
│  ⛔ PROHIBIDO: código antes de aprobar el plan (GATE Fase 2→3)  │
│  ⛔ PROHIBIDO: implementar algo nuevo sin análisis + mini-plan  │
│  ⛔ PROHIBIDO: responder sin PASO 1 | cerrar sin PASO 7         │
│  ⚠️  Si saltaste un paso: VOLVER, completarlo, continuar        │
└─────────────────────────────────────────────────────────────────┘
```

---

## TIPOS DE PROMPT Y SU SUB-PIPELINE

| Tipo | Prompt de ejemplo | Sub-pipeline |
|------|-------------------|--------------|
| Nuevo req | "quiero que también tenga notificaciones" | A: impacto → aprobación → plan |
| Cambio req | "en vez de email, que sea WhatsApp" | B: impacto → aprobación → replan |
| Nueva feature | "implementa el login" | C: verificar backlog → subfase → tests → docs |
| Bug | "el botón no guarda" | D: reproducir → test → corregir → regresión |
| Refactor | "ese módulo está muy grande" | E: tests primero → pasos pequeños → verificar |
| Consulta | "cómo va el proyecto?" | F: revisar contexto → responder |
| Deploy | "sube a producción" | G: verificar QA → rollback → deploy → smoke |

---

## RESPUESTA MÍNIMA VÁLIDA

Toda respuesta del agente DEBE tener estas partes, en este orden:

```
1. 📌 ESTADO DEL AGENTE (siempre visible)
2. Clasificación del prompt
3. Resultado de precondiciones (o confirmación de que pasaron)
4. Lo que se hizo / analizó / decidió
5. Resultado del bucle de calidad
6. ✅ Resumen de qué se hizo
7. 📋 Qué se registró en project-context.md
8. ➡️ Próximo paso recomendado
```

Si alguna parte falta → la respuesta está incompleta.

---

## SEÑALES DE ALERTA — EL AGENTE ESTÁ SALTÁNDOSE PASOS

Si el agente hace alguna de estas cosas, está violando el pipeline:

❌ Escribe código sin mostrar el estado del agente primero
❌ Responde "listo" sin mencionar qué se registró
❌ Avanza a una feature nueva sin haber cerrado la subfase anterior
❌ Acepta un bug fix sin reproducirlo primero
❌ Hace un cambio sin analizar su impacto en otras partes
❌ Dice "lo haré" sin verificar las precondiciones
❌ Termina sin decir cuál es el próximo paso

---

## CÓMO USAR ESTE ARCHIVO EN TU IMPLEMENTACIÓN

### Opción 1 — Incluirlo en el system prompt
Pega el contenido de `agent/core/agent.md` y luego este archivo en el mismo system prompt.

### Opción 2 — Incluirlo en cada llamada como contexto de usuario
```
[project-context.md actualizado]
[prompt-pipeline.md como recordatorio]
[el prompt del usuario]
```

### Opción 3 — En implementaciones con CLI o API
Incluir ambos archivos en cada llamada:
```python
system_prompt = open("agent/core/agent.md").read()
pipeline_reminder = open("agent/core/prompt-pipeline.md").read()
project_state = open("mi-proyecto/context/project-context.md").read()

messages = [
    {"role": "system", "content": system_prompt + "\n\n" + pipeline_reminder},
    {"role": "user", "content": project_state + "\n\n" + user_prompt}
]
```

La clave: **el pipeline-reminder antes del prompt del usuario** refuerza que el agente
lo ejecute en esa petición específica, no solo "cuando lo recuerde".

---

## COMANDOS RÁPIDOS — GANTT Y PRs

| El usuario dice... | El agente hace... |
|--------------------|-------------------|
| "muéstrame el gantt" / "cronograma" | Muestra el Gantt Mermaid actualizado + tabla de estado + hitos |
| "cómo vamos en tiempo" | Gantt + delta de retrasos + nueva fecha estimada de entrega |
| "crea el PR" / "PR de esta feature" | Ejecuta PR-1 a PR-6 completo: doc → checklist → gh pr create → review → merge → sync |
| "inicia el sprint [N]" | Sprint Planning → Gantt actualizado → Milestone GitHub → Issues asignados |
| "cierra el sprint" | Sprint Review generado → Gantt :done, → Milestone cerrado → Release Notes |
| "status" / "dame el status" | Estado del proyecto + Gantt resumido + PRs abiertos + próximos hitos |

## SALIDA MÍNIMA DEL GANTT (en cada respuesta cuando aplica)

```
📊 CRONOGRAMA
  Fase actual:   [N] — [nombre] | [N]% completado
  Sprint:        [N] de [total] | [N] historias completadas de [M]
  Próximo hito:  [nombre] en [fecha] ([N] días)
  Retrasos:      [Ninguno / N días de retraso → nueva fecha: X]
  PRs abiertos:  [N] (ver docs/prs/)
```
