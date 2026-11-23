---
name: idea
description: Captura una Idea nueva y la lleva por el flujo completo de Builder hasta el PR.
disable-model-invocation: true
argument-hint: "<descripción libre de la idea>"
---

# Idea

La Idea: $ARGUMENTS

Las referencias están en `../../reference/` relativo al directorio base de esta skill. Resuelve esa ruta a absoluta: es el `REFERENCIAS` de cada despacho. Lee `orquestacion.md`, `expediente.md` y `hallazgos.md` ahora.

## 1. Preparar el repo destino

El repo destino es el directorio de trabajo actual.

1. Exige un working tree limpio (`git status --porcelain` vacío). Si no lo está, pide al humano que haga commit o stash y detente.
2. Si existe otra Idea con `etapa` distinta de `promovida` en `docs/ideas/*/estado.json`, avisa: v1 trabaja una Idea a la vez por repo. Ofrece retomarla con `/builder:continuar <slug>` o seguir con la nueva de todas formas.
3. Si falta `.builder/config.json`, créalo. Infiere valores de `package.json` y del framework, y confírmalos con el humano en una sola pregunta:

   ```json
   {
     "rama_base": "main",
     "dev": "npm run dev",
     "url": "http://localhost:3000",
     "test": "npm test",
     "e2e": "npx playwright test",
     "e2e_dir": "e2e"
   }
   ```

   Si el repo no tiene código, deja los valores por defecto del stack TS y anota `"scaffold": true`. Desarrollo lo propondrá en su plan técnico y el humano lo aprueba en la Puerta 2.

## 2. Entrevista de captura

Entrevista al humano con la skill `grilling` (rondas numeradas, con tu respuesta recomendada en cada pregunta), de 1 a 3 rondas. Cubre: el problema y a quién le duele, cómo se resuelve hoy, cómo se ve el éxito (una métrica observable), las restricciones y lo que queda fuera. La entrevista termina cuando puedes escribir cada uno de esos puntos en una frase sin inventar nada.

## 3. Crear la Idea

1. Elige un `slug` kebab-case corto a partir de la Idea.
2. `git switch -c idea/<slug>` desde `rama_base`.
3. Crea `docs/ideas/<slug>/entrevista.md` con la Idea original y las respuestas de la entrevista, tal cual.
4. Crea `estado.json` con `etapa: producto`, `subfase: brief`, `iteracion: 0`, `rebotes: 0`.
5. Commit: `docs(<slug>): capture idea`.

## 4. Correr el flujo

Sigue `orquestacion.md` desde `subfase: brief` hasta una Puerta humana, una escalada o la promoción.
