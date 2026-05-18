# PROTOCOLO DE RECUPERACIÓN ANTE FALLOS Y REPLANIFICACIÓN AUTÓNOMA
> Este protocolo se activa AUTOMÁTICAMENTE ante cualquier fallo, bloqueo, resultado inesperado
> o desviación del plan. No espera instrucción del usuario para activarse.

---

## DEFINICIÓN DE FALLO

Un fallo es cualquiera de estas situaciones:

```
TIPOS DE FALLO
│
├── FALLO DE SUBFASE
│   Una subfase no produjo el resultado esperado o lo produjo incompleto
│
├── FALLO DE CALIDAD
│   El artefacto generado no pasa el bucle de calidad después de 2 intentos
│
├── FALLO DE COHERENCIA
│   El código, la arquitectura o los docs se contradicen entre sí
│
├── FALLO DE REQUERIMIENTO
│   El resultado no cumple lo que el usuario realmente necesitaba
│
├── FALLO DE DEPENDENCIA
│   Una tecnología, librería o servicio externo no funciona como se esperaba
│
├── REGRESIÓN
│   Algo que funcionaba dejó de funcionar por un cambio reciente
│
└── FALLO SILENCIOSO
    El resultado parece correcto pero tiene un problema oculto que el agente detecta
```

---

## PROTOCOLO DE DETECCIÓN AUTOMÁTICA

El agente ejecuta este chequeo interno DESPUÉS de cada acción, sin que el usuario lo pida:

```
CHEQUEO POST-ACCIÓN (obligatorio, silencioso)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. ¿El resultado coincide exactamente con el criterio de aceptación definido?
2. ¿Hay efectos secundarios no previstos en otros módulos?
3. ¿Los tests pasan? ¿La cobertura se mantiene o sube?
4. ¿Los documentos de entrega siguen sincronizados?
5. ¿El context del proyecto sigue siendo coherente con la realidad?
6. ¿Hay deuda técnica nueva generada por este cambio?

Si alguna respuesta es NO → activar PROTOCOLO DE RECUPERACIÓN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## FLUJO DE RECUPERACIÓN AUTÓNOMA

```
FALLO DETECTADO
      │
      ▼
┌─────────────────────────────────┐
│  PASO 1: REGISTRAR EL FALLO     │
│  Inmediatamente y antes de      │
│  cualquier otra acción          │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  PASO 2: CONGELAR EL AVANCE     │
│  El bucle de calidad se abre    │
│  No se puede pasar a la         │
│  siguiente subfase o fase       │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  PASO 3: ANÁLISIS DE CAUSA RAÍZ │
│  5 WHYs o análisis de impacto   │
│  completo antes de tocar nada   │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  PASO 4: CLASIFICAR LA DECISIÓN │
│                                 │
│  ¿Puede el agente resolverlo    │
│  autónomamente?                 │
│                                 │
│  SÍ ──────────────────────────► │  PASO 5A: REPLANIFICACIÓN AUTÓNOMA
│                                 │
│  NO (necesita input del usuario)│
│  ────────────────────────────► ►│  PASO 5B: ESCALACIÓN AL USUARIO
└─────────────────────────────────┘
```

---

## PASO 5A — REPLANIFICACIÓN AUTÓNOMA

El agente puede resolver autónomamente cuando:
- El error es técnico y tiene solución conocida
- No afecta requerimientos ya aprobados por el usuario
- La solución no introduce nuevos riesgos significativos
- El tiempo estimado de corrección es razonable (< 1 sesión)

```
REPLANIFICACIÓN AUTÓNOMA — [Subfase/Feature afectada]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Fallo detectado:    [descripción concisa]
Causa raíz:         [por qué ocurrió exactamente]
Impacto:            [qué se ve afectado]
Enfoque anterior:   [qué se intentó y por qué no funcionó]

NUEVO PLAN:
  Paso 1: [acción concreta]
  Paso 2: [acción concreta]
  Paso 3: [acción concreta]
  ...

Criterio de éxito: [cómo sabemos que esto quedó bien]
Tests de regresión: [qué pruebas confirman que nada se rompió]
Tiempo estimado:    [N minutos/horas]

