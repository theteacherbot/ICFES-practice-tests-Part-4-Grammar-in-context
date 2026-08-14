# Exam Engine 2.0 · Part 4 — Grammar in Context

Aplicación web de página única (single-file HTML) para practicar inglés gramatical en formato *gap-fill*, alineada con la Prueba Saber 11 (ICFES) — sección de inglés. No requiere instalación, servidor ni dependencias externas: se abre directamente en cualquier navegador.

## Contenido

- **50 lecturas originales** de 250–300 palabras cada una, en niveles **A2, A2+ y B1**.
- **11 espacios numerados por lectura** — `(0)` a `(10)`:
  - **Espacio 0**: ejemplo, con la respuesta correcta ya visible dentro del texto. No puntúa.
  - **Espacios 1–10**: 10 preguntas de opción múltiple (A/B/C), una por cada categoría gramatical:
    1. Pronombre
    2. Forma verbal
    3. Artículo
    4. Preposición
    5. Conector
    6. Cuantificador
    7. Comparativo
    8. Superlativo
    9. Adverbio
    10. Adjetivo
- Cada pregunta incluye una **explicación gramatical** breve, mostrada al finalizar el examen.
- Las opciones de respuesta están distribuidas de forma aleatoria entre A/B/C (no siempre en la misma posición).

## Clasificación temática

Las 50 lecturas están etiquetadas con una de las siguientes categorías, filtrables desde el Banco de Exámenes:

| Categoría | Lecturas |
|---|---|
| Culture | 12 |
| Technology | 7 |
| History | 6 |
| Food | 6 |
| Environment | 5 |
| Animals | 3 |
| Nature | 3 |
| Sports | 3 |
| Science | 2 |
| Travel | 2 |
| Geography | 1 |
| Historical Figures | 0 * |

\* Categoría reservada en la taxonomía; actualmente sin lecturas asignadas.

## Funcionalidades

- **Select Exam** — elige una lectura específica desde el Banco de Exámenes.
- **Random Exam** — inicia una lectura aleatoria del banco completo.
- **Exam Bank** — cuadrícula de las 50 lecturas con filtro por categoría (chips con contador) y estado de completado.
- **Progress** — resumen de exámenes completados, puntaje promedio, mejor puntaje y total de preguntas respondidas/correctas (persistido en el navegador).
- Navegación **Previous / Next** entre preguntas dentro de una lectura, con retroalimentación de correcto/incorrecto y explicación gramatical al finalizar.
- Menú principal con los cuatro accesos (Select Exam, Random Exam, Exam Bank, Progress) dispuestos en fila horizontal, responsivo en pantallas angostas.
- Modo claro/oscuro (toggle en el encabezado).

## Estructura técnica

Todo vive en un único archivo `exam-engine-part4.html`:

- **CSS** embebido con variables de tema (`--color-*`, `--bg-*`, etc.).
- **`EXAM_BANK`**: arreglo JavaScript con las 50 lecturas. Cada objeto tiene:
  ```js
  {
    id, title, topic, category, level, image,
    text,        // texto con marcadores (0) ____ ... (10) ____
    questions: [ // 11 elementos, uno por espacio
      { gap, options: [{id, text}], answer, grammarFocus, explanation, scored }
    ]
  }
  ```
  - `scored: false` únicamente en el gap 0 (ejemplo).
- **Motor de renderizado**: inserta cada `(N) ____` del texto como una "badge" clicable (excepto el ejemplo, que se muestra fijo con su respuesta).
- **Estado y progreso**: se guarda en el navegador entre sesiones (exámenes completados, puntajes, contadores).

## Uso

1. Abre `exam-engine-part4.html` en un navegador (doble clic o arrastrar al navegador).
2. Elige **Select Exam** para escoger una lectura por categoría, o **Random Exam** para practicar al azar.
3. Lee el texto y responde cada espacio numerado (1–10); el espacio (0) es solo de referencia.
4. Pulsa **Finish Exam** para ver tu puntaje, la retroalimentación por pregunta y la explicación gramatical de cada una.
5. Consulta **Progress** para ver tu evolución acumulada.

## Notas de mantenimiento

- Para añadir una nueva lectura: agrega un objeto al arreglo `EXAM_BANK` siguiendo el mismo esquema (11 gaps numerados `(0)`–`(10)` en el texto, 250–300 palabras, categoría válida).
- Las categorías disponibles están definidas en `EXAM_CATEGORIES` dentro del script; añadir una categoría nueva ahí también la habilita como filtro.
- El contador `/50` en la pantalla de progreso refleja el tamaño actual de `EXAM_BANK`; si el banco crece, actualízalo en las dos referencias del código (`progressExamsCompleted`).
