# Fase 03 — Estrategia

## Contexto

Convierte el research en decisiones: roadmap, modelo de negocio/técnico, GTM y risk register. Produce `STRATEGY.md`. Es la fase más iterativa — el PM suele volver varias veces antes de aprobar.

**Tipo de proyecto**: todos (el contenido varía significativamente por tipo)

---

## Entrevista

### Grupo A — Posicionamiento
1. ¿Cuál es la propuesta de valor diferenciada frente a lo que encontró el research?
2. ¿A qué segmento se ataca primero? ¿Por qué?
3. ¿Hay una ventana temporal que aprovechar (tendencia, vacío competitivo)?

### Grupo B — Modelo y recursos
4. ¿Cuál es el modelo de negocio / modelo técnico central?
5. ¿Con qué recursos se cuenta realmente para ejecutar? (personas, presupuesto, tiempo)
6. ¿Qué se hace internamente vs qué se terceriza o integra?

### Grupo C — Riesgos
7. ¿Cuál es el riesgo más crítico que podría matar el proyecto?
8. ¿Qué suposiciones están haciendo que podrían estar equivocadas?

---

## Agents a despachar

| Agent | Input | Output esperado |
|-------|-------|----------------|
| `product-strategist` | PROJECT.md + RESEARCH.md + Grupo A-B | Roadmap priorizado con rationale |
| `risk-manager` | PROJECT.md + RESEARCH.md + Grupo C | Risk register con probabilidad, impacto y mitigación |
| `research-synthesizer` | Outputs de los otros dos agentes | Síntesis coherente → draft de STRATEGY.md |

Despachar `product-strategist` y `risk-manager` en paralelo. `research-synthesizer` espera a ambos.

---

## Skills a invocar (según tipo de proyecto)

| Skill | Cuándo | Aplica a |
|-------|--------|----------|
| `pricing-strategy` | Si hay componente de monetización | `software` / `marketing` / `mixed` |
| `launch-strategy` | Si hay lanzamiento al mercado | `marketing` / `mixed` |
| `ceo-advisor` | Si hay decisiones de modelo de negocio de alto impacto | todos |

---

## Output

| Documento | Path | Contenido |
|-----------|------|-----------|
| `STRATEGY.md` | raíz del proyecto | Posicionamiento, roadmap, GTM, risk register |

**Estructura de `STRATEGY.md`:**
```
## Propuesta de valor
## Segmento objetivo y por qué
## Roadmap (fases / milestones)
## Modelo (negocio o técnico)
## GTM / Plan de lanzamiento
## Risk register (tabla: riesgo | probabilidad | impacto | mitigación)
## Suposiciones críticas
```

---

## Gate de aprobación

**Condición para avanzar a Fase 04:**
- [ ] PM aprueba `STRATEGY.md` — especialmente roadmap y risk register
- [ ] Los riesgos críticos (Grupo C) tienen mitigación documentada
- [ ] `apex.config.json` actualizado: `currentPhase → 04-ejecucion`

---

## Qué escribir a memoria al cerrar

**`patterns.md`:**
- Tipo de riesgos más comunes en esta industria/tipo de proyecto
- Decisiones de roadmap más frecuentemente debatidas
- ¿Qué sección de STRATEGY.md requirió más iteración?

**`pm-profile.md`:**
- ¿El PM propuso su propio roadmap o adoptó el propuesto?
- Nivel de tolerancia al riesgo observado
- ¿Usó datos del research para argumentar o tomó decisiones por intuición?
