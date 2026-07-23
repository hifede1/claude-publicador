---
description: "Pre-flight completo del repo (historia sin secretos, metadata, nombre disponible) y publicación firmada en GitHub / npm / marketplace — con UN check rojo no se publica nada"
argument-hint: "[destino: github | npm | marketplace — opcional]"
---

# /publicar — Pre-flight y publicación firmada

> ⚠️ **ESQUELETO (v0.1.0 — S01).** El comando existe y es instalable; el pre-flight (S02) y la
> ejecución (S03/S04) todavía NO están construidos. Este placeholder NO ejecuta ninguna acción:
> ni lee historia, ni crea repos, ni publica nada. Su único trabajo hoy es declarar el contrato.

Cuando esté construido (plan S02–S05, issues #2–#5 del repo), este comando va a:

**Fase 1 — Pre-flight (solo lectura):**
- Historia de git SIN secretos ni archivos personales — **TODOS los commits, no solo HEAD** (motor: gitleaks + checks caseros del taller).
- README y LICENSE presentes; metadata completa (package.json / plugin.json).
- Nombre disponible en el registry destino; cuenta/scope correctos; `.gitignore` sano.

**Fase 2 — Informe:** checklist verde/rojo con evidencia trazable. **Con UN check rojo no se publica nada.**

**Fase 3 — Ejecución (solo con firma humana explícita):** crear repo, push, publish, verificación post-publicación en el registry. Cada paso irreversible se confirma. **El silencio no es aprobación.**

## Qué hacer hoy

Informá al usuario que el publicador está en construcción (S01 cerrada: esqueleto instalable) y
que el contrato completo vive en `docs/FICHA.md` del repo `hifede1/claude-publicador`. Si el
usuario necesita publicar YA, decile explícitamente que lo haga a mano con el checklist de la
Fase 1 como guía — y que cada mina que encuentre alimente los issues del repo.

**No ejecutes ninguna acción de escritura ni de lectura de historia. Este comando aún no verifica nada — decirlo claro es parte del contrato: jamás un verde falso.**
