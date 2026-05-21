# Fase 01 — Inicio

## Contexto

Establece la base del proyecto. Produce el `PROJECT.md` validado con visión, objetivo, restricciones y definición de éxito. La aprobación del PM sobre este documento es el gate obligatorio antes de investigar o estrategizar.

**Tipo de proyecto**: todos (`software` / `strategy` / `marketing` / `mixed`)

---

## Entrevista

> Si el proyecto llegó aquí desde MODO NUEVO, las respuestas del Grupo 1-3 de la entrevista inicial ya están disponibles. Usar esos datos para pre-llenar — solo preguntar lo que faltó.

### Grupo A — Contexto de mercado
1. ¿En qué mercado o industria opera este proyecto?
2. ¿Quiénes son los competidores o referencias más relevantes?
3. ¿Hay alguna investigación previa o dato de contexto disponible?

### Grupo B — Usuarios y valor
4. ¿Quién es el usuario/beneficiario final?
5. ¿Cuál es el problema central que este proyecto resuelve?
6. ¿Qué valor genera y para quién?

### Grupo C — Constraints y éxito
7. ¿Cuáles son las restricciones más críticas? (tiempo, presupuesto, tecnología, regulación)
8. ¿Cómo se mide el éxito? ¿Qué métricas o entregables lo confirman?
9. ¿Hay decisiones ya tomadas que no están a discusión?

---

## Agents a despachar

| Agent | Input | Output esperado |
|-------|-------|----------------|
| `product-manager` | Respuestas de entrevista | Draft de `PROJECT.md` con OKRs |
| `business-analyst` | Draft de PROJECT.md | Validación de lógica: ¿el objetivo es alcanzable con los constraints? |

Despachar en paralelo después de completar los grupos de entrevista.

---

## Skills a invocar

### `/brainstorming` — **HARD-GATE**

Invocar obligatoriamente al definir la primera feature, workstream o módulo del proyecto.

- APEX invoca `/brainstorming`, **no lo reemplaza**
- El PM pasa por el flujo completo de Superpowers
- APEX espera que `/brainstorming` termine y produzca su spec aprobada
- Sin esta aprobación, **no se avanza a Fase 02**

---

## Output

| Documento | Path | Contenido |
|-----------|------|-----------|
| `PROJECT.md` | raíz del proyecto | Visión, objetivo, constraints, definición de éxito, OKRs |
| Spec de brainstorming | `docs/superpowers/plans/` | Producida por Superpowers — archivada aquí |

---

## Gate de aprobación

**Condición para avanzar a Fase 02:**
- [ ] PM aprueba `PROJECT.md` explícitamente
- [ ] Spec de `/brainstorming` aprobada (Superpowers hard-gate cumplido)
- [ ] `apex.config.json` actualizado: `currentPhase → 02-investigacion`, fase 01 en `completedPhases`

---

## Qué escribir a memoria al cerrar

**`patterns.md`:**
- Tipo de proyecto + industria
- ¿Cuántas iteraciones necesitó PROJECT.md antes de aprobación?
- Decisiones más debatidas en Grupo C

**`pm-profile.md`:**
- Velocidad observada en esta fase (¿aprobó rápido o iteró mucho?)
- Nivel de detalle pedido en PROJECT.md
- ¿Aceptó los OKRs propuestos o los reformuló?
