# Plan de pruebas

Proyecto: `sitio-web-qa-testing` (`index.html`, `style.css`, `script.js`)

## 1. Objetivo

Comprobar que el sitio cumple los criterios de calidad definidos en `docs/TESTING-CHECKLIST.md` antes de fusionar cualquier cambio en `main`: HTML válido, accesible, adaptable a distintas pantallas, con buen rendimiento y con formularios que funcionan correctamente.

## 2. Alcance

**Dentro del alcance**
- Validez del HTML.
- Accesibilidad (WAVE, contraste, foco con teclado).
- Adaptación a la pantalla (320, 768 y 1440 px).
- Rendimiento (Lighthouse).
- Formularios (si los hay): campos obligatorios, formato del correo y mensajes de error.
- Calidad del trabajo en Git (commits, propuestas y ramas).

**Fuera del alcance**
- Pruebas de carga o seguridad.
- Navegadores antiguos.
- Pruebas automatizadas (por ahora todas son manuales).

## 3. Entorno de pruebas

| Elemento | Detalle |
|---|---|
| Navegador principal | Google Chrome (última versión) |
| Servidor local | `python3 -m http.server 8000` → http://localhost:8000 |
| Código | Rama de la propuesta, bajada con `git switch <rama>` o `gh pr checkout <n>` |
| Dispositivos | Emulados con DevTools (F12 → modo dispositivo) |

## 4. Herramientas

| Área | Herramienta |
|---|---|
| Validación HTML | W3C Markup Validator |
| Accesibilidad | Extensión WAVE |
| Contraste | WAVE y selector de color de DevTools |
| Pantalla | DevTools, modo dispositivo |
| Rendimiento | Lighthouse (modo móvil, ventana de incógnito) |
| Formularios | Prueba manual |
| Control de versiones | Git y GitHub (issues y pull requests) |

## 5. Casos de prueba

| ID | Área | Qué se prueba | Resultado esperado |
|---|---|---|---|
| TP-01 | HTML | Validar `index.html` en el validador W3C | 0 errores; avisos revisados |
| TP-02 | Accesibilidad | Pasar WAVE por la página | 0 errores y 0 errores de contraste; alertas revisadas |
| TP-03 | Accesibilidad | Contraste de texto, enlaces y botones | Texto ≥ 4.5:1; texto grande y botones ≥ 3:1 |
| TP-04 | Accesibilidad | Recorrer la página con Tab / Shift+Tab | Indicador de foco visible en cada elemento interactivo |
| TP-05 | Pantalla | Abrir el sitio a 320 px | Sin scroll horizontal; botones ≥ 44 px; rejilla reorganizada |
| TP-06 | Pantalla | Abrir el sitio a 768 px | Sin scroll horizontal; imágenes escaladas |
| TP-07 | Pantalla | Abrir el sitio a 1440 px | Sin scroll horizontal; maquetación completa |
| TP-08 | Rendimiento | Lighthouse en modo móvil | Rendimiento ≥ 90, Accesibilidad ≥ 95, Buenas prácticas ≥ 90, SEO ≥ 90 |
| TP-09 | Formularios | Enviar el formulario vacío | No se envía; se indican los campos obligatorios |
| TP-10 | Formularios | Correo con formato inválido (`usuario@`) | Se rechaza con mensaje claro |
| TP-11 | Formularios | Datos válidos | Se envía sin mensajes de error |
| TP-12 | Git | Revisar commits, descripción del PR y nombre de rama | Mensajes descriptivos, PR con descripción, rama con nombre válido |

## 6. Criterios de entrada y de salida

**Entrada (se puede empezar a probar cuando):**
- Existe una propuesta (PR) abierta con descripción.
- La rama tiene un nombre válido según `docs/BRANCHING-STRATEGY.md`.
- El sitio se abre en local sin errores.

**Salida (se da por terminado cuando):**
- Todos los casos de prueba aplicables están en estado "Pasa".
- Los fallos encontrados están corregidos y vueltos a probar, o registrados en un issue aceptado.
- La checklist de `docs/TESTING-CHECKLIST.md` está completa.

## 7. Gestión de fallos

Cuando un caso falla, se sigue `QA-WORKFLOW.md`:
1. Reproducir el fallo y anotar el valor medido.
2. Abrir un issue con pasos para reproducirlo.
3. Comentar en la propuesta enlazando el issue.
4. Corregir, volver a probar y cerrar el issue con `Closes #N`.

## 8. Registro de resultados

Se copia esta tabla en un comentario de la propuesta y se rellena.

| ID | Resultado (Pasa / Falla) | Valor medido | Issue / Nota |
|---|---|---|---|
| TP-01 | | | |
| TP-02 | | | |
| TP-03 | | | |
| TP-04 | | | |
| TP-05 | | | |
| TP-06 | | | |
| TP-07 | | | |
| TP-08 | | | |
| TP-09 | | | |
| TP-10 | | | |
| TP-11 | | | |
| TP-12 | | | |

## 9. Responsabilidades

| Rol | Tarea |
|---|---|
| Desarrollo | Preparar la propuesta, corregir los fallos y volver a pedir revisión |
| QA | Ejecutar los casos, registrar resultados, abrir issues y aprobar o pedir cambios |

## 10. Documentos relacionados

- `docs/TESTING-CHECKLIST.md`: umbrales y herramientas.
- `QA-WORKFLOW.md`: cómo se revisa, se prueba y se fusiona.
- `docs/BRANCHING-STRATEGY.md`: nombres de rama y versiones.