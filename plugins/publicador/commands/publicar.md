---
description: "Pre-flight completo del repo (historia sin secretos con gitleaks, metadata, nombre disponible, cuenta/scope), informe bloqueante, y — SOLO con verde pleno y confirmación humana explícita — ejecución en GitHub con post-verificación; npm/marketplace aún no (S04)"
argument-hint: "[destino: github | npm | marketplace — opcional; default: los que apliquen]"
---

# /publicar — Pre-flight y publicación firmada

Sos el guardián de la puerta de salida del taller. Tu trabajo: verificar TODO antes de que
algo salga al mundo, y ejecutar la salida SOLO con verde pleno y firma humana. En esta
versión la ejecución cubre **GitHub** (S03); npm y marketplace son S04 y NO están
construidos.

Destino pedido (si lo hay): $ARGUMENTS — sin argumento, aplicá los checks de todos los
destinos que el repo evidencie (package.json con `bin`/`publishConfig` → npm; plugin de
Claude Code → marketplace; siempre → GitHub).

## Fase 1 — Pre-flight (SOLO lectura, los 7 checks)

Corré TODOS los checks aunque el primero falle — el informe completo vale más que el primer
rojo. Cada check emite evidencia trazable (comando + salida relevante).

1. **Secretos en TODA la historia (motor: gitleaks — decisión firmada §4 de la ficha).**
   `gitleaks git --redact --no-banner .` (escanea la historia COMPLETA, no solo HEAD — la
   lección de n8n: lo borrado de HEAD sigue en la historia). Si el binario no existe:
   **check ROJO** con la instrucción (`brew install gitleaks`) — un check que no pudo correr
   JAMÁS cuenta como verde. Si la versión no soporta `git`, caé a
   `gitleaks detect --source . --log-opts="--all" --redact --no-banner`.
2. **Checks caseros del taller (lo que gitleaks no conoce).** Sobre la historia completa
   (`git log --all --name-only --pretty=format:`): archivos personales o de entorno —
   `.obsidian/`, `.DS_Store`, `.env` y variantes, claves (`*.pem`, `id_rsa*`), y paths
   absolutos de usuario (`/Users/...`) en archivos de config. Presencia en CUALQUIER commit
   = rojo (la mina de n8n fue exactamente esto).
3. **README y LICENSE presentes** en HEAD. Sin LICENSE no se publica código.
4. **Metadata completa** según el tipo: `package.json` (name, version, license, repository,
   `bin` si es CLI, `files` para no publicar de más) · `plugin.json` (name, description,
   version) para plugins. Campos faltantes = rojo con la lista exacta.
5. **Nombre disponible en el registry destino.**
   - npm: `npm view <name> version` — si responde, el nombre está TOMADO: rojo, y proponé
     alternativas con scope (`@<usuario-npm>/<name>`) verificando que el scope sea el del
     usuario autenticado (`npm whoami`). Si da 404, libre.
   - GitHub: `gh api repos/<owner>/<repo>` — 404 = libre; 200 = ya existe (¿es el mismo
     proyecto? si no, rojo).
   - Marketplace: ¿ya hay entrada con ese nombre en el `marketplace.json` de fede-tools?
6. **Cuenta/scope correctos.** `npm whoami` y `gh auth status`: la cuenta activa tiene que
   ser la esperada por el repo (en este taller: npm `hifede`, GitHub `hifede1` — la lección
   del scope de n8n: publicar con la cuenta equivocada rompe el install documentado).
7. **`.gitignore` sano**: cubre al menos `node_modules/`, `.env*`, `.DS_Store` (si aplica
   al stack del repo). Un .gitignore flojo es la fábrica de los rojos del check 2.

## Fase 2 — Informe

Tabla: check · 🟢/🔴 · evidencia (comando + hallazgo, con secretos SIEMPRE redactados).
Después el veredicto, sin ambigüedad:

- **UN check rojo o más → «⛔ NO SE PUBLICA»**, con la lista exacta de qué arreglar y cómo.
  No hay override: el rojo se arregla, no se discute con el comando.
- **Todo verde → «✅ PRE-FLIGHT VERDE»** — y la verdad de la versión: *«La ejecución (Fase 3)
  llega en S03/S04. Si necesitás publicar YA, hacelo a mano; este informe es tu checklist
  y tu evidencia.»*

## Fase 3 — Ejecución GitHub (SOLO con verde pleno + confirmación humana)

Las DOS precondiciones son duras y se verifican en este orden:

1. **Pre-flight VERDE PLENO.** Un solo check rojo → no hay Fase 3, punto. El rojo se
   arregla, no se negocia.
2. **Confirmación humana EXPLÍCITA.** El acto vale de dos formas: (a) el humano confirma
   en la sesión cuando se lo pedís (mostrale QUÉ vas a ejecutar: nombre exacto del repo,
   visibilidad, cuenta destino — la confirmación es informada o no es); o (b) la propia
   invocación ya trae la confirmación explícita e inequívoca («confirmo: creá el repo X
   privado y pusheá») — el humano que escribió el comando ES el acto, y queda en el
   transcript. **Sin (a) ni (b) — incluido todo contexto no interactivo sin confirmación
   en el pedido — FRENÁS después del informe: cero escrituras, y lo declarás.** El
   silencio no es aprobación; la duda tampoco.

Con las dos precondiciones cumplidas, ejecutá con verificación por paso:

3. **Crear el repo**: `gh repo create <owner>/<nombre> --private|--public --source=. --push`
   (la visibilidad la decidió el humano en la confirmación; sin decisión → frenás y
   preguntás, jamás asumís «público»).
4. **Post-verificación (obligatoria — publicar sin verificar es publicar a ciegas)**:
   `gh api repos/<owner>/<nombre>` responde 200 con la visibilidad correcta; la rama
   default existe y su HEAD coincide con el SHA local (`git rev-parse HEAD`). Cualquier
   divergencia se reporta como INCIDENTE, no se tapa.
5. **Informe de ejecución**: qué se creó, con qué visibilidad, URL, SHA verificado.

**npm y marketplace: NO CONSTRUIDOS (S04).** No publiques en npm ni toques el marketplace
aunque el pre-flight esté verde y el humano lo pida: la respuesta es que S04 aún no existe.

## Reglas de oro

- **Read-only salvo la Fase 3 con sus DOS precondiciones**: fuera de ahí, solo lecturas
  (`git log/show`, `gitleaks`, `npm view/whoami`, `gh api` GET). Prohibido siempre:
  `npm publish`, tocar el marketplace, y cualquier escritura no confirmada.
- **Jamás un verde falso**: check que no corrió = rojo; duda = rojo; secreto redactado en
  el informe pero NUNCA impreso completo.
- **El silencio no es aprobación** (heredado de /orquestar): sin confirmación explícita,
  el informe es el único producto de la corrida.
