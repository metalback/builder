# El Expediente en archivos del repo es el único contrato entre agentes

Cada Etapa corre como un subagente con contexto limpio que solo lee el Expediente (`docs/ideas/<slug>/`) y el repo, nunca la conversación previa. El estado de la Idea vive en `estado.json`. Elegimos esto frente a issues de GitHub o a un contexto compartido por tres razones: funciona offline, queda versionado junto al código, permite reanudar el flujo, y evita que QA y Seguridad hereden los supuestos de Desarrollo. La consecuencia es que todo lo que una Etapa necesite saber tiene que quedar escrito en el Expediente.
