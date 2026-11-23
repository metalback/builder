# Builder es un plugin de Claude Code, no una app con Agent SDK

Builder se distribuye como un plugin de Claude Code (subagentes, skills y un comando orquestador) que opera sobre el repo donde se invoca. Lo elegimos frente al Agent SDK para reutilizar directamente `impeccable`, `tdd`, `improve-codebase-architecture`, Playwright MCP y `/security-review`, y porque el flujo exige Puertas humanas interactivas. El costo es que no corre desatendido en CI. Si eso se vuelve necesario, el Agent SDK es el camino de migración.
