# Gantt — [Nombre del Proyecto]
> Última actualización: [YYYY-MM-DD]
> Generado automáticamente por SDLC Agent — no editar manualmente
> Estado: [Fase actual] | Sprint [N] | [N]% completado

---

## Vista General del Cronograma

```mermaid
gantt
    title [Nombre del Proyecto] — Cronograma SDLC
    dateFormat  YYYY-MM-DD
    excludes    weekends

    section 🔍 Fase 0 — Entrevista
    Entrevista inicial          :done, f0-1, YYYY-MM-DD, 1d
    Validación con usuario      :done, f0-2, after f0-1, 1d

    section 📋 Fase 1 — Requerimientos
    Levantamiento de reqs       :done, f1-1, YYYY-MM-DD, 3d
    Priorización y MVP          :done, f1-2, after f1-1, 2d
    Criterios de aceptación     :done, f1-3, after f1-2, 1d
    Validación con usuario      :done, f1-4, after f1-3, 1d

    section 🏗️ Fase 2 — Diseño
    Decisión de arquitectura    :f2-1, YYYY-MM-DD, 2d
    Diseño modelo de datos      :f2-2, after f2-1, 2d
    Diseño de APIs              :f2-3, after f2-2, 2d
    Revisión de seguridad       :f2-4, after f2-3, 1d

    section ⚙️ Sprint 1 — [Objetivo]
    Setup del entorno           :s1-0, YYYY-MM-DD, 1d
    [Historia #1]               :s1-1, after s1-0, 3d
    [Historia #2]               :s1-2, after s1-0, 3d
    Tests Sprint 1              :s1-t, after s1-1, 2d
    PR Review Sprint 1          :s1-r, after s1-t, 1d

    section 🧪 Fase 4 — QA
    Suite pruebas               :f4-1, YYYY-MM-DD, 3d
    Pruebas E2E                 :f4-2, after f4-1, 2d
    Corrección bugs             :f4-3, after f4-2, 2d

    section 🚀 Fase 5 — Deploy
    Configuración CI/CD         :f5-1, YYYY-MM-DD, 2d
    Deploy Staging              :f5-2, after f5-1, 1d
    Deploy Producción           :milestone, f5-3, after f5-2, 0d

    section 📄 Fase 7 — Entrega
    Compilar docs               :f7-1, YYYY-MM-DD, 3d
    Validación y entrega        :milestone, f7-2, after f7-1, 0d
```

---

## Resumen de Estado por Fase

| Fase | Estado | Inicio | Fin Real/Est. | Retrasos |
|------|--------|--------|---------------|----------|
| Fase 0 — Entrevista | ⬜ Pendiente | - | - | - |
| Fase 1 — Requerimientos | ⬜ Pendiente | - | - | - |
| Fase 2 — Diseño | ⬜ Pendiente | - | - | - |
| Sprint 1 | ⬜ Pendiente | - | - | - |
| Sprint 2 | ⬜ Pendiente | - | - | - |
| Fase 4 — QA | ⬜ Pendiente | - | - | - |
| Fase 5 — Deploy | ⬜ Pendiente | - | - | - |
| Fase 7 — Entrega | ⬜ Pendiente | - | - | - |

Leyenda: ✅ Completa | 🔄 En progreso | 🔴 Bloqueada | ⬜ Pendiente

---

## Hitos Clave

| Hito | Fecha Estimada | Estado |
|------|---------------|--------|
| Requerimientos aprobados | - | ⬜ |
| Arquitectura aprobada | - | ⬜ |
| MVP listo para QA | - | ⬜ |
| Deploy en Staging | - | ⬜ |
| Deploy en Producción | - | ⬜ |
| Entrega final al cliente | - | ⬜ |

---

## PRs del Proyecto

| PR # | Título | Sprint | Estado | Mergeado |
|------|--------|--------|--------|----------|
| - | - | - | - | - |

---

## Historial de Cambios al Cronograma

| Fecha | Evento | Impacto en fechas |
|-------|--------|-------------------|
| [fecha] | Inicio del proyecto | Cronograma inicial generado |
