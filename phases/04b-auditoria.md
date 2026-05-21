# Fase 04b — Auditoría

## Contexto

Revisión sistemática antes de cerrar o entregar. Detecta hallazgos críticos — vulnerabilidades, inconsistencias, gaps de calidad — y los documenta con resolución en `LOG.md`. Sin aprobación del PM sobre los hallazgos, no se avanza a Fase 05.

**Aplica a**: todos los tipos de proyecto (scope varía según tipo)

---

## Entrevista

### Grupo A — Alcance de la auditoría
1. ¿Hay áreas de especial riesgo que priorizar? (seguridad, performance, consistencia con spec)
2. ¿Hay reviewers externos o stakeholders que deban ver el output antes de Fase 05?
3. ¿Cuál es el criterio de "hallazgo crítico" en este proyecto? (¿qué bloquea el cierre?)

---

## Flujo de auditoría

APEX ejecuta la auditoría directamente con los skills — no despacha agents en esta fase.

### Tipo `software` / `mixed`

Ejecutar en orden:

1. **`security-review`** — vulnerabilidades, secrets expuestos, permisos, OWASP Top 10
2. **`code-reviewer`** — calidad de código, antipatterns, cobertura de tests, deuda técnica
3. **`senior-qa`** — estrategia de tests, edge cases no cubiertos, regresiones posibles
4. **`caveman:caveman-review`** (si Caveman disponible) — review del diff final antes de merge

**Reglas de seguridad obligatorias (siempre verificar):**
- Sin secrets, tokens ni contraseñas hardcodeados en código
- `.env` y archivos de credenciales en `.gitignore` y nunca commiteados
- Sin force push a `main`/`master`
- Sin `--no-verify` en hooks sin justificación explícita

### Tipo `strategy` / `marketing`

Ejecutar:

1. **`review`** — consistencia del entregable vs spec original (PROJECT.md + STRATEGY.md)
   - ¿El entregable responde a los OKRs definidos en Fase 01?
   - ¿Hay secciones incompletas o contradicciones internas?
   - ¿Los supuestos críticos documentados en STRATEGY.md se sostienen?

---

## Clasificación de hallazgos

| Severidad | Definición | Acción |
|-----------|------------|--------|
| **Crítico** | Bloquea el cierre — vulnerabilidad de seguridad, incumplimiento de spec | Resolver antes de continuar |
| **Alto** | Degradación significativa de calidad o riesgo operacional | Resolver o documentar deuda con plan |
| **Medio** | Mejora recomendada, no bloquea | Documentar en LOG.md para siguiente iteración |
| **Bajo** | Nit, estilo, sugerencia | Opcional — a criterio del PM |

---

## Output

No genera un documento nuevo — **agrega una entrada a `LOG.md`**:

```markdown
## Auditoría — Fase 04b — [fecha]

### Hallazgos críticos
- [descripción] → [resolución / commit]

### Hallazgos altos
- [descripción] → [resolución / deuda documentada]

### Hallazgos medios (próxima iteración)
- [descripción]

### Aprobación
- [ ] Críticos resueltos
- [ ] PM aprueba cierre de auditoría → avanzar a Fase 05
```

---

## Gate de aprobación

**Condición para avanzar a Fase 05:**
- [ ] Todos los hallazgos críticos resueltos y documentados en LOG.md
- [ ] Hallazgos altos resueltos o con deuda documentada y plan
- [ ] PM aprueba explícitamente el cierre de la auditoría
- [ ] `apex.config.json` actualizado: `currentPhase → 05-cierre`

---

## Recomendación Caveman

Mantener **`caveman full`** activo — los reviews deben ser directos y sin relleno.

---

## Qué escribir a memoria al cerrar

**`patterns.md`:**
- Tipo de hallazgos más frecuentes en este tipo de proyecto (seguridad / calidad / consistencia)
- Severidades promedio encontradas
- ¿La auditoría descubrió algo que debería haberse detectado antes?

**`pm-profile.md`:**
- Tolerancia del PM ante hallazgos altos (¿los resolvió o los documentó como deuda?)
- ¿El PM cuestionó alguna clasificación de severidad?
