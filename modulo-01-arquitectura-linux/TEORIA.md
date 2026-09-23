# Módulo 1: Arquitectura y Ecosistema Linux

---

## 1. ¿Qué es Linux?

**Linux** (referido formalmente como **GNU/Linux**) es un sistema operativo libre, de código abierto y tipo UNIX, creado originalmente por **Linus Torvalds** en 1991. En combinación con las herramientas de software del proyecto GNU y desarrollos de la comunidad de código abierto, conforma un sistema operativo completo y robusto.

### Características Principales:
* **Código Abierto y Software Libre:** Distribuido bajo la Licencia Pública General de GNU (GPL), lo que permite que cualquier persona inspeccione, modifique y distribuya su código fuente.
* **Filosofía UNIX:**
  * Todo en el sistema se representa como un archivo o un proceso.
  * Herramientas modulares y especializadas que se comunican mediante tuberías (`|`) y flujos estándar (`stdin`, `stdout`, `stderr`).
* **Multiusuario y Multitarea:** Diseñado desde su origen para aislar procesos y gestionar múltiples usuarios con permisos estrictos de acceso.
* **Portabilidad:** Se ejecuta en microcontroladores, computadoras monoplaca (Raspberry Pi), computadoras de escritorio, servidores empresariales y supercomputadoras.

```
+-------------------------------------------------------------------+
|                     Aplicaciones de Usuario                       |
|         (Bash, Python, Docker, Navegadores, Modelos IA)           |
+-------------------------------------------------------------------+
|                   Librerías y APIs del Sistema                    |
|                 (glibc, interfaz POSIX, systemd)                  |
+-------------------------------------------------------------------+
|                           NÚCLEO (KERNEL)                         |
| (Planificador, Gestión de Memoria, Drivers, Pila de Red)          |
+-------------------------------------------------------------------+
|                             HARDWARE                              |
|       (CPU, RAM, Almacenamiento, Interfaces de Red, USB/Medicina) |
+-------------------------------------------------------------------+
```

---

## 2. ¿Qué es el Kernel (Núcleo)?

El **Kernel** es el componente central y fundamental del sistema operativo. Opera en el nivel de mayor privilegio del hardware (**Kernel Space** o Anillo 0) y actúa como puente de comunicación e intermediación entre las aplicaciones de usuario (**User Space** o Anillo 3) y los recursos físicos del equipo.

### Funciones Principales del Kernel:

1. **Gestión de Procesos:**
   * Asigna tiempo de procesador a cada programa en ejecución mediante el planificador **Completely Fair Scheduler (CFS)**.
   * Administra el ciclo de vida de los procesos, hilos de ejecución (*threads*) y la comunicación entre procesos (IPC).

2. **Gestión de Memoria:**
   * Asigna y libera memoria RAM física y memoria virtual (Paging y espacio de Intercambio / Swap).
   * Mantiene el aislamiento de espacio de memoria entre procesos para evitar que un fallo en una aplicación corrompa a otras.

3. **Controladores de Dispositivos (Drivers):**
   * Modula las peticiones del software en instrucciones comprensibles por el hardware (tarjetas de red, controladores de disco, convertidores USB-Serial como `/dev/ttyACM0`).

4. **Sistema de Archivos y Almacenamiento:**
   * Traduce las operaciones de lectura y escritura a través de la capa de abstracción VFS (*Virtual File System*) hacia sistemas de archivos como ext4, Btrfs o XFS.

5. **Pila de Red (Network Stack):**
   * Gestiona los protocolos de red (TCP/IP, UDP), sockets y filtrado de paquetes en el firewall interno (`netfilter`/`iptables`/`nftables`).

6. **Llamadas al Sistema (Syscalls):**
   * Expone funciones de control (como `open()`, `read()`, `write()`, `fork()`, `execve()`) para que los programas soliciten servicios al núcleo de forma segura.

---

