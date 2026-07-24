# S05 — Dogfooding: el guardián auditó su propia casa (y encontró minas)

**Fecha:** 2026-07-24 · **Encargo:** issue #5 · **Transcripts:** `s05-corridas/`

## Corrida 1 — /publicar sobre claude-publicador → DOS MINAS REALES

1. **LICENSE AUSENTE** mientras `plugin.json` declara `"license": "MIT"` — el repo público
   prometía una licencia que no otorgaba. El drift exacto que el producto caza, en sí mismo.
2. **Check 5 rojo por ambigüedad del PROPIO comando**: buscó el catálogo en un repo
   `fede-tools` inexistente — el catálogo real vive en `claude-audit-tracker` (el nombre
   del marketplace y el del repo no coinciden, y el doc no lo decía). Aplicó la regla como
   corresponde: check que no pudo correr = rojo, jamás verde.

Y el cierre honesto de la corrida: dos rojos → «no hubo Fase 3 y no corresponde», cero
escrituras.

## Los fixes (mismo encargo)

- `LICENSE` MIT completo commiteado.
- `publicar.md` (v0.5.0): el check 5 ahora dice DÓNDE vive el catálogo
  (`hifede1/claude-audit-tracker/.claude-plugin/marketplace.json`).

## Corrida 2 — re-corrida → ✅ PRE-FLIGHT VERDE 7/7

Los 7 checks verdes sobre sí mismo, incluidos: gitleaks sobre los 27 commits (no leaks),
LICENSE==metadata, y la entrada del marketplace verificada con **descripción idéntica** a
la del plugin.json (cero drift). La deuda «nació publicado a mano» queda saldada: el motor
real re-verificó la publicación manual del 2026-07-23.

## El saldo formal de la delegación gitleaks (batuta/010)

`decisiones/010` de batuta difirió el escaneo de secretos a publicador cuando publicador no
existía. Hoy el motor corrió sobre la historia COMPLETA de los 7 repos propios del taller:

| Repo | Commits | Veredicto |
|---|---|---|
| claude-audit-tracker | 71 | no leaks |
| claude-batuta | 64 | no leaks |
| claude-cartera | 16 | no leaks |
| claude-doc-arquitecto | 47 | no leaks |
| claude-verificador | 24 | no leaks |
| n8n-mcp-server | 19 | no leaks |
| claude-publicador | 29 | no leaks |

**270 commits, cero leaks.** El bloqueo «sin fecha de resolución» que batuta arrastraba
desde el 2026-07-19 tiene fecha: hoy.

## Criterios del encargo
- «Corrida completa de /publicar sobre claude-publicador con informe verde» → ✅ (con el
  camino completo: minas encontradas → arregladas → verde legítimo)
- «Deuda de verificación saldada o explícita en el tracker» → ✅ (nació-publicado-a-mano
  saldada; la 010 de batuta saldada con el barrido de flota)
