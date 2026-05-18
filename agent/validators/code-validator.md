# VALIDADOR DE CÓDIGO
> El agente ejecuta este validador sobre CADA fragmento de código que genera o revisa.
> Las reglas BLOQUEANTES impiden que el código avance al siguiente paso.
> No existe "lo arreglo después" — se corrige antes de continuar.

---

## ORDEN DE VALIDACIÓN (siempre en este orden, nunca saltarse pasos)

```
1. LONGITUD DE FUNCIONES    → BLOQUEANTE si supera 20 líneas
2. HARDCODING               → BLOQUEANTE si hay valores literales prohibidos
3. CORRECCIÓN               → BLOQUEANTE si no cumple criterios de aceptación
4. SEGURIDAD                → BLOQUEANTE si hay vulnerabilidades críticas
5. LEGIBILIDAD              → BLOQUEANTE si viola clean code básico
6. DISEÑO / SOLID           → ALTO si viola principios de diseño
7. PRUEBAS                  → BLOQUEANTE si cobertura < 70%
8. RENDIMIENTO              → MEDIO/ALTO según impacto
```

---

## REGLA 1 — LONGITUD DE FUNCIONES ⛔ BLOQUEANTE

**Límite absoluto: 20 líneas por función/método.**

Cómo contar:
- Se cuentan líneas de código ejecutable
- NO se cuentan: líneas vacías, solo comentario, llaves solas `{ }`
- SÍ se cuenta: todo lo demás — declaraciones, returns, llamadas, condicionales

```
❌ BLOQUEANTE — función de 45 líneas con múltiples responsabilidades:
  function procesarPedido(pedido) {
    // validación + cálculo + persistencia + notificación = 45 líneas
  }

✅ CORRECTO — misma lógica, responsabilidades separadas:
  function procesarPedido(pedido) {    // orquestador: 5 líneas
    validarPedido(pedido)
    const total = calcularTotal(pedido)
    const guardado = guardarPedido(pedido, total)
    notificarCliente(guardado)
    return guardado
  }
  // Cada subfunción: máximo 20 líneas también
```

Cuando el agente detecta una función > 20 líneas:
```
🔴 VIOLACIÓN DE LONGITUD — BLOQUEANTE
  Función:  [nombre]
  Líneas:   [N] (límite: 20, exceso: [N-30])
  Problema: [responsabilidades mezcladas detectadas]

  Refactorización propuesta:
    [nombre_original]() → orquesta N subfunciones
    [subfuncion_A]()    → responsabilidad A
    [subfuncion_B]()    → responsabilidad B

  ¿Genero el código refactorizado?
```
El agente no avanza hasta corregir o recibir confirmación explícita.

---

## REGLA 2 — SIN HARDCODING ⛔ BLOQUEANTE

**Ningún valor literal prohibido puede existir en el código fuente.**

### Qué está prohibido y su corrección

```
NÚMEROS MÁGICOS
  ❌  if (role === 3)           ✅  if (role === ROLES.ADMIN)
  ❌  setTimeout(fn, 5000)      ✅  setTimeout(fn, CONFIG.REFRESH_MS)
  ❌  slice(0, 100)             ✅  slice(0, CONFIG.MAX_PAGE_SIZE)

STRINGS MÁGICOS
  ❌  fetch("https://api.x.com/v2/users")   ✅  fetch(`${CONFIG.API_URL}/users`)
  ❌  if (status === "active")              ✅  if (status === STATUS.ACTIVE)
  ❌  connect("mongodb://localhost/db")     ✅  connect(process.env.DATABASE_URL)

CREDENCIALES Y SECRETS
  ❌  apiKey = "sk-abc123"      ✅  apiKey = process.env.API_KEY
  ❌  password = "admin1234"    ✅  password = process.env.DB_PASSWORD

URLS Y PUERTOS
  ❌  app.listen(3000)          ✅  app.listen(process.env.PORT ?? DEFAULT_PORT)
  ❌  redirect("https://app.com/dash")  ✅  redirect(`${CONFIG.APP_URL}/dash`)

RUTAS DE ARCHIVOS
  ❌  readFile("/var/log/app.log")   ✅  readFile(process.env.LOG_PATH)

TIMEOUTS Y LÍMITES
  ❌  cache.set(k, v, 3600)    ✅  cache.set(k, v, CONFIG.CACHE_TTL_S)
  ❌  if (attempts > 5)        ✅  if (attempts > CONFIG.MAX_RETRIES)

NOMBRES DE AMBIENTES
  ❌  if (env === "production")    ✅  if (CONFIG.IS_PRODUCTION)
  // CONFIG.IS_PRODUCTION se define UNA vez en config/index — no disperso
```

### Valores permitidos (no son hardcoding)
- Índices `0` y `1` en contexto obvio: `arr[0]`
- `true` / `false`
- Comparación con string vacío: `=== ""`
- Constantes matemáticas estándar: `Math.PI`

### Estructura de configuración obligatoria

```
src/
├── config/
│   ├── index.ts          ← punto de entrada único, exporta CONFIG
│   ├── app.config.ts     ← timeouts, límites, flags de ambiente
│   ├── database.config.ts
│   └── external.config.ts ← URLs y configs de servicios externos
├── constants/
│   ├── index.ts          ← exporta todas las constantes
│   ├── roles.ts          ← ROLES.ADMIN, ROLES.USER
│   ├── status.ts         ← STATUS.ACTIVE, STATUS.INACTIVE
│   └── errors.ts         ← ERROR_CODES, mensajes de error
└── .env.example          ← TODAS las variables de entorno documentadas
```

Regla: un valor configurable se define en UN lugar. Si cambia, se cambia solo ahí.

Cuando el agente detecta hardcoding:
```
🔴 HARDCODING DETECTADO — BLOQUEANTE
  Archivo:  [ruta]
  Línea(s): [N]
  Tipo:     [magic_number / url / credencial / etc.]

  Código actual:   [fragmento problemático]
  Riesgo:          [qué pasa si ese valor cambia / qué datos expone]
  Corrección:
    1. Definir en: [config/constants/env — cuál aplica]
    2. Como:       [NOMBRE_CONSTANTE = valor]
    3. Usar como:  [cómo referenciarlo]

  Código corregido: [listo para copiar]
  ¿Aplico la corrección?
```

---

## REGLA 3 — CORRECCIÓN ⛔ BLOQUEANTE

- [ ] Cumple todos los criterios de aceptación de la user story
- [ ] Maneja caso base correctamente
- [ ] Maneja casos límite: vacío, nulo, cero, negativo, extremo
- [ ] Manejo explícito de errores — `catch(e) {}` es BLOQUEANTE
- [ ] Sin regresiones en funcionalidad existente

---

## REGLA 4 — SEGURIDAD ⛔ BLOQUEANTE si es crítica

- [ ] Inputs validados Y sanitizados antes de usar
- [ ] Sin datos sensibles en logs o respuestas de error
- [ ] Sin credenciales en código (ver Regla 2)
- [ ] Queries parametrizadas — sin concatenación de strings en SQL
- [ ] Sanitización antes de renderizar en HTML — sin XSS
- [ ] Auth verificada en cada endpoint protegido

---

## REGLA 5 — LEGIBILIDAD ⛔ BLOQUEANTE si viola lo básico

- [ ] Nombres descriptivos: `userEmail` no `ue` ni `data` ni `tmp`
- [ ] Funciones con verbo: `calcularImpuesto` no `calc` ni `process`
- [ ] Sin abreviaciones crípticas fuera de estándares del dominio
- [ ] Sin comentarios que expliquen lo obvio
- [ ] Sin código comentado — se elimina (git lo guarda si se necesita)
- [ ] Sin código muerto: funciones definidas que nunca se llaman

---

## REGLA 6 — DISEÑO / SOLID 🟠 ALTO

- [ ] Cada función/clase tiene UNA responsabilidad
- [ ] Sin duplicación de lógica — si aparece dos veces, se extrae
- [ ] Sin lógica de negocio en infraestructura
- [ ] Sin acceso directo a BD desde controladores
- [ ] Depende de abstracciones, no de implementaciones concretas

---

## REGLA 7 — PRUEBAS ⛔ BLOQUEANTE

- [ ] Cobertura ≥ 70% en módulo modificado
- [ ] Al menos 1 test por criterio de aceptación
- [ ] Tests para happy path y casos de error principales
- [ ] Tests independientes entre sí
- [ ] Sin tests sin assertion (test que nunca puede fallar)

---

## REGLA 8 — RENDIMIENTO 🟡 MEDIO/ALTO

- [ ] Sin queries N+1 en loops
- [ ] Sin `SELECT *` si no se usan todos los campos
- [ ] Sin operaciones IO dentro de loops
- [ ] Sin memory leaks: listeners removidos al destruir

---

## ANTIPATRONES — DETECCIÓN AUTOMÁTICA

| Antipatrón | Severidad | Señal |
|------------|-----------|-------|
| Función > 20 líneas | 🔴 BLOQUEANTE | Longitud |
| Magic number/string | 🔴 BLOQUEANTE | Literal sin nombre |
| Credencial hardcodeada | 🔴 BLOQUEANTE | Key/pass en código |
| URL hardcodeada | 🔴 BLOQUEANTE | `http://` en string |
| Catch vacío | 🔴 BLOQUEANTE | `catch(e) {}` |
| > 4 parámetros | 🟠 ALTO | Firma de función |
| > 3 niveles anidamiento | 🟠 ALTO | Indentación |
| Lógica duplicada | 🟠 ALTO | Código idéntico en 2+ lugares |
| SELECT * | 🟡 MEDIO | Query sin campos |

---

## REPORTE DE CODE REVIEW

```
CODE REVIEW — [archivo / PR]
Fecha: [fecha] | Revisor: SDLC Agent
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VALIDACIÓN RÁPIDA:
  Funciones ≤ 20 líneas: [✅ Todas / 🔴 N violan el límite]
  Sin hardcoding:        [✅ Limpio / 🔴 N violaciones]
  Corrección:            [✅ Cumple / 🔴 Falla N criterios]
  Seguridad:             [✅ OK / 🔴 N issues]
  Pruebas:               [✅ N% cobertura / 🔴 Insuficiente]

🔴 BLOQUEANTE:
  Línea [N]: [problema + solución concreta]

🟠 ALTO:
  [observación + solución]

🟡 MEDIO:
  [observación + recomendación]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
VEREDICTO: [APROBADO ✅ / CAMBIOS REQUERIDOS 🔴 / RECHAZADO ❌]
Bloqueantes: [N] | Próximo paso: [qué corregir primero]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
