Generá un mapa visual de flujo de trabajo para el área o proceso: $ARGUMENTS

Este comando genera un mapa de flujo de trabajo (HTML autocontenido con Mermaid.js) para este repositorio de Tierramor Workflows, y lo agrega a la galería de `index.html`.

## Paso 0 — Si no hay tema definido

Si `$ARGUMENTS` está vacío, preguntá al usuario qué área o proceso quiere mapear antes de continuar.

## Paso 1 — Identificar el departamento

Cada workflow vive dentro de la carpeta de su departamento (`farm/`, `experiences/`, `kitchen/`, etc.), así el repositorio se mantiene organizado a medida que se suman más equipos.

1. Abrí `index.html` y leé el array `DEPTS` para ver la lista vigente de departamentos (campo `id`, `title`, `subtitle`).
2. Preguntale al usuario a cuál de esos departamentos pertenece este flujo.
3. Si ninguno corresponde, no asumas — confirmá con el usuario si quiere que crees una carpeta y una entrada nueva en `DEPTS` para un departamento que todavía no existe.
4. Confirmá también el nombre de carpeta para el área/proceso específico (minúsculas, sin espacios ni tildes, guiones bajos entre palabras — ej: `huevos_pastoreo`). El archivo va a vivir en `[departamento]/[nombre_area]/`.

## Paso 2 — Reunir contexto

Preguntá al usuario si tiene un SOP, documento, transcripción de reunión, o notas que describan el proceso. Si los comparte, leelos completos antes de avanzar. Si no tiene nada escrito, pedile que te explique el proceso paso a paso.

No asumas pasos que no te hayan confirmado. Si algo no está claro, hacé una pregunta a la vez — nunca varias juntas.

## Paso 3 — Confirmar comprensión

Antes de generar el archivo, resumí el proceso completo de vuelta al usuario (pasos, responsables, frecuencia, datos que se registran) y pedile que confirme que está correcto. Ajustá según su feedback antes de seguir.

## Paso 4 — Clasificar cada paso

Cada paso del proceso se clasifica en una de estas categorías:

- **Solo Humano** — requiere presencia física, criterio situacional, o ejecución manual sin asistencia de IA
- **IA + Humano** — la IA puede generar un borrador, sugerencia, o agregación de datos, pero un humano debe revisar y confirmar antes de que tenga efecto
- **Agente IA** — lógica completamente automatizable, se ejecuta sin intervención humana
- **Captura de datos** — un punto donde se registra información (no es un paso de ejecución, es un parallelogramo de datos)
- **Entidad externa** — otro departamento, proveedor, o persona fuera del proceso que interactúa con él

## Paso 5 — Identificar puntos de captura de datos

Por cada paso que registra información, especificá los campos exactos. Sé específico: no "información relevante" sino algo como "fecha · proveedor · monto · línea presupuestaria".

## Paso 6 — Conexiones entre departamentos o áreas

Si un paso involucra a otro departamento o activa un proceso en otra área, representalo como una flecha punteada con una etiqueta corta describiendo qué pasa ("activa workflow de Biofábrica", "envía copia a Administración", etc.).

## Paso 7 — Generar el archivo HTML

Usá como plantilla cualquier archivo `workflow_*.html` ya existente en este repositorio (por ejemplo `farm/produccion_agricola/workflow_produccion_agricola_en.html`) — mantené exactamente el mismo CSS, la misma estructura de página, y el mismo bloque de inicialización de Mermaid al final del archivo. No reinventes el estilo.

Elementos obligatorios:
1. **Encabezado** — nombre del área/proceso, responsable(s), fecha de actualización
2. **Leyenda de colores** (siempre las 5 categorías, aunque no todas se usen en el diagrama):

   | Color | Clase Mermaid | Significado |
   |-------|---------------|-------------|
   | Azul `#7B9CDA` | `aiHuman` | IA + Humano |
   | Naranja `#F4A261` | `human` | Solo Humano |
   | Verde `#52B788` | `aiAgent` | Agente IA |
   | Amarillo `#E9C46A` | `data` | Captura de datos (paralelogramo) |
   | Gris `#d0cfc8` | `external` | Entidad externa |

3. **Diagrama Mermaid** (`flowchart TD`) con los pasos, los puntos de datos, las conexiones externas como flechas punteadas (`-.-> `), y subgrafos (`subgraph`) cuando haya pasos de distinta frecuencia (ej: una vez por temporada vs. semanal vs. por evento) o distintos responsables ejecutando el mismo proceso en paralelo
4. **Notas de diseño** al final — explicá decisiones no obvias del proceso. Si algo quedó sin confirmar con el usuario o un colaborador específico, marcalo con un badge `pending` (ver ejemplos en el repo) en vez de asumir.

### Nombre y ubicación del archivo

Guardalo en: `[departamento]/[nombre_area]/workflow_[nombre_area].html` — usando el departamento y el nombre de carpeta que confirmaste en el Paso 1.

- Minúsculas, sin espacios ni tildes, guiones bajos entre palabras (ej: `huevos_pastoreo`, no `Huevos de Pastoreo`)
- Si el equipo necesita una copia publicada en otro idioma además de la original, usá el sufijo correspondiente, ej: `workflow_[nombre_area]_en.html`
- La versión que se vaya a enlazar desde la galería (`index.html`) debe incluir este link al inicio del `<body>`, justo antes del `system-tag` — notá que son **dos** niveles hacia arriba (`departamento/area/archivo.html` → raíz del repo):
  ```html
  <a href="../../index.html" class="back-link">← All Workflows</a>
  ```

## Paso 8 — Agregar el mapa a la galería (`index.html`)

Este paso es obligatorio — un mapa que no está en `index.html` no es descubrible por el resto del equipo.

1. Abrí `index.html` y ubicá el array `DEPTS`.
2. Buscá la entrada del departamento que confirmaste en el Paso 1. Si tiene `status: "dept"`, agregá el nuevo workflow a su array `workflows: []`. Si tiene `status: "coming-soon"` (todavía sin flujos), cambialo a `status: "dept"` al agregar el primer flujo real.
3. Si confirmaste con el usuario que hay que crear un departamento nuevo, agregá también su carpeta vacía en la raíz del repo (con un `README.md` placeholder, igual al de `kitchen/`, `marketing/`, etc.) y su entrada en `DEPTS`.
4. El objeto del nuevo workflow sigue esta forma exacta:
   ```js
   {
       title: "Nombre en inglés para la card",
       subtitle: "Nombre en español",
       description: "1-2 oraciones describiendo el flujo de punta a punta.",
       tags: ["tag corto", "tag corto", "tag corto"],
       href: "[departamento]/[nombre_area]/workflow_[nombre_area]_en.html",
       accent: "#xxxxxx",
       status: "complete"
   }
   ```
5. Elegí un color `accent` que no esté ya en uso por otra card del mismo departamento, dentro de la misma paleta tierra/verde/azul apagado que usan las demás cards.
6. Si agregaste un workflow a un departamento existente, actualizá también su contador en `tags` (ej: de `"4 workflows"` a `"5 workflows"`).

## Paso 9 — Cerrar el loop

Después de crear el archivo y actualizar `index.html`, preguntale al usuario si hay algún paso, dato, o conexión que quiera ajustar antes de darlo por terminado.
