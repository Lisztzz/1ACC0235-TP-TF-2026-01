# Dataset

Este proyecto utiliza el **Banana Ripeness Classification Dataset**, disponible públicamente en Kaggle.

Las imágenes **no se incluyen en este repositorio** debido a su tamaño (13 478 imágenes, varios cientos de MB). A continuación se explica cómo obtenerlas.

## Fuente

- **Nombre:** Banana Ripeness Classification Dataset
- **Autor:** S. M. Shahriar (2023)
- **Enlace:** https://www.kaggle.com/datasets/shahriar26s/banana-ripeness-classification-dataset

## Descripción

| Clase | Train | Valid | Test |
|-------|------:|------:|-----:|
| unripe (Verde) | 1 902 | 167 | 110 |
| ripe (Maduro) | 3 522 | 339 | 154 |
| overripe (Sobremaduro) | 2 349 | 229 | 113 |
| rotten (Podrido) | 4 020 | 388 | 185 |

Total: **13 478 imágenes** organizadas en carpetas por clase, dentro de tres particiones (`train`, `valid`, `test`).

## Cómo descargar (desde Google Colab)

1. Obtener un token de la API de Kaggle:
   - Entrar a la cuenta de Kaggle → *Settings* → sección *API* → *Create Legacy API Key*.
   - Se descarga un archivo `kaggle.json`.

2. En el notebook, subir el `kaggle.json` y descargar el dataset:

```python
from google.colab import files
import os, zipfile, subprocess

# Subir el token
files.upload()  # seleccionar kaggle.json
os.makedirs("/root/.kaggle", exist_ok=True)
os.rename("kaggle.json", "/root/.kaggle/kaggle.json")
os.chmod("/root/.kaggle/kaggle.json", 0o600)

# Descargar y descomprimir
subprocess.run(["kaggle", "datasets", "download", "-d",
                "shahriar26s/banana-ripeness-classification-dataset",
                "-p", "/content/data"])
with zipfile.ZipFile("/content/data/banana-ripeness-classification-dataset.zip") as z:
    z.extractall("/content/data/banana")
```

> **Nota:** el archivo `kaggle.json` es una credencial privada. No debe subirse a este ni a ningún repositorio público.
