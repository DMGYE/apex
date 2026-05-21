# Fase 05 — Cierre

## Contexto

Produce el entregable final, documenta lecciones aprendidas y cierra formalmente el proyecto o la iteración. El output varía según el tipo de proyecto: presentación, informe, retrospectiva técnica o combinación.

---

## Entrevista

### Grupo A — Entregable final
1. ¿Qué formato requiere el entregable final? (presentación / informe / documento técnico / dashboard)
2. ¿A quién va dirigido? (cliente, directivos, equipo técnico, todos)
3. ¿Hay fecha o evento específico para la entrega?

### Grupo B — Retrospectiva
4. ¿Qué salió mejor de lo esperado en este proyecto?
5. ¿Qué haría diferente en la próxima iteración?
6. ¿Hay algo que quedó pendiente y debe pasar a `inProgress` o próxima fase?

---

## Agents a despachar

| Agent | Input | Output esperado |
|-------|-------|----------------|
| `report-generator` | Todos los docs del proyecto + Grupo A | Draft del entregable final en formato solicitado |
| `research-synthesizer` | Todos los docs + LOG.md | Síntesis de lecciones aprendidas |
| `data-analyst` | Métricas y resultados del proyecto (si aplica) | Análisis cuantitativo de resultados |

`report-generator` y `research-synthesizer` en paralelo. `data-analyst` solo si hay datos cuantitativos.

---

## Skills a invocar (según entregable)

| Skill | Cuándo | Output |
|-------|--------|--------|
| `anthropic-skills:pptx` | Entregable es presentación | Archivo .pptx |
| `anthropic-skills:docx` | Entregable es informe formal | Archivo .docx |
| `content-research-writer` | Entregable es documento narrativo largo | Markdown / doc estructurado |

---

## Output

| Documento | Path | Contenido |
|-----------|------|-----------|
| `CLOSE.md` | raíz del proyecto | Resumen ejecutivo, resultados vs OKRs, lecciones, pendientes |
| Entregable final | según formato acordado | Presentación / informe / doc técnico |

**Estructura de `CLOSE.md`:**
```
## Resumen ejecutivo
## Resultados vs OKRs (tabla)
## Fases completadas (timeline)
## Lecciones aprendidas (qué salió bien / qué cambiaría)
## Pendientes para próxima iteración
## Cierre formal (fecha, aprobado por)
```

---

## Gate de aprobación (cierre del proyecto)

- [ ] PM aprueba `CLOSE.md` y el entregable final
- [ ] Retrospectiva completada (Grupo B documentado)
- [ ] Pendientes capturados en `apex.config.json → inProgress` o nuevo ciclo
- [ ] `apex.config.json` actualizado: `status → closed`, `currentPhase → done`

---

## Actualización de memoria al cerrar el proyecto

**`patterns.md`** — actualización completa:
- Tipo de proyecto, industria, duración total
- Agentes más útiles por fase
- Documentos que más iteraron
- Decisiones más comunes tomadas

**`pm-profile.md`** — actualización completa:
- Incrementar `projects_count`
- Agregar proyecto a `proyectos[]` con tipo, industria, última fase, aprendizaje clave
- Refinar `velocidad`, `enfoque`, `acepta_recomendaciones` según comportamiento observado en todo el proyecto
- Actualizar `fase_mas_iterada` si aplica

---

## Recomendación Caveman

Desactivar Caveman para esta fase — los entregables finales son documentos formales que requieren tono normal.
