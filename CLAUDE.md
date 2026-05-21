# APEX — Sistema de gestión de proyectos con IA

Skill universal para Claude Code. Integra APEX Framework, Superpowers y Caveman
en un único punto de entrada para iniciar y gestionar cualquier tipo de proyecto.

## Instalación

```bash
git clone https://github.com/DMGYE/apex ~/.claude/skills/apex
```

## Uso

```
/apex                   # inicia o retoma el proyecto en el directorio actual
```

## Dependencias recomendadas

- **Superpowers** (obra/superpowers) — requerido para `/brainstorming`, `/writing-plans` y `/using-git-worktrees` en Fases 01 y 04
- **Caveman** (JuliusBrussee/caveman) — requerido para cavecrew agents en Fases 04 y 04b

Sin estas dependencias, APEX opera en **modo degradado**:
- Fases 01–03 y 04b–05 completamente funcionales
- Fase 04: sin worktrees ni cavecrew agents; APEX genera el plan directamente
- Fase 04b: sin `caveman:caveman-review`; review ejecutado por Claude directamente

## Fases

| Fase | Nombre | Output |
|------|--------|--------|
| 01 | Inicio | `PROJECT.md` |
| 02 | Investigación | `RESEARCH.md` |
| 03 | Estrategia | `STRATEGY.md` |
| 04 | Ejecución | `SPRINT.md` |
| 04b | Auditoría | entrada en `LOG.md` |
| 05 | Cierre | `CLOSE.md` + entregable final |

## Créditos

| Framework | Autor | Repo |
|-----------|-------|------|
| APEX Framework | Luis Zúñiga | github.com/DMGYE |
| Superpowers | Jesse Vincent | github.com/obra/superpowers |
| Caveman | Julius Brussee | github.com/JuliusBrussee/caveman |
