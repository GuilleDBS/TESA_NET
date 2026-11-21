# TESA_NET

Notebook para detectar y analizar grietas visibles en superficies de hormigón mediante una CNN propia y técnicas de visión por computador. El flujo también genera un informe orientativo de inspección.

## Integrantes
- Micaela Gonzalez
- Axel López
- Guillermo Benítez
- Marcio Martínez
- Gustavo Añazco

## Qué hace

El notebook `Tesa_V0.ipynb` cubre el flujo completo:

1. Descarga desde Kaggle el dataset **Concrete Crack Images for Classification** y localiza las clases `Negative` (sin grieta visible) y `Positive` (con grieta visible).
2. Divide los datos de forma estratificada en entrenamiento (70 %), validación (15 %) y prueba (15 %). En el notebook actual, el conjunto de prueba queda reservado y no se usa para evaluar el modelo.
3. Preprocesa las imágenes a RGB, tamaño 160 × 160 y valores normalizados entre 0 y 1; aplica aumento de datos durante el entrenamiento.
4. Entrena desde cero una CNN de Keras con cuatro bloques convolucionales. Usa parada temprana y guarda el mejor modelo en `outputs/models/best_custom_cnn.keras`.
5. Calcula mapas Grad-CAM para visualizar las regiones que influyen en la clasificación.
6. Permite cargar imágenes nuevas, clasifica cada una y estima una máscara de grieta con operaciones clásicas de visión por computador guiadas por Grad-CAM. Calcula medidas geométricas y genera un informe PDF en formato 16:9 y un resumen Excel/CSV, además de máscaras y un ZIP de resultados.

## Requisitos y ejecución

El flujo de descarga y carga de imágenes está pensado para **Google Colab**. Se recomienda activar una GPU si está disponible. Ejecuta las celdas en orden. La celda de credenciales lee `KAGGLE_KEY` desde los Secretos de Colab; crea ese secreto con una clave válida de Kaggle y habilita el acceso desde el notebook. No pegues claves ni tokens en el código.

El notebook instala `kaggle`; además, requiere TensorFlow/Keras, scikit-learn, pandas, NumPy, OpenCV, Matplotlib, Pillow, SciPy, scikit-image y `openpyxl` para Excel. El notebook importa estas dependencias, pero no instala todas automáticamente.

Dataset: [Concrete Crack Images for Classification en Kaggle](https://www.kaggle.com/datasets/arnavr10880/concrete-crack-images-for-classification). Debes tener acceso al dataset y aceptar sus condiciones en Kaggle cuando corresponda.

## Interpretación y limitaciones

- Grad-CAM es una explicación visual aproximada de la clasificación, no una máscara exacta ni una prueba de causalidad.
- La CNN clasifica imágenes; **no fue entrenada para segmentar**. La máscara se estima con filtros y reglas heurísticas, por lo que puede incluir errores.
- El tipo de grieta, la prioridad y las causas o acciones del informe se asignan mediante reglas; no constituyen un diagnóstico de ingeniería.
- Las dimensiones físicas y la severidad dependen de calibrar `PX_POR_MM`. Sin una escala conocida, las medidas quedan en píxeles.
- Las imágenes del dataset son recortes pequeños. Las fotografías generales pueden perder detalle al reducirse a 160 × 160 píxeles.
- El conjunto de prueba está separado pero no se evalúa en el flujo actual; por ello este notebook no presenta una métrica independiente de rendimiento.
- Este resultado es orientativo y no sustituye una inspección ni el diagnóstico de un profesional estructural.

## Salidas

Los artefactos se guardan en `outputs/`: el mejor modelo, el informe PDF, el resumen Excel/CSV, las máscaras segmentadas y un archivo ZIP descargable desde Colab.
