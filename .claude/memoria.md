# Memoria del proyecto — prediccion_nivel_educativo

Última actualización: 2026-06-11 (README reescrito en inglés; historia git
purgada de cualquier atribución de Claude; ver HANDOFF abajo).

---

## ⭐ ESTADO ACTUAL / HANDOFF (leer esto primero)

**Qué es:** repo del paper/TFM sobre nivel educativo (NE) en hogares
argentinos con datos EPH (INDEC) y ML. Repo público:
https://github.com/santiagoriverti/prediccion_nivel_educativo (rama `main`).
Notebook único: `scripts/prediccion_nivel_educativo.ipynb` (40 celdas,
Colab-ready). Local: `C:\Users\sriverti\Desktop\INECO\Repositorios\prediccion_nivel_educativo`.

**Estado del código (correcto y consistente):**
- Modelos usan SOLO los 8 predictores del paper + efectos fijos de región
  (REGION dummies, drop_first): HAC, INDICE_CARGA_COMPARTIDA, EDA, FEM,
  ANALFABET, DESOCUP, INACTIVOS, IPCF. (Antes usaba ~98 → corregido.)
- Protocolo: split 75/25 sin estratificar; CV 10-fold sobre muestra completa;
  inner CV 5-fold para lineales; GridSearch solo en RF clf; k=5 fijo en KNN.
- Todos los resultados se guardan en `outputs/`. Gráficos en inglés a 600 dpi.
- 10 tablas .tex (booktabs, inglés) + csv; AUC/AP se exporta a `auc_ap.tex/csv`.

**Valores de verificación (deterministas, datos 2024T4, n=15.906, τ=0.78):**
- Regresión XGBoost: R² test = 0.3665 | CV 10-fold = 0.3490 ± 0.0284.
- Clasif. XGBoost: Accuracy = 0.7053, F1 = 0.7069 (test).
- AUC: XGB 0.7812 | RF 0.7706 | SVM 0.7544 | Logit 0.7307 | KNN 0.6980.
- AP:  XGB 0.7765 | RF 0.7597 | SVM 0.7293 | Logit 0.7007 | KNN 0.6509.
- **Logit OR de ANALFABET = 0.91** (reproduce el paper).

**⚠️ Importante para continuar:** los `outputs/` commiteados son del commit
`bc99842` (ANTES de pasar figuras a inglés y de agregar `auc_ap`). El CÓDIGO
está adelantado respecto de esos outputs. Para tener los outputs finales
(7 PNG en inglés + `auc_ap.tex/csv` + tablas), **el usuario debe re-ejecutar
el notebook**; al ser determinista dará los valores de arriba. En las últimas
sesiones se commiteó solo código + memoria (el usuario regenera los outputs).

**Pendientes:** (1) redactar sección ANALFABET (narrativa de supresión por
estructura etaria, OR=0.91, tabla `analfabet_biv.tex`: cruda nula p=0.17, con
EDA+IPCF β=−0.13 p<0.001); (2) unificar comentarios "2023"/"2024" en el
notebook; (3) opcional: robustez recalculando ANALFABET sin menores
(CH06≥10) para confirmar el mecanismo de supresión.

**Operativa:** push con `.claude/push.ps1` (git push directo falla por
credencial vencida). Runner local en `.claude/_run_nb.py`. Commits SIEMPRE
sin Co-Authored-By (config global ya puesta). Scripts de `.claude/`
(push.ps1, _run_nb.py, run_log.txt) y `pyeph/` excluidos vía
`.git/info/exclude`; `CLAUDE.md` y `.claude/memoria.md` SÍ versionados.

---

## Qué es el proyecto

Repo: https://github.com/santiagoriverti/prediccion_nivel_educativo (rama `main`).
Código del paper/TFM sobre nivel educativo en hogares argentinos (EPH, INDEC).
Variable central: **NE** = IEAE promedio del hogar (escolaridad alcanzada /
esperada, ajustada por edad, acotada a 1). Clasificación binaria de **pobreza
educativa**: `PE = 1 si NE < τ`, con τ = mediana de NE (≈ 0.78).

