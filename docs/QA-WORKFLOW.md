# Flujo de trabajo de QA

Cómo se trabaja en este repositorio (`sitio-web-qa-testing`): cómo se revisa una propuesta, cómo se prueba una rama, qué hacer ante un fallo y qué hacer tras fusionar.

Documentos relacionados:
- `docs/TESTING-CHECKLIST.md`: qué debe cumplirse para dar algo por terminado.
- `docs/BRANCHING-STRATEGY.md`: nombres de rama y reglas.

Las órdenes usan Git y, donde se indica, la CLI de GitHub (`gh`). Todo se puede hacer también desde la web de GitHub.

---

## 1. Cómo se revisa una propuesta (pull request)

1. Lee la descripción de la propuesta: qué cambia, por qué y cómo se probó. Si la descripción está vacía, se pide que la complete antes de revisar.
2. Mira los archivos modificados (pestaña *Files changed*): ¿el cambio coincide con lo que dice la descripción? ¿Hay cambios que no tienen que ver?
3. Comprueba la rama y los commits: nombre de rama válido y mensajes descriptivos.
4. Baja la rama y pruébala (ver sección 2).
5. Pasa la checklist de `docs/TESTING-CHECKLIST.md` y anota los resultados en un comentario.
6. **Decide:**
   - Todo cumple → *Approve* (aprobar).
   - Algo falla → *Request changes* y sigue la sección 3.
7. **No se fusiona hasta que la revisión haya terminado.** Aprobar y fusionar son dos pasos distintos; fusionar sin haber completado la revisión es lo que ocurrió con el PR #2 (ver caso real).

---

## 2. Cómo se baja una rama para probarla

```bash
# 1. Asegúrate de partir de un estado limpio
git status

# 2. Trae las ramas nuevas del remoto
git fetch origin

# 3. Cámbiate a la rama de la propuesta
git switch feature/mejorar-accesibilidad-v2
#    (si es la primera vez, Git la crea siguiendo a origin/...)

#    Alternativa con la CLI de GitHub, usando el número de la propuesta:
gh pr checkout 4

# 4. Sirve el sitio en local
python3 -m http.server 8000
#    y abre http://localhost:8000 en el navegador

# 5. Prueba (checklist) y, al terminar, vuelve a main
git switch main
```

Si la rama ya estaba bajada y se ha actualizado:

```bash
git switch feature/mejorar-accesibilidad-v2
git pull
```

---

## 3. Qué se hace al encontrar un fallo

1. **Reprodúcelo** al menos una vez más para confirmar que es real.
2. **Anota los datos:** qué esperabas, qué pasó, en qué ancho de pantalla / navegador, y el valor medido.
3. **Abre un issue** con título descriptivo y los pasos para reproducirlo.
4. **Comenta en la propuesta** enlazando el issue (`Relacionado con #N`) y marca *Request changes* si el fallo impide fusionar.
5. **Quien desarrolla corrige en la misma rama**, con commits descriptivos, y avisa en la propuesta. Si el arreglo es independiente, va en una rama `fix/...` aparte.
6. **Vuelve a probar** desde el principio de la sección 2 (`git pull` + checklist) y aprueba cuando cumpla.
7. El issue se cierra con la propuesta que lo arregla, escribiendo `Closes #N` en su descripción, y se comenta qué se comprobó.

---

## 4. Qué se hace después de fusionar

```bash
# 1. Vuelve a main y actualízala
git switch main
git pull

# 2. Verifica main: sirve el sitio y repasa los puntos críticos de la checklist
python3 -m http.server 8000

# 3. Borra la rama ya fusionada (local y remota)
git branch -d feature/mejorar-accesibilidad-v2
git push origin --delete feature/mejorar-accesibilidad-v2

# 4. Limpia referencias antiguas
git fetch --prune
```

Además:
- Confirma que los issues enlazados se han cerrado y deja un comentario con el resultado.
- Si el cambio completa una versión, se etiqueta (ver `docs/BRANCHING-STRATEGY.md`).

### Si se fusionó algo por error

`main` debe quedar estable. No se borra la historia: se **revierte** con una propuesta.

```bash
git switch main
git pull
git switch -c revert/pr-2
git log --oneline --merges -5            # localiza el commit de fusión
git revert -m 1 <hash-del-commit-de-fusion>
git push -u origin revert/pr-2
gh pr create --base main --head revert/pr-2 \
  --title "Revertir PR #2 (fusionado antes de terminar la revisión)" \
  --body "Revierte la fusión del PR #2 para dejar main estable. El trabajo se recupera en una rama nueva."
```

Para **recuperar el trabajo** después de revertir:

```bash
git switch main
git pull
git switch -c feature/mejorar-accesibilidad-v2
git revert <hash-del-commit-del-revert>   # deshace el revert y reaplica los cambios
git push -u origin feature/mejorar-accesibilidad-v2
gh pr create --base main --head feature/mejorar-accesibilidad-v2
```

---

## 5. Caso real de esta actividad

Este es el recorrido real de la actividad, que sirve de ejemplo de todo lo anterior.

| Paso | Qué ocurrió | Qué debería haberse hecho / qué se hizo |
|---|---|---|
| 1 | Se abrió el **PR #2** desde `feature/mejorar-accesibilidad`. | Correcto: rama de novedad con nombre válido. |
| 2 | El PR #2 se **fusionó por error** antes de terminar la revisión. | La revisión no había terminado: aprobar y fusionar son pasos separados (sección 1, punto 7). |
| 3 | Se abrió el **issue #1** para documentar los fallos encontrados y se comentó con los resultados de las pruebas. | Sección 3: reproducir, anotar valores, abrir issue. |
| 4 | Se hizo la **Prueba 3 de adaptación a la pantalla**: F12 en modo dispositivo a **320, 768 y 1440 px**, anotando scroll horizontal, altura de botones, escalado de imágenes y reorganización de la rejilla. | Sección 3 de la checklist, con los umbrales de 44 px de botón y 0 scroll horizontal. |
| 5 | Para dejar `main` estable se abrió el **PR #3**, que **revierte** el PR #2, y se fusionó. | Sección 4, "Si se fusionó algo por error". |
| 6 | Para recuperar el trabajo de accesibilidad se abrió el **PR #4** desde `feature/mejorar-accesibilidad-v2`, que reaplica los cambios del PR #2. | Se revisa ahora siguiendo las secciones 1 y 2, y no se fusiona hasta pasar la checklist. |

### Lección aprendida

1. Fusionar solo con la checklist completada y la aprobación explícita.
2. Si algo se fusiona por error, se revierte con una propuesta (no se fuerza el historial).
3. El trabajo no se pierde: se recupera en una rama nueva con nombre válido y se vuelve a revisar.