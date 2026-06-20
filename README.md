# Ejercicio pre-sesión · Sesión 3 — Copilotos IA

> **Importante sobre la entrega:** este ejercicio debe llegar a tu TA con al menos **48 horas de antelación** respecto al directo. Esto permite revisar los entregables, detectar bloqueos comunes y preparar el inicio de la sesión con material concreto. Las entregas fuera de plazo no se podrán incorporar a la dinámica del directo.

---

## Objetivo

Llegar a la sesión con dos cosas hechas:

1. **Saber si un copiloto entiende tu proyecto** — y tener identificado qué le falta.
2. **Una skill creada en Claude Code a tu manera**, sin guía formal de especificación.

---

## Prerrequisitos

- [ ] Tener **Claude Code** instalado y autenticado en tu máquina.
- [ ] Tener **Cursor** instalado y con sesión iniciada.
- [ ] Tener un **repo para auditar en la Parte A** — idealmente **un proyecto propio real** (de tu trabajo o personal); si no, el sandbox de S2 sirve.
- [ ] Tener el **sandbox de S2** accesible para la Parte B (ahí construyes la skill).
- [ ] Haber completado el ejercicio previo a la S2.

> Si Claude Code no está instalado, sigue la guía oficial en `https://code.claude.com` antes de empezar.

---

## Parte A — ¿Entiende un copiloto tu proyecto? (15-20 min)

El objetivo de esta parte es que descubras, con tus propios ojos, la diferencia entre lo que un copiloto **puede inferir leyendo tu código** y lo que **tienes que decirle explícitamente**. Ese hueco es exactamente lo que se gestiona con la primera primitiva que verás en el directo: la memoria de proyecto.

> 💡 **Hazlo con un proyecto propio real** si puedes — uno de tu trabajo o personal, con cierto recorrido. Cuanto más maduro el repo, más convenciones acumuladas tiene que el agente no puede adivinar, y más rico es el ejercicio. Un sandbox recién creado revela pocos huecos; un repo real de meses revela muchos. Si no tienes ninguno a mano, usa el sandbox de S2.

> ⚠️ **Privacidad:** en el entregable solo van tus **hallazgos** (descripciones del tipo "el agente no supo que usamos tal convención"), nunca código propietario ni secretos. La auditoría se queda en tu máquina; lo que compartes con el TA son las conclusiones, no el repo.

### Paso 1 — Readiness mínima

Verifica y marca:

- [ ] Claude Code arranca en el repo que vas a auditar (`claude` dentro de la carpeta) y está autenticado.
- [ ] Cursor abre el mismo proyecto sin errores.
- [ ] El código del proyecto está accesible (no hace falta que el proyecto *corra*, solo que el código esté ahí para leerse).

### Paso 2 — La prueba real

Abre Claude Code en el repo elegido y lanza **un único prompt de exploración**. No le pidas que cambie nada — solo que lea y resuma:

```
> Explora este repositorio y dime qué entiendes de su arquitectura:
  stack, estructura de carpetas, cómo se arranca, convenciones que
  detectes. No modifiques nada, solo análisis.
```

Observa la respuesta con atención. Fíjate en dos cosas:

- **Qué acertó** el agente sin que tú le dijeras nada (lo que pudo inferir del código).
- **Qué falló, omitió o asumió mal** (lo que NO pudo inferir).

> 💡 Si el agente lanza un subagent de exploración (verás algo como `[Task] Searching codebase…`), estás viendo en acción una de las primitivas del asíncrono. No hagas nada especial — solo obsérvalo.

### Paso 3 — El hallazgo

Identifica **3 a 5 cosas que el agente NO pudo inferir** del código y que tendrías que decirle explícitamente para que trabajara bien en tu proyecto. Piensa en cosas como:

- Comandos de build, test o arranque no triviales.
- Convenciones internas (naming, estructura, dónde va cada cosa).
- Restricciones operativas ("nunca commitear X", "no tocar la carpeta Y").
- Tooling no obvio (herramientas internas, configuraciones particulares).

> ⚠️ **No escribas estas cosas en ningún archivo del proyecto.** No crees `AGENTS.md` ni `CLAUDE.md` todavía — eso lo trabajamos en el directo. En esta parte solo **identificas y anotas** los hallazgos en tu entregable.

**Criterio de completitud de la Parte A:** sabes que terminaste cuando tienes una lista de 3-5 puntos concretos que el agente no supo, redactados de forma que otra persona los entendería sin contexto adicional.

---

## Parte B — Crear una skill en Claude Code, a tu manera (30-40 min)

> Esta parte es el corazón del ejercicio. El diario de decisiones que produces aquí es el material que vamos a comparar en el directo.

### Paso 4 — Familiarización con el concepto de skill

Lee la documentación oficial de skills en Claude Code (enlaces en Recursos). Cinco a diez minutos como máximo. Quédate con lo esencial:

- Qué es una skill.
- Dónde se ubica físicamente: `.claude/skills/<nombre-skill>/SKILL.md`.
- Qué metadatos básicos requiere (frontmatter YAML con `name` y `description`).
- Cuándo y cómo se activa Claude para usar una skill.