Ejecutando plan ahora...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Después de ejecutar el nuevo plan, el agente vuelve a correr el CHEQUEO POST-ACCIÓN.
Si falla de nuevo → máximo 3 intentos autónomos → si sigue fallando → PASO 5B.

---

## PASO 5B — ESCALACIÓN AL USUARIO

El agente escala al usuario cuando:
- El fallo afecta requerimientos o decisiones ya aprobadas
- Han fallado 3 intentos autónomos
- El problema requiere información que el agente no tiene
- La solución implica un trade-off que el usuario debe decidir
- El fallo cambia el alcance o el tiempo del proyecto

```
🔴 FALLO — REQUIERO TU DECISIÓN

Subfase afectada: [nombre]
Qué falló: [descripción clara, sin tecnicismos innecesarios]
Por qué no puedo resolverlo solo: [razón concreta]

Causa raíz identificada:
  [análisis de 5 WHYs resumido]

OPCIONES DISPONIBLES:

  Opción A — [nombre de la opción]
  ├── Qué implica: [descripción]
  ├── Ventaja: [beneficio principal]
  ├── Desventaja: [costo o riesgo]
  └── Tiempo estimado: [N]

  Opción B — [nombre de la opción]
  ├── Qué implica: [descripción]
  ├── Ventaja: [beneficio principal]
  ├── Desventaja: [costo o riesgo]
  └── Tiempo estimado: [N]

  Opción C — Redefinir el alcance de esta subfase
  ├── Qué implica: ajustar los criterios de aceptación
  ├── Ventaja: desbloqueamos rápido
  ├── Desventaja: puede afectar otras fases
  └── Tiempo estimado: [N]

Mi recomendación: Opción [X] porque [razón concreta].

¿Qué decidimos?
```

---

## REGISTRO OBLIGATORIO DE CADA FALLO

Independientemente de si el fallo se resolvió autónoma o manualmente,
el agente SIEMPRE registra en `project-context.md`:

```
FALLO #[N] — [fecha y hora aproximada]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Subfase:          [donde ocurrió]
Tipo de fallo:    [técnico / requerimiento / coherencia / regresión / dependencia]
Descripción:      [qué pasó exactamente]
Causa raíz:       [por qué ocurrió]
Intentos autónomos: [N intentos fallidos antes de resolver]
Resolución:       [autónoma / escalada al usuario]
Solución aplicada: [qué se hizo]
Tiempo perdido:   [estimado]
Lección:          [qué hacer diferente para que no vuelva a ocurrir]
Impacto en plan:  [se replanificó / se ajustó el alcance / sin impacto]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## REPLANIFICACIÓN DE SUBFASE COMPLETA

Cuando una subfase falla en su totalidad (no solo una tarea dentro de ella):

```
REPLANIFICACIÓN DE SUBFASE — [nombre]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Estado anterior del plan:
  [qué decía el plan original]

Por qué falló:
  [análisis completo]

Nuevo plan de subfase:
  Objetivo: [resultado esperado — redefinido si necesario]

  Tarea 1: [nombre]
    Criterio de aceptación: [condición verificable]
    Riesgo: [qué puede fallar]
    Contingencia: [qué hacer si falla]

  Tarea 2: [nombre]
    Criterio de aceptación: [condición verificable]
    Riesgo: [qué puede fallar]
    Contingencia: [qué hacer si falla]

  ...

Dependencias ajustadas: [qué otras subfases o fases se ven afectadas]
Documentos a actualizar: [lista]
GitHub Issues a actualizar: [lista]

Aprobación requerida: [SÍ / NO]
  → Si SÍ: presentar al usuario antes de ejecutar
  → Si NO: ejecutar y notificar resultado
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## REGLAS ABSOLUTAS DE ESTE PROTOCOLO

1. **Todo fallo se registra siempre** — no existe fallo "menor" que no valga la pena documentar
2. **Máximo 3 intentos autónomos** — si falla 3 veces, escalar al usuario sin excepción
3. **No avanzar sobre un fallo** — nunca construir sobre una subfase que falló sin haberla cerrado
4. **El nuevo plan siempre tiene contingencia** — cada tarea del nuevo plan tiene un plan B explícito
5. **Notificar siempre** — incluso si el agente resuelve autónomamente, informa al usuario el resultado
6. **Lección extraída siempre** — cada fallo produce una lección concreta que entra a "Errores Aprendidos"