Modelos: regresión (Lasso, Ridge, ElasticNet, RF, SVR, XGBoost, KNN) y
clasificación (Logit, RF con GridSearch, SVM, XGBoost, KNN). Interpretabilidad
con SHAP. El notebook descarga la EPH con `pyeph` (hoy configurado: año=2024,
trimestre=4, celda 5).

## Trabajo hecho (sesión 2026-06-10)

Tres modificaciones agregadas al notebook `scripts/prediccion_nivel_educativo.ipynb`
(commits `e04d3f2` y `c1682b8`, pusheados):

1. **Robustez del umbral τ** (celda 38, al final de la sección 6): reentrena
   los 5 clasificadores para τ ∈ {0.70, 0.75, 0.78, 0.83}; tabla multiíndice
   (Modelo, τ) con Accuracy/Precision/Recall/F1 → `outputs/robustez_umbral.csv`
   y `.tex`. RF usa `grid_search.best_params_`.
2. **Regresión bivariada NE ~ ANALFABET** (celda 27, después del Logit):
   OLS sin controles y con EDA + IPCF (statsmodels) → `outputs/analfabet_biv.txt`
   (summaries completos) y `.tex` (tabla comparativa).
3. **CV 10-fold de clasificación** (celda 37): StratifiedKFold(10), media ±
   desvío de Accuracy/F1/Precision/Recall, ordenada por Accuracy →
   `outputs/cv_clasificacion.csv` y `.tex` (formato `media $\pm$ desvío`).

Además:
- Badge de Google Colab en el README (apunta a
  `scripts/prediccion_nivel_educativo.ipynb` en `main`) + sección Reproducibility.
- Celda pip: `!pip install --quiet pyeph scikit-learn openpyxl shap xgboost
  statsmodels jinja2`.
- **Fix importante**: `pd.get_dummies(..., dtype=int)` en celdas 19, 26 y 28.
  Con pandas ≥ 2.0 las dummies salen bool y `select_dtypes(include=[np.number])`
  las descartaba TODAS (X quedaba con ~8 columnas en vez de ~90). Sin este fix,
  los resultados en Colab no reproducen el paper.
- Fix: import faltante de `precision_score` en la celda 3 (la celda de la
  tabla final de clasificación habría dado NameError).
- Historia reescrita con filter-branch para quitar trailers Co-Authored-By
  (force-push hecho; los hashes viejos 3370768/500ba33 ya no existen).

## Verificación realizada

- Las 40 celdas compilan como Python válido.
- Las 3 celdas nuevas se ejecutaron de punta a punta contra una muestra de
  1500 filas de `document/df_modelo_estimaciones_2023T4.xlsx` (con stub de
  grid_search). Generaron los 6 archivos de outputs correctamente; LaTeX
  revisado a mano (multirow, $\pm$, $\tau$ a 2 decimales OK).
- El notebook completo NO se volvió a ejecutar (requiere descarga EPH +
  entrenamientos largos). Los outputs/ reales están pendientes de generar.

## Hallazgo empírico clave (¡revisar antes de redactar el paper!)

La hipótesis del usuario era: relación cruda NE~ANALFABET positiva que se
invierte al controlar (efecto de supresión), para explicar el OR = 0.91 del
Logit. **Con la base completa 2023T4 (n = 15.840) NO se cumple**:
- Sin controles: β = −0.0349 (EE 0.0100, p ≈ 0.0005) → ya es negativa (intuitivo).
- Con EDA + IPCF: β = −0.1446 (EE 0.0093, p ≈ 1.6e-54) → más negativa aún.
El signo no se invierte con esos dos controles. Posibilidades: correr con la
base definitiva 2024T4, o el efecto de supresión aparece recién con el set
completo de controles del Logit (que además usa variables estandarizadas).
Discutir con el usuario cómo reportarlo.

## Resultados de la ejecución completa (2026-06-10, tarde, datos 2024T4)

El usuario ejecutó el notebook completo (n = 15.906 hogares; mediana NE = 0.783;
las 3 celdas nuevas funcionaron y generaron los 6 archivos de outputs/).
El fix de dummies opera bien: las categóricas aparecen en SHAP y Logit
(II8_2, V2_2, REGION_44, etc.).

