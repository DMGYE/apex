# APEX

Sistema de gestión de proyectos con IA para Claude Code. Integra **APEX Framework** (governance y fases), **Superpowers** (brainstorming y ejecución) y **Caveman** (comunicación comprimida) en un único skill instalable.

---

## Instalación

```bash
git clone https://github.com/DMGYE/apex ~/.claude/skills/apex
```

### Dependencias recomendadas

```bash
# Superpowers — brainstorming + writing-plans + worktrees
git clone https://github.com/obra/superpowers ~/.claude/skills/superpowers

# Caveman — terse mode + cavecrew agents
git clone https://github.com/JuliusBrussee/caveman ~/.claude/skills/caveman
```

Sin dependencias, APEX opera en modo degradado (ver abajo).

---

## Uso

Desde cualquier directorio de proyecto:

```
/apex
```

APEX detecta automáticamente si el proyecto es **nuevo** (no hay `apex.config.json`) o **existente** (retoma la fase activa).

---

## Proyecto nuevo

Al invocar `/apex` por primera vez en un directorio, APEX hace una entrevista en 3 grupos:

1. **Identidad** — nombre, tipo, industria
2. **Equipo** — PM, roles, cliente
3. **Alcance** — objetivo, restricciones, definición de éxito

Luego genera:
- `apex.config.json` — configuración del proyecto
- `PROJECT.md` — visión, objetivo, OKRs
- `TEAM.md` — equipo y stakeholders
- `LOG.md` — trazabilidad desde el primer día

Y arranca **Fase 01 — Inicio**.

---

## Retomar un proyecto

Si `apex.config.json` existe, APEX lee la fase activa y resume desde el último checkpoint:

```
Retomando Fase [N] — [nombre].
Último checkpoint: [item de inProgress]
¿Continuamos desde aquí?
```

---

## Fases

| Fase | Nombre | Agentes | Output |
|------|--------|---------|--------|
| 01 | Inicio | product-manager, business-analyst | `PROJECT.md` |
| 02 | Investigación | market-researcher, competitive-analyst, trend-analyst | `RESEARCH.md` |
| 03 | Estrategia | product-strategist, risk-manager, research-synthesizer | `STRATEGY.md` |
| 04 | Ejecución | project-manager, workflow-orchestrator + cavecrew | `SPRINT.md` |
| 04b | Auditoría | — (Claude directo con skills) | entrada en `LOG.md` |
| 05 | Cierre | report-generator, research-synthesizer, data-analyst | `CLOSE.md` |

---

## Memoria

APEX aprende de cada proyecto y adapta su comportamiento:

- `~/.claude/skills/apex/pm-profile.md` — perfil del PM (velocidad, detalle, preferencias)
- `~/.claude/skills/apex/patterns.md` — patrones cross-proyecto (agentes útiles, decisiones frecuentes)

Los archivos de memoria se crean automáticamente en la primera sesión y se actualizan al cerrar cada fase.

---

## Modo degradado (sin dependencias)

| Dependencia ausente | Funcionalidad afectada |
|--------------------|----------------------|
| Sin Superpowers | Fase 01: sin `/brainstorming` hard-gate. Fase 04: sin worktrees ni `/writing-plans`. |
| Sin Caveman | Fase 04/04b: sin cavecrew agents ni compresión de tokens. |
| Sin ambas | Fases 01–03 y 04b–05 completamente funcionales. |

---

## Tipos de proyecto soportados

| Tipo | Descripción |
|------|-------------|
| `software` | Apps, APIs, pipelines, sistemas técnicos |
| `strategy` | Estrategia de negocio, consultoría, planificación |
| `marketing` | Campañas, lanzamientos, posicionamiento |
| `mixed` | Combinación de tecnología y estrategia |

---

## Estructura del repo

```
apex/
├── SKILL.md                  ← skill principal (lógica de orquestación)
├── CLAUDE.md                 ← descripción, créditos, instalación
├── README.md                 ← esta guía
├── phases/
│   ├── 01-inicio.md
│   ├── 02-investigacion.md
│   ├── 03-estrategia.md
│   ├── 04-ejecucion.md
│   ├── 04b-auditoria.md
│   └── 05-cierre.md
├── memory/
│   ├── pm-profile.md         ← template (se instala en ~/.claude/skills/apex/)
│   └── patterns.md           ← template (se instala en ~/.claude/skills/apex/)
└── templates/
    ├── apex.config.json
    ├── PROJECT.md
    ├── TEAM.md
    └── LOG.md
```

---

## Créditos

| Framework | Autor | Repo |
|-----------|-------|------|
| APEX Framework | Luis Zúñiga | [github.com/DMGYE](https://github.com/DMGYE) |
| Superpowers | Jesse Vincent | [github.com/obra/superpowers](https://github.com/obra/superpowers) |
| Caveman | Julius Brussee | [github.com/JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |
