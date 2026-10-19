# Checklist de pruebas (Definition of Done)

Una propuesta (pull request) está **terminada** cuando cumple todos los puntos de esta lista. Si un punto no se cumple, no se fusiona: se abre un issue o se pide cambios en la propia propuesta (ver `QA-WORKFLOW.md`).

Los umbrales están pensados para un sitio estático (`index.html`, `style.css`, `script.js`) y son los que podemos cumplir y mantener en cada cambio.

## Resumen rápido

| Área | Umbral | Herramienta |
|---|---|---|
| HTML | 0 errores; avisos revisados | W3C Markup Validator |
| Accesibilidad (WAVE) | 0 errores, 0 errores de contraste; alertas revisadas | WAVE (extensión) |
| Contraste | Texto ≥ 4.5:1; texto grande, botones y bordes ≥ 3:1 | WAVE / DevTools |
| Foco | Visible en todo elemento interactivo | Teclado (Tab) + DevTools |
| Pantalla | 320, 768 y 1440 px sin scroll horizontal | DevTools modo dispositivo (F12) |
| Botones | Alto y ancho ≥ 44 px | DevTools (inspeccionar) |
| Lighthouse | Rendimiento ≥ 90, Accesibilidad ≥ 95, Buenas prácticas ≥ 90, SEO ≥ 90 | Lighthouse (DevTools) |
| Formularios | Obligatorios, formato de correo y mensajes de error comprobados | Prueba manual |
| Git | Mensajes descriptivos, PR con descripción, rama con nombre válido | Revisión en GitHub |

## 1. Validación del HTML

- [ ] El validador no muestra **ningún error**.
- [ ] Cada **aviso (warning)** se ha revisado: se corrige o se anota por qué se deja.
- [ ] Hay un solo `<h1>` y los encabezados siguen un orden lógico (h1 → h2 → h3).
- [ ] `<html lang="es">`, `<title>` y `<meta name="viewport">` presentes.
- [ ] Todas las imágenes tienen atributo `alt` (vacío solo si son decorativas).

**Herramienta:** https://validator.w3.org/ (pestaña "Validate by direct input" o por URL con el sitio publicado).

## 2. Accesibilidad

### 2.1 WAVE
- [ ] **0 errores**.
- [ ] **0 errores de contraste** ("Contrast Errors").
- [ ] Las **alertas** (alerts) se han revisado una a una; cada alerta que se deja queda justificada en la descripción de la propuesta.

**Herramienta:** extensión WAVE (WebAIM) en el navegador.

### 2.2 Contraste (WCAG 2.1 AA)
- [ ] Texto normal: relación de contraste **≥ 4.5:1** frente al fondo.
- [ ] Texto grande (≥ 24 px, o ≥ 18.66 px en negrita): **≥ 3:1**.
- [ ] **Enlaces:** texto con **≥ 4.5:1** y distinguibles del texto que los rodea por algo más que el color (subrayado, por ejemplo).
- [ ] **Botones:** el texto del botón con **≥ 4.5:1** sobre el color del botón, y el borde o el fondo del botón con **≥ 3:1** frente a lo que lo rodea.

**Herramienta:** WAVE (pestaña Contrast) o el selector de color de DevTools, que muestra la relación de contraste con su ratio.

### 2.3 Indicador de foco
- [ ] Recorriendo la página solo con **Tab / Shift+Tab**, todo enlace, botón y campo muestra un indicador de foco **visible**.
- [ ] El indicador tiene un grosor de **al menos 2 px** y un contraste **≥ 3:1** con el fondo.
- [ ] No hay `outline: none` (ni `outline: 0`) en `style.css` sin un estilo de foco alternativo equivalente.
- [ ] El orden de tabulación coincide con el orden visual.

**Herramienta:** teclado + DevTools.

## 3. Adaptación a la pantalla

Se prueba en tres anchos: 320 px , 768 px y 1440 px.

- [ ] Sin **desplazamiento horizontal** en ninguno de los tres anchos.
- [ ] Las imágenes escalan y no se salen de su contenedor.
- [ ] La rejilla se reorganiza (menos columnas a menor ancho) sin solapar elementos.
- [ ] Tamaño de los botones: área táctil de al menos 44 × 44 px a 320 y 768 px.
- [ ] El texto se lee sin hacer zoom (cuerpo ≥ 16 px).

**Herramienta:** DevTools (F12) → modo dispositivo (Ctrl+Shift+M), introduciendo el ancho manualmente.

## 4. Rendimiento (Lighthouse)

Abrir local host 8080: python -m http.server 8080

- [ ] Rendimiento (Performance): ≥ 90
- [ ] Accesibilidad (Accessibility): ≥ 95
- [ ] Buenas prácticas (Best Practices): ≥ 90
- [ ] SEO: ≥ 90
- [ ] Se anotan las puntuaciones obtenidas en la descripción de la propuesta.

**Herramienta:** Lighthouse en DevTools (pestaña Lighthouse).

## 5. Formularios

- [ ] **Campos obligatorios:** llevan `required` y una indicación visible (no solo color). Enviar el formulario vacío no lo envía y avisa de qué falta.
- [ ] **Formato del correo:** `type="email"`; un valor como `usuario@` o `usuario.com` se rechaza y uno como `usuario@dominio.com` se acepta.
- [ ] **Mensajes de error:** son claros y específicos ("Introduce un correo con formato nombre@dominio.com"), aparecen junto al campo, no dependen solo del color, y están asociados al campo (`aria-describedby` o `aria-live`).
- [ ] Cada campo tiene una `<label>` asociada.
- [ ] Los datos válidos se envían (o se procesan) sin mensajes de error residuales.

**Herramienta:** prueba manual con casos vacío / inválido / válido, y revisión con WAVE.

## 6. Git

- [ ] Mensajes de commit descriptivos.
- [ ] Propuesta (PR) con descripción: qué cambia, por qué, cómo se ha probado y qué issue cierra (`Closes #N`).
- [ ] Nombre de rama válido según `docs/BRANCHING-STRATEGY.md`.
- [ ] Ningún commit directo a `main`.

**Herramienta:** revisión de la propuesta en GitHub y `git log --oneline`.

## Cómo se usa esta lista

1. Quien prueba copia esta lista en un comentario de la propuesta.
2. Marca cada punto y anota el resultado (valor medido, captura si hace falta).
3. Si todo está marcado, se aprueba. Si falla algo, se actúa según `QA-WORKFLOW.md`.