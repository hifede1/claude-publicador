# Ficha de diseño: publicador

> Estado: 🔵 Lista para construir (decisiones firmadas por Fede el 2026-07-17)
> Diseñada: 2026-07-17 · Próxima sesión de construcción: S01 — Esqueleto

## 1. Propósito

**Problema que resuelve:** publicar una herramienta a mano es un campo minado — lo vivimos con n8n-mcp-server: config personal (`.obsidian`) metida en la historia de git, sin README ni LICENSE, remote roto, nombre tomado en npm, scope equivocado, 2FA. Cada publicación repite el gauntlet y cada mina que explota es pública.

**Para quién:** Fede, al publicar cualquier herramienta del taller (y cualquier proyecto en general).

**Una frase:** `publicador` verifica TODO antes de que algo salga al mundo, y recién con checks verdes y firma humana ejecuta la publicación.

## 2. Tipo y forma

**Plugin de Claude Code** con un comando: `/publicar`. Cierra el ciclo del taller: diseñar → construir → auditar → **publicar**.

## 3. Funciones

| Función | Entrada | Salida | Comportamiento |
|---|---|---|---|
| `/publicar` | Repo local listo para salir + destino (GitHub / npm / plugin marketplace) | Informe de pre-flight y, con firma humana, la publicación ejecutada | **Fase 1 — pre-flight (solo lectura):** historia de git sin secretos ni archivos personales (escanea TODOS los commits, no solo HEAD), README y LICENSE presentes, metadata completa (package.json / plugin.json), nombre disponible en el registry destino, cuenta/scope correctos, `.gitignore` sano. **Fase 2 — informe:** checklist verde/rojo con evidencia; con UN check rojo no se publica nada. **Fase 3 — ejecución (solo con firma):** crear repo, push, publish, verificación post-publicación en el registry. Cada paso irreversible se confirma. |

**Regla transversal:** jamás publica con checks rojos ni sin confirmación explícita — el silencio no es aprobación (heredado de `/orquestar`).

## 4. Decisiones técnicas

| Decisión | Elegido | Alternativas descartadas | Por qué |
|---|---|---|---|
| Modo por defecto | Dry-run (pre-flight + informe, sin ejecutar) | Publicar directo | Publicar es irreversible; lo irreversible se firma. |
| Alcance del scan de historia | Todos los commits | Solo HEAD | La lección de hoy: lo borrado de HEAD sigue en la historia. |
| **Registries v1** (firmada) | **GitHub + npm + marketplace de plugins de Claude Code** | Menos registries | Son las tres salidas que las herramientas del taller ya usan: GitHub para todo repo, npm para lo instalable por `npx` (n8n-mcp-server), marketplace para los plugins. Se formaliza lo existente, no se inventa. |
| **Scan de secretos** (firmada) | **gitleaks como motor + checks caseros mínimos del taller** (p. ej. carpetas `.obsidian`) | Checks 100% propios | La seguridad no se reinventa: gitleaks es probado y mantiene su catálogo de patrones al día. Los checks caseros solo cubren lo específico del taller que gitleaks no conoce. |

## 5. Fuera de alcance (v1)

- CI/CD continuo y releases automáticos por push.
- Versionado semántico automático y generación de changelogs.
- Publicación en registries fuera de los elegidos en la decisión pendiente.

## 6. Criterios de aceptación

- [ ] Con un secreto sembrado en un commit viejo (no en HEAD), el pre-flight lo detecta y **bloquea** la publicación (repo de prueba).
- [ ] Con un nombre ya tomado en npm, lo detecta ANTES de intentar publicar y propone alternativas (scoped).
- [ ] Sin confirmación explícita del humano, no se ejecuta ninguna acción de escritura (revisión de transcript).
- [ ] Un repo de prueba limpio se publica end-to-end (GitHub + npm de prueba) y la verificación post-publicación confirma que está vivo.

## 7. Plan de construcción

1. **S01 — Esqueleto:** plugin instalable, comando placeholder → `/plugin install` funciona.
2. **S02 — Pre-flight:** todos los checks de solo lectura + informe → criterios 1 y 2.
3. **S03 — Ejecución GitHub:** crear repo, push, verificación → parte del criterio 4.
4. **S04 — Ejecución npm/marketplace:** publish + post-verificación → criterios 3 y 4 completos.
5. **S05 — Integración y cierre:** publicarse a sí mismo con `/publicar` (dogfooding final).

## 8. Referencias

- Sesión del 2026-07-17 en el taller: publicación manual de n8n-mcp-server (el catálogo de minas reales).
- `claude-audit-tracker` — estructura de plugin y regla de firma humana.