**ANALFABET bivariada (resultado definitivo 2024T4):**
- Sin controles: β = −0.0144 (EE 0.0104, p = 0.166) → **NULA, no significativa**.
- Con EDA + IPCF: β = −0.1316 (EE 0.0096, p < 0.001) → negativa fuerte.
- Logit completo (sobre PE, estandarizado): β = −0.1726, OR = 0.84.

Narrativa acordada para el paper: hay efecto de supresión pero invertido
respecto de la hipótesis original. La relación cruda es nula porque los
hogares con niños pequeños tienen ANALFABET alto (CH09=2 en niños de 2-5
años que aún no leen) y a la vez NE alto (un niño con EDA_ESC = EDA_ESP
tiene IEAE = 1); esa correlación espuria positiva cancela la relación
negativa del analfabetismo adulto. Al controlar por EDA emerge el efecto
negativo. El signo del Logit (protector) sigue siendo opuesto al OLS con
controles mínimos → el coeficiente es inestable según el conjunto de
controles; reportarlo como supresión/colinealidad con la estructura etaria,
sin lectura causal de "factor protector".

**CV 10-fold clasificación (media ± std):** XGBoost 0.7293 ± 0.0122 >
RF 0.7249 ± 0.0106 > SVM 0.7163 ± 0.0099 > Logit 0.7142 ± 0.0084 >
KNN 0.6354 ± 0.0090 (Accuracy). Ranking consistente con el 5-fold.

**Robustez τ:** ranking de modelos estable en los 4 umbrales (XGBoost
siempre primero). Patrón esperable: con τ = 0.70 cae el Recall (clase
positiva chica, ~25%); con τ = 0.83 sube F1 (~0.84) por prevalencia alta.
Accuracy en τ = 0.78 es la más baja (clases balanceadas, el problema es
más difícil) — vale aclararlo en el paper para que no parezca un defecto.

**Otros resultados de la corrida:** regresión XGBoost R² CV = 0.4278;
SHAP regresión y clasificación dominados por EDA, luego IPCF, V2_2
(jubilación), II8_2 (combustible), FEM, INACTIVOS. RF grid: max_depth=20,
min_samples_leaf=2.

## Cambios posteriores (2026-06-10, tarde 2)

- **document/ depurado**: eliminados `PTFM_SR.pdf`, `PTFM_SR.txt` y
  `bibliografia.bib.txt` (pedido del usuario).
- **Todos los archivos generados van ahora a `outputs/`**: las 3 bases
  xlsx (df_hogar, df_individual, df_modelo_estimaciones 2024T4), los 5
  gráficos de distribución, las curvas PR/ROC y las tablas csv/tex
  (16 escrituras auditadas, todas con prefijo `outputs/`).
- **Gráficos a 600 dpi**: los `savefig` de distribución pasaron de 300 a
  600 dpi y `plt.rcParams['savefig.dpi'] = 600`. PR/ROC ya estaban en 600.
- `os.makedirs("outputs", exist_ok=True)` agregado al final de la celda 3
  (imports), así cualquier celda puede escribir sin fallar.
- README actualizado: estructura sin PTFM_SR.pdf y descripción de outputs/
  con tablas + gráficos 600 dpi + bases procesadas.

## CORRECCIÓN MAYOR (2026-06-10, tarde 3): predictores restringidos al paper

El usuario detectó que los modelos usaban ~98 predictores (todas las
variables del hogar dummificadas) cuando el paper especifica SOLO:
HAC, INDICE_CARGA_COMPARTIDA, EDA, FEM, ANALFABET, DESOCUP, INACTIVOS,
IPCF + efectos fijos de REGION (dummies drop_first, GBA referencia) =
13 columnas. Corregido en las celdas 19 (regresión), 26 (Logit) y 28
(RF clf); la lista vive en `predictores_paper`. Outputs embebidos del
notebook limpiados (eran del feature set viejo).

