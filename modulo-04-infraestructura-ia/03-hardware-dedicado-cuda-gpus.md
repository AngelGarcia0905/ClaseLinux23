# Subtema 4.3: Hardware Dedicado — Arquitectura CUDA, Tensor Cores y `nvidia-smi`

---

## 1. NVIDIA CUDA & Tensor Cores

CUDA es la plataforma de cómputo en paralelo que permite ejecutar tareas matriciales masivas sobre GPUs NVIDIA. Los **Tensor Cores** son unidades de aceleración por hardware diseñadas para operaciones de multiplicación matricial en precisión mixta (FP16/INT8).

---

## 2. Diagnóstico con `nvidia-smi`

El comando `nvidia-smi` monitorea el estado del hardware de video en servidores Linux y entornos WSL 2:

```bash
nvidia-smi
```
Métricas clave: Uso de GPU (%), Consumo de VRAM, Temperatura y Procesos activos de PyTorch/TensorFlow.
