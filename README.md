# 📊 Predicción del Nivel Educativo en Hogares Argentinos

Este proyecto aplica técnicas de aprendizaje automático para predecir el **índice educativo ajustado por edad (IEAE)** en hogares urbanos de Argentina. Utiliza microdatos de la **Encuesta Permanente de Hogares (EPH)** del INDEC correspondientes al **4º trimestre de 2023**.

El IEAE es una métrica que compara la escolaridad alcanzada por los miembros del hogar con la escolaridad esperada según su edad. Esta aproximación permite identificar **rezagos educativos estructurales**, más allá del nivel educativo promedio.

---

## 📌 Objetivo

Desarrollar modelos predictivos de regresión y clasificación para estimar el nivel educativo del hogar, y explorar la importancia relativa de variables sociodemográficas, estructurales y de ingresos.

---

## ⚙️ Funcionalidades principales

- ✅ Descarga y validación de bases EPH (`pyeph`)
- 🧹 Limpieza de variables y filtrado por rangos válidos según la documentación oficial
- 🧠 Cálculo de escolaridad efectiva (`EDA_ESC`) y esperada (`EDA_ESP`) por individuo
- 🏡 Agregación de indicadores a nivel hogar
- 📈 Modelos de regresión: `Lasso`, `Ridge`, `ElasticNet`, `SVR`, `Random Forest`
- 🧮 Clasificación binaria de hogares con bajo nivel educativo (`PE`) usando `RandomForestClassifier` y `SVM`
- 🧪 Evaluación de desempeño: `R²`, `RMSE`, `accuracy`, `F1-score`, `confusion matrix`

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