Exportaciones nuevas agregadas (todas en inglés, booktabs):
- `desempeño_regresion.tex` (celda 22, test set, 7 modelos)
- `cv_regresion.tex` (celda 24, ahora 10-fold, media ± desvío)
- `logit_odds.tex` (celda 26, top 10 coef/OR, underscores escapados)
- `desempeño_clasificacion.tex` (celda 32, τ = 0.78)
- `shap_regresion.tex` / `shap_clasificacion.tex` (celdas 23/33, top 10,
  escape=True)
Captions de las 3 tablas previas (biv, cv_clasif, robustez) pasadas a
inglés; etiquetas de modelo 'Modelo'→'Model' en los .tex.

**Notebook ejecutado completo LOCALMENTE desde cero** (pyeph descargó
2024T4; 6.7 min; runner: exec celda por celda con MPLBACKEND=Agg).
Los 22 archivos quedaron en `outputs/` del repo (tablas + 7 png 600dpi
+ 3 xlsx) y fueron commiteados.

**Valores de verificación contra el paper (feature set correcto):**
- Regresión XGBoost: R² test = 0.3665; CV 10-fold = 0.3490 ± 0.0284.
- Clasificación XGBoost (τ=0.78): Accuracy = 0.7053, F1 = 0.7069 (test).
- **Logit ANALFABET: OR = 0.9122 ≈ 0.91 → COINCIDE con el paper** (con
  98 features daba 0.84). Confirma que este es el feature set del paper.
- Ranking clf: XGBoost > RF > SVM > Logit > KNN (igual que antes).
- Logit top: EDA (OR 2.56), IPCF (0.65), FEM (0.73), HAC (1.20),
  REGION_44 (1.20), INDICE_CARGA_COMPARTIDA (0.88), ANALFABET (0.91).

## Pendientes para la próxima sesión

1. Redactar en el paper la sección de ANALFABET con la narrativa de
   supresión por estructura etaria (ver arriba), ahora con OR = 0.91
   reproducido y la tabla `analfabet_biv.tex` (cruda nula p=0.17;
   con EDA+IPCF β=−0.13 p<0.001).
2. Detalle menor: comentarios del notebook dicen "4º trimestre 2023" pero la
   descarga es 2024T4; unificar si se desea.
3. Posible análisis de robustez adicional: recalcular ANALFABET excluyendo
   menores (p. ej. CH06 >= 10) para confirmar el mecanismo de supresión.
4. El runner local quedó en `.claude/_run_nb.py` (si se borra, rehacer:
   exec celda a celda saltando líneas ! y %, cwd=repo, MPLBACKEND=Agg).

## Entorno / operativa

- Repo local: `C:\Users\sriverti\Desktop\INECO\Repositorios\prediccion_nivel_educativo`
  (movido desde `C:\Users\sriverti\proyectos` el 2026-06-10).
- Python local 3.14: tiene pandas 3.0.3, statsmodels, sklearn, xgboost, jinja2
  (instalados en esta sesión). En consola Windows usar `$env:PYTHONUTF8='1'`
  para los prints con "✓"/"τ".
- **Push a GitHub**: ejecutar el script local `.claude/push.ps1` (no
  versionado, autodocumentado). `git push` directo falla por una credencial
  vencida del Credential Manager; el script resuelve la autenticación sin
  exponer secretos.
- Commits SIEMPRE sin Co-Authored-By (preferencia global del usuario, ya
  configurada en `~/.claude/settings.json` con attribution vacía).
- Este archivo y el `CLAUDE.md` de la raíz están versionados en el repo.
  Los scripts operativos de `.claude/` (push.ps1, _run_nb.py, run_log.txt)
  y la caché `pyeph/` quedan excluidos vía `.git/info/exclude`.

## Sesión 2026-06-11: curvas ROC/PR en formato paper + valores AUC/AP

Claude chat (que edita el main_anon.tex en Overleaf) pidió: (a) regenerar
las figuras de curvas con etiquetas en inglés y sin título, y (b) los
valores de AUC y AP del modelo nuevo (los del paper eran del feature set
viejo). Hecho:
- Celda 36 (ROC): "False Positive Rate" / "True Positive Rate",
  leyenda "Chance (AUC = 0.5)", sin título. Celda 35 (PR): sin título
  (ejes ya estaban en inglés). Ambas imprimen ahora AUC/AP por consola
  (loop al final que lista cada modelo con su valor).
