# APEX Skill — Implementation Design

**Goal:** Skill universal instalable desde GitHub que integra APEX (governance), Superpowers (ejecución) y Caveman (comunicación) en un único punto de entrada para gestionar proyectos desde Claude Code.

**Architecture:** Skill maestro en hilo principal que orquesta agents por fase. Una fase por sesión. Estado persistido en `apex.config.json` por proyecto + memoria global del PM.

**Tech Stack:** SKILL.md + phase files + memory templates + config templates. Sin dependencias de runtime. Superpowers y Caveman son dependencias recomendadas (modo degradado sin ellos).

---

## Créditos

- **APEX Framework** — diseño de fases, governance y trazabilidad · Luis Zúñiga · github.com/DMGYE
- **Superpowers** — brainstorming → writing-plans → worktrees · Jesse Vincent · github.com/obra/superpowers
- **Caveman** — estilo de comunicación y cavecrew agents · Julius Brussee · github.com/JuliusBrussee/caveman

---

## 1. Estructura del repo `DMGYE/apex`

```
apex/
├── SKILL.md                        ← skill principal + lógica de orquestación
├── CLAUDE.md                       ← descripción, créditos, dependencias
├── README.md                       ← guía de instalación
├── phases/
│   ├── 01-inicio.md
│   ├── 02-investigacion.md
│   ├── 03-estrategia.md
│   ├── 04-ejecucion.md
│   ├── 04b-auditoria.md
│   └── 05-cierre.md
├── memory/
│   ├── pm-profile.md               ← template vacío — se instala en ~/.claude/skills/apex/
│   └── patterns.md                 ← template vacío — se instala en ~/.claude/skills/apex/
└── templates/
    ├── apex.config.json
    ├── PROJECT.md
    ├── TEAM.md
    └── LOG.md
```

**Memoria global** (fuera del repo, en máquina del usuario):
```
~/.claude/skills/apex/
├── pm-profile.md                   ← perfil PM — persiste entre todos los proyectos
└── patterns.md                     ← patrones aprendidos cross-proyecto
```

---

## 2. SKILL.md — Lógica principal

### Flujo de inicio (cada invocación)

```
1. Leer ~/.claude/skills/apex/pm-profile.md (si existe)
2. Leer ~/.claude/skills/apex/patterns.md (si existe)
3. Buscar apex.config.json en el proyecto actual
   ├── Existe  → MODO RETOMAR: leer currentPhase → ejecutar fase activa
   └── No existe → MODO NUEVO: ejecutar entrevista de proyecto → crear docs base → Fase 01
4. Al cerrar sesión → escribir aprendizajes a memoria
```

### Modo PROYECTO NUEVO — entrevista en 3 grupos

Esperar respuesta completa de cada grupo antes de continuar.

**Grupo 1 — Identidad**
1. Nombre del proyecto
2. Tipo: `software` / `strategy` / `marketing` / `mixed`
3. Industria y contexto general

**Grupo 2 — Equipo y stakeholders**
4. Nombre del PM (quién aprueba decisiones)
5. Equipo disponible (roles, no personas necesariamente)
6. Cliente o destinatario final del proyecto

**Grupo 3 — Alcance y éxito**
7. Objetivo principal en una línea
8. Restricciones conocidas (tiempo, presupuesto, tecnología)
9. Cómo se ve el éxito al terminar

→ Genera: `apex.config.json` + `PROJECT.md` + `TEAM.md` + `LOG.md` (primera entrada)
→ Arranca Fase 01 directamente

### Modo RETOMAR

```
1. Lee apex.config.json → currentPhase + inProgress + completedInPlanning
2. Lee pm-profile.md → adapta tono y nivel de detalle
3. Resume con contexto:
   "Retomando Fase [N] — [nombre]. Último checkpoint: [último item de inProgress].
    ¿Continuamos desde aquí?"
```

### Adaptación por pm-profile

| Perfil detectado | Comportamiento |
|-----------------|---------------|
| velocidad: rápido | Da recomendación directa, menos opciones |
| velocidad: deliberado | Presenta 3 opciones con trade-offs |
| acepta_recomendaciones: raramente | Presenta opciones sin sesgar hacia ninguna |
| nivel_detalle: alto | Expande explicaciones y ejemplos |
| nivel_detalle: bajo | Solo puntos clave, sin contexto extra |

