# Predicción del Nivel Educativo en Hogares Argentinos

Este proyecto aplica técnicas de aprendizaje automático para estimar el **nivel educativo promedio ajustado por edad (IEAE)** en hogares urbanos de Argentina, utilizando microdatos de la **Encuesta Permanente de Hogares (EPH)** del INDEC.

---

## 📦 Contenido

- Limpieza y validación de datos EPH (4º trimestre 2023)
- Construcción de variables educativas y sociodemográficas por hogar
- Modelos de **regresión**: Lasso, Ridge, ElasticNet, SVR, Random Forest
- Modelos de **clasificación** binaria (por debajo de la mediana): Random Forest y SVM
- Evaluación con métricas de desempeño: R², RMSE, accuracy, F1-score

---

## 📁 Estructura

- `scripts/`: notebook con el código completo
- `document/`: 
  - PDFs metodológicos de EPH e INDEC
  - Bases exportadas (`.xlsx`) del procesamiento

---

## 🧾 Datos y fuentes

Los datos provienen de la EPH (INDEC), procesados con `pyeph` y validados según:

- `EPH_registro_4T2023.pdf`
- `EPH_consideraciones_metodologicas_2t20.pdf`
- `EPH_nota_metodologica_1_trim_2019.pdf`
- `PTFM_SR.pdf`

## GitHub
---

## ▶️ Requisitos

```bash
pip install pyeph scikit-learn openpyxl
