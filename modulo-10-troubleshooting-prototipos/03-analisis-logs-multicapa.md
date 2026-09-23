# Subtema 10.3: Análisis Metódico de Logs Multi-capa

---

## 1. Metodología de Depuración en 4 Capas

```
+-------------------+     +-------------------+     +-------------------+     +-------------------+
| 1. HARDWARE FÍSICO| --> | 2. KERNEL LINUX   | --> | 3. CONTENEDORES   | --> | 4. API CLOUD      |
| Sensor/Voltaje/GND|     | dmesg -wH / serial|     | docker logs / init|     | HTTP status / jq  |
+-------------------+     +-------------------+     +-------------------+     +-------------------+
```

1. **Hardware:** Medición de señales y continuidad.
2. **Kernel:** `dmesg -wH` y lectura cruda en `/dev/ttyUSB0`.
3. **Contenedor / Servicio:** `journalctl -u servicio -f` y `docker logs -f`.
4. **API:** Diagnóstico de códigos HTTP (`429`, `503`).