---

## 3. Fases — estructura de cada archivo

Cada `phases/0X-*.md` sigue este esquema:

```
CONTEXTO     — qué resuelve esta fase, qué doc produce
ENTREVISTA   — preguntas específicas (grupos secuenciales, una a la vez)
AGENTS       — cuáles despachar, qué input reciben, qué output se espera
SKILLS       — cuáles invocar y en qué momento exacto
OUTPUT       — doc a generar, path, formato
GATE         — condición de aprobación para avanzar a la siguiente fase
APRENDIZAJE  — qué observar y escribir a memoria al cerrar
```

### Fase 01 — Inicio
- **Doc generado**: `PROJECT.md`
- **Agents**: `product-manager`, `business-analyst`
- **Skills**: `brainstorming` (hard-gate — no avanza sin spec aprobada)
- **Gate**: PM aprueba PROJECT.md + spec de brainstorming

### Fase 02 — Investigación
- **Doc generado**: `RESEARCH.md`
- **Agents**: `market-researcher`, `competitive-analyst`, `trend-analyst` (paralelo)
- **Skills**: `marketing-strategy-pmm`, `senior-data-scientist` (si aplica)
- **Gate**: PM aprueba síntesis de investigación

### Fase 03 — Estrategia
- **Doc generado**: `STRATEGY.md`
- **Agents**: `product-strategist`, `risk-manager`, `research-synthesizer`
- **Skills**: `pricing-strategy`, `launch-strategy`, `ceo-advisor` (según tipo de proyecto)
- **Gate**: PM aprueba roadmap + risk register

### Fase 04 — Ejecución

Bifurcación por tipo de proyecto:

**strategy / marketing:**
- Skills: `agile-product-owner`, `writing-plans`
- Agents: `project-manager`, `workflow-orchestrator`
- Doc: `SPRINT.md`

**software / mixed:**
- Skills: `writing-plans` → `using-git-worktrees` → `senior-[stack]`
- Agents: `project-manager`, `workflow-orchestrator` + cavecrew agents
- Caveman: full activado
- Doc: `SPRINT.md` + plan en `docs/superpowers/plans/`

**Stack detection (software):**
```
Lee PROJECT.md → detecta keywords →
  Next.js / React      → senior-frontend
  Python / FastAPI     → senior-backend + senior-data-engineer
  Docker / infra       → senior-devops
  AI / LLM / Claude    → senior-prompt-engineer
  No detectado         → pregunta al PM
```

### Fase 04b — Auditoría

- **Doc generado**: entrada en `LOG.md` (hallazgos + resolución)
- **Aplica a**: todos los tipos de proyecto (scope varía)
- **Agents**: ninguno — la auditoría la ejecuta Claude directamente con los skills
- **Skills (software/mixed)**:
  - `security-review` — vulnerabilidades, secrets, permisos, OWASP
  - `code-reviewer` — calidad, antipatterns, cobertura
  - `senior-qa` — estrategia de tests, edge cases
  - `caveman:caveman-review` — review de diff final antes de merge
- **Skills (strategy/marketing)**:
  - `review` — consistencia del entregable vs spec original
- **Gate**: todos los hallazgos críticos resueltos → PM aprueba → Fase 05
- **Aprendizaje**: tipo de hallazgos más frecuentes por tipo de proyecto

### Fase 05 — Cierre
- **Doc generado**: `CLOSE.md`
- **Agents**: `report-generator`, `research-synthesizer`, `data-analyst`
- **Skills**: `anthropic-skills:pptx`, `anthropic-skills:docx`, `content-research-writer` (según entregable)
- **Gate**: PM aprueba entregable final + retrospectiva

---

## 4. Memoria

### pm-profile.md (global)

```yaml
---
pm_name: ""
last_updated: ""
projects_count: 0
---

## Estilo de decisión
velocidad: ""           # rapido | deliberado | mixto
enfoque: ""             # datos | intuicion | mixto
acepta_recomendaciones: ""  # siempre | frecuente | raramente

## Preferencias de trabajo
nivel_detalle_docs: ""  # alto | medio | bajo
preguntas_frecuentes: []
fase_mas_iterada: ""

## Historial de proyectos
# - tipo: software | strategy | marketing | mixed
#   industria: ""
#   ultima_fase_completada: ""
#   aprendizaje_clave: ""
proyectos: []
```