No hace falta que domines el tema. Sólo que tengas el vocabulario básico antes de construir la tuya.

### Paso 5 — Diseña tu skill

Elige una skill útil para tu día a día. Algunas opciones (puedes proponer otra):

- Generar mensajes de commit siguiendo Conventional Commits.
- Revisar pull requests según un checklist propio.
- Escribir tests siguiendo el patrón de tu equipo.
- Documentar funciones con un formato concreto (docstring, JSDoc, etc.).
- Crear migraciones de base de datos según las convenciones de tu stack.
- Generar logs estructurados con un formato dado.

Elige una que de verdad te resolvería un problema real. Cuanto más cercana a tu trabajo, mejor.

### Paso 6 — Construye la skill como tú creas que debe hacerse

Crea la skill en `.claude/skills/<nombre-skill>/SKILL.md` del proyecto sandbox (no uses el proyecto real del bloque). Construye la skill **siguiendo tu intuición y experiencia**, sin guiarte por ningún protocolo formal de especificación. Confía en tu criterio.

Algunas preguntas que probablemente tendrás que responder mientras la construyes:

- ¿Qué pongo en el `description` para que Claude active la skill cuando toca y no cuando no toca?
- ¿Qué nivel de detalle doy en las instrucciones del cuerpo?
- ¿Incluyo ejemplos? ¿Cuántos?
- ¿Defino límites claros de qué no debe hacer?
- ¿Necesito archivos auxiliares o me basta con el SKILL.md?
- ¿Qué herramientas externas debe poder usar la skill?

No busques "la forma correcta". No la perfecciones de más. Queremos ver tu enfoque natural para poder contrastarlo en el directo con lo que aparece al aplicar SDD dentro de Claude Code.

### Paso 7 — Lleva un diario de decisiones

Mientras construyes la skill, anota en paralelo un diario breve con esta estructura:

```markdown
*Skill creada:* [nombre y propósito en una línea]

*Decisiones de diseño tomadas:*
  [Decisión 1: qué decidiste y por qué]
  [Decisión 2: ...]
  [Decisión 3: ...]

*Qué me resultó fácil:*
  [Lista corta]

*Qué me resultó ambiguo o difícil de decidir:*
  [Lista corta — sé específico, no genérico]

*Tiempo real invertido:*
  [Tiempo total, separando lectura previa, diseño y escritura si puedes]

*Qué probarías si tuvieras más tiempo:*
  [Una o dos cosas]
```

Este diario es tan importante como la skill misma.

### Paso 8 — Prueba la skill al menos una vez

Antes de cerrar, ejecuta una conversación con Claude Code en la que la skill **debería** activarse. Observa:

- ¿Se activó cuando esperabas?
- ¿El resultado fue el que querías?
- Si no, ¿qué crees que falló?

Añade una sección final al diario con el resultado de esta prueba. **No iteres para "arreglarlo"** — queremos ver el resultado de tu primer intento.

---

## Entregable

Envía a tu TA **como máximo dos días antes del directo** un único documento (Markdown o equivalente) que contenga:

1. **Los 3-5 hallazgos de la auditoría** (Parte A, Paso 3): qué cosas no pudo inferir el agente de tu proyecto.
2. **El archivo `SKILL.md`** de la skill creada (pega su contenido o enlázalo en tu repositorio sandbox).
3. **El diario de decisiones completo** (Paso 7), incluyendo el resultado de la prueba (Paso 8).

Si no logras completar alguna parte, **envía igual lo que tengas** dentro del plazo. Tener el material en plazo, aunque sea parcial, es más valioso que entregar tarde un trabajo completo.

> 📄 Tienes una plantilla lista para rellenar en [`entregas/PLANTILLA-entregable.md`](entregas/PLANTILLA-entregable.md). Cópiala dentro de tu carpeta de entrega (ver siguiente sección).

---

## Pasos para entregar mediante Pull Request

1. Haz un **fork** del repositorio usando el botón ubicado arriba a la derecha.
2. **Clona tu fork** en tu computadora. Será un proyecto con el mismo nombre, pero bajo tu usuario.
3. **Completa el ejercicio** dentro de tu carpeta:
   - Crea una carpeta con tu nombre o usuario dentro de `entregas/` (por ejemplo `entregas/jane-doe/`).
   - Sube ahí el documento único con los hallazgos de la Parte A, el `SKILL.md` y el diario de decisiones (puedes partir de [`entregas/PLANTILLA-entregable.md`](entregas/PLANTILLA-entregable.md)).
   - Agrega o modifica los archivos correspondientes.
4. Crea una **nueva rama** para tu solución, por ejemplo:

   ```bash
   git checkout -b solved-ejercicio-s3
   ```

5. Haz **commit** de tus cambios:

   ```bash
   git add .
   git commit -m "Entrega ejercicio S3 - copilotos"
   ```

6. **Sube tu rama** al repositorio:

   ```bash
   git push origin solved-ejercicio-s3
   ```

7. En la interfaz de GitHub de tu fork aparecerá un aviso para crear el **Pull Request**.
8. Crea el Pull Request hacia el **repositorio original**. Esa será tu entrega final.

---