- **Commit solo del CÓDIGO** (notebook) + esta memoria. Los outputs NO se
  commitearon en esta sesión: el usuario ejecuta el notebook y genera las
  figuras y valores oficiales (al ser deterministas coincidirán con los de
  abajo, que provienen de una corrida local de verificación).

**Valores de referencia (corrida de verificación, feature set correcto,
τ = 0.78; el usuario debe regenerarlos):**
- AUC:  XGBoost 0.7812 | RF 0.7706 | SVM 0.7544 | Logit 0.7307 | KNN 0.6980
- AP:   XGBoost 0.7765 | RF 0.7597 | SVM 0.7293 | Logit 0.7007 | KNN 0.6509
(En el paper viejo: AUC 0.775/0.768/0.753/0.729/0.705 — el orden de
modelos se mantiene.)
La figura `outputs/curva_ROC_modelos.png` (ya en inglés) reemplaza a
`figuras/curvas_ROC_PE.png` en Overleaf.

**Pendiente de esta sesión**: el usuario corre el notebook, copia los
AUC/AP de la consola (celdas 35 y 36) para Claude chat, y sube el nuevo
`curva_ROC_modelos.png` a Overleaf.

### Tarea A y B (2026-06-11, continuación)

A) **Los 7 gráficos quedan en inglés**. Se tradujo la celda 15 (los 5
gráficos de distribución del NE): ejes "Educational Level (NE)" /
"Frequency" / "Density", anotaciones "Mean" / "Median", y leyendas
"Region" / "Urban agglomeration" / "Age (years)" / "IPCF decile
(national)". Los nombres de regiones/aglomerados son topónimos y quedan
igual. Las curvas ROC/PR ya estaban en inglés.

B) **AUC y AP ahora se exportan a archivo**: la celda 36 genera
`outputs/auc_ap.csv` y `outputs/auc_ap.tex` (booktabs, ordenado por AUC
desc, columnas Model/AUC/AP), además de imprimirlos en consola. Ya no
hay que copiarlos de la leyenda.

Total de tablas exportadas ahora: 9 .tex (+ csv donde aplica). El usuario
debe re-ejecutar el notebook para regenerar los 7 PNG en inglés y la
nueva tabla auc_ap. Solo se commiteó código + memoria (sin outputs).

## Cierre 2026-06-10

Estado: el repo está completo y consistente. El notebook usa los 8
predictores del paper + FE de región, fue ejecutado entero localmente con
datos EPH 2024T4, y los 22 archivos de resultados están commiteados en
`outputs/` (commit `bc99842`). El usuario va a descargar las tablas desde
GitHub para integrarlas al artículo. OR de ANALFABET = 0.91 reproducido.
Mañana se continúa con los pendientes listados arriba (redacción de la
sección ANALFABET, unificar comentarios 2023/2024, posible robustez de
ANALFABET sin menores).

## Sesión 2026-06-11 (cierre): README en inglés + limpieza de atribución

- **README reescrito 100% en inglés** (título, descripción, features,
  protocolo de evaluación, estructura, tabla de contenidos de `outputs/`,
  reproducibilidad Colab/local, workflow git). Documenta los 8 predictores,
  el protocolo 75/25 + CV 10-fold + inner 5-fold + k=5, y lista las 10
  tablas, 7 figuras y 3 xlsx de `outputs/`.
- **Atribución de Claude eliminada por completo**: se verificó que los 57
  commits están autorados por Santiago Riverti y que NINGUNA cuenta "Claude"
  es colaboradora (los push usan el token propio del usuario). Quedaban dos
  commits viejos con trailer `Co-Authored-By: Claude` SOLO en el backup
  local de filter-branch (`refs/original/`, hashes 3370768/500ba33, ya
  ausentes de GitHub); se borró el ref, se expiró el reflog y se hizo
  `gc --prune=now`. Verificación final: `git log --all --grep=Co-Authored`
  → vacío. No hay colaborador de GitHub que remover (Claude nunca lo fue).
- Config global ya garantiza commits sin Co-Authored-By a futuro.
