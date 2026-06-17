# Cómo agregar tu workflow a Tierramor Workflows

Esta guía es para cualquier líder de equipo o departamento que quiera documentar su proceso como un mapa visual y agregarlo a esta galería.

## Qué es este repositorio

`index.html` es la galería pública de mapas de flujo de trabajo de Tierramor — se publica en GitHub Pages.

El repositorio está organizado por departamento, y dentro de cada departamento hay una carpeta por proceso:

```
farm/
  produccion_agricola/
  biofabrica/
  vivero/
  reporte_financiero/
experiences/
  workshops_eventos/
kitchen/
marketing/
operations/
bioconstruction/
finance/
```

Si tu departamento todavía no tiene ningún workflow, vas a encontrar su carpeta vacía (con un `README.md` placeholder) — tu mapa va a ser el primero ahí adentro.

## Qué necesitás antes de empezar

1. **Claude Code** instalado ([claude.com/claude-code](https://claude.com/claude-code)).
2. Este repositorio clonado en tu computadora.
3. Tu proceso documentado de alguna forma — un SOP, notas de una reunión, o simplemente la explicación paso a paso en tu cabeza. No necesita estar perfectamente escrito, Claude te va a ir preguntando lo que falte.

## Cómo generar tu mapa

1. Abrí Claude Code dentro de la carpeta de este repositorio.
2. Corré:
   ```
   /workflow-chart [nombre de tu área o proceso]
   ```
3. Claude te va a preguntar primero a qué departamento pertenece tu proceso (Farm, Kitchen, Marketing, Operations, Bioconstruction, Finance, Experiences) — así sabe en qué carpeta guardar el mapa.
4. Si tenés un documento con el SOP, compartilo cuando Claude te lo pida — podés simplemente pegar el contenido o decirle la ruta del archivo.
5. Claude te va a hacer preguntas para entender el proceso completo antes de generar nada. Respondé con la mayor precisión posible, especialmente sobre:
   - Quién hace cada paso y con qué frecuencia
   - Qué datos se registran en cada punto (fechas, cantidades, montos, etc.)
   - Qué pasos involucran a otro departamento
6. Claude va a generar el archivo HTML del mapa dentro de la carpeta de tu departamento y va a agregarlo a `index.html` automáticamente.
7. Revisá el resultado abriendo el archivo HTML generado en tu navegador.

## Convención de colores

Cada paso del proceso se clasifica en uno de estos colores — no porque se vea bonito, sino para que cualquiera pueda entender de un vistazo qué tan automatizable es cada parte del proceso:

| Color | Significado |
|-------|-------------|
| 🔵 Azul | IA + Humano — la IA propone o agrega datos; un humano revisa y confirma |
| 🟠 Naranja | Solo Humano — requiere presencia física o criterio humano directo |
| 🟢 Verde | Agente IA — se ejecuta automáticamente sin intervención humana |
| 🟡 Amarillo | Captura de datos — se debe registrar información en este punto |
| ⚪ Gris | Entidad externa — otro departamento o persona involucrada |

## Si algo queda sin confirmar

Si tu proceso involucra a otra persona y todavía no validaste los detalles con ella (por ejemplo, un paso que pertenece a otro equipo), decile a Claude que lo marque como pendiente en vez de inventar el detalle. Vas a ver un badge amarillo de "pendiente" en esos casos — es intencional, no un error.

## Publicar tu cambio

Una vez que tu mapa se ve bien:

```
git add .
git commit -m "Agregar mapa de workflow: [tu área]"
git push
```

Si no tenés permiso de escritura directa sobre este repositorio, abrí un Pull Request en su lugar y alguien del equipo lo va a revisar.

## Dudas

Si algo de este proceso no te queda claro, preguntale directamente a Claude Code mientras corrés el comando — está diseñado para ir preguntando en vez de asumir.
