# Subtema 1.3: Entorno de Trabajo — WSL 2, Debian 13 y Soporte Gráfico

---

## 1. WSL 2 (Windows Subsystem for Linux v2)

**WSL 2** es la infraestructura elegida para la impartición de este curso. Utiliza una arquitectura de virtualización ligera sobre Hyper-V que ejecuta un **núcleo Linux real de 64 bits** dentro de Windows.

### Comparación WSL 1 vs. WSL 2:

| Característica | WSL 1 | WSL 2 |
|---|---|---|
| **Arquitectura** | Traducción de llamadas al sistema a Windows NT | Núcleo Linux real sobre hipervisor ligero |
| **Rendimiento de Disco (I/O)** | Limitado por la capa de traducción | Alto rendimiento en el sistema de archivos nativo |
| **Compatibilidad con Docker** | Incompleta (requiere hacks) | **100% Nativa** (soporta contenedores y systemd) |
| **Passthrough de Hardware** | Limitado | Compatible con **USB/IP** (`usbipd-win`) |

---

## 2. Infraestructura Configurada en Clase

El entorno montado el 23 de septiembre de 2026 comprende:
* **Distribución:** Debian GNU/Linux 13 (*trixie*).
* **Kernel:** `6.18.33.2-microsoft-standard-WSL2`.
* **Red en Modo Espejo (*Mirrored Mode*):** Configurada en `C:\Users\yoryi\.wslconfig` (`networkingMode=mirrored`). Permite que Debian comparta la misma IP física de Windows (`192.168.1.78`), garantizando que servidores web o sockets IoT sean alcanzables desde la red local.
* **Passthrough USB:** Servicio `usbipd-win` 5.3.0 que reenvía puertos serie (ESP32, sensores AD8232) desde Windows hacia `/dev/ttyUSB0` o `/dev/ttyACM0` en Debian.

---

## 3. Entornos de Escritorio Gráficos (GUI) y KDE Plasma

### ¿Qué es un Entorno de Escritorio (*Desktop Environment - DE*)?
A diferencia de otros sistemas operativos, la interfaz gráfica en Linux es una capa modular independiente del kernel. Se compone de:
1. **Servidor Gráfico:** Wayland o X11.
2. **Gestor de Ventanas (Window Manager):** Controla el encuadre y renderizado de ventanas (ej. KWin).
3. **Entorno de Escritorio (DE):** Suite gráfica integrada con barra de tareas, menús y gestor de archivos.

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

### ¿Qué es KDE Plasma y por qué se revirtió su instalación?
* **KDE Plasma:** Uno de los entornos gráficos más avanzados y personalizables del mundo Linux.
* **Evaluación en WSL2:** Se probó su instalación completa (1365 paquetes), pero fue desinstalada limpiamente (`autoremove --purge`). La razón técnica es que **WSLg** ya permite abrir aplicaciones gráficas individuales (como Wireshark) directamente como ventanas integradas en Windows, ahorrando más de 1.5 GB de memoria RAM para nuestros modelos de IA y contenedores Docker.

