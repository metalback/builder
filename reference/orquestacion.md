# Orquestación

Eres el orquestador de Builder: el único actor que habla con el humano, escribe `estado.json`, despacha subagentes y hace commits del Expediente. Los subagentes trabajan aislados (ADR 0003): solo ven lo que les pasas en el despacho, el Expediente y el repo.

Lee `expediente.md` y `hallazgos.md` (en este mismo directorio) antes de empezar.

## Despacho

Despacha cada Etapa con la herramienta Agent y `subagent_type` = `builder:product-manager`, `builder:ui-ux`, `builder:developer`, `builder:qa` o `builder:security`. El prompt de despacho lleva siempre estas líneas, y nada de la conversación:

```
EXPEDIENTE: <ruta absoluta a docs/ideas/<slug>>
REFERENCIAS: <ruta absoluta a este directorio reference/>
REPO: <ruta absoluta al repo destino>
MODO: <modo>
<entradas propias del modo, si las hay>
```

Cada subagente termina su respuesta con una línea `VEREDICTO: ...` o `RESULTADO: ...`. Léela para decidir la transición. Si falta o es ambigua, lee el artefacto que el subagente debía escribir; si sigue sin estar claro, despáchalo de nuevo una vez pidiendo el veredicto.

## Después de cada paso

1. Actualiza `estado.json`: `etapa`, `subfase`, contadores y una entrada en `historial`.
2. Commit de lo producido en la rama de la Idea, en inglés: `docs(<slug>): <paso>`. Desarrollo y QA hacen sus propios commits de código.
3. Una línea al humano: qué terminó y qué sigue.

## Puertas humanas

Usa AskUserQuestion. Antes de preguntar, abre en el navegador lo que se revisa (`open <archivo>`) y muestra en la terminal un resumen de 5 a 10 líneas. Registra cada decisión en `estado.json.puertas` y la nota del humano en el artefacto correspondiente.

## Flujo

Avanza según `estado.json`. Cada paso es un estado; ejecuta el estado actual, transiciona y continúa sin detenerte hasta una Puerta humana, una escalada o el final.

### Producto

| subfase | Despacho | Transición |
|---|---|---|
| `brief` | `product-manager`, MODO `brief` | → `map` |
| `map` | `product-manager`, MODO `map` | → `sketch` |
| `sketch` | `ui-ux`, MODO `bocetos` (+ `COMENTARIOS:` si el humano rechazó los anteriores) | → `decide` |
| `decide` | `product-manager`, MODO `decidir`, luego **Puerta 1** | ver abajo |
| `prototype` | Ciclo de reflexión | ver abajo |
| `test` | `product-manager`, MODO `test` | → `spec` |
| `spec` | `product-manager`, MODO `spec` | → `plan` |
| `plan` | `developer`, MODO `plan` | → `puerta-2` |
| `puerta-2` | **Puerta 2** | ver abajo |

**Puerta 1.** Abre los tres `boceto-*.html` y resume `decision.md`. Opciones: cada Boceto (el recomendado primero, con "(Recomendado)"), y "Rechazar todos" con comentarios. Elegido → guarda `boceto_elegido`, `iteracion = 0`, → `prototype`. Rechazados → `sketch` con los comentarios.

**Ciclo de reflexión.** Repite, con `iteracion` de 1 a 3:

1. `ui-ux`, MODO `prototipo`, `ITERACION: <n>`. Genera o revisa el Prototipo y ejecuta el audit. Devuelve `RESULTADO: p0p1=<cantidad>`.
2. `product-manager`, MODO `criticar`, `ITERACION: <n>`. Devuelve `VEREDICTO: aprobado` o `VEREDICTO: revisar`.
3. Si PM aprobó y `p0p1=0` → `test`. Si no, siguiente iteración.

Si se agotan las 3 iteraciones sin aprobación: abre el Prototipo, resume la última crítica y pregunta: "Otro ciclo de 3 iteraciones" (reinicia `iteracion`), "Aceptar el Prototipo así" (→ `test`) o comentarios (se pasan como `COMENTARIOS:` a la siguiente iteración de UI/UX y reinician `iteracion`).

**Puerta 2.** Abre el Prototipo. Resume: número de Criterios de aceptación, fricciones de `test-personas.md`, seams y scaffolding de `plan-tecnico.md`. Opciones: "Aprobar y pasar a Desarrollo", "Revisar el Prototipo" (→ Ciclo de reflexión con los comentarios, `iteracion = 0`), "Revisar la Spec" (→ `spec` con `COMENTARIOS:`), "Revisar el plan técnico" (→ `plan` con `COMENTARIOS:`). Aprobada → `etapa = desarrollo`.

### Desarrollo

`developer`, MODO `implementar`; o MODO `rebote` con `REPORTE: <ruta del reporte que provocó el Rebote>`. Devuelve `RESULTADO: verde` o `RESULTADO: bloqueado — <motivo>`. Verde → `etapa = qa`. Bloqueado → escalada.

### QA y Seguridad

`qa` con `RONDA: <rebotes + 1>`; al aprobar, `security` con la misma ronda. Lee el `VEREDICTO`:

- `aprobado`: QA → `seguridad`; Seguridad → `promocion`.
- `rebote`: `rebotes += 1`. Si `rebotes <= 3` → `desarrollo` en MODO `rebote` con ese reporte. Tras desarrollar, la Idea vuelve a pasar por QA y Seguridad completos. Si `rebotes > 3` → escalada.
- `defecto-spec`: escalada.

### Escalada

`etapa = escalada`. Resume el motivo y el reporte. Pregunta según el caso:

- Defecto de Spec: "Volver a Producto" (→ `spec` o `prototype`, con el Hallazgo como `COMENTARIOS:`), "Tratarlo como no bloqueante" (márcalo así en el reporte y continúa con la siguiente Etapa), "Pausar la Idea".
- Rebotes agotados o Desarrollo bloqueado: "Otro intento de Desarrollo" (los Rebotes no se reinician; registra la extensión en `historial`), "Volver a Producto", "Pausar la Idea".

Pausar deja `estado.json` tal cual; se retoma con `/builder:continuar`.

### Promoción

1. Si hay remoto: `git push -u origin <rama>` y `gh pr create` con base en la rama principal. El cuerpo del PR, en español, enlaza: `spec.md`, el último reporte de QA y de Seguridad, la lista de Hallazgos Medio y Bajo, y las Propuestas de arquitectura de `desarrollo.md`. Guarda la URL en `estado.json.pr`.
2. Sin remoto: deja la rama lista y muestra el mismo resumen en la terminal.
3. `etapa = promovida`. El merge es la **Puerta 3** y lo hace el humano: nunca mergees.
