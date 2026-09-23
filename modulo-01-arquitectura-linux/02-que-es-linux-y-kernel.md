# Subtema 1.2: ¿Qué es Linux y la Arquitectura del Kernel?

---

## 1. Concepto de Linux y el Proyecto GNU

**Linux** es formalmente un núcleo o *kernel* de sistema operativo de código abierto y tipo UNIX, creado por Linus Torvalds en 1991. En combinación con el conjunto de herramientas del proyecto GNU (iniciado por Richard Stallman en 1983), conforma el sistema operativo **GNU/Linux**.

### Pilares Fundamentales:
* **Código Abierto y Licencia GPL:** Garantiza la libertad de estudiar, modificar y redistribuir el software.
* **Filosofía UNIX:** "Construir programas pequeños que hagan una sola cosa bien y que trabajen juntos mediante flujos de texto plano".
* **Aislamiento de Privilegios:** Separación estricta entre el espacio de usuario (*User Space*) y el espacio del núcleo (*Kernel Space*).

---

## 2. La Arquitectura del Kernel de Linux

El Kernel opera en **Ring 0** (el nivel de mayor privilegio del hardware) y actúa como mediador exclusivo entre los programas de aplicación y los componentes físicos del equipo.

```
+-------------------------------------------------------------------+
|                        ESPACIO DE USUARIO                         |
|         (Aplicaciones Python, Docker, Bash, Servidores Web)       |
+-------------------------------------------------------------------+
|               INTERFAZ DE LLAMADAS AL SISTEMA (SYSCALLS)          |
|                  open(), read(), write(), fork(), execve()        |
+-------------------------------------------------------------------+
|                        ESPACIO DEL KERNEL                         |
|  +---------------------+  +---------------------+                 |
|  | Planificador (CFS)  |  | Gestión de Memoria  |                 |
|  +---------------------+  +---------------------+                 |
|  | Drivers / USB/Serial|  | Pila de Red (TCP/IP)|                 |
|  +---------------------+  +---------------------+                 |
+-------------------------------------------------------------------+
|                            HARDWARE                               |
|          (CPU, RAM, Discos, Tarjetas de Red, Sensores USB)        |
+-------------------------------------------------------------------+
```

### Funciones Principales del Kernel:
1. **Planificación de Procesos:** Asignación de tiempo de CPU a través del algoritmo *Completely Fair Scheduler (CFS)*.
2. **Administración de Memoria Virtual:** Mapeo de espacio de direcciones de memoria, paginación y gestión del espacio de intercambio (*Swap*).
3. **Abstracción de Dispositivos (Drivers):** Conversión de peticiones de software en instrucciones electrónicas (por ejemplo, comunicación con `/dev/ttyUSB0`).
4. **Sistema de Archivos Virtual (VFS):** Proporciona una interfaz unificada para interactuar con archivos en ext4, Btrfs, XFS o sistemas de archivos virtuales (`/proc`, `/sys`).

---

## 3. Demonio de Inicialización (`systemd`) y Registros (`journalctl`)

### `systemd` (PID 1):
`systemd` es el primer proceso ejecutado por el kernel al arrancar. Administra los servicios en segundo plano, puntos de montaje y dispositivos mediante *unidades* (`.service`, `.socket`, `.mount`).

* **Comandos esenciales:**
  * `systemctl status <servicio>`: Revisa el estado de un servicio.
  * `systemctl start|stop|restart <servicio>`: Controla la ejecución del servicio.
  * `systemctl enable <servicio>`: Configura el servicio para que inicie automáticamente al arrancar.

### `journalctl`:
Herramienta para inspeccionar los registros centralizados indexados por `systemd-journald`.

* **Comandos de diagnóstico:**
  * `journalctl -u <servicio> -f`: Sigue los logs de un servicio en tiempo real.
  * `journalctl -b`: Filtra los registros correspondientes al arranque actual.

