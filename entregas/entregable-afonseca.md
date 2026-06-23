# Entregable · Sesión 3 — Copilotos IA

- **Nombre / usuario:** afonseca@nice.com.mx
- **Fecha de entrega:** 2026-06-22
- **Repo auditado en la Parte A:** API REST .NET Core, ~2598 controllers, proyecto de backend empresarial en producción

---

## 1. Hallazgos de la auditoría (Parte A)

> 3-5 cosas que el agente no pudo inferir del código y que hay que decirle explícitamente.

1. **La librería de conexión a base de datos no tiene código fuente en el repositorio.** El archivo que maneja toda la conectividad con SQL Server es un binario externo compilado. Si algo falla a nivel de conexión o respuesta de la base de datos, el fix no está en este repositorio sino en otro proyecto separado.

2. **El SQL se construye pegando texto — no es un error, es el estándar del proyecto.** Todos los endpoints construyen sus consultas concatenando variables directamente en el texto. No hay mecanismos de consultas parametrizadas. Cambiar eso en código nuevo crearía una inconsistencia con los más de 2500 endpoints existentes.

3. **La rama de trabajo correcta es MASTER-WMS, no main ni master.** Las ramas main y master existen pero están desactualizadas. Cualquier trabajo nuevo debe partir de MASTER-WMS con el formato de nombre TKT-{número de ticket}.

4. **No hay pruebas automatizadas — la validación es manual.** No existe ningún proyecto de pruebas. La forma de verificar que un endpoint funciona es publicarlo al ambiente de prueba y ejecutarlo manualmente desde la interfaz web del API, enviando cinco datos de cabecera específicos en cada llamada.

5. **El nombre de la base de datos nunca se escribe directamente en el código.** Siempre se lee desde un archivo de configuración del proyecto. Si se escribe el nombre a mano, el endpoint apuntará al servidor equivocado en producción sin mostrar ningún error.

---

## 2. SKILL.md de la skill creada (Parte B)

```markdown
# austero

## description

Activa un modo de trabajo ordenado y sin desperdicio para toda la conversación.
Usar cuando la sesión empiece, cuando se sienta lenta o cargada, o cuando el usuario escriba `/austero`.
Solo se activa una vez por conversación.

---

## Qué hace Claude al activarse

**Paso 1 — Resumir dónde estamos**
En 3 líneas: qué se está haciendo, qué ya está listo y qué falta. Si la conversación acaba de empezar, omitir este paso.

**Paso 2 — Trabajar de forma austera por el resto de la sesión**

1. **Leer solo lo necesario** — Si un documento es muy largo, leerlo en partes pequeñas en lugar de todo de golpe.
2. **No saturar la conversación con resultados masivos** — Cuando una tarea genera muchísimo texto, hacerla aparte para que ese volumen no llene la conversación principal.
3. **Traer solo lo relevante** — Antes de mostrar resultados, filtrarlos para incluir solo lo que importa.
4. **Llevar un registro de avance** — Mantener una lista de lo que ya se hizo y lo que falta, para no repetir pasos ni perder el hilo.
5. **Avisar antes de hacer algo que tome mucho tiempo** — Resumir en una línea qué se va a hacer y por qué, antes de empezar.
6. **No repetir lo que el usuario ya dijo** — Si algo ya está en la conversación, usarlo directamente sin volver a escribirlo.
7. **Respuestas cortas** — Solo lo esencial, sin introducciones largas ni resúmenes al final de cada respuesta.

---

## Qué NO hacer

- No bajar la calidad del trabajo, solo la cantidad de palabras
- No dejar de hacer cosas importantes por ahorrar — el ahorro es en explicaciones innecesarias, no en el trabajo útil
- No activarse más de una vez en la misma conversación
- No pedir permiso al usuario antes de activar

Confirmar activación respondiendo: `[austero ON]` seguido del resumen de 3 líneas (o solo `[austero ON]` si la conversación acaba de empezar).
```

---

## 3. Diario de decisiones

*Skill creada:* `austero` — hace que las conversaciones con Claude sean más ordenadas y sin desperdicio, desde el momento en que se activa hasta el final de la sesión.

*Decisiones de diseño tomadas:*
- **Un solo modo, no dos.** Se empezó con dos modos separados (uno preventivo desde el inicio, otro de emergencia para sesiones largas) y se fusionaron en uno. Obligar al usuario a elegir entre modos agrega una carga innecesaria.
- **El resumen inicial es condicional.** Al activarse, Claude resume dónde está la conversación solo si ya hay contenido. Si la sesión acaba de empezar, ese paso se omite solo.
- **La descripción dice cuándo activar y también cuándo no.** Se incluyó explícitamente que solo se activa una vez, para evitar llamadas repetidas sin sentido.
- **Lenguaje para cualquier persona.** El archivo pasó por varias iteraciones hasta eliminar toda la jerga técnica. Cualquiera puede leerlo y entender qué hace, cuándo usarlo y qué esperar.
- **Confirmación visible.** Claude responde con `[austero ON]` para que el usuario sepa que el modo está activo sin tener que adivinar.
- **Nombre: `austero`.** Se evaluaron varias opciones en español. Se eligió porque transmite disciplina sin gastar de más y lo propuso el propio usuario.

*Qué me resultó fácil:*
- Definir cuándo activar — el uso era claro desde el principio
- Los 7 hábitos de trabajo — ya estaban implícitos en la forma de trabajar
- La fusión de los dos modos — una vez tomada la decisión, el archivo quedó más limpio

*Qué me resultó ambiguo o difícil de decidir:*
- **¿Un modo o dos?** Dos modos dan más control, uno solo es más simple. Se eligió uno porque el usuario no debería tener que pensar en eso.
- **¿El resumen siempre o solo a veces?** Se resolvió haciendo ese paso condicional sin que el usuario tenga que indicarlo.
- **¿El ahorro aplica también a las acciones?** El ahorro es en explicaciones innecesarias, no en el trabajo útil — pero sigue siendo una línea difícil de trazar en la práctica.

*Tiempo real invertido:*
- Diseño inicial: ~3 min · Primera versión: ~4 min · Separar modos: ~3 min · Renombrar: ~1 min · Traducción al español: ~2 min · Simplificación de lenguaje: ~3 min · Fusión de modos: ~2 min
- **Total: ~18 min**

*Qué probaría si tuviera más tiempo:*
- Pedir a alguien sin conocimientos técnicos que lea el archivo y explique con sus palabras qué hace — esa prueba diría si el lenguaje llegó al nivel correcto.
- Probar la activación automática al inicio de cada sesión sin que el usuario tenga que escribir `/austero`.

*¿Usaste IA para crear la skill?*
- Sí. Claude generó las versiones del archivo y propuso la estructura. Las decisiones de diseño (fusionar modos, elegir el nombre, simplificar el lenguaje, definir las 7 reglas) fueron del usuario. Claude ejecutó los cambios en cada iteración.

### Resultado de la prueba (Paso 8)

- **¿Se activó cuando lo esperabas?** Sí. En cuanto se escribió `/austero`, respondió de inmediato con el resumen de la conversación y mantuvo las respuestas cortas por el resto de la sesión sin que el usuario tuviera que recordárselo.
- **¿El resultado fue el que querías?** En parte sí. Las respuestas fueron más cortas y directas al problema. Las instrucciones para el siguiente paso quedaron en pocas líneas.
- **Si no, ¿qué crees que falló?** Cuando el resultado es una tabla o un bloque de instrucciones organizadas, la respuesta sigue siendo larga aunque la habilidad esté activa — ese contenido es necesario y no se puede recortar. Ahí la habilidad tiene un límite natural.
