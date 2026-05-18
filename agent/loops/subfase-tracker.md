# RASTREADOR DE SUBFASES — REGISTRO AUTOMÁTICO Y CONTINUO
> El agente usa este protocolo para desglosar, ejecutar y registrar cada subfase
> de forma granular. Nada ocurre sin quedar registrado.

---

## ESTRUCTURA DE UNA SUBFASE

Cada subfase tiene exactamente este ciclo de vida:

```
CICLO DE VIDA DE SUBFASE
│
├── 1. PLANIFICADA    → existe en el plan pero no ha empezado
├── 2. EN PROGRESO    → el agente está ejecutando tareas dentro de ella
├── 3. EN REVISIÓN    → tareas completadas, ejecutando bucle de calidad
├── 4. BLOQUEADA      → fallo detectado, esperando resolución
├── 5. REPLANIFICADA  → nuevo plan creado tras un fallo
└── 6. CERRADA        → bucle de calidad CERRADO, documentación sincronizada
```

---

## REGISTRO AUTOMÁTICO AL INICIAR UNA SUBFASE

Antes de ejecutar cualquier tarea, el agente SIEMPRE genera este registro:

```
📋 SUBFASE INICIADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Fase padre:     [N] — [nombre de la fase]
Subfase:        [N.M] — [nombre de la subfase]
Estado:         EN PROGRESO
Inicio:         [fecha/sesión]

Objetivo:
  [Descripción clara de qué debe producirse al finalizar]

Tareas:
  [ ] Tarea 1: [descripción] | Criterio: [condición verificable]
  [ ] Tarea 2: [descripción] | Criterio: [condición verificable]
  [ ] Tarea N: [descripción] | Criterio: [condición verificable]

Artefactos esperados:
  - [archivo/componente/documento que debe existir al terminar]

Riesgos identificados:
  - [riesgo 1] → Contingencia: [qué hacer]
  - [riesgo 2] → Contingencia: [qué hacer]

Dependencias:
  - [qué debe existir previamente para que esta subfase funcione]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## REGISTRO AUTOMÁTICO AL COMPLETAR CADA TAREA

Después de completar CADA tarea individual dentro de la subfase:

```
✅ TAREA COMPLETADA
  Tarea:    [descripción]
  Resultado: [qué se produjo exactamente]
  Criterio cumplido: [SÍ / NO — si NO, activa failure-recovery.md]
  Artefactos generados: [lista de archivos/componentes creados o modificados]
  Tests: [nuevos tests escritos / tests existentes que pasan]
  Docs actualizados: [lista de documentos actualizados]
```

---

## REGISTRO AUTOMÁTICO AL CERRAR UNA SUBFASE

Cuando el bucle de calidad se cierra para una subfase:

```
🔒 SUBFASE CERRADA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Subfase:    [N.M] — [nombre]
Duración:   [sesiones / tiempo aproximado]
Estado:     CERRADA

Tareas completadas:
  [✅] Tarea 1 — [resultado]
  [✅] Tarea 2 — [resultado]
  [✅] Tarea N — [resultado]

Fallos ocurridos durante esta subfase:
  [Ninguno / lista de fallos con su resolución]

Artefactos producidos:
  - [archivo o componente] — [descripción de qué hace]

Métricas al cierre:
  Cobertura de tests:  [%]
  Bugs encontrados:    [N] (críticos: [N], altos: [N])
  Deuda técnica nueva: [Ninguna / descripción]
  Docs sincronizados:  [SÍ / NO — si NO, no se puede cerrar]

Lecciones de esta subfase:
  - [lección 1]
  - [lección 2]

Actualización requerida en project-context.md:
  [ ] Estado de subfase marcado como CERRADA
  [ ] Errores aprendidos registrados (si los hubo)
  [ ] Métricas actualizadas
  [ ] Documentos de entrega actualizados
  [ ] Próxima subfase definida
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## DESGLOSE ESTÁNDAR DE SUBFASES POR FASE

El agente desglosa SIEMPRE cada fase en subfases antes de empezar a ejecutar.
Este desglose se registra en `project-context.md` al inicio de cada fase.

### Fase 1 — Requerimientos
```
1.1 Identificación de stakeholders y usuarios
1.2 Levantamiento de requerimientos funcionales
1.3 Levantamiento de requerimientos no funcionales
1.4 Priorización y alcance del MVP
1.5 Definición de criterios de aceptación
1.6 Validación con el usuario
1.7 Cierre y firma del bucle de calidad
```

### Fase 2 — Diseño y Arquitectura
```
2.1 Análisis de opciones de arquitectura
2.2 Decisión de arquitectura (ADR #1)
2.3 Diseño del modelo de datos
2.4 Diseño de APIs y contratos
2.5 Diseño de infraestructura
2.6 Revisión de seguridad en el diseño
2.7 Validación con el usuario
2.8 Cierre y firma del bucle de calidad
```

### Fase 3 — Desarrollo
```
3.0 Setup del entorno y estructura del proyecto
3.N [Una subfase por cada User Story o módulo mayor]
    3.N.1 Análisis técnico de la historia
    3.N.2 Implementación
    3.N.3 Tests unitarios
    3.N.4 Tests de integración
    3.N.5 Code review
    3.N.6 Merge y cierre del bucle
3.X Integración y prueba del sistema completo
```

### Fase 4 — Testing / QA
```
4.1 Plan de pruebas
4.2 Suite de pruebas unitarias (cobertura ≥ 70%)
4.3 Suite de pruebas de integración
4.4 Pruebas E2E (flujos críticos)
4.5 Pruebas de rendimiento (si aplica)
4.6 Revisión de seguridad OWASP
4.7 UAT con usuario real (si aplica)
4.8 Corrección de bugs encontrados
4.9 Cierre y firma del bucle de calidad
```

### Fase 5 — Despliegue
```
5.1 Configuración de CI/CD
5.2 Deploy en staging
5.3 Pruebas en staging
5.4 Plan de rollback documentado
5.5 Configuración de monitoreo y alertas
5.6 Deploy en producción
5.7 Smoke tests en producción
5.8 Cierre y firma del bucle de calidad
```

---

## BLOQUEO DE SUBFASE — NOTIFICACIÓN AUTOMÁTICA

Si una subfase queda bloqueada, el agente notifica inmediatamente:

```
🔴 SUBFASE BLOQUEADA — [N.M] [nombre]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Bloqueada en:  Tarea [N] — [descripción]
Motivo:        [descripción clara del bloqueo]
Tiempo en bloqueo: [duración]

Activando protocolo de recuperación...
→ Ver failure-recovery.md para el plan de acción
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## REGLA FUNDAMENTAL

**El agente NUNCA dice "listo" sin haber cerrado el registro de la subfase.**

Si la subfase no tiene su registro de cierre completo → el bucle de calidad está ABIERTO.
Si el bucle está abierto → no se inicia la siguiente subfase.
