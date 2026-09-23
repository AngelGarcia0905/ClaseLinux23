# Módulo 6: Integración de Hardware Biomédico (Índice Maestro de Teoría)

---

## Subtemas del Módulo 6:

1. 📌 **[01-protocolos-uart-i2c.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/01-protocolos-uart-i2c.md)**
   * Protocolo asíncrono UART (Baud Rate, bits de parada/paridad).
   * Bus síncrono I2C (Líneas SDA/SCL, direcciones de 7 bits).
   * Interfaz con sensores fisiológicos (AD8232, MAX30102, MPU6050).

2. 📌 **[02-configuracion-platformio-ini.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/02-configuracion-platformio-ini.md)**
   * Estructura del framework PlatformIO.
   * Gestión de bibliotecas C++ y plataformas embebidas en `platformio.ini`.

3. 📌 **[03-depuracion-hardware-dmesg-dialout.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/03-depuracion-hardware-dmesg-dialout.md)**
   * Monitoreo de registros del kernel con `dmesg` y `lsusb`.
   * Solución a permisos *Permission Denied* mediante el grupo `dialout`.
   * Integración de passthrough USB en WSL 2 con `usbipd-win`.
