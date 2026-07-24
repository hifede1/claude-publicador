# Criterios clasificados — claude-publicador

> Clasificación DECIDIDA por el dueño (Fede) en sesión interactiva del 2026-07-24 —
> primera corrida positiva real de `/verificador:criterios-a-tests` (rama positiva del
> criterio 3 del verificador: recomendación de la máquina, decisión del humano, registro).
> Contrato que la pestaña Tests de `/audit-tracker` consume sin adaptación manual.

| Id | Criterio | Clase (decidida por el dueño) | Artefacto |
|---|---|---|---|
| S02_C1 | Con un secreto sembrado en un commit viejo (no en HEAD), el pre-flight lo detecta y bloquea | Testeable automático | `docs/audits/s02-corridas/corrida-A-secreto.txt` (corrida sembrada, fixture `prueba-s02pub-secreto`) |
| S02_C2 | Con un nombre ya tomado en npm, lo detecta ANTES y propone alternativas scoped | Testeable automático | `docs/audits/s02-corridas/corrida-B-nombre.txt` (corrida sembrada, fixture `prueba-s02pub-nombre`) |
| S03_C3 | Sin confirmación explícita del humano, no se ejecuta ninguna acción de escritura | Testeable con esfuerzo | `docs/audits/s03-corridas/corrida-A-sin-confirmacion.txt` (headless + revisión de transcript + 404 verificado) |
| S04_C4 | Un repo de prueba limpio se publica end-to-end (GitHub + npm de prueba) con post-verificación viva | Testeable con esfuerzo | GitHub: `docs/audits/s03-corridas/corrida-B-e2e.txt` (SHA verificado) · npm: pendiente de S04 |

## Verificación manual registrada
— Ninguna: los 4 criterios quedaron testeables (2 automáticos, 2 con esfuerzo). Cero limbo.
