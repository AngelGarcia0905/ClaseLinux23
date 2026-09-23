# Módulo 4: Infraestructura Híbrida para IA (Edge & Cloud)

---

## 1. Delegación de Cargas: Edge Computing vs. Cloud Computing

En proyectos de Inteligencia Artificial aplicada a la medicina o IoT, el diseño de la infraestructura requiere decidir dónde se ejecuta el procesamiento informático.

```
+------------------------------------+      Petición REST / API      +------------------------------------+
|          DISPOSITIVO EDGE          | ----------------------------> |         INFRAESTRUCTURA CLOUD      |
| (Raspberry Pi, Microcontroladores) |                               | (Google Cloud, AWS, Antigravity)   |
| - Captura de señales en vivo       | <---------------------------- | - Inferencia de Grandes Modelos    |
| - Preprocesamiento liviano (CPU)   |      Respuesta JSON / Token   | - Entrenamiento con GPUs Masivas   |
+------------------------------------+                               +------------------------------------+
```

### Edge Computing (Cómputo en el Borde):
* **Definición:** Procesamiento de datos directamente en el dispositivo local de captura (ej. un nodo Linux integrado conectado a sensores biomédicos).
* **Ventajas:**
  * **Baja Latencia:** Respuestas inmediatas sin requerir conexión a internet.
  * **Privacidad de Datos:** Los datos sensibles del paciente no abandonan la red local.
  * **Operatividad Offline:** Continúa funcionando aunque se caiga la red.
* **Limitaciones:** Potencia de procesamiento de CPU y memoria RAM restringidas.

### Cloud Computing (Cómputo en la Nube):
* **Definición:** Envío de cargas de trabajo pesadas a servidores remotos de alto rendimiento mediante APIs REST o WebSockets.
* **Ventajas:** Capacidad casi ilimitada de cómputo, acceso a GPUs/TPUs masivas y modelos LLM avanzados (como Google Gemini o modelos de visión computacional).
* **Estrategia Híbrida:** Limpiar y filtrar señales en el borde (Edge) usando Python en Linux, y enviar solo resúmenes o estructuras complejas a la nube (Cloud) para inferencia avanzada.

---

## 2. Modelos y Librerías de IA Optimizado para CPU

No todas las tareas de aprendizaje automático requieren aceleración por GPU. Para datos tabulares, series temporales biomédicas y sensores, los algoritmos clásicos optimizados para CPU son sumamente eficientes.

### Ecosistema de Datos en Python sobre Linux:
1. **NumPy & SciPy:** Operaciones matriciales algebraicas y procesamiento digital de señales (filtrado de ruido ECG/EEG, transformadas de Fourier).
2. **Pandas:** Manipulación estructurada de DataFrames, limpieza de valores nulos e integración con archivos CSV/Parquet.
3. **Scikit-learn:**
   * **Regresión Lineal y Logística:** Modelado estadístico rápido para predicciones continuas o clasificación binaria.
   * **Árboles de Decisión y Random Forest:** Modelos interpretables con excelente rendimiento en CPU para diagnóstico asistido.
   * **K-Means / PCA:** Clustering y reducción de dimensionalidad para análisis exploratorio de datos médicos.

---

## 3. Hardware Dedicado: Arquitectura CUDA y Cores Tensor de NVIDIA

Para modelos de Aprendizaje Profundo (*Deep Learning*) como Redes Neuronales Convolucionales (CNN) y Transformers, se requiere cómputo masivamente paralelo mediante GPUs.

### Compute Unified Device Architecture (CUDA):
CUDA es la plataforma de computación paralela y modelo de programación creado por NVIDIA que permite utilizar GPUs para tareas de propósito general (GPGPU).

* **Hilos y Bloques:** CUDA divide una tarea en miles de pequeños hilos (*threads*) que se ejecutan simultáneamente en núcleos CUDA.
* **Cores Tensor (Tensor Cores):** Núcleos de hardware de propósito específico integrados en las GPUs modernas de NVIDIA, optimizados para la multiplicación y acumulación de matrices de precisión mixta (FP16/INT8), acelerando la inferencia y entrenamiento de IA.

### Diagnóstico de GPU en Linux (`nvidia-smi`):
La herramienta `nvidia-smi` (*NVIDIA System Management Interface*) permite monitorear el estado del hardware de video en servidores Linux.

```bash
nvidia-smi
```

* **Métricas clave reportadas:**
  * **GPU Utility (%):** Porcentaje de uso del procesador gráfico.
  * **VRAM Memory Usage:** Consumo de memoria de video (crucial para evitar errores *Out of Memory* al cargar modelos).
  * **Temperature & Power:** Temperatura física del chip y consumo en Watts.
  * **Process List:** Lista de PIDs de procesos (ej. scripts de PyTorch/TensorFlow) asignados a la GPU.

