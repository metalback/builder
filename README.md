# Builder

Plugin de Claude Code que lleva una Idea desde su captura hasta un PR: design sprint (Product Manager ↔ UI/UX con reflexión e `impeccable`), desarrollo con TDD, QA con Playwright y revisión de ciberseguridad. Vocabulario en [GLOSSARY.md](GLOSSARY.md), decisiones en [docs/adr/](docs/adr/).

## Instalación

```
/plugin marketplace add ~/Proyectos/builder
/plugin install builder@builder
```

Requiere estas skills instaladas a nivel de usuario: `impeccable`, `tdd`, `codebase-design` y `grilling`. También `gh` autenticado (para abrir el PR) y Node (para Playwright MCP, que el plugin declara en `.mcp.json`).

## Uso

En el repo destino (app web TS):

- `/builder:idea <descripción>`: captura la Idea y corre el flujo hasta la siguiente Puerta humana.
- `/builder:continuar [slug] [--etapa <etapa>]`: retoma la Idea, o fuerza una Etapa.

## Flujo

```
captura ─► Producto: brief → map → sketch (3 Bocetos) → decide ─► 🚪1 elegir Boceto
        ─► Ciclo de reflexión (UI/UX genera · PM critica · ≤3 iteraciones) → test (Personas simuladas)
        → spec → plan técnico ─► 🚪2 aprobar Prototipo + Spec + plan
        ─► Desarrollo (TDD por CA, revisión de arquitectura)
        ─► QA (E2E por CA, exploración, axe, fidelidad)
        ─► Seguridad (OWASP estático + ataques locales)
        ─► PR ─► 🚪3 merge humano

Crítico/Alto en QA o Seguridad ─► Rebote a Desarrollo (máx. 3 por Idea) ─► QA ─► Seguridad
Defecto de Spec o Rebotes agotados ─► escalada a humano
```

## Estructura

| Ruta | Qué es |
|---|---|
| `skills/idea`, `skills/continuar` | Comandos del orquestador (hilo principal: habla con el humano y despacha) |
| `agents/` | Los 5 agentes de Etapa, aislados entre sí |
| `reference/orquestacion.md` | Máquina de estados del flujo |
| `reference/expediente.md` | Estructura del Expediente, `estado.json` y formato de los Criterios |
| `reference/hallazgos.md` | Severidades, defectos de Spec y veredictos |
