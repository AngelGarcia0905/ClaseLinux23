# Módulo 6: Integración de Hardware Biomédico y PlatformIO en Linux

---

## 1. Protocolos de Comunicación Serie para Sensores Biomédicos

Los prototipos médicos y sistemas embebidos (ESP32, STM32, Arduino) leen señales analógicas de sensores (ECG, pulsioxímetros, acelerómetros) y las transmiten mediante buses de comunicación digital.

### UART / Serial RS-232 (Universal Asynchronous Receiver-Transmitter):
* **Funcionamiento:** Comunicación asíncrona punto a punto utilizando dos líneas de datos: **TX** (Transmisión) y **RX** (Recepción).
* **Baud Rate (Velocidad de Baudios):** Define el número de símbolos transmitidos por segundo (ej. 9600, 115200 bps). Ambos dispositivos deben estar configurados a la misma velocidad.
* **Trama de Datos:** Compuesta por 1 bit de inicio (*Start bit*), 8 bits de datos, 1 bit de paridad opcional para verificación de errores, y 1-2 bits de parada (*Stop bits*).

### Bus I2C (Inter-Integrated Circuit):
* **Funcionamiento:** Bus síncrono maestro-esclavo con direccionamiento de 7 bits que requiere solo dos líneas de señal con resistencias de *pull-up*:
  * **SDA (Serial Data):** Línea bidireccional de datos.
  * **SCL (Serial Clock):** Línea de señal de reloj generada por el maestro.
* **Uso Biomédico:** Conexión de sensores de pulso cardíaco (MAX30102) y unidades de medición inercial / IMU (MPU6050).

---

## 2. Configuración de Entornos Embebidos con PlatformIO

**PlatformIO** es una herramienta profesional de código abierto para desarrollo de software embebido integrado en Linux, reemplazando al IDE tradicional de Arduino.

### Archivo de Configuración `platformio.ini`:
Define la plataforma de hardware, el framework de desarrollo (Arduino o ESP-IDF), las bibliotecas de C++ requeridas y la velocidad del puerto serie.

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

---

## 3. Depuración de Hardware y Manejo de Puertos Serie en Linux

Cuando se conecta un microcontrolador por USB a una computadora Linux, el núcleo detecta el dispositivo y crea un archivo de dispositivo en `/dev/`.

### Diagnóstico del Kernel con `dmesg`:
El comando `dmesg` muestra los mensajes del búfer circular del kernel. Es la herramienta principal para rastrear conexiones físicas de hardware.

* **Monitoreo en tiempo real al conectar un USB:**
  ```bash
  sudo dmesg -wH | grep -E "tty|USB"
  ```
  *Muestra cuándo el controlador de FTDI o CH340 es reconocido y qué archivo de dispositivo le asignó el sistema (ej. `/dev/ttyACM0` o `/dev/ttyUSB0`).*

### Permisos de Acceso a Puertos Serie (`dialout`):
Por defecto en Linux (Debian/Ubuntu), los puertos serie en `/dev/ttyACM*` y `/dev/ttyUSB*` pertenecen al usuario `root` y al grupo `dialout` con permisos `660` (`rw-rw----`).

* **Solución de error *Permission Denied*:**
  Para permitir que un usuario regular o un script de Python pueda leer el puerto serie sin usar `sudo`, se debe agregar el usuario al grupo `dialout`:
  ```bash
  sudo usermod -aG dialout $USER
  ```
  *(Se requiere cerrar e iniciar sesión para que el cambio de grupo surta efecto).*

### Reglas de `udev` para Puertos Persistentes:
Permiten asignar nombres fijos a los dispositivos serie (ej. `/dev/sensor_ecg`) basados en su ID de fabricante (*Vendor ID*) y producto (*Product ID*), evitando que el nombre cambie entre reinicios.

