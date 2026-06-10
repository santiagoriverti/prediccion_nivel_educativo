# 📊 Predicción del Nivel Educativo en Hogares Argentinos

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/santiagoriverti/prediccion_nivel_educativo/blob/main/scripts/prediccion_nivel_educativo.ipynb)

Este proyecto aplica técnicas de aprendizaje automático para predecir el **nivel educativo ajustado por edad (NE)** en hogares urbanos de Argentina. Utiliza microdatos de la **Encuesta Permanente de Hogares (EPH)** del INDEC correspondientes al **4º trimestre de 2023**.

El NE se calcula como la razón entre la escolaridad alcanzada y la escolaridad esperada de los miembros del hogar. Esta métrica permite identificar **rezagos educativos estructurales**, más allá del nivel educativo promedio, considerando la composición etaria del hogar.

---

## 📌 Objetivo

Desarrollar modelos predictivos de regresión y clasificación para estimar el nivel educativo del hogar, y explorar la importancia relativa de variables sociodemográficas, estructurales y de ingresos.

---

## ⚙️ Funcionalidades principales

- ✅ Descarga y validación automática de bases EPH (`pyeph`)
- 🧹 Limpieza y filtrado de variables según rangos válidos establecidos en la documentación oficial
- 🧠 Cálculo de escolaridad efectiva (`EDA_ESC`), esperada (`EDA_ESP`) e índice educativo ajustado (`IEAE`)
- 🏡 Agregación de indicadores al nivel hogar (edad, género, analfabetismo, ocupación, hacinamiento, etc.)
- 📈 Modelos de regresión: `Lasso`, `Ridge`, `ElasticNet`, `Random Forest`, `SVR`, `XGBoost`, `KNN`
- 🧮 Clasificación binaria de hogares con bajo nivel educativo (`PE`) usando `Logistic Regression`, `RandomForestClassifier`, `SVM`, `XGBoostClassifier`, `KNN`
- 📊 Visualización de resultados: histogramas, KDE, curvas ROC y PR, matrices de correlación
- 🔍 Interpretabilidad con SHAP y Permutation Importance (regresión y clasificación)
- 🧪 Evaluación de desempeño: `R²`, `RMSE`, `accuracy`, `precision`, `recall`, `F1-score`
- 🔁 Validación cruzada 5-fold para regresión y clasificación
- 🔁 Validación cruzada 10-fold para clasificación con exportación a CSV y LaTeX (`outputs/cv_clasificacion.*`)
- 🎯 Selección de umbral óptimo para clasificación (basado en curva ROC)
- 🧪 Análisis de robustez del umbral de pobreza educativa τ ∈ {0.70, 0.75, 0.78, 0.83} (`outputs/robustez_umbral.*`)
- 📉 Regresión bivariada de NE sobre ANALFABET, con y sin controles, para analizar el efecto de supresión (`outputs/analfabet_biv.*`)

---

## 📁 Estructura del proyecto

```text
├── scripts/
│   └── notebook con el pipeline completo
├── outputs/
│   └── tablas exportadas (CSV y LaTeX) generadas por el notebook
├── document/
│   ├── PTFM_SR.pdf                            # Proyecto académico
│   ├── EPH_registro_4T2023.pdf                # Diccionario de variables
│   ├── EPH_consideraciones_metodologicas_2t20.pdf
│   ├── EPH_nota_metodologica_1_trim_2019.pdf
│   └── Bases exportadas (df_*.xlsx)
```

---

## 🔁 Reproducibility

El pipeline completo está en [`scripts/prediccion_nivel_educativo.ipynb`](scripts/prediccion_nivel_educativo.ipynb). Los datos de la EPH se descargan automáticamente con `pyeph`, por lo que no hace falta descargar nada a mano.

### ▶️ Correr en Google Colab (recomendado)

1. Hacer clic en el badge [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/santiagoriverti/prediccion_nivel_educativo/blob/main/scripts/prediccion_nivel_educativo.ipynb) al inicio de este README.
2. Ejecutar todas las celdas en orden (`Entorno de ejecución → Ejecutar todas`). La primera celda instala las dependencias que no vienen por defecto en Colab:

   ```python
   !pip install --quiet pyeph scikit-learn openpyxl shap xgboost statsmodels jinja2
   ```

3. Las tablas exportadas (CSV y LaTeX) quedan en la carpeta `outputs/` del entorno de Colab (panel lateral de archivos), desde donde se pueden descargar.

### 💻 Correr en local

Requiere Python 3.10 o superior.

```bash
# 1. Clonar el repositorio
git clone https://github.com/santiagoriverti/prediccion_nivel_educativo.git
cd prediccion_nivel_educativo

# 2. (Opcional) Crear un entorno virtual
python -m venv .venv
# Windows: .venv\Scripts\activate | Linux/Mac: source .venv/bin/activate

# 3. Instalar dependencias
pip install pyeph scikit-learn openpyxl shap xgboost statsmodels jinja2 pandas numpy matplotlib seaborn scipy jupyter

# 4. Abrir el notebook
jupyter notebook scripts/prediccion_nivel_educativo.ipynb
```

Las tablas exportadas se guardan en una carpeta `outputs/` creada en el directorio de trabajo desde el que se ejecuta el notebook.


## Instrucciones para el Repositorio

### Clonar el Repositorio
Abrir cmd

cd C:... (completar con directorio local)

git clone https://github.com/santiagoriverti/prediccion_nivel_educativo.git

### Actualizar repositorio local
Abrir cmd 

cd C:... (completar con direccion del repositorio local)

git pull

### Actualizar repositorio remoto en rama principal
Abrir cmd

cd C:... (completar con direccion del repositorio local)

git status

git add .

git commit -m "comentario"

git push origin main
