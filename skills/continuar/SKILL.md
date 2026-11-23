---
name: continuar
description: Retoma una Idea de Builder donde quedó, o fuerza una Etapa concreta.
disable-model-invocation: true
argument-hint: "[slug] [--etapa producto|desarrollo|qa|seguridad|promocion]"
---

# Continuar

Argumentos: $ARGUMENTS

Las referencias están en `../../reference/` relativo al directorio base de esta skill. Resuelve esa ruta a absoluta: es el `REFERENCIAS` de cada despacho. Lee `orquestacion.md`, `expediente.md` y `hallazgos.md` ahora.

1. **Encontrar la Idea.** Si no hay slug, lista las Ideas de `docs/ideas/*/estado.json` con su etapa. Si solo una no está `promovida`, usa esa; si hay varias, pregunta cuál.
2. **Ubicarse.** Cámbiate a la rama de `estado.json.rama` si no estás en ella. Exige un working tree limpio.
3. **Forzar una Etapa** (solo con `--etapa`): fija `etapa` (y `subfase: brief` si es `producto`), registra en `historial` que fue forzada y por qué, y haz commit.
4. **Resumir.** Muestra en 3 a 5 líneas dónde está la Idea: etapa, subfase, iteración, Rebotes y la última entrada del `historial`.
5. **Correr el flujo.** Sigue `orquestacion.md` desde el estado actual. Si la Idea está `escalada`, empieza por la pregunta de escalada; si está `promovida`, informa la URL del PR y termina.
