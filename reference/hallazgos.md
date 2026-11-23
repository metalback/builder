# Hallazgos

Un Hallazgo es un problema detectado por QA o Seguridad. Ambas Etapas usan esta misma escala y este mismo formato.

## Severidad

| Severidad | Criterio | Efecto |
|---|---|---|
| **Crítico** | Pérdida o exposición de datos, ejecución de código, bypass de autenticación/autorización, o un Criterio de aceptación central que no se cumple en absoluto. | Rebote |
| **Alto** | Un Criterio de aceptación incumplido, una vulnerabilidad explotable con esfuerzo moderado, una violación WCAG A que bloquea una tarea, o un caso borde que rompe el flujo principal. | Rebote |
| **Medio** | Comportamiento degradado con workaround, vulnerabilidad que requiere condiciones improbables, violación WCAG AA, divergencia visible respecto del Prototipo. | Al PR, no bloquea |
| **Bajo** | Detalle cosmético, endurecimiento recomendado, mejora menor. | Al PR, no bloquea |

En la duda entre dos severidades, elige la mayor y explica por qué.

## Defecto de Spec

Si la causa del problema es la Spec (un criterio ambiguo, contradictorio, ausente o una falla del diseño aprobado) y no el código, márcalo como **defecto de Spec** en vez de asignarle severidad. Un defecto de Spec nunca provoca Rebote: el orquestador lo escala a la Puerta humana.

## Formato en el reporte

```
### H-2 [Alto] Exportación ignora filtros de fecha — CA-3
- **Evidencia**: pasos exactos, test que falla (`e2e/ca-3.spec.ts`), captura o request/response.
- **Esperado**: lo que dice CA-3.
- **Observado**: lo que ocurre.
- **Sugerencia**: dirección de la corrección, sin implementarla.
```

## Veredicto

Todo reporte termina con exactamente una de estas líneas, que el orquestador lee:

- `VEREDICTO: aprobado`: sin Hallazgos Crítico ni Alto, sin defectos de Spec.
- `VEREDICTO: rebote`: al menos un Hallazgo Crítico o Alto.
- `VEREDICTO: defecto-spec`: al menos un defecto de Spec (tiene prioridad sobre `rebote`).
