# 📊 Predicción del Nivel Educativo en Hogares Argentinos

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
- 🎯 Selección de umbral óptimo para clasificación (basado en curva ROC)

---

## 📁 Estructura del proyecto

```text
├── scripts/
│   └── notebook con el pipeline completo
├── document/
│   ├── PTFM_SR.pdf                            # Proyecto académico
│   ├── EPH_registro_4T2023.pdf                # Diccionario de variables
│   ├── EPH_consideraciones_metodologicas_2t20.pdf
│   ├── EPH_nota_metodologica_1_trim_2019.pdf
│   └── Bases exportadas (df_*.xlsx)


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
