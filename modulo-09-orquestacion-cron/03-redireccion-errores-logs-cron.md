# Subtema 9.3: Redirección de Errores y Generación de Logs en Tareas Automáticas

---

## 1. Captura de Salidas en Cron

Dado que `cron` ejecuta tareas en segundo plano sin una terminal visible, se deben combinar los descriptores de salida:

```bash
# Guardar salida normal (1) y errores (2) en un único archivo de log
0 2 * * * /usr/bin/python3 /app/script.py >> /var/log/tarea.log 2>&1
```
