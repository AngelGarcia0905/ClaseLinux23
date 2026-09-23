# Subtema 10.2: Monitoreo de Recursos del Sistema y Terminación de Procesos

---

## 1. Diagnóstico de Recursos (`top` / `htop`)

* **Navegación en `htop`:** Ordenar por uso de CPU (`P`) y memoria RAM (`M`).
* **Inspección de Procesos con `ps`:** `ps aux | grep python`

---

## 2. Terminación Forzada (`kill`)

```bash
# Envío de señal SIGTERM (15) para cierre limpio
kill <PID>

# Envío de señal SIGKILL (9) para cierre forzado por el kernel
kill -9 <PID>

# Cierre masivo de procesos de Python desbocados
killall -9 python3
```