## 3. Sistema de Inicialización (`systemd`) y Registros (`journalctl`)

### `systemd`:
Es el demonio de inicialización de Linux (PID 1) que reemplazó al antiguo SysVinit. Se encarga de arrancar el sistema, administrar los servicios en segundo plano (*units*), controlar dispositivos y gestionar el inicio de sesión de usuarios.

* **Servicios (`.service`):** Archivos de configuración en `/etc/systemd/system/` que definen cómo iniciar, detener y reiniciar un programa.
* **Comandos clave:**
  * `systemctl status <servicio>`: Muestra el estado actual de un servicio.
  * `systemctl start <servicio>`: Inicia un servicio.
  * `systemctl stop <servicio>`: Detiene un servicio.
  * `systemctl enable <servicio>`: Configura el servicio para que arranque automáticamente con el sistema.

### `journalctl`:
Es la herramienta centralizada para consultar los registros (*logs*) recopilados por `systemd-journald`.

* **Comandos de inspección de registros:**
  * `journalctl -u <servicio>`: Filtra registros por un servicio específico.
  * `journalctl -f`: Sigue los registros en tiempo real (*live stream*).
  * `journalctl -b`: Muestra solo los registros del arranque actual del sistema.
  * `journalctl -p err`: Sigue únicamente los eventos con nivel de severidad de error.

---

## 4. ¿Qué es WSL (Windows Subsystem for Linux) y Por Qué lo Usaremos?

**WSL** es una característica de Microsoft Windows que permite a los desarrolladores ejecutar un entorno Linux nativo de 64 bits directamente en Windows, sin la sobrecarga de máquinas virtuales tradicionales ni particiones de doble arranque (*dual-boot*).

### WSL 1 vs. WSL 2:

* **WSL 1:** Traducía las llamadas del sistema operativo Linux a llamadas del núcleo de Windows NT. Tenía limitaciones de compatibilidad y rendimiento de lectura/escritura en disco.
* **WSL 2 (Estándar Actual):** Ejecuta un **núcleo Linux real y completo** dentro de una arquitectura de máquina virtual ligera administrada por Hyper-V.

### Ventajas de WSL 2:
* **Compatibilidad 100% con el Kernel de Linux:** Permite ejecutar contenedores Docker nativos, `systemd`, `fuse` y pilas complejas de red.
* **Alto Rendimiento en Disco:** Lectura y escritura ultrarrápida cuando se trabaja dentro del sistema de archivos Linux (`/home/usuario/`).
* **Integración Transparente:**
  * Acceso a archivos de Windows desde Linux (`/mnt/c/`).
  * Acceso a archivos de Linux desde Windows (`\\wsl$\Debian`).
  * Ejecución cruzada de binarios entre la terminal Bash y PowerShell.
* **Aceleración por GPU:** Soporta NVIDIA CUDA directamente desde Linux dentro de Windows para cargas de trabajo de Inteligencia Artificial.

### ¿Por qué lo usaremos en este curso?
Permite tener un entorno idéntico a un servidor de producción Linux (con Debian) manteniendo la versatilidad de la máquina anfitriona Windows, garantizando que el código de hardware biomédico, redes y modelos de IA se ejecute de manera profesional sin poner en riesgo el sistema operativo principal.

---

## 5. Distribuciones Linux y Ventajas de Cada Una

Una **Distribución Linux** ("Distro") es un paquete integrado por el kernel Linux, utilidades GNU, un gestor de paquetes, controladores y software preconfigurado.

```
                  +-------------------------+
                  |      Kernel Linux       |
                  +------------+------------+
                               |
       +-----------------------+-----------------------+
       |                       |                       |
+------v------+         +------v------+         +------v------+
|Familia Debian|        |Familia RedHat|        | Familia Arch|
+------+------+         +------+------+         +------+------+
       |                       |                       |
  +----+----+             +----+----+             +----+----+
  |         |             |         |             |         |
Debian   Ubuntu         RHEL   Fedora/Rocky     Arch     Manjaro
```