### patterns.md (global)

```yaml
---
last_updated: ""
---

## Por tipo de proyecto
software:
  agents_mas_utiles: []
  docs_que_mas_iteran: []
  decisiones_comunes: []

strategy:
  agents_mas_utiles: []
  fase_mas_compleja: ""
  decisiones_comunes: []

marketing:
  agents_mas_utiles: []
  fase_mas_compleja: ""

mixed:
  agents_mas_utiles: []
  fase_mas_compleja: ""
```

### Cuándo escribe APEX a memoria

- **Al cerrar cada fase** → actualiza `patterns.md` con agents usados + docs iterados
- **Al final de sesión** → actualiza `pm-profile.md` con observaciones de velocidad, detalle, aceptación
- **Al detectar patrón (3+ ocurrencias)** → lo registra explícitamente como patrón confirmado

---

## 5. Integración Superpowers

| Fase | Skill | Momento exacto |
|------|-------|---------------|
| 01 Inicio | `brainstorming` | Al definir primera feature/workstream — APEX lo invoca, no lo reemplaza |
| 04 Ejecución | `writing-plans` | Después de aprobar diseño de brainstorming |
| 04 Ejecución | `using-git-worktrees` | Antes de arrancar implementación (solo tipo software/mixed) |
| 04b Auditoría | `caveman:caveman-review` | Review del diff final antes de merge (solo tipo software/mixed) |

**Regla crítica**: Superpowers mantiene sus propios hard-gates. APEX invoca `brainstorming` — el PM pasa por el flujo completo de Superpowers. APEX espera que `brainstorming` termine antes de continuar.

---

## 6. Integración Caveman

Caveman no se activa automáticamente — APEX lo recomienda por fase:

| Fase | Recomendación |
|------|--------------|
| 01-03 Estrategia/diseño | `caveman lite` — hay que pensar en voz alta |
| 04 Ejecución técnica | `caveman full` — velocidad máxima |
| 05 Cierre/entregables | modo normal — docs formales |

Los cavecrew agents (`builder`, `investigator`, `reviewer`) están disponibles en Fase 04 sin invocación especial — son parte del toolkit de ejecución técnica.

---

## 7. CLAUDE.md del repo

```markdown
# APEX — Sistema de gestión de proyectos con IA

Skill universal para Claude Code. Integra APEX Framework, Superpowers y Caveman
en un único punto de entrada para iniciar y gestionar cualquier tipo de proyecto.

## Instalación

git clone https://github.com/DMGYE/apex ~/.claude/skills/apex

## Uso

/apex                   # inicia o retoma el proyecto en el directorio actual

## Dependencias recomendadas

- Superpowers (obra/superpowers) — requerido para Fases 01 y 04
- Caveman (JuliusBrussee/caveman) — requerido para cavecrew agents en Fase 04

Sin estas dependencias, APEX opera en modo degradado: Fases 01-03 y 05 completas,
Fase 04 sin worktrees ni cavecrew agents.

## Créditos

APEX Framework       Luis Zúñiga          github.com/DMGYE
Superpowers          Jesse Vincent        github.com/obra/superpowers
Caveman              Julius Brussee       github.com/JuliusBrussee/caveman
```

---

## 8. Decisiones de diseño tomadas

| Decisión | Alternativa descartada | Razón |
|----------|----------------------|-------|
| Una fase por sesión | Wizard lineal completo | Las fases duran días/semanas, no una sesión |
| File-based memory | Obsidian vault | Cero dependencias, trazable en git, Claude consume MD directamente |
| Repo independiente | Dentro de The-Room | Reutilizable en cualquier proyecto fuera del ecosistema MABECA |
| Superpowers como dependencia | Reimplementar brainstorming | No duplicar — invocar el original con sus propios hard-gates |
| Caveman como recomendación, no auto-activación | Auto-activar por fase | Caveman es personal/global, no decisión del skill |

---

## 9. Pendientes antes de implementar

- [ ] Crear repo `DMGYE/apex` en GitHub
- [ ] Definir estructura de `marketplace.json` para instalación via Claude Code marketplace
- [ ] Confirmar si Superpowers tiene mecanismo de invocación desde otro skill o solo desde usuario
- [ ] Decidir versioning del skill (semver vs fecha)
