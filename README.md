# 1ACC0235 – Clasificación de Madurez de Plátanos

Trabajo Final del curso **Procesamiento de Imágenes (1ACC0235)** – UPC, ciclo 2026-01.

Sistema de clasificación automática del estado de madurez de plátanos (Verde, Maduro, Sobremaduro, Podrido) a partir de imágenes digitales, comparando tres enfoques: un modelo clásico de segmentación por color (HSV), una red neuronal convolucional (MobileNetV2) y un modelo del estado del arte (YOLO11).

---

## Objetivo del trabajo

Desarrollar y comparar tres modelos de clasificación del estado de madurez del plátano a partir de imágenes, con el fin de apoyar la toma de decisiones logísticas en la cadena de suministro (determinar qué frutos destinar a mercados lejanos, cuáles a consumo inmediato y cuáles descartar), reduciendo así las mermas por un manejo poscosecha deficiente.

El sistema responde a tres preguntas de investigación:
1. ¿En qué estado de madurez se encuentra un plátano a partir de su imagen?
2. ¿Es apto para consumo inmediato, para procesamiento industrial, o debe descartarse?
3. ¿Qué ventana de transporte y mercado de destino se recomienda según el estado detectado?

---

## Integrantes

| Código | Nombres y apellidos |
|--------|---------------------|
| u202219719 | Luis Enrique Aguilar Benites |
| u20231b249 | Ivan Antonio Almeida Aguilar |
| u202221068 | Juan Miguel Quijano Calderon |

**Docente:** Carlos Fernando Montoya Cubas
**Sección:** 3237

---

## Descripción del dataset

Se utilizó el **Banana Ripeness Classification Dataset**, disponible públicamente en Kaggle. Contiene **13 478 imágenes** de plátanos individuales sobre fondo controlado, clasificadas en cuatro clases de madurez y organizadas en tres particiones (train / valid / test).

| Clase | Train | Valid | Test | Total |
|-------|------:|------:|-----:|------:|
| Verde (unripe) | 1 902 | 167 | 110 | 2 179 |
| Maduro (ripe) | 3 522 | 339 | 154 | 4 015 |
| Sobremaduro (overripe) | 2 349 | 229 | 113 | 2 691 |
| Podrido (rotten) | 4 020 | 388 | 185 | 4 593 |
| **Total** | **11 793** | **1 123** | **562** | **13 478** |

Las imágenes **no se incluyen** en este repositorio por su tamaño. En la carpeta [`data`](./data) se encuentra el enlace y las instrucciones para descargarlas desde Kaggle.

---

## Estructura del repositorio

```
1ACC0235-TP-TF-2026-01/
├── code/                        # Notebooks de Google Colab
│   ├── modelo_clasico_y_cnn.ipynb    # Modelo clásico (HSV) + CNN (MobileNetV2)
│   └── modelo_yolo.ipynb             # Modelo YOLO11 + interfaz gráfica
├── data/                        # Instrucciones para obtener el dataset
│   └── README.md
├── README.md
└── LICENSE
```

---

## Modelos y resultados

Los tres modelos fueron evaluados sobre el conjunto de prueba (562 imágenes no vistas durante el entrenamiento).

| Métrica | Clásico (HSV) | MobileNetV2 | YOLO11n |
|---------|:-------------:|:-----------:|:-------:|
| Exactitud (accuracy) | 59.3 % | 95.4 % | **98.9 %** |
| Recall (macro) | 0.641 | 0.955 | **0.990** |
| Tiempo por imagen | 6.5 ms | 41.4 ms | 17.8 ms |

El modelo clásico funciona bien en clases separables por color (Verde) pero falla en Podrido (recall 0.151), cuya identificación depende de la textura. Los modelos profundos resuelven esta limitación, y YOLO11 obtiene el mejor desempeño.

---

## Cómo ejecutar

1. Abrir los notebooks de la carpeta `code` en Google Colab.
2. Activar la GPU (Entorno de ejecución → Cambiar tipo de entorno → T4 GPU).
3. Seguir las instrucciones de [`data/README.md`](./data/README.md) para descargar el dataset desde Kaggle (se requiere un token `kaggle.json`).
4. Ejecutar las celdas en orden.

Tecnologías: Python, OpenCV, TensorFlow/Keras, Ultralytics (YOLO11), scikit-learn, ipywidgets.

---

## Conclusiones

- Se implementaron y compararon tres enfoques de clasificación de madurez. El modelo clásico (HSV) alcanzó 59.3 % de exactitud, MobileNetV2 un 95.4 % y YOLO11n un 98.9 %, siendo los enfoques de aprendizaje profundo claramente superiores.
- El principal hallazgo es la demostración cuantitativa del límite del enfoque clásico: el color permite separar bien las clases cromáticamente distintas (Verde), pero es insuficiente para la clase Podrido (recall 0.151), cuya identificación depende de la textura y el patrón de daño. Los modelos profundos superan esta limitación (recall de Podrido superior a 0.93).
- Al validar con fotografías propias, se observó que YOLO11 presenta una mayor capacidad de generalización que MobileNetV2 ante imágenes fuera del dominio de entrenamiento, un factor no capturado por las métricas estándar pero relevante para el despliegue real.
- Como trabajo futuro se plantea el uso de YOLO en modo detección para clasificar múltiples frutos por imagen, y ampliar el dataset con más variedades y condiciones.

---

## Licencia

Este proyecto se distribuye bajo la licencia MIT. Ver el archivo [LICENSE](./LICENSE) para más detalles.