### Tabla Comparativa de Distribuciones Principales:

| Distribución | Familia Origen | Gestor de Paquetes | Modelo de Lanzamiento | Ventajas y Casos de Uso |
| :--- | :--- | :--- | :--- | :--- |
| **Debian** | Debian | `apt` / `dpkg` | Fijo / Ultra-estable | Máxima estabilidad, bajo consumo de recursos, universal para servidores y nodos de cómputo. |
| **Ubuntu** | Debian | `apt` / `snap` | Fijo (LTS cada 2 años) | Gran soporte de comunidad, excelente detección de hardware, estándar en imágenes cloud. |
| **Red Hat (RHEL)** | Red Hat | `dnf` / `rpm` | Fijo (LTS Empresarial) | Estándar corporativo global, soporte técnico comercial, seguridad estricta con SELinux. |
| **Rocky / AlmaLinux** | Red Hat | `dnf` / `rpm` | Fijo (Compatible 1:1 con RHEL) | Reemplazo gratuito de CentOS para servidores de misión crítica. |
| **Fedora** | Red Hat | `dnf` / `rpm` | Rápido (Ciclo de 6 meses) | Incorpora las últimas tecnologías y versiones de software; laboratorio de innovación de Red Hat. |
| **Arch Linux** | Arch | `pacman` | *Rolling Release* (Actualización continua) | Personalización total, software siempre actualizado (*bleeding-edge*), documentación excepcional (Arch Wiki). |
| **Alpine Linux** | Independiente | `apk` | Fijo | Tamaño extremadamente reducido (~5MB base), ideal para contenedores Docker ultra-ligeros. |

---

## 6. Servidores: ¿Cuáles Distribuciones Usan y Por Qué?

Los servidores de producción (servidores web, infraestructura cloud, clústeres de IA, nodos médicos) priorizan la **estabilidad**, **seguridad** y **predecibilidad** sobre tener la versión más reciente de un software.

### Distribuciones Más Usadas en Servidores:
1. **Debian GNU/Linux:** Utilizado por infraestructura independiente, servidores de aplicaciones y como base principal de contenedores Docker.
2. **Ubuntu Server:** La distribución preferida en nubes públicas (AWS, GCP, Azure) por su facilidad de automatización y soporte de paquetes modernos.
3. **RHEL / Rocky Linux / AlmaLinux:** La opción preferida por corporaciones bancarias, industrias de salud y organismos gubernamentales que requieren certificaciones de seguridad estrictas.

---

## 7. Ramas de Debian y Por Qué Usaremos Debian en este Curso

Debian organiza su desarrollo en tres ramas principales:
* **Debian Stable (Estable):** Recibe únicamente parches de seguridad. Es la versión recomendada para servidores y entornos donde la estabilidad sea prioritaria.
* **Debian Testing (Pruebas):** Contiene paquetes que han superado pruebas iniciales; se prepara para ser la siguiente versión estable.
* **Debian Unstable / Sid (Inestable):** Rama de desarrollo continuo donde se prueban las últimas versiones de software.

### ¿Por qué usaremos Debian en este curso?
1. **El Sistema Operativo Universal:** Es la base de donde derivan Ubuntu, Linux Mint y Raspberry Pi OS (Raspbian). Aprender Debian garantiza el dominio de toda esa familia.
2. **Estabilidad Inigualable:** Garantiza que las dependencias de Python, herramientas de red y librerías de hardware no cambien de forma inesperada rompiendo nuestros prototipos.
3. **Eficiencia de Recursos:** Consume un mínimo de memoria RAM y CPU, siendo ideal para correr en WSL 2, contenedores Docker y dispositivos integrados.
4. **Filosofía de Código Libre:** Sin software propietario innecesario ni recolección de datos de telemetría.
