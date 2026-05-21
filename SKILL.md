---
name: apex
description: "Sistema de gestión de proyectos con IA. Integra APEX Framework (governance), Superpowers (ejecución) y Caveman (comunicación) en un único punto de entrada. Detecta si el proyecto es nuevo o existente y ejecuta la fase activa. Una fase por sesión."
---

# APEX — Sistema de gestión de proyectos con IA

Skill universal para Claude Code. Orquesta agentes, skills e interacción con el PM para guiar cualquier proyecto a través de sus fases, mantener trazabilidad y aprender de cada sesión.

---

## Flujo de inicio (cada invocación)

Al invocar `/apex`:

1. Leer `~/.claude/skills/apex/pm-profile.md` — si existe, cargar perfil del PM
2. Leer `~/.claude/skills/apex/patterns.md` — si existe, cargar patrones cross-proyecto
3. Buscar `apex.config.json` en el directorio actual:
   - **Existe** → **MODO RETOMAR**: leer `currentPhase` → ejecutar fase activa
   - **No existe** → **MODO NUEVO**: ejecutar entrevista → crear docs base → iniciar Fase 01
4. Ejecutar la fase activa (ver `phases/0X-*.md`)
5. Al cerrar la sesión → escribir aprendizajes a memoria global

---

## MODO NUEVO — Entrevista de proyecto

Esperar respuesta completa de cada grupo antes de pasar al siguiente.

### Grupo 1 — Identidad
1. ¿Cuál es el nombre del proyecto?
2. ¿Qué tipo de proyecto es? `software` / `strategy` / `marketing` / `mixed`
3. ¿Cuál es la industria y el contexto general?

### Grupo 2 — Equipo y stakeholders
4. ¿Quién es el PM (la persona que aprueba las decisiones)?
5. ¿Con qué equipo se cuenta? (roles, no necesariamente personas)
6. ¿Quién es el cliente o destinatario final del proyecto?

### Grupo 3 — Alcance y éxito
7. ¿Cuál es el objetivo principal en una línea?
8. ¿Qué restricciones existen? (tiempo, presupuesto, tecnología)
9. ¿Cómo se ve el éxito al terminar?

**Al completar los 3 grupos, generar:**
- `apex.config.json` — configuración del proyecto (usar `templates/apex.config.json` como base)
- `PROJECT.md` — visión, objetivo, restricciones, OKRs (usar `templates/PROJECT.md`)
- `TEAM.md` — PM, equipo, stakeholders, cliente (usar `templates/TEAM.md`)
- `LOG.md` — primera entrada con fecha y kickoff (usar `templates/LOG.md`)

Luego arrancar directamente **Fase 01** (ver `phases/01-inicio.md`).

---

## MODO RETOMAR

1. Leer `apex.config.json` → extraer `currentPhase`, `inProgress`, `completedPhases`
2. Leer `pm-profile.md` → adaptar tono y nivel de detalle
3. Resumir con contexto:

```
Retomando Fase [N] — [nombre].
Completadas: [lista de fases completadas]
En progreso: [último item de inProgress]

¿Continuamos desde aquí?
```

4. Ejecutar la fase activa cargando su archivo `phases/0X-*.md`

---

## Adaptación por pm-profile

| Perfil detectado | Comportamiento |
|-----------------|----------------|
| `velocidad: rapido` | Recomendación directa, menos opciones |
| `velocidad: deliberado` | Presenta 3 opciones con trade-offs |
| `acepta_recomendaciones: raramente` | Presenta opciones sin sesgar hacia ninguna |
| `nivel_detalle: alto` | Expande explicaciones y ejemplos |
| `nivel_detalle: bajo` | Solo puntos clave, sin contexto extra |
| `enfoque: datos` | Justificaciones con métricas y evidencia |
| `enfoque: intuicion` | Narrativa y razonamiento cualitativo |

---

## Memoria — cuándo y qué escribir

APEX mantiene dos archivos en `~/.claude/skills/apex/`:

### `pm-profile.md`
Actualizar **al final de cada sesión** con observaciones sobre:
- Velocidad de decisión observada en esta sesión
- Nivel de detalle solicitado
- Frecuencia de aceptación de recomendaciones
- Preguntas recurrentes

### `patterns.md`
Actualizar **al cerrar cada fase** con:
- Agentes que resultaron más útiles para este tipo de proyecto
- Documentos que más se iteraron antes de aprobación
- Decisiones comunes tomadas en esta fase/tipo

**Regla de patrón confirmado**: si un comportamiento ocurre 3+ veces en proyectos distintos → registrarlo explícitamente como patrón confirmado.

### `apex.config.json` del proyecto
Actualizar al cierre de cada fase:
- Mover la fase completada a `completedPhases`
- Actualizar `currentPhase` a la siguiente
- Registrar aprobación en `approvals`
- Actualizar `lastRun`

---

## Integración Superpowers

Superpowers mantiene sus propios hard-gates. APEX lo invoca — el PM pasa por el flujo completo. APEX **espera** que Superpowers termine antes de continuar.

| Fase | Skill | Momento exacto |
|------|-------|----------------|
| 01 Inicio | `/brainstorming` | Al definir primera feature/workstream — APEX lo invoca, no lo reemplaza |
| 04 Ejecución | `/writing-plans` | Después de aprobar el diseño del brainstorming |
| 04 Ejecución | `/using-git-worktrees` | Antes de arrancar implementación (solo `software`/`mixed`) |
| 04b Auditoría | `caveman:caveman-review` | Review del diff final antes de merge (solo `software`/`mixed`) |

**Modo degradado** (sin Superpowers): Fases 01-03 y 04b-05 completas. Fase 04 sin worktrees — APEX genera el plan de ejecución directamente.

---

## Integración Caveman

Caveman no se activa automáticamente — APEX lo recomienda por fase:

| Fase | Recomendación |
|------|--------------|
| 01–03 Estrategia/diseño | `caveman lite` — hay que pensar en voz alta |
| 04 Ejecución técnica | `caveman full` — velocidad máxima |
| 04b Auditoría | `caveman full` — reviews concisos y directos |
| 05 Cierre/entregables | modo normal — docs formales |

Los cavecrew agents (`caveman:cavecrew-builder`, `caveman:cavecrew-investigator`, `caveman:cavecrew-reviewer`) están disponibles en Fase 04 y 04b sin invocación especial.

**Modo degradado** (sin Caveman): misma funcionalidad, sin compresión de tokens ni cavecrew agents. Los agents equivalentes (`claude` / `general-purpose`) reemplazan a los cavecrew.

---

## Créditos

| Framework | Autor | Repo |
|-----------|-------|------|
| APEX Framework | Luis Zúñiga | github.com/DMGYE/apex |
| Superpowers | Jesse Vincent | github.com/obra/superpowers |
| Caveman | Julius Brussee | github.com/JuliusBrussee/caveman |
