# Expediente

El Expediente es el único contrato entre Etapas (ADR 0003). Todo lo que una Etapa necesita saber está escrito aquí; lo que no está escrito, no existe. Vive en el repo destino, en la rama de la Idea:

```
docs/ideas/<slug>/
├── estado.json              ← lo escribe solo el orquestador
├── entrevista.md            ← notas de la entrevista de captura
├── brief.md                 ← Producto
├── sprint/
│   ├── map.md               ← Producto: objetivo de largo plazo, preguntas del sprint, mapa del recorrido
│   ├── bocetos/
│   │   ├── bocetos.md       ← UI/UX: descripción y racional de cada Boceto
│   │   └── boceto-{1,2,3}.html
│   ├── decision.md          ← Producto: rúbrica, puntajes, recomendación; elección humana
│   ├── prototipo/index.html ← UI/UX: HTML autocontenido y clicable
│   ├── reflexion/iteracion-<n>.md  ← UI/UX (generación + audit) y Producto (crítica + veredicto)
│   └── test-personas.md     ← Producto: Personas simuladas y fricciones observadas
├── spec.md                  ← Producto: Criterios de aceptación CA-1..CA-n
├── plan-tecnico.md          ← Desarrollo: seams, stack/scaffolding, orden de los criterios
├── desarrollo.md            ← Desarrollo: bitácora por criterio y por Rebote, Propuestas de arquitectura
├── qa/reporte-<ronda>.md    ← QA (+ qa/capturas/)
└── seguridad/reporte-<ronda>.md ← Seguridad
```

`<ronda>` = `rebotes + 1` al momento de la revisión.

## estado.json

```json
{
  "slug": "exportar-reportes",
  "titulo": "Exportar reportes a PDF",
  "rama": "idea/exportar-reportes",
  "etapa": "producto",
  "subfase": "prototype",
  "iteracion": 2,
  "rebotes": 0,
  "boceto_elegido": 2,
  "puertas": {
    "1": { "aprobada": true, "fecha": "2026-11-21T15:00:00Z", "nota": "Boceto 2, con la navegación del 3" }
  },
  "pr": null,
  "historial": [
    { "fecha": "2026-11-21T14:00:00Z", "evento": "captura", "detalle": "Idea creada" }
  ]
}
```

- `etapa`: `producto` | `desarrollo` | `qa` | `seguridad` | `promocion` | `promovida` | `escalada`
- `subfase` (solo en `producto`): `brief` | `map` | `sketch` | `decide` | `prototype` | `test` | `spec` | `plan` | `puerta-2`
- `iteracion`: iteración actual del Ciclo de reflexión (máximo 3 por ciclo).
- `rebotes`: total de Rebotes de la Idea, sumando QA y Seguridad (máximo 3).

## Idioma

El Expediente se escribe en español. El código, los tests y los mensajes de commit, en inglés.

## Criterios de aceptación

Formato fijo en `spec.md`, numerados y estables (un criterio nunca se renumera):

```
### CA-3: Exportar con filtros activos
- **Dado** un reporte con filtros de fecha aplicados
- **Cuando** el usuario presiona "Exportar PDF"
- **Entonces** el PDF descargado contiene solo las filas filtradas
```

Desarrollo nombra sus tests por criterio (`CA-3: ...`), QA escribe un E2E por criterio con el mismo prefijo, y los Hallazgos citan el criterio que afectan.
