# SindromIA

Patología General (asignatura 1931, Grado en Medicina, Universidad Miguel Hernández de Elche): del síntoma al síndrome, practicando. Aplicación web de un solo fichero (`index.html`), sin servidor.

**Usar la app:** https://fborrasumh.github.io/sindromia/

**Idiomas:** español (por defecto), inglés y portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL).

## Qué hace

- **Diagnóstico inicial**: 12 preguntas, una por aparato y dos de interpretación (gasometría y ECG).
- **Ruta de hoy**: 10 actividades elegidas por el código. Incluye repasos que vencen (intervalos de 1, 3, 7, 16 y 35 días), los síndromes con menor dominio, temas nuevos y un caso clínico. Las actividades se intercalan por aparatos y cada una muestra por qué toca. La ruta se recalcula tras cada respuesta.
- **Tipos de actividad**:
  - preguntas de 4 opciones con seguridad declarada (seguro, dudo, adivino);
  - discriminación entre síndromes con tabla de contraste;
  - explicación del mecanismo con tus palabras, donde el código cuenta los conceptos clave;
  - laboratorio e imagen;
  - matriz de la exploración pulmonar.
- **Mapa de síndromes**: los 47 temas de la guía como guiones de enfermedad (mecanismo, clínica clave, pruebas, diferencial), con comparación lado a lado.
- **Consulta**: 8 pacientes virtuales ficticios.
  - Entrevista con un menú de 46 preguntas o texto libre (reconocimiento por palabras clave, o paciente con IA opcional).
  - Exploración con 30 maniobras y auscultación sintética.
  - 28 pruebas con coste orientativo.
  - Juicio clínico, justificación y presentación breve del caso.
  - Lista de comprobación tipo ECOE calculada por el código. El modo ECOE retira el menú de preguntas.
- **Laboratorio e imagen**: 13 generadores con solución calculada y errores típicos detectados por recálculo: gasometría y ácido-base (Winter y compensaciones), osmolalidad y deshidratación, anemias (índice reticulocitario, Mentzer), ictericia (factor R), FENa, criterios de Light, Friedewald, tiroides, criterios de diabetes, ECG (12 ritmos), radiografía de tórax (8 patrones, índice cardiotorácico), frotis (8 patrones) y fonocardiograma. Tiene tres niveles: ejemplo resuelto, pasos parciales y problema completo.
- **Simulacro de examen**: test de 4 opciones con −0,33 por error, como en la guía, y preguntas cortas con imagen; se corrige al final.
- **Progreso**: dominio por síndrome (Elo con olvido), por resultado de aprendizaje (texto literal de la guía) y preparación orientativa por bloque de evaluación. Exportación e importación JSON e informe en Word.
- **Profesorado**:
  - preguntas e imágenes propias («Imagen-Caso de la Semana»);
  - registro de validación del contenido;
  - analítica de ítems a partir de las copias que el alumnado entregue voluntariamente (dificultad, discriminación, distractores);
  - exportación a Moodle XML y GIFT.

## Cómo se usa la IA (opcional)

Todo funciona **sin clave y sin conexión**. Con la clave propia de OpenAI, Google Gemini o Anthropic Claude, la IA hace tres cosas, y ninguna pone nota:

1. **Paciente con IA**. Responde solo con los hechos de su caso. El código descarta los identificadores de hechos que no existen y retira las respuestas que adelantan el diagnóstico.
2. **Comentario del razonamiento clínico**. El código comprueba que cada cita de tu texto existe, señala las cifras nuevas y elimina las frases que ponen nota.
3. **Comentario de una explicación de mecanismo**, con la misma verificación.

## Privacidad

El progreso, las respuestas y la clave se guardan solo en el navegador (IndexedDB y localStorage). A la IA solo sale lo que se pide comentar, con correos, DNI, teléfonos y el seudónimo enmascarados, y antes del primer envío se muestra lo que sale. Las copias exportadas no llevan la clave ni textos libres.

## Límites

- **Contenido pendiente de validación.** El contenido didáctico (síndromes, preguntas y casos) está pendiente de validación por el profesorado; el panel del profesorado registra lo que se valida.
- **Casos ficticios.** Los costes de las pruebas son orientativos.
- **Imágenes y sonidos esquemáticos.** El ECG, la radiografía, el frotis y el fonocardiograma son esquemas dibujados por código, y la auscultación es sonido sintético. No sustituyen a material clínico real.
- **No sustituye la docencia ni la evaluación.** No sustituye a clases, talleres, seminarios ni prácticas hospitalarias, ni a la evaluación de la guía (examen 70 %, evaluación continua 20 %, ECOE 10 %).
- **Efecto no medido.** Las técnicas usadas tienen evidencia publicada (referencias con DOI en la sección «Cómo aprende SindromIA»), pero el efecto de esta app en la asignatura no se ha medido.

## Comprobaciones realizadas

- **Lógica en Node** (`tests/logica.test.js`):
  - integridad de datos;
  - 300 variantes por generador, con determinismo y soluciones aceptadas o rechazadas;
  - casos de verdad conocida (Winter, FENa, pH);
  - motor adaptativo: mismos eventos producen la misma ruta, olvido y repasos.
- **Navegador** (`tests/smoke_sindromia.py`, Playwright):
  - IA simulada que inventa citas, identificadores y diagnósticos, pone nota y devuelve un error 429;
  - tres proveedores;
  - idiomas sin huecos;
  - accesibilidad WCAG 2.2 AA con axe-core (cero infracciones en escritorio y móvil);
  - funcionamiento sin red;
  - móvil sin desbordes en los tres idiomas;
  - CPU ×4;
  - descargas (Word, JSON, Moodle XML, CSV);
  - persistencia tras recargar.
- **Sin probar** con claves reales de IA.

## Adoptarla en otra universidad o asignatura

Cambia `src/curriculum.js` (ficha, resultados de aprendizaje, unidades y evaluación), `src/sindromes.js` y `src/casos.js`, traduce lo nuevo en `src/i18n.js` y ejecuta `python3 build.py`. El código no necesita cambios.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche) y José Manuel Ramos Rincón (Universidad Miguel Hernández de Elche, Departamento de Medicina Clínica).

Construida a partir de la Guía Docente de Patología General (asignatura 1931, curso 2026-2027) del Grado en Medicina de la UMH.

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573) · José Manuel Ramos Rincón [0000-0002-6501-9867](https://orcid.org/0000-0002-6501-9867)

## Cómo citar

Borrás Rocher, F. y Ramos Rincón, J. M. (2026). *SindromIA* (v1.0.0) [Software]. (DOI en trámite)

## Licencia

MIT (software). Véase [LICENSE](LICENSE).
