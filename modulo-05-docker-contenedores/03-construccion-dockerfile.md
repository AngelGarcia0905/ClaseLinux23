# Subtema 5.3: Construcción de Imágenes Optimizadas (`Dockerfile`)

---

## 1. Optimización de Capas de Caché

Cada instrucción en un `Dockerfile` crea una capa de solo lectura. Para minimizar los tiempos de compilación y el tamaño final de la imagen:
1. Utilizar imágenes base ligeras (`python:3.11-slim` o `alpine`).
2. Copiar e instalar `requirements.txt` **antes** del código fuente para aprovechar la caché de Docker.

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "main.py"]
```
