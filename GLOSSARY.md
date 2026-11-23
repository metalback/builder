# Builder

Flujo agéntico que lleva una idea desde su captura hasta un PR listo para merge, pasando por etapas de producto, diseño, desarrollo, QA y ciberseguridad.

## Lenguaje

### Flujo

**Idea**:
La unidad de trabajo que recorre el flujo, desde su captura hasta su PR.
_Evitar_: Feature, historia, ticket, requerimiento

**Etapa**:
Cada tramo del flujo a cargo de un agente: Producto, Desarrollo, QA, Seguridad.
_Evitar_: Fase, paso

**Promoción**:
El paso de una Idea de una Etapa a la siguiente, una vez que la Etapa actual la aprueba.
_Evitar_: Handoff, avance, deploy

**Puerta humana**:
Un punto del flujo donde un humano debe aprobar antes de que la Idea continúe.
_Evitar_: Checkpoint, aprobación manual

**Rebote**:
La devolución de una Idea a Desarrollo cuando QA o Seguridad la rechazan, acompañada de su reporte.
_Evitar_: Rechazo, retry, vuelta atrás

**Expediente**:
El conjunto de artefactos que una Idea acumula a lo largo del flujo (brief, decisiones, prototipo, spec, reportes).
_Evitar_: Contexto, carpeta, documentación

### Design sprint

**Persona simulada**:
Un usuario ficticio definido por Producto para probar el prototipo durante la fase Test del sprint.
_Evitar_: Usuario de prueba, tester

**Boceto**:
Una de las soluciones divergentes, de baja fidelidad, que UI/UX propone en la fase Sketch.
_Evitar_: Mockup, wireframe, opción

**Prototipo**:
Representación clicable y desechable de la solución elegida; sirve como referencia visual, nunca como código a heredar.
_Evitar_: Maqueta, POC, MVP

**Ciclo de reflexión**:
La iteración en que UI/UX genera, Producto critica y UI/UX revisa, hasta aprobación o hasta agotar el máximo de iteraciones.
_Evitar_: Loop de feedback, review

### Contrato y hallazgos

**Spec**:
El documento que Producto entrega al promover a Desarrollo; contiene los Criterios de aceptación.
_Evitar_: PRD, requerimientos, historia

**Criterio de aceptación**:
Una condición numerada, en forma Dado/Cuando/Entonces, que la Idea debe cumplir; es el contrato común de Desarrollo, QA y Seguridad.
_Evitar_: Requisito, caso de uso, AC

**Plan técnico**:
El documento con el que Desarrollo propone módulos, seams y orden de los Criterios antes de la Puerta 2.
_Evitar_: Diseño técnico, RFC

**Hallazgo**:
Un problema detectado por QA o Seguridad, con severidad Crítico, Alto, Medio o Bajo.
_Evitar_: Bug, issue, finding, vulnerabilidad

**Propuesta de arquitectura**:
Una mejora estructural detectada por Desarrollo que excede el alcance de la Idea y queda registrada sin implementarse.
_Evitar_: Deuda técnica, refactor pendiente
