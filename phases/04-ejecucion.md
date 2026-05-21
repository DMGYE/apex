# Fase 04 — Ejecución

## Contexto

Traduce la estrategia en trabajo concreto. El flujo varía según el tipo de proyecto: `software`/`mixed` usa worktrees y cavecrew; `strategy`/`marketing` usa planes de trabajo y agile. Produce `SPRINT.md` con backlog y tracking.

---

## Entrevista

### Grupo A — Alcance del primer sprint
1. ¿Qué es lo más importante que debe estar listo al final de este sprint?
2. ¿Hay dependencias externas o bloqueos conocidos?
3. ¿Cuánto tiempo tiene este sprint? (1 semana / 2 semanas / milestone)

### Grupo B — Recursos disponibles ahora
4. ¿Quién está disponible para ejecutar y en qué capacidad?
5. ¿Hay herramientas, accesos o entornos que necesiten preparación?

---

## Bifurcación por tipo de proyecto

### Tipo `strategy` / `marketing`

**Skills:**
- `/writing-plans` — genera el plan de trabajo estructurado con tareas y owners
- `agile-product-owner` — define backlog priorizado y criterios de aceptación

**Agents:**
- `project-manager` — tracking de avance, dependencias, riesgos de ejecución
- `workflow-orchestrator` — si hay procesos multi-paso que coordinar

**Output:** `SPRINT.md` con plan de trabajo, tareas, owners y criterios de completitud.

---

### Tipo `software` / `mixed`

**Flujo obligatorio (en orden):**

1. `/writing-plans` — genera el plan técnico con tasks y checkpoints
2. `/using-git-worktrees` — crea el worktree aislado para implementación
3. `senior-[stack]` — ejecuta la implementación (ver stack detection abajo)
4. Cavecrew agents disponibles durante toda la fase

**Skills adicionales:**
- `agile-product-owner` — para gestión del backlog si hay múltiples features

**Agents:**
- `project-manager` — coordinación y tracking
- `workflow-orchestrator` — si hay múltiples componentes en paralelo
- `caveman:cavecrew-builder` — implementación táctica, edits quirúrgicos
- `caveman:cavecrew-investigator` — búsqueda de código, localización de símbolos
- `caveman:cavecrew-reviewer` — review de diffs antes de merge

**Stack detection** — leer `PROJECT.md` y detectar por keywords:

| Keywords en PROJECT.md | Agent recomendado |
|------------------------|-------------------|
| Next.js, React, Vue, frontend | `senior-frontend` |
| Python, FastAPI, Django, Node, backend | `senior-backend` |
| Docker, Kubernetes, Terraform, infra | `senior-devops` |
| AI, LLM, Claude, Gemini, embeddings | `senior-prompt-engineer` |
| Data, pipeline, ETL, SQL | `senior-data-engineer` |
| No detectado | Preguntar al PM |

**Output:** `SPRINT.md` + plan en `docs/superpowers/plans/`

---

## Output

| Documento | Path | Contenido |
|-----------|------|-----------|
| `SPRINT.md` | raíz del proyecto | Backlog, sprint actual, progreso, decisiones de ejecución |

**Estructura de `SPRINT.md`:**
```
## Sprint [N] — [fechas]
## Objetivo del sprint
## Backlog (tabla: tarea | owner | estado | notas)
## Decisiones tomadas
## Bloqueos activos
## Criterios de cierre del sprint
```

---

## Gate de aprobación

**Condición para avanzar a Fase 04b:**
- [ ] Sprint completado (todos los criterios de cierre cumplidos)
- [ ] `SPRINT.md` actualizado con estado final
- [ ] PM aprueba el output de ejecución
- [ ] `apex.config.json` actualizado: `currentPhase → 04b-auditoria`

---

## Recomendación Caveman

Activar **`caveman full`** al inicio de esta fase — velocidad máxima durante ejecución técnica.

---

## Qué escribir a memoria al cerrar

**`patterns.md`:**
- Stack detectado + agentes más útiles en este tipo de proyecto
- Tareas que más se expandieron vs estimación inicial
- Bloqueos recurrentes en este tipo de proyecto

**`pm-profile.md`:**
- ¿El PM hizo micromanagement o delegó completamente?
- Frecuencia de checkpoints pedidos
- ¿Prefirió worktrees o trabajo en rama directa?
