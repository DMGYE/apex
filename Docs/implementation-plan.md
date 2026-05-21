# APEX Skill — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Crear el repo `DMGYE/apex` con el skill `/apex` — integración de APEX Framework, Superpowers y Caveman en un único skill universal instalable desde GitHub.

**Architecture:** SKILL.md maestro + 5 phase files + memory templates + config templates. Sin dependencias de runtime. Superpowers y Caveman son dependencias recomendadas (modo degradado sin ellas).

**Spec:** `docs/superpowers/specs/2026-05-20-apex-skill-design.md`

---

## Archivos a crear

```
(repo nuevo: DMGYE/apex — directorio de trabajo: crear local primero)

apex/
├── SKILL.md
├── CLAUDE.md
├── README.md
├── phases/
│   ├── 01-inicio.md
│   ├── 02-investigacion.md
│   ├── 03-estrategia.md
│   ├── 04-ejecucion.md
│   ├── 04b-auditoria.md          ← NUEVO
│   └── 05-cierre.md
├── memory/
│   ├── pm-profile.md
│   └── patterns.md
└── templates/
    ├── apex.config.json
    ├── PROJECT.md
    ├── TEAM.md
    └── LOG.md
```

---

## Task 0: Actualizar spec con Fase 04b

**Files:**
- Modify: `docs/superpowers/specs/2026-05-20-apex-skill-design.md`

- [ ] **Step 1:** Agregar `04b-auditoria.md` a la estructura del repo (sección 1)
- [ ] **Step 2:** Agregar Fase 04b entre Fase 04 y Fase 05 (sección 3)
```
### Fase 04b — Auditoría

- **Doc generado**: entrada en LOG.md (hallazgos + resolución)
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
```
- [ ] **Step 3:** Agregar 04b a tabla de integración Superpowers (sección 5)
- [ ] **Step 4:** Actualizar `apex.config.json` template para incluir `04b-auditoria` en fases
- [ ] **Step 5:** Commit spec actualizada
```bash
git add docs/superpowers/specs/2026-05-20-apex-skill-design.md
git commit -m "docs(apex): agregar Fase 04b auditoría a spec"
```

---

## Task 1: Crear repo local + estructura de directorios

**Files:**
- Create: directorio `apex/` en ubicación a confirmar con usuario

- [ ] **Step 1:** Confirmar path local donde crear el repo
- [ ] **Step 2:** Crear estructura de directorios
```bash
mkdir -p apex/phases apex/memory apex/templates
cd apex && git init
```
- [ ] **Step 3:** Verificar estructura creada
```bash
find apex -type d
```

---

## Task 2: SKILL.md — skill principal

**Files:**
- Create: `apex/SKILL.md`

- [ ] **Step 1:** Escribir frontmatter + descripción
```yaml
---
name: apex
description: "Sistema de gestión de proyectos con IA. Integra APEX Framework (governance), Superpowers (ejecución) y Caveman (comunicación) en un único punto de entrada. Detecta si el proyecto es nuevo o existente y ejecuta la fase activa. Una fase por sesión."
---
```

- [ ] **Step 2:** Escribir sección FLUJO DE INICIO
```
## Flujo de inicio (cada invocación)

1. Leer `~/.claude/skills/apex/pm-profile.md` (si existe)
2. Leer `~/.claude/skills/apex/patterns.md` (si existe)
3. Buscar `apex.config.json` en directorio actual
   ├── Existe  → MODO RETOMAR
   └── No existe → MODO NUEVO
4. Ejecutar fase activa
5. Al cerrar sesión → escribir aprendizajes a memoria
```

- [ ] **Step 3:** Escribir sección MODO NUEVO (entrevista 3 grupos)
- [ ] **Step 4:** Escribir sección MODO RETOMAR
- [ ] **Step 5:** Escribir tabla ADAPTACIÓN POR PM-PROFILE
- [ ] **Step 6:** Escribir sección MEMORIA (cuándo y qué escribir)
- [ ] **Step 7:** Escribir sección INTEGRACIÓN SUPERPOWERS (tabla 3 puntos)
- [ ] **Step 8:** Escribir sección INTEGRACIÓN CAVEMAN (recomendaciones por fase)
- [ ] **Step 9:** Escribir sección CRÉDITOS

---

## Task 3: Phase files (6 archivos)

**Files:**
- Create: `apex/phases/01-inicio.md`
- Create: `apex/phases/02-investigacion.md`
- Create: `apex/phases/03-estrategia.md`
- Create: `apex/phases/04-ejecucion.md`
- Create: `apex/phases/04b-auditoria.md`
- Create: `apex/phases/05-cierre.md`

Cada archivo sigue el esquema:
```
# Fase 0X — [Nombre]

## Contexto
## Entrevista (grupos secuenciales)
## Agents a despachar
## Skills a invocar
## Output
## Gate de aprobación
## Qué escribir a memoria al cerrar
```

- [ ] **Step 1:** Crear `01-inicio.md`
  - Agents: `product-manager`, `business-analyst`
  - Skills: `brainstorming` (hard-gate)
  - Output: `PROJECT.md`
  - Gate: PM aprueba PROJECT.md + spec de brainstorming

