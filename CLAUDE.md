# Instrucciones del proyecto — prediccion_nivel_educativo

Código del paper académico (TFM) sobre predicción del nivel educativo ajustado
por edad (NE) en hogares urbanos argentinos, con microdatos de la EPH (INDEC)
y modelos de machine learning.

## Convenciones

- **Comentarios y commits en español.** Mantener el estilo de banners
  `# ====` / `# ----` del notebook existente.
- **NUNCA agregar `Co-Authored-By: Claude`** ni otra atribución de Claude en
  commits o PRs. Todos los commits van solo con el usuario Santiago Riverti.
- Todos los resultados (tablas CSV/LaTeX, gráficos a 600 dpi y bases
  procesadas) van a la carpeta `outputs/`.
- Este archivo y `.claude/memoria.md` están versionados. Los scripts
  operativos de `.claude/` (push.ps1, runner, logs) son solo locales,
  excluidos vía `.git/info/exclude`.

## Estructura

- `scripts/prediccion_nivel_educativo.ipynb` — pipeline completo (40 celdas):
  descarga EPH con pyeph (configurada en 2024T4), procesamiento, estadística
  descriptiva, regresión (sección 5) y clasificación (sección 6).
- `scripts/analisis_NE.ipynb` — notebook secundario de análisis.
- `document/` — PDFs metodológicos de la EPH y bases de referencia 2023T4
  exportadas a Excel (`df_modelo_estimaciones_2023T4.xlsx` sirve para probar
  código sin descargar datos).
- `outputs/` — tablas generadas al ejecutar el notebook (solo `.gitkeep`
  commiteado).

## Cómo pushear desde esta máquina

Ejecutar el script local `.claude/push.ps1` (no versionado; es
autodocumentado). El push directo con `git push` falla por una
credencial vencida del Credential Manager.

## Estado y pendientes

Ver `.claude/memoria.md` para el estado detallado de la sesión, hallazgos
empíricos y pendientes.
