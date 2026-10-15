# Estrategia de ramas

Reglas para nombrar las ramas y para moverse entre ellas en `sitio-web-qa-testing`. El objetivo es simple: **`main` siempre está estable** y todo cambio llega a ella mediante una propuesta (pull request) revisada.

## `main`

- Es la rama principal y contiene **siempre una versión estable**: que pasa la checklist de `docs/TESTING-CHECKLIST.md`.
- **No se trabaja directamente en `main`.** Nada de commits ni `push` directos.
- Todo cambio llega mediante una **propuesta (PR)** con descripción, revisada y aprobada.
- Si algo inestable llega a `main`, se **revierte con una propuesta** (ver `QA-WORKFLOW.md`), no se reescribe la historia.

## Nombres de rama

Formato: `tipo/descripcion-corta`

- Todo en **minúsculas**, palabras separadas por guiones.
- Sin espacios, tildes ni caracteres especiales.
- Descriptiva y corta.
- Se puede incluir el número de issue: `fix/1-contraste-boton`.

| Tipo | Para qué | Ejemplo |
|---|---|---|
| `feature/` | **Novedades:** funcionalidad o mejora nueva | `feature/mejorar-accesibilidad` |
| `fix/` | **Arreglos:** corrección de un fallo | `fix/scroll-horizontal-320px` |
| `docs/` | Cambios solo de documentación | `docs/checklist-pruebas` |
| `revert/` | Revertir una fusión errónea | `revert/pr-2` |

### Nombres no válidos

| Nombre | Problema |
|---|---|
| `cambios` | No tiene tipo ni describe nada |
| `Mejoras Accesibilidad` | Mayúsculas y espacios |
| `matias-prueba` | Describe a la persona, no el cambio |
| `fix` | Sin descripción |

### Segundas versiones

Si una rama hay que rehacerla (por ejemplo, tras un revert), se crea una rama nueva con sufijo de versión: `feature/mejorar-accesibilidad-v2`. No se reutiliza el nombre de una rama ya fusionada.

## Ciclo de vida de una rama

1. **Se crea desde `main` actualizada:**

   ```bash
   git switch main
   git pull
   git switch -c feature/mi-novedad
   ```

2. **Se trabaja con commits pequeños y descriptivos** (`feat:`, `fix:`, `docs:`).
3. **Se sube y se abre la propuesta** con descripción:

   ```bash
   git push -u origin feature/mi-novedad
   gh pr create --base main --head feature/mi-novedad
   ```

4. **Se revisa y se prueba** según `QA-WORKFLOW.md` y la checklist.
5. **Se fusiona** solo con la revisión terminada y aprobada.
6. **Se borra la rama** (local y remota) y se actualiza `main`.

Reglas de higiene:
- Una rama = un propósito. No se mezclan arreglos con novedades.
- Las ramas viven poco: se fusionan o se descartan en días, no en semanas.
- Se actualiza la rama con `main` antes de pedir revisión: `git merge main` (o `git pull origin main`).

## Etiquetas de versión

Se usa **versionado semántico**: `vMAYOR.MENOR.PARCHE`.

| Parte | Cuándo sube | Ejemplo |
|---|---|---|
| `MAYOR` | Cambio que rompe o rediseña de forma importante el sitio | `v2.0.0` |
| `MENOR` | Novedad nueva que no rompe nada | `v1.1.0` |
| `PARCHE` | Arreglo de un fallo | `v1.1.1` |

**Cuándo se etiqueta:** solo cuando **`main` está estable** (la checklist completa en verde) **justo después de fusionar** una propuesta o un conjunto de propuestas que forman una versión. Nunca se etiqueta una rama de trabajo, ni una `main` sin verificar.

```bash
git switch main
git pull
git tag -a v1.0.0 -m "Primera versión estable: HTML válido, accesibilidad y responsive verificados"
git push origin v1.0.0
```

La primera versión etiquetada es `v1.0.0`; antes de ella, el sitio está en desarrollo y no se etiqueta.