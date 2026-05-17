# BUCLE DE CAMBIOS Y REFACTORIZACIONES
> Se activa ante cualquier cambio, refactorización, optimización o corrección.

---

## FLUJO OBLIGATORIO ANTE CUALQUIER CAMBIO

```
CAMBIO SOLICITADO
       │
  ┌────▼──────────────────┐
  │  1. ANALIZAR           │
  │  ¿Qué fase afecta?    │
  │  ¿Es nuevo o cambia   │
  │  algo existente?      │
  └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  2. MEMORIA            │
  │  ¿Hay decisiones      │
  │  previas relacionadas?│
  │  ¿Errores aprendidos? │
  └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  3. IMPACTO            │
  │  ¿Qué módulos afecta? │
  │  ¿Qué se puede romper?│
  │  ¿Qué pruebas cubren? │
  └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  4. PLANIFICAR         │
  │  Pasos concretos      │
  │  para hacerlo bien    │
  └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  5. EJECUTAR           │
  │  Con estándares       │
  │  de calidad activos   │
  └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  6. BUCLE DE CALIDAD  │
  │  Checklist de la fase │
  │  afectada             │
  └────┬──────────────────┘
       │
   ┌────▼──────────────────┐
   │  7. DOCUMENTAR         │
   │  ADR si es decisión   │
   │  Error aprendido si   │
   │  hubo problema        │
   │  Docs de entrega      │
   │  afectados            │
   └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  8. VALIDAR            │
  │  Pruebas de regresión │
  │  Nada se rompió       │
  └────┬──────────────────┘
       │
  ┌────▼──────────────────┐
  │  CAMBIO COMPLETADO    │
  └───────────────────────┘
```

---

## TIPOS DE CAMBIO Y SU TRATAMIENTO

### Refactorización
**Definición:** Mejorar el código sin cambiar su comportamiento externo.
**Riesgo principal:** Introducir regresiones sin querer.

Antes de refactorizar:
- [ ] ¿Existen pruebas que cubran el código a refactorizar?
- [ ] Si no hay pruebas, ¿las escribimos antes de tocar el código?
- [ ] ¿Se entiende exactamente el comportamiento actual?

Durante:
- Cambios pequeños e incrementales
- Ejecutar pruebas después de cada paso
- Commit por cada refactorización atómica

Después:
- [ ] Las pruebas existentes siguen pasando
- [ ] El comportamiento externo es idéntico
- [ ] El código es notablemente más limpio/simple
- [ ] Docs de entrega afectados actualizados (si cambió estructura o interfaces)

### Optimización de rendimiento
**Definición:** Mejorar velocidad, memoria o recursos sin cambiar funcionalidad.
**Riesgo principal:** Optimizar algo que no era el cuello de botella real.

Antes de optimizar:
- [ ] ¿Hay evidencia medida de que esto es un problema? (no intuición)
- [ ] ¿Qué dice el profiler o las métricas?
- [ ] ¿Cuál es el baseline de rendimiento actual?

Durante:
- Optimizar solo lo que las métricas señalan
- Medir antes y después de cada cambio

Después:
- [ ] Mejora medida y documentada (N% más rápido)
- [ ] Sin regresiones funcionales
- [ ] La optimización es comprensible para el equipo

### Corrección de bug
**Definición:** Corregir comportamiento incorrecto del sistema.
**Riesgo principal:** Corregir el síntoma sin resolver la causa raíz.

Antes de corregir:
- [ ] ¿Se entendió la causa raíz? (no solo el síntoma)
- [ ] ¿Se tiene un caso de prueba que reproduzca el bug?
- [ ] ¿Qué otros módulos podrían tener el mismo problema?

Durante:
- Primero escribir la prueba que falla por el bug
- Luego corregir hasta que la prueba pase

Después:
- [ ] La prueba que reproducía el bug ahora pasa
- [ ] Las pruebas existentes siguen pasando
- [ ] Se documentó en "Errores Aprendidos" si fue significativo
- [ ] Release Notes actualizadas con la corrección
- [ ] Manual de Usuario actualizado si el bug afectaba flujos de usuario

### Cambio de requerimientos
**Definición:** El usuario o negocio cambia qué debe hacer el sistema.
**Riesgo principal:** Impacto no evaluado en todo lo que ya existe.

Antes de implementar:
- [ ] ¿El cambio está documentado y aprobado?
- [ ] ¿Qué user stories existentes se ven afectadas?
- [ ] ¿Qué pruebas hay que actualizar?
- [ ] ¿Hay decisiones arquitectónicas que deban revisarse?
- [ ] ¿Qué documentos de entrega se ven afectados?
    - [ ] Manual de Usuario
    - [ ] API Reference
    - [ ] Documento de Arquitectura
    - [ ] Release Notes
    - [ ] Otros docs afectados

---

## EVALUACIÓN DE IMPACTO

Cuando se solicita un cambio, el agente evalúa:

```
EVALUACIÓN DE IMPACTO — [nombre del cambio]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Tipo de cambio: [Refactorización / Optimización / Bug fix / Requerimiento]
Fase SDLC afectada: [fase]

Módulos afectados:
- [módulo 1] → [cómo se ve afectado]
- [módulo 2] → [cómo se ve afectado]

Pruebas que podrían fallar:
- [suite o test específico]

Riesgos del cambio:
🔴 [riesgo crítico si existe]
🟡 [riesgo medio si existe]

Decisiones previas relacionadas:
- ADR #[N]: [breve descripción de cómo aplica]

Errores aprendidos relacionados:
- Error #[N]: [breve descripción de cómo aplica]

Esfuerzo estimado: [XS / S / M / L / XL]
Recomendación: [proceder / pausar y analizar más / escalar]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
