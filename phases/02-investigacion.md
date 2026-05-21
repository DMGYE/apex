# Fase 02 — Investigación

## Contexto

Construye el mapa de contexto externo: mercado, competencia y tendencias. Produce `RESEARCH.md` con hallazgos sintetizados y validados por el PM. Esta fase es la base empírica de la estrategia.

**Tipo de proyecto**: todos (profundidad varía — `software` puede ser más técnico, `marketing` más orientado a consumidor)

---

## Entrevista

### Grupo A — Foco de investigación
1. ¿Hay áreas de mercado o competidores específicos a priorizar?
2. ¿Existe data interna previa (encuestas, analytics, entrevistas) que los agentes deben considerar?
3. ¿Cuál es el horizonte temporal relevante para tendencias? (12 meses / 3 años / 5+ años)

### Grupo B — Decisiones clave a responder
4. ¿Qué preguntas estratégicas debe responder esta investigación?
5. ¿Hay hipótesis del equipo que se quieran validar o refutar?

---

## Agents a despachar

Despachar en **paralelo** — los 3 trabajan simultáneamente:

| Agent | Input | Output esperado |
|-------|-------|----------------|
| `market-researcher` | PROJECT.md + respuestas de entrevista | Tamaño de mercado, segmentos, dinámicas |
| `competitive-analyst` | PROJECT.md + lista de competidores | Benchmarking, gaps, posicionamiento |
| `trend-analyst` | PROJECT.md + horizonte temporal | Tendencias emergentes con curvas de adopción |

**Síntesis posterior**: una vez los 3 terminan, sintetizar hallazgos cruzados en `RESEARCH.md`.

---

## Skills a invocar

| Skill | Cuándo | Aplica a |
|-------|--------|----------|
| `marketing-strategy-pmm` | Al sintetizar hallazgos para posicionamiento | `marketing` / `mixed` |
| `senior-data-scientist` | Si hay data cuantitativa para analizar | `software` / `mixed` con datos |

---

## Output

| Documento | Path | Contenido |
|-----------|------|-----------|
| `RESEARCH.md` | raíz del proyecto | Mercado, competencia, tendencias, conclusiones accionables |

**Estructura de `RESEARCH.md`:**
```
## Mercado
## Competencia (tabla comparativa)
## Tendencias (con horizonte temporal)
## Preguntas respondidas
## Hallazgos clave (bullets accionables)
## Fuentes
```

---

## Gate de aprobación

**Condición para avanzar a Fase 03:**
- [ ] PM aprueba síntesis de investigación en `RESEARCH.md`
- [ ] Las preguntas estratégicas del Grupo B tienen respuesta
- [ ] `apex.config.json` actualizado: `currentPhase → 03-estrategia`

---

## Qué escribir a memoria al cerrar

**`patterns.md`:**
- ¿Qué agente produjo los hallazgos más útiles para este tipo de proyecto?
- ¿Qué sección de RESEARCH.md requirió más iteración?
- Tipo de preguntas estratégicas más frecuentes en esta industria

**`pm-profile.md`:**
- ¿El PM leyó el research completo o pidió resumen ejecutivo?
- ¿Cuántos ciclos de revisión antes de aprobar?
