Este documento recogerá el resultado de las pruebas de validación realizadas a la documentación del proyecto.
Se ha utilizado los navegadores Chrome y Firefox para estas pruebas.

Prueba 1. Validez del HTML.
    No se muestran errores de validación W3

Prueba 2. Accesibilidad con WAVE. 
    Se marcan dos errores de contraste ubicados en el pie de página. 

Prueba 3. Adaptación a la pantalla.  
    -320px
        aparece desplazamiento horizontal: No.
        altura real de los botones en píxeles: 48px.
        existe escalación de las imágenes: n/a no hay <img> ni imágenes de fondo
        reorganización de la rejilla: Se ordenan las tarjetas de forma vertical: 1 columna (6 filas), 288 px por tarjeta

    -768px
        aparece desplazamiento horizontal: No.
        altura real de los botones en píxeles: 48px.
        existe escalación de las imágenes:  n/a no hay <img> ni imágenes de fondo
        reorganización de la rejilla: 2 columnas (3 filas), 356 px por tarjeta.

    -1440px
        aparece desplazamiento horizontal: No.
        altura real de los botones en píxeles: 48px.
        existe escalación de las imágenes:  n/a no hay <img> ni imágenes de fondo
        reorganización de la rejilla:

Prueba 4. Contraste.
Modo claro predeterminado:
   - Pareja de texto normal y texto de pie de página: En el texto de pie de página se encuentra un fallo, y no pasa la validación, el texto es invisible con respecto al fondo, con una ratio de 1. El texto de la web pasa la validación con una ratio de 14.67.
   - Pareja botones primario y secundario: cumplen con una ratio 5.16
   - Pareja enlaces navegación y footer: los enlaces de navegación cumplen con una ratio de 14.67 y el del footer pasa con un 2,84.

Modo oscuro:
   - Pareja de texto normal y texto de pie de página: el texto normal pasa con una ratio 13.33 y el texto de footer falla con una ratio de 1.
   - Pareja botones primario y secundario: el botón primario cumple con una ratio de 5.16, sin embargo el botón secundario falla con una ratio de 2.84
   - Pareja enlaces navegación y footer: los enlaces de navegación pasan con una ratio de 13.33, y el enlace de footer pasa con 4.69

Prueba 5. Navegación con teclado.
Prueba 6. Rendimiento con Lighthouse.