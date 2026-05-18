# GESTOR DE PULL REQUESTS — Documentación y Proceso Completo
> Cada feature, bugfix o cambio que toca el código DEBE tener un PR documentado.
> El agente genera la documentación del PR automáticamente y guía el proceso paso a paso.
> Sin PR documentado = bucle de calidad ABIERTO. No se puede mergear sin completar esto.

---

## CUÁNDO SE CREA UN PR

```
TRIGGERS DE CREACIÓN DE PR
│
├── Al completar una Feature (sub-pipeline C, paso C9)
├── Al corregir un Bug (sub-pipeline D, paso D9)
├── Al completar una Refactorización (sub-pipeline E, paso E7)
├── Al completar cualquier cambio que modifique código en producción
└── NUNCA se mergea código sin PR — sin excepción
```

---

## FLUJO COMPLETO DE UN PR

```
CÓDIGO LISTO EN BRANCH
        │
        ▼
┌───────────────────────┐
│  PASO PR-1            │
│  Generar documentación│
│  del PR completa      │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│  PASO PR-2            │
│  Verificar checklist  │
│  pre-merge            │
└──────────┬────────────┘
           │
      ¿Pasa todo?
           │
     NO ◄──┴──► SÍ
     │              │
     ▼              ▼
┌─────────┐   ┌───────────────────────┐
│CORREGIR │   │  PASO PR-3            │
│y volver │   │  Crear PR en GitHub   │
└─────────┘   │  con la documentación │
              └──────────┬────────────┘
                         │
                         ▼
              ┌───────────────────────┐
              │  PASO PR-4            │
              │  Code Review          │
              │  (auto o por par)     │
              └──────────┬────────────┘
                         │
                   ¿Aprobado?
                         │
                   NO ◄──┴──► SÍ
                   │              │
                   ▼              ▼
              ┌─────────┐   ┌───────────────────────┐
              │COMENTAR │   │  PASO PR-5            │
              │y corregir│  │  Merge + Cierre       │
              └─────────┘   │  del issue            │
                            └──────────┬────────────┘
                                       │
                                       ▼
                            ┌───────────────────────┐
                            │  PASO PR-6            │
                            │  Registro y Sync      │
                            │  (parte del PASO 7    │
                            │  del pipeline)        │
                            └───────────────────────┘
```

---

## PASO PR-1 — DOCUMENTACIÓN AUTOMÁTICA DEL PR

El agente genera este documento completo antes de crear el PR:

```markdown
# PR: [tipo]: [descripción concisa en imperativo]
> ej: "feat: agregar autenticación con JWT"
> ej: "fix: corregir cálculo de descuentos en carrito"
> ej: "refactor: extraer lógica de validación a módulo separado"

---

## 📋 Resumen

**¿Qué hace este PR?**
[1-3 frases describiendo el cambio desde la perspectiva del usuario o del sistema]

**¿Por qué es necesario?**
[Problema que resuelve o mejora que introduce]

**Issue relacionado:** Closes #[N]
**Sprint:** [N] — [nombre del sprint]
**Subfase:** [N.M] — [nombre de la subfase]

---

## 🏷️ Tipo de cambio

- [ ] ✨ `feat` — Nueva funcionalidad
- [ ] 🐛 `fix` — Corrección de bug
- [ ] 🔧 `refactor` — Mejora interna sin cambio de comportamiento
- [ ] ⚡ `perf` — Optimización de rendimiento
- [ ] 🧪 `test` — Agregar o corregir pruebas
- [ ] 📝 `docs` — Solo documentación
- [ ] 🔨 `chore` — Mantenimiento, deps, config

---

## 🔄 Cambios realizados

### Archivos modificados
| Archivo | Tipo de cambio | Descripción |
|---------|---------------|-------------|
| `src/[ruta]` | Creado / Modificado / Eliminado | [qué hace] |
| `tests/[ruta]` | Creado / Modificado | [qué prueba] |
| `docs/[ruta]` | Actualizado | [qué documenta] |

### Descripción de cambios
- **[Componente/Módulo]:** [qué cambió y por qué]
- **[Componente/Módulo]:** [qué cambió y por qué]

---

## ✅ Criterios de aceptación verificados

> Copiados de la User Story / Issue #[N]

- [x] [criterio 1] — ✅ Verificado
- [x] [criterio 2] — ✅ Verificado
- [ ] [criterio 3] — 🔄 En verificación
- [x] [criterio N] — ✅ Verificado

---

## 🧪 Tests

### Tests escritos en este PR
| Test | Tipo | Resultado |
|------|------|-----------|
| `[nombre del test]` | Unitario | ✅ Pasa |
| `[nombre del test]` | Integración | ✅ Pasa |
| `[nombre del test]` | E2E | ✅ Pasa |

### Cobertura
| Métrica | Antes | Después | Delta |
|---------|-------|---------|-------|
| Cobertura total | [N]% | [N]% | [+N%] |
| Módulo afectado | [N]% | [N]% | [+N%] |

### Suite de regresión
- [ ] Suite completa ejecutada: [N] tests, [N] pasan, 0 fallan
- [ ] Sin regresiones en funcionalidad existente

---

## 🔍 Checklist de Calidad del Código

- [ ] Nombres descriptivos en variables, funciones y clases
- [ ] Sin código duplicado (principio DRY aplicado)
- [ ] Manejo explícito de errores y casos límite
- [ ] Sin valores hardcodeados (usar constantes o config)
- [ ] Sin código muerto o comentarios de debug
- [ ] Funciones de máximo [20] líneas (según framework.yml)
- [ ] Máximo [3] niveles de anidamiento
- [ ] Máximo [4] parámetros por función
- [ ] Logging adecuado donde corresponde
- [ ] Sin credenciales ni secrets en el código

---

## 📄 Documentación actualizada

- [ ] `docs/delivery/02-user-manual.md` — [actualizado / no aplica]
- [ ] `docs/delivery/03-api-reference.md` — [actualizado / no aplica]
- [ ] `docs/delivery/06-release-notes.md` — [actualizado con esta feature/fix]
- [ ] `README.md` del proyecto — [actualizado / no aplica]
- [ ] Comentarios en código para lógica compleja — [agregados / no necesarios]

---

## 🚨 Riesgos e impacto

**¿Este PR puede romper algo en producción?**
- [ ] No — los cambios son aditivos y backward compatible
- [ ] Sí — [descripción del riesgo y plan de mitigación]

**¿Requiere migraciones de base de datos?**
- [ ] No
- [ ] Sí — script incluido en `migrations/[fecha]-[nombre].sql`

**¿Requiere variables de entorno nuevas?**
- [ ] No
- [ ] Sí — documentadas en `.env.example` y en la Guía de Despliegue

**¿Requiere pasos especiales de deploy?**
- [ ] No — deploy normal
- [ ] Sí — [instrucciones específicas]

---

## 🖼️ Evidencia (si aplica)

[Capturas de pantalla, GIFs, logs de consola, o salida de tests]

---

## 💬 Notas para el reviewer

[Contexto adicional que ayude al revisor: por qué se tomó cierta decisión,
qué alternativas se descartaron, qué áreas revisar con más atención]

---

## 📊 Métricas post-merge esperadas

[Solo para features visibles al usuario]
- KPI afectado: [ej: tiempo de login, tasa de error, etc.]
- Valor esperado: [ej: reducción de 500ms a 200ms]
- Cómo se verificará: [ej: monitoreo en Sentry / dashboard]
```

---

## PASO PR-2 — CHECKLIST PRE-MERGE

El agente ejecuta este checklist ANTES de crear el PR en GitHub. Si algún ítem falla, el PR no se crea hasta resolverlo:

```
CHECKLIST PRE-MERGE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CALIDAD DE CÓDIGO — VERIFICAR PRIMERO (bloqueantes absolutos):
  [ ] Ninguna función supera 20 líneas ejecutables
        → Ejecutar: grep + contar líneas por función
        → Si alguna supera → refactorizar antes de continuar
  [ ] Cero hardcoding en todo el código del PR
        → Revisar: números mágicos, strings, URLs, credenciales, puertos
        → Si existe alguno → mover a config/constants/.env antes de continuar

CÓDIGO:
  [ ] La branch tiene el nombre correcto: feat/fix/refactor/chore-[descripcion]
  [ ] Los commits siguen Conventional Commits
  [ ] Lint pasa sin errores
  [ ] El código compila / no tiene errores de sintaxis
  [ ] No hay conflictos con la branch base (main/develop)

TESTS:
  [ ] Todos los tests pasan (unitarios + integración)
  [ ] Cobertura ≥ 70% en módulos modificados
  [ ] Se escribió al menos 1 test por criterio de aceptación
  [ ] Suite de regresión completa ejecutada: 0 fallos

DOCUMENTACIÓN DEL PR:
  [ ] Descripción completa llenada
  [ ] Todos los criterios de aceptación marcados
  [ ] Archivos modificados documentados
  [ ] Riesgos evaluados

SINCRONIZACIÓN:
  [ ] project-context.md actualizado
  [ ] Docs de entrega afectados actualizados
  [ ] Release Notes actualizado
  [ ] Issue relacionado tiene comentario de progreso

Si algún ítem está en ❌ → NO SE CREA EL PR. Se resuelve primero.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## PASO PR-3 — CREACIÓN EN GITHUB

```bash
# El agente provee el comando exacto a ejecutar:

gh pr create \
  --title "[tipo]: [descripción concisa]" \
  --body "$(cat docs/prs/pr-[N]-[nombre].md)" \
  --label "[feature|bug|refactor]" \
  --label "sprint-[N]" \
  --milestone "Sprint [N]" \
  --assignee "@me" \
  --reviewer "[reviewer si aplica]"

# El archivo de documentación del PR se guarda previamente en:
# docs/prs/pr-[N]-[nombre-corto].md
```

El agente guarda la documentación del PR en `docs/prs/` antes de subirla a GitHub.

---

## PASO PR-4 — CODE REVIEW

### Auto-review (cuando no hay revisor externo)

El agente hace un code review documentado:

```
CODE REVIEW — PR #[N]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Reviewer: SDLC Agent (auto-review)
Fecha: [fecha]

REVISIÓN POR CATEGORÍA:

🔍 Corrección
  [ ] La lógica hace lo que dice hacer
  [ ] Los casos límite están manejados
  [ ] Los errores se propagan correctamente
  Observaciones: [ninguna / lista de observaciones]

🧹 Claridad
  [ ] El código es legible sin comentarios adicionales
  [ ] Los nombres son descriptivos y consistentes
  Observaciones: [ninguna / lista]

🔒 Seguridad
  [ ] No hay vulnerabilidades obvias (inyección, XSS, etc.)
  [ ] Las entradas del usuario se validan/sanitizan
  [ ] No hay datos sensibles expuestos
  Observaciones: [ninguna / lista]

⚡ Rendimiento
  [ ] No hay N+1 queries obvios
  [ ] No hay loops innecesarios sobre datasets grandes
  Observaciones: [ninguna / lista]

RESULTADO: [APROBADO ✅ / CAMBIOS REQUERIDOS 🔄 / RECHAZADO ❌]

Issues encontrados:
  - CRÍTICO: [descripción] → debe resolverse antes del merge
  - ALTO: [descripción] → debe resolverse antes del merge
  - MEDIO: [descripción] → puede resolverse en siguiente PR
  - BAJO: [descripción] → sugerencia opcional
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Si hay issues CRÍTICOS o ALTOS → el PR no se aprueba. Se corrige y se re-revisa.**

---

## PASO PR-5 — MERGE Y CIERRE

```bash
# Solo cuando el PR está aprobado y todos los checks pasan:

# Merge con squash (recomendado para mantener historial limpio)
gh pr merge [N] --squash --delete-branch

# Cerrar el issue relacionado con comentario
gh issue close [N] --comment "✅ Completado en PR #[N]. Mergeado en [hash]."

# Actualizar milestone
gh api repos/:owner/:repo/milestones/[N] -X PATCH
```

---

## PASO PR-6 — REGISTRO Y SINCRONIZACIÓN

Después de mergear, el agente actualiza automáticamente:

```
POST-MERGE SYNC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ ] project-context.md:
      - User story marcada como COMPLETADA
      - Cobertura de tests actualizada
      - Métricas actualizadas

[ ] docs/delivery/06-release-notes.md:
      - Entrada agregada para este cambio

[ ] docs/delivery/02-user-manual.md (si fue una feature):
      - Nueva funcionalidad documentada

[ ] docs/delivery/03-api-reference.md (si cambió la API):
      - Endpoints actualizados

[ ] Gantt actualizado:
      - Subfase correspondiente marcada :done,
      - Fecha real registrada

[ ] Sprint backlog actualizado:
      - Historia marcada como completada
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## NOMENCLATURA DE BRANCHES Y COMMITS

### Branches
```
feat/[nombre-corto-sin-espacios]     → nueva feature
fix/[nombre-del-bug]                 → corrección de bug
refactor/[descripcion]               → refactorización
chore/[descripcion]                  → mantenimiento
docs/[descripcion]                   → solo documentación
hotfix/[descripcion]                 → fix urgente en producción
```

### Commits (Conventional Commits)
```
feat(scope): descripción en imperativo
fix(scope): descripción en imperativo
refactor(scope): descripción en imperativo
test(scope): descripción en imperativo
docs(scope): descripción en imperativo
chore(scope): descripción en imperativo

Ejemplos:
  feat(auth): agregar autenticación con JWT (#12)
  fix(cart): corregir cálculo de descuentos cuando precio es 0 (#15)
  refactor(user): extraer validación a módulo separado
  test(auth): agregar tests para expiración de token
```

---

## INTEGRACIÓN CON EL PIPELINE DE 8 PASOS

El PR es parte del PASO 5 de cada sub-pipeline:

```
Sub-pipeline C (Feature):
  C9:  Crear documentación del PR (PR-1)
  C10: Checklist pre-merge (PR-2)
  C11: Crear PR en GitHub (PR-3)
  C12: Code Review (PR-4)
  C13: Merge y cierre (PR-5)
  C14: Post-merge sync (PR-6)

Sub-pipeline D (Bug):
  D9:  Crear documentación del PR (PR-1)
  D10: Checklist pre-merge (PR-2)
  D11: Crear PR en GitHub (PR-3)
  ...
```

---

## COMANDO RÁPIDO

Cuando el usuario diga: **"crea el PR"** / **"está listo para mergear"** / **"PR de esta feature"**

El agente:
1. Ejecuta el checklist pre-merge (PASO PR-2)
2. Si pasa: genera la documentación completa
3. Provee el comando `gh pr create` con todo llenado
4. Ejecuta el auto code review (PASO PR-4)
5. Si todo aprueba: provee el comando de merge
6. Ejecuta post-merge sync automáticamente
