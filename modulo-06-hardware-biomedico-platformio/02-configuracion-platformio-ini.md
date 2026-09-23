# Subtema 6.2: Configuración de Proyectos Embebidos con `platformio.ini`

---

## 1. El Entorno PlatformIO en Linux

PlatformIO reemplaza al IDE tradicional de Arduino ofreciendo una herramienta CLI profesional basada en Python/C++ para compilar y desplegar firmware en microcontroladores.

### Estructura de `platformio.ini`:
```ini
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
lib_deps =
    sparkfun/SparkFun MAX3010x Pulse and Proximity Sensor Library @ ^1.1.2
    adafruit/Adafruit MPU6050 @ ^2.2.4
```

