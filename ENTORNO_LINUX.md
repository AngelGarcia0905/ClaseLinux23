# Entorno Linux para la clase — Documentación Completa de Infraestructura

> [!NOTE]
> **Propósito de este documento:**
> Registro técnico detallado del entorno Linux configurado el **23 de septiembre de 2026** para impartir la clase de *Linux, IA y Prototipos Biomédicos*.
> Documenta las decisiones de arquitectura, comandos de instalación, integraciones de hardware (USB/Serie), configuración de red en modo espejo (*mirrored*), diferencias clave respecto a un Linux nativo y la explicación conceptual sobre **KDE Plasma**.

---

## 1. Resumen Ejecutivo de la Infraestructura

| Componente | Especificación Técnica |
|---|---|
| **Distribución Base** | Debian GNU/Linux 13 (*trixie*) — Rama Estable Actual |
| **Plataforma de Ejecución** | WSL 2.7.10.0 (Windows Subsystem for Linux, Versión 2) |
| **Kernel de Linux** | `6.18.33.2-microsoft-standard-WSL2` |
| **Soporte Gráfico (GUI)** | WSLg 1.0.73.2 — **Activo** (Integración transparente con Wayland/X11) |
| **Usuario Principal** | `Mundkey23` (uid 1000, grupo `sudo`) |
| **Sistema de Inicialización** | `systemd` — Activo y funcional (`systemctl is-system-running`) |
| **Modo de Red** | *Mirrored* (Espejo) — IP compartida con Windows (`192.168.1.78`) |
| **Passthrough de Hardware USB** | `usbipd-win` 5.3.0 |
| **Equipo Anfitrión** | Lenovo ThinkPad T14 Gen 2i · 31.7 GB RAM · Intel Core i7 |

---

## 2. Justificación de Arquitectura

### 2.1 Comparativa: WSL2 vs. Máquinas Virtuales Tradicionales

Se evaluaron tres alternativas para montar el entorno de clase:

| Plataforma | Soporte de Servidores | Passthrough USB / Serie | Evaluación Final |
|---|---|---|---|
| **Hyper-V** | Excelente | ❌ **No soporta** passthrough de USB/Serie genérico a Linux | Descartado (Imposibilita el Módulo 6) |
| **VirtualBox** | Bueno | ✅ Disponible (Requiere Extension Pack) | Viable, pero genera conflictos con Hyper-V activo |
| **WSL 2** | Excelente | ✅ Compatible vía `usbipd-win` | **ELEGIDO (Solución Óptima)** |

> [!IMPORTANT]
> **El factor decisivo fue el acceso al Hardware Biomédico:**
> El plan de estudios incluye la conexión de prototipos (ESP32, microcontroladores, sensores AD8232/MAX30102) por puerto serie. Hyper-V no implementa passthrough de dispositivos USB a máquinas virtuales Linux por limitaciones de su hipervisor. WSL2 combinado con `usbipd-win` permite mapear puertos serie directamente a `/dev/ttyUSB0` o `/dev/ttyACM0`.

### 2.2 ¿Por qué Debian Trixie?
Debian es la distribución madre de Ubuntu y Raspberry Pi OS. Al utilizar Debian 13 (*trixie*) en clase, se demuestra de forma práctica el funcionamiento de la rama estable (*Stable*) frente a *Testing* y *Sid* (Módulo 1).

---

## 3. Instalación y Despliegue Paso a Paso

### 3.1 Verificación Inicial y Despliegue
Se verificó que WSL estuviera habilitado en Windows sin distribuciones cargadas:

```powershell
wsl --list --verbose
# Muestra: "Windows Subsystem for Linux has no installed distributions."
```

Se instaló la distribución de Debian sin arranque inmediato para proteger la asignación de credenciales:

```powershell
wsl --install -d Debian --no-launch
```

Al iniciar manualmente (`wsl -d Debian`), el instructor configuró el usuario `Mundkey23` (UID 1000) asignándole grupos de administración (`sudo`, `adm`, `dialout`, `plugdev`).

---

## 4. Configuración del Entorno de Producción

### 4.1 Paso 1 — Integración de Hardware (`usbipd-win`)
Instalación del puente USB sobre IP en Windows:

```powershell
winget install --exact --id dorssel.usbipd-win
```

### 4.2 Paso 2 — Paquería del Sistema por Módulo
Actualización del repositorio e instalación de herramientas esenciales:

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y \
  openssh-server curl wget git nano vim \
  net-tools iproute2 iputils-ping dnsutils \
  python3 python3-pip python3-venv \
  htop usbutils picocom ca-certificates
```

### 4.3 Paso 3 — Configuración de Red en Modo Espejo (*Mirrored Mode*)
Para permitir que dispositivos externos (alumnos, tarjetas ESP32 vía Wi-Fi, sensores IoT) accedan a los servidores levantados en Debian, se creó el archivo de configuración `C:\Users\yoryi\.wslconfig`:

```ini
[wsl2]
networkingMode=mirrored
dnsTunneling=true
```

Tras aplicar un reinicio del subsistema (`wsl --shutdown`), Debian adoptó la misma dirección IP física que el anfitrión Windows:
* **IP Windows:** `192.168.1.78`
* **IP Debian (WSL2):** `192.168.1.78`

---

## 5. Entorno Gráfico y Explicación sobre KDE Plasma

### 5.1 ¿Qué es KDE / KDE Plasma?

**KDE Plasma** es uno de los **Entornos de Escritorio** (*Desktop Environments - DE*) más populares, avanzados y personalizables del ecosistema Linux/UNIX.

```
+-------------------------------------------------------------------+
|                        KDE PLASMA DESKTOP                         |
|   (Panel, Menú de Aplicaciones, Widgets, Gestor de Archivos Dolphin) |
+-------------------------------------------------------------------+
|                  GESTOR DE VENTANAS (KWin)                        |
+-------------------------------------------------------------------+
|            SERVIDOR DE DESPLIEGUE (Wayland / X11)                 |
+-------------------------------------------------------------------+
|                      SISTEMA OPERATIVO LINUX                      |
+-------------------------------------------------------------------+
```

#### Componentes Principales de KDE Plasma:
* **KWin:** El gestor de ventanas encargado de renderizar efectos visuales y bordes.
* **Dolphin:** El gestor de archivos nativo de KDE.
* **Konsole:** La emulación de terminal avanzada de KDE.
* **Aplicaciones de la Suite KDE Gear:** Colección de herramientas nativas (Kate, Spectacle, Okular).

### 5.2 Evaluación de KDE Plasma en el Entorno WSL2

Durante la preparación, se evaluó instalar el escritorio completo de KDE Plasma. La instalación requería 1365 paquetes (~1.3 GB de descargas y consumo alto de RAM).

#### Conclusión y Reversión Limpia:
Se decidió **cancelar la instalación de KDE Plasma** por dos razones técnicas:
1. **WSLg ya incluye soporte GUI nativo:** WSLg permite ejecutar aplicaciones gráficas individuales de Linux (como Wireshark, VS Code o navegadores) de forma transparente. Se abren como ventanas nativas de Windows sin necesidad de cargar un escritorio completo.
2. **Uso Eficiente de Recursos:** Eliminar el escritorio gráfico completo ahorra más de 1.5 GB de RAM para cargas de IA y Docker.

La cancelación se revirtió limpiamente sin dejar paquetes huérfanos:
```bash
sudo dpkg --configure -a
sudo apt-get -f install
sudo apt-get autoremove --purge
```
*Verificación final: 0 paquetes rotos en `apt-get check`.*

> [!TIP]
> Si en el futuro se requiriera un entorno de escritorio gráfico completo, la mejor práctica en WSL2 es instalar `xrdp` en Linux y conectarse a través del **Escritorio Remoto de Windows (RDP)**.

---

## 6. Procedimiento para Conectar Prototipos Biomédicos (ESP32 / AD8232)

Para conectar un prototipo por USB y exponerlo al entorno Debian:

1. **Desde PowerShell (Administrador en Windows):**
   ```powershell
   # 1. Listar dispositivos USB
   usbipd list

   # 2. Asociar el puerto USB (solo la primera vez)
   usbipd bind --busid <BUSID>

   # 3. Adjuntar el dispositivo a la instancia activa de WSL
   usbipd attach --wsl --busid <BUSID>
   ```

2. **Verificación dentro de la terminal de Debian:**
   ```bash
   lsusb                          # Verifica la presencia del chip (ej. CH340 / CP2102)
   ls -l /dev/ttyUSB* /dev/ttyACM* # Muestra el archivo de dispositivo asignado
   dmesg | tail                   # Evento de detección registrado por el kernel
   ```

3. **Permisos de usuario para comunicación serie sin `sudo`:**
   ```bash
   sudo usermod -aG dialout Mundkey23
   ```

---

## 7. ⚠️ Diferencias Cruciales entre WSL2 y Linux Nativo

Es fundamental comprender estas 4 diferencias durante las clases para evitar confusiones al realizar demostraciones en vivo:

### 7.1 El Kernel es compilado por Microsoft
El sistema ejecuta `6.18.33.2-microsoft-standard-WSL2`.
* ✅ **Funciona igual que Linux nativo:** Gestión de procesos, memoria, `journalctl`, comandos Bash, `systemd`.
* ❌ **Difiere:** Compilación de módulos de kernel personalizados, gestor de arranque GRUB y configuración de `/boot`.

### 7.2 Interacción entre Firewall de Windows y UFW (Módulo 3)
Al activar el modo de red *mirrored*, la interfaz de red primaria está regulada por el **Firewall de Windows Host**.
* **Impacto:** Si se configura UFW en Debian para bloquear un puerto, el Firewall de Windows puede permitir el paso antes de que UFW procese el paquete.
* **Solución de Clase:** Impartir la demostración de reglas de firewall **dentro de contenedores Docker** (Módulo 5), donde cada contenedor posee su propio espacio de nombres de red (*network namespace*) e `iptables` opera al 100% de forma aislada.

### 7.3 Flujo de Asignación de Dispositivos USB (Módulo 6)
En Linux nativo, conectar un cable USB genera un evento instantáneo en `dmesg`. En WSL2, se debe ejecutar primero `usbipd attach` desde Windows para reenviar el puerto hacia Linux.

### 7.4 Ciclo de Vida de `cron` (Módulo 9)
En un servidor físico, `cron` corre ininterrumpidamente 24/7. En WSL2, si se cierran todas las ventanas de la terminal y no hay procesos activos en segundo plano, la instancia de WSL se suspende y las tareas de `cron` no se ejecutarán hasta reabrir la terminal.

---

## 8. Herramientas Pendientes por Módulo

| Herramienta | Módulo correspondiente | Propósito |
|---|---|---|
| `docker` | Módulo 5 | Contenedores e aislamiento de aplicaciones |
| `ufw` | Módulo 3 | Demostración de reglas de firewall interno |
| `rsync` | Módulo 3 | Sincronización eficiente de archivos y datasets |
| `jq` | Módulo 8 | Procesamiento de respuestas JSON en terminal |
| `PlatformIO` | Módulo 6 | Compilación y despliegue de firmware C++ |
| `scikit-learn` | Módulo 4 | Modelos de Machine Learning en CPU |

---

*Documento actualizado el 23 de septiembre de 2026 para la clase de Linux, IA y Prototipos Biomédicos.*

