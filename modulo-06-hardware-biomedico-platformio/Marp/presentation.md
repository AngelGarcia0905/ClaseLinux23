---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 6"
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

# Módulo 6: Integración de Hardware Biomédico
### PlatformIO, Buses Serie, dmesg y usbipd

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

- **6.1** Protocolos de Comunicación Serie (UART / RS-232)
- **6.2** Bus I2C para Sensores Fisiológicos (ECG, SpO2, IMU)
- **6.3** Configuración del Entorno PlatformIO (`platformio.ini`)
- **6.4** Detección de Hardware en Linux (`lsusb`, `dmesg`)
- **6.5** Permisos del Grupo `dialout` en `/dev/ttyUSB0`
- **6.6** Passthrough USB en WSL 2 mediante `usbipd-win`

---

## 6.1 Protocolos Serie para Sensores Biomédicos

- **UART (Universal Asynchronous Receiver-Transmitter):**
  - Asíncrono punto a punto vía **TX** y **RX**.
  - Parámetros: Baud Rate (115200 bps), 8 bits datos, 1 bit parada.
- **Bus I2C (Inter-Integrated Circuit):**
  - Síncrono maestro-esclavo mediante dos líneas: **SDA** (datos) y **SCL** (reloj).
  - Conexión de biopotenciales: ECG AD8232, Oximetría MAX30102, Acelerómetro MPU6050.

---

## 6.2 Entorno Embebido PlatformIO

Plataforma CLI profesional de desarrollo embebido integrada en Linux:

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

## 6.3 Depuración y Puertos Serie en Linux

- **Monitoreo de Kernel (`dmesg` y `lsusb`):**
  - `lsusb`: Muestra dispositivos USB reconocidos (CH340, CP2102).
  - `sudo dmesg -wH | grep -E "tty|USB"`: Seguimiento de asignación de puertos (`/dev/ttyUSB0` o `/dev/ttyACM0`).
- **Permisos del Grupo `dialout`:**
  - `sudo usermod -aG dialout Mundkey23` (Evita usar `sudo` para leer la terminal serie).
- **WSL 2 Passthrough:**
  - `usbipd bind --busid <BUSID>` ➔ `usbipd attach --wsl --busid <BUSID>`

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 6?
