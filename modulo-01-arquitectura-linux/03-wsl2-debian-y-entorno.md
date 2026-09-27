# Subtema 1.3: Entorno de Trabajo — WSL 2, Debian 13 y Soporte Gráfico

---

## 1. ¿Qué es un "Entorno" en Informática?

Antes de explicar WSL, es fundamental comprender qué significa **entorno** (*environment*) en el contexto de sistemas operativos.

Un **entorno de ejecución** es el conjunto completo de software, configuraciones, bibliotecas, variables y servicios que un programa necesita para funcionar correctamente. En términos prácticos:

* Un script de Python necesita un **entorno de Python** (intérprete, librerías instaladas, `PATH` configurado).
* Un servidor web necesita un **entorno Linux** (kernel, red TCP/IP, sistema de archivos, permisos de usuarios).
* Un prototipo biomédico necesita un **entorno de desarrollo embebido** (compilador de C++, acceso a puertos serie, PlatformIO).

Cuando decimos que "montamos un entorno Linux en Windows", estamos creando una copia funcional completa de ese ecosistema de software — con su propio kernel, su propio sistema de archivos, sus propios usuarios y su propia red — que se ejecuta **dentro** de la máquina Windows sin reemplazarla.

---

## 2. ¿Por Qué Simular Linux Dentro de Windows?

En un escenario ideal, cada estudiante tendría una computadora con Linux instalado directamente en su disco duro (*Linux nativo en bare-metal*). Sin embargo, en la práctica:

* La mayoría de los alumnos traen laptops con **Windows preinstalado**, licenciado y configurado con sus herramientas académicas.
* Formatear una partición para instalar Linux genera **riesgo de pérdida de datos** y problemas de compatibilidad con drivers propietarios.
* Administrar un **dual-boot** (dos sistemas operativos en el mismo disco con menú GRUB) añade complejidad técnica que no aporta al temario.

### Las Tres Opciones para Tener Linux en un Equipo Windows:

| Método | Descripción | Ventajas | Desventajas |
|---|---|---|---|
| **Dual-Boot** | Instalar Linux en una partición separada del disco y elegir al encender. | Linux nativo al 100%. | Riesgo al particionar; hay que reiniciar para cambiar de SO. |
| **Máquina Virtual (VM)** | Ejecutar Linux dentro de VirtualBox/VMware como un "computador dentro del computador". | Aislamiento total. | Consumo pesado de RAM; passthrough USB complejo. |
| **WSL 2** | Ejecutar un kernel Linux real dentro de una capa de virtualización ultra-ligera integrada en Windows. | Arranque instantáneo; integración nativa con Windows; Docker compatible; passthrough USB con `usbipd-win`. | Algunas diferencias con Linux nativo (ver sección 5). |

> **Decisión del curso:** Se eligió **WSL 2** porque combina la autenticidad de un kernel Linux real con la practicidad de no modificar las laptops de los alumnos, y porque soporta el passthrough USB necesario para los sensores biomédicos del Módulo 6.

---

## 3. WSL 2: Arquitectura Técnica

**WSL 2** (Windows Subsystem for Linux, versión 2) no es un emulador ni un traductor de comandos. Ejecuta un **núcleo Linux real** compilado por Microsoft (`6.18.33.2-microsoft-standard-WSL2`) dentro de una máquina virtual ultra-ligera gestionada por el hipervisor **Hyper-V**.

```
+------------------------------------------------------------------+
|                     Windows 10 / 11 Host                         |
+---------------------------------+--------------------------------+
|  Aplicaciones Windows           |  Instancia de WSL 2            |
|  (VS Code, PowerShell, Chrome)  |  (Debian 13 Trixie)           |
|                                 |  - Kernel Linux real           |
|                                 |  - systemd activo              |
|                                 |  - Archivos en ext4            |
|                                 |  - Red propia o compartida     |
+---------------------------------+--------------------------------+
|               Hipervisor Hyper-V (Capa de Virtualización)        |
+------------------------------------------------------------------+
|                        Hardware Físico                            |
+------------------------------------------------------------------+
```

