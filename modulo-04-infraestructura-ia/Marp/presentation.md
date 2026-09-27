---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 4"
footer: "Jorge Angel Garcia Alvarado  |  2068683  |  IMC"
style: |
  section {
    font-family: "Segoe UI", "Helvetica Neue", Arial, sans-serif;
    font-size: 25px;
    color: #1f2933;
    background: #ffffff;
    padding: 50px 80px 45px 80px;
  }
  section h1 { color: #0B4D2B; font-weight: 600; letter-spacing: -0.5px; }
  section h2 { color: #0B4D2B; font-weight: 600; border-bottom: 3px solid #0B4D2B; padding-bottom: 8px; }
  section a, section strong { color: #0B4D2B; }
  header, footer { color: #7b8794; font-size: 15px; }
  section::after { color: #7b8794; font-size: 16px; }
  section.portada {
    display: grid;
    grid-template-columns: 1.1fr 1fr;
    grid-template-rows: auto 1fr 1fr;
    column-gap: 56px;
    border-left: 28px solid #0B4D2B;
    padding: 50px 80px 50px 76px;
  }
  section.portada > p:first-of-type { grid-column-start: 1; grid-column-end: span 2; grid-row-start: 1; margin: 0; }
  section.portada img { margin-right: 20px; }
  section.portada > h1 {
    grid-column-start: 1; grid-row-start: 2; align-self: end;
    font-size: 52px; line-height: 1.12; margin: 0 0 14px 0;
  }
  section.portada > h3 {
    grid-column-start: 1; grid-row-start: 3; align-self: start;
    font-size: 25px; font-weight: 400; color: #52606d; margin: 0;
  }
  section.portada > table {
    grid-column-start: 2; grid-row-start: 2; grid-row-end: span 2; align-self: center;
    width: 100%; display: table; font-size: 21px; border-collapse: collapse;
    background: #E9F4EE; border-left: 6px solid #0B4D2B; padding: 0;
  }
  section.portada thead { display: none; }
  section.portada table tr { background: none; }
  section.portada table td { border: none; background: none; padding: 6px 16px; vertical-align: top; }
  section.portada table td:first-child {
    color: #52606d; text-transform: uppercase; font-size: 13px;
    letter-spacing: 1.2px; white-space: nowrap; padding-top: 10px; width: 32%;
  }
  section.cierre {
    border-left: 28px solid #0B4D2B; justify-content: center; text-align: center;
  }
  section.cierre h1 { font-size: 64px; margin-bottom: 6px; }
  section.cierre h3 { font-size: 25px; font-weight: 400; color: #52606d; margin: 0; }
---

<!-- _class: portada -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

![h:105](assets/logo_uanl.jpg)![h:105](assets/FIME_LOGO.png)

# Módulo 4: Infraestructura Híbrida para IA
### Edge vs. Cloud, ML en CPU y Aceleración CUDA

| | |
|---|---|
| Instructor | Jorge Angel Garcia Alvarado |
| Matrícula | 2068683 |
| Programa | IMC · 7.º semestre |
| Materia | Linux, IA y Prototipos Biomédicos |
| Grupo | 001 |
| Periodo | Agosto-Diciembre 2026 |
| Fecha | 23 de Septiembre de 2026 |

---

## Temario

- **4.1** Arquitectura Híbrida: Edge vs. Cloud Computing
- **4.2** Privacidad y Latencia en Procesamiento de Datos Biomédicos
- **4.3** Ecosistema de Machine Learning para CPU en Python
- **4.4** Modelos Clásicos con Scikit-learn (Regresión y Random Forest)
- **4.5** Hardware Dedicado: NVIDIA CUDA y Tensor Cores
- **4.6** Diagnóstico y Monitoreo de GPUs con `nvidia-smi`

---

## 4.1 Edge vs. Cloud Computing

- **Edge Computing (Borde):**
  - Procesamiento local en nodos Linux integrados (Raspberry Pi).
  - **Ventajas:** Latencia nula, privacidad de datos de salud y funcionamiento offline.
- **Cloud Computing (Nube):**
  - Delegación de inferencia pesada (LLMs) vía APIs REST.
- **Estrategia Híbrida:** Preprocesar y filtrar señales en el borde; consultar modelos complejos en la nube.

---

## 4.2 Ecosistema de IA en CPU (Python/Linux)

Para datos tabulares y series temporales biomédicas, los modelos en CPU son ligeros y eficaces:

- **NumPy & SciPy:** Filtrado digital de señales (FFT, filtros Butterworth para ECG).
- **Pandas:** Estructuración de DataFrames y limpieza de datos.
- **Scikit-learn:**
  - Regresión Lineal / Logística: Tendencias fisiológicas.
  - Árboles de Decisión y Random Forest: Diagnóstico asistido interpretable.

---

## 4.3 Hardware Dedicado: CUDA y Tensor Cores

- **NVIDIA CUDA:** Plataforma de cómputo en paralelo que ejecuta miles de hilos concurrentes en GPU.
- **Tensor Cores:** Unidades de hardware optimizadas para multiplicación matricial en precisión mixta (FP16/INT8).
- **Herramienta `nvidia-smi`:**

```bash
nvidia-smi
```
- **Métricas:** % Uso de GPU, Consumo de VRAM, Temperatura y Procesos PIDs.

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 4?
