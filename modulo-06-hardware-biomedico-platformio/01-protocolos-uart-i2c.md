# Subtema 6.1: Protocolos de Comunicación Serie para Sensores Biomédicos

---

## 1. UART / Serial RS-232

* **Comunicación Asíncrona Punto a Punto:** Utiliza líneas TX (transmisión) y RX (recepción).
* **Parámetros de Trama:** Baud Rate (ej. 115200 bps), bits de datos (8), paridad (Ninguna) y bits de parada (1).

---

## 2. Bus I2C (Inter-Integrated Circuit)

* **Comunicación Síncrona Maestro-Esclavo:** Utiliza dos líneas digitales con resistencias de pull-up: **SDA** (datos) y **SCL** (reloj).
* **Aplicación Biomédica:** Conexión de sensores de electrocardiograma (AD8232), oximetría (MAX30102) y acelerometría (MPU6050) a microcontroladores ESP32/Arduino.