- [ ] **Step 2:** Crear `02-investigacion.md`
  - Agents: `market-researcher`, `competitive-analyst`, `trend-analyst` (paralelo)
  - Skills: `marketing-strategy-pmm`, `senior-data-scientist` (si aplica)
  - Output: `RESEARCH.md`
  - Gate: PM aprueba síntesis

- [ ] **Step 3:** Crear `03-estrategia.md`
  - Agents: `product-strategist`, `risk-manager`, `research-synthesizer`
  - Skills: `pricing-strategy`, `launch-strategy`, `ceo-advisor` (según tipo)
  - Output: `STRATEGY.md`
  - Gate: PM aprueba roadmap + risk register

- [ ] **Step 4:** Crear `04-ejecucion.md`
  - Bifurcación por tipo (strategy/marketing vs software/mixed)
  - Stack detection keywords en PROJECT.md
  - Software: `writing-plans` → `using-git-worktrees` → `senior-[stack]` + cavecrew agents
  - Strategy: `agile-product-owner` + `writing-plans`
  - Output: `SPRINT.md`

- [ ] **Step 5:** Crear `04b-auditoria.md`
  - Aplica a todos los tipos, scope varía
  - Software/mixed: `security-review` + `code-reviewer` + `senior-qa` + `caveman:caveman-review`
  - Strategy/marketing: `review` (consistencia entregable vs spec)
  - Output: entrada en LOG.md con hallazgos + resolución
  - Gate: hallazgos críticos resueltos → PM aprueba → Fase 05

- [ ] **Step 6:** Crear `05-cierre.md`
  - Agents: `report-generator`, `research-synthesizer`, `data-analyst`
  - Skills: `anthropic-skills:pptx`, `anthropic-skills:docx`, `content-research-writer`
  - Output: `CLOSE.md`
  - Gate: PM aprueba entregable final + retrospectiva

---

## Task 4: Memory templates

**Files:**
- Create: `apex/memory/pm-profile.md`
- Create: `apex/memory/patterns.md`

- [ ] **Step 1:** Crear `pm-profile.md` con schema YAML completo (ver spec sección 4)
- [ ] **Step 2:** Crear `patterns.md` con schema YAML por tipo de proyecto

---

## Task 5: Templates de proyecto

**Files:**
- Create: `apex/templates/apex.config.json`
- Create: `apex/templates/PROJECT.md`
- Create: `apex/templates/TEAM.md`
- Create: `apex/templates/LOG.md`

- [ ] **Step 1:** `apex.config.json` — template con todos los campos vacíos/defaults
```json
{
  "version": "1.0.0",
  "project": "",
  "type": "software|strategy|marketing|mixed",
  "currentPhase": "01-inicio",
  "status": "active",
  "createdAt": "",
  "lastRun": "",
  "pm": "",
  "completedPhases": [],
  "inProgress": [],
  "approvals": {}
}
```

- [ ] **Step 2:** `PROJECT.md` — template con secciones: Visión, Objetivo, Restricciones, Definición de éxito, OKRs
- [ ] **Step 3:** `TEAM.md` — template con secciones: PM, Equipo, Stakeholders, Cliente
- [ ] **Step 4:** `LOG.md` — template con primera entrada de ejemplo y formato estándar

---

## Task 6: CLAUDE.md + README.md

**Files:**
- Create: `apex/CLAUDE.md`
- Create: `apex/README.md`

- [ ] **Step 1:** `CLAUDE.md` — contenido exacto de spec sección 7 (créditos, instalación, uso, modo degradado)
- [ ] **Step 2:** `README.md` — guía de instalación, requisitos, uso básico, descripción de fases

---

## Task 7: Primer commit + push a GitHub

- [ ] **Step 1:** `git add SKILL.md CLAUDE.md README.md phases/ memory/ templates/ && git commit -m "feat: initial APEX skill"`
- [ ] **Step 2:** Crear repo `DMGYE/apex` en GitHub (público)
- [ ] **Step 3:** `git remote add origin https://github.com/DMGYE/apex.git`
- [ ] **Step 4:** `git push -u origin main`
- [ ] **Step 5:** Verificar repo visible en GitHub

---

## Task 8: Instalación y smoke test

- [ ] **Step 1:** Instalar en entorno local
```bash
git clone https://github.com/DMGYE/apex ~/.claude/skills/apex
```
- [ ] **Step 2:** Crear proyecto de prueba vacío
```bash
mkdir /tmp/apex-test && cd /tmp/apex-test && git init
```
- [ ] **Step 3:** Invocar `/apex` — verificar que detecta proyecto nuevo y arranca entrevista Grupo 1
- [ ] **Step 4:** Verificar que al terminar la entrevista genera `apex.config.json` + `PROJECT.md` + `TEAM.md` + `LOG.md`
- [ ] **Step 5:** Invocar `/apex` segunda vez — verificar que detecta modo RETOMAR y carga fase activa

---

## Pendientes post-implementación (no bloquean)

- [ ] `marketplace.json` para instalación via Claude Code marketplace
- [ ] Versioning: decidir semver (recomendado `v1.0.0`) vs fecha