### Comparación WSL 1 vs. WSL 2:

| Característica | WSL 1 | WSL 2 |
|---|---|---|
| **Arquitectura** | Traducción de syscalls de Linux a llamadas de Windows NT | **Núcleo Linux real** sobre hipervisor Hyper-V |
| **Rendimiento de Disco** | Lento (cada operación se traducía) | Alto rendimiento en `/home/` (ext4 nativo) |
| **Compatibilidad Docker** | Parcial, requería hacks | **100% nativa** (contenedores reales) |
| **`systemd`** | No disponible | **Funcional** (PID 1 activo) |
| **Passthrough USB** | Limitado | Compatible vía **`usbipd-win`** |

---

## 4. Infraestructura Específica Configurada en Clase

El entorno montado el 23 de septiembre de 2026 comprende:

* **Distribución:** Debian GNU/Linux 13 (*Trixie*) — Rama Estable.
* **Kernel:** `6.18.33.2-microsoft-standard-WSL2`.
* **Usuario:** `Mundkey23` (uid 1000, grupo `sudo`).
* **Red en Modo Espejo (*Mirrored Mode*):** Configurada en `C:\Users\yoryi\.wslconfig` con `networkingMode=mirrored`. Permite que Debian comparta la dirección IP física de Windows (`192.168.1.78`), haciendo que cualquier servidor que se levante en Debian sea directamente accesible desde la red local del salón de clase.
* **Passthrough USB:** Servicio `usbipd-win` 5.3.0 que permite mapear físicamente placas ESP32, sensores AD8232 y convertidores CH340 desde Windows hacia `/dev/ttyUSB0` o `/dev/ttyACM0` en Debian.

---

## 5. Diferencias Clave entre WSL 2 y un Linux Nativo

Estas son las diferencias que pueden aparecer durante las demostraciones en clase:

| Aspecto | Linux Nativo | WSL 2 |
|---|---|---|
| **Kernel** | Distribuido por Debian (paquete `linux-image`) | Compilado por Microsoft (`-microsoft-standard-WSL2`) |
| **Arranque / GRUB** | El estudiante ve el menú GRUB al encender | No existe; WSL arranca con `wsl -d Debian` |
| **Firewall** | `iptables`/`nftables` controla todo el tráfico | El **Firewall de Windows** tiene prioridad en modo espejo |
| **USB** | Conectas el sensor y aparece automáticamente en `dmesg` | Requiere paso intermedio: `usbipd attach --wsl` |
| **Cron / Servicios** | Corren 24/7 mientras el servidor esté encendido | Se suspenden si se cierran todas las terminales de WSL |

---

## 6. Entornos de Escritorio Gráficos (GUI) y KDE Plasma

### ¿Qué es un Entorno de Escritorio (*Desktop Environment - DE*)?
A diferencia de Windows y macOS, donde la interfaz gráfica está fusionada con el kernel, en Linux la GUI es una **capa modular e intercambiable**:

1. **Servidor Gráfico:** Wayland (moderno) o X11 (clásico).
2. **Gestor de Ventanas (*Window Manager*):** Controla bordes, posición y animaciones (ej. KWin, Mutter).
3. **Entorno de Escritorio (*Desktop Environment*):** Suite completa con barra de tareas, menús, explorador de archivos y aplicaciones integradas (ej. KDE Plasma, GNOME, XFCE).

### ¿Qué es KDE Plasma?
KDE Plasma es uno de los entornos gráficos más avanzados y personalizables del mundo Linux, con componentes como KWin (gestor de ventanas), Dolphin (explorador de archivos) y Konsole (terminal).

### Evaluación en WSL 2 y Decisión Final:
Se probó la instalación de KDE Plasma (1365 paquetes), pero fue **desinstalada limpiamente** (`autoremove --purge`). La razón: **WSLg** (integrado en WSL 2) ya permite abrir aplicaciones gráficas individuales de Linux (como Wireshark o editores) directamente como ventanas nativas de Windows, ahorrando más de 1.5 GB de RAM que se destina a contenedores Docker y modelos de IA.
