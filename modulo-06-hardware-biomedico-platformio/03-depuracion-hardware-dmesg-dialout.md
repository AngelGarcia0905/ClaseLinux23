# Subtema 6.3: Depuración de Hardware, Kernel Logs y Permisos (`dialout`, `usbipd`)

---

## 1. Monitoreo de Conexiones del Kernel (`dmesg` y `lsusb`)

```bash
# Verificación de dispositivos USB conectados
lsusb

# Seguimiento en tiempo real de eventos USB en el kernel
sudo dmesg -wH | grep -E "tty|USB"
```

---

## 2. Permisos de Usuario (`dialout`) y Flujo WSL 2 (`usbipd-win`)

* **Asignación de grupo de puerto serie:**
  ```bash
  sudo usermod -aG dialout Mundkey23
  ```
* **Reenvío USB en WSL 2 desde PowerShell Host:**
  ```powershell
  usbipd list
  usbipd bind --busid <BUSID>
  usbipd attach --wsl --busid <BUSID>
  ```
  *Permite mapear la placa física ESP32/CH340 hacia `/dev/ttyUSB0` o `/dev/ttyACM0`.*
