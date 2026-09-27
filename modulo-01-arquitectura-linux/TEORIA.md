# Módulo 1: Arquitectura y Ecosistema Linux (Índice Maestro de Teoría)

> 📄 **Documento de Infraestructura:** Para revisar la configuración técnica completa del sistema desplegado en clase (Debian 13 *trixie*, WSL 2 modo espejo, `usbipd-win` y soporte GUI), consulte [ENTORNO_LINUX.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/ENTORNO_LINUX.md).

---

## Subtemas del Módulo 1:

1. 📌 **[01-introduccion-curso-y-roadmap.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/01-introduccion-curso-y-roadmap.md)**
   * Visión general del curso de *Linux, IA y Prototipos Biomédicos*.
   * Descripción detallada de lo que se llevará a cabo en los 10 módulos.
   * Metodología de laboratorio y casos de uso biomédicos.

2. 📌 **[02-que-es-linux-y-kernel.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/02-que-es-linux-y-kernel.md)**
   * ¿Qué es Linux y el Proyecto GNU?
   * Arquitectura del Kernel (User Space vs Kernel Space, Syscalls).
   * Sistema de inicialización `systemd` y registros centralizados con `journalctl`.

3. 📌 **[03-wsl2-debian-y-entorno.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/03-wsl2-debian-y-entorno.md)**
   * ¿Qué es un "entorno" en informática y por qué se simula Linux dentro de Windows?
   * Comparación de opciones: Dual-Boot vs Máquina Virtual vs WSL 2.
   * WSL 2 vs WSL 1 (arquitectura y ventajas).
   * Diferencias clave entre WSL 2 y un Linux nativo (lo que aparecerá en demos).
   * Configuración de la infraestructura de clase (Debian 13 *trixie*, modo espejo `192.168.1.78`, `usbipd-win`).
   * Entornos de escritorio gráficos (GUI), soporte WSLg y análisis sobre **KDE Plasma**.

4. 📌 **[04-distribuciones-y-servidores.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/04-distribuciones-y-servidores.md)**
   * Familias de distribuciones Linux (Debian, Red Hat, Arch, Alpine).
   * Distribuciones para servidores y por qué dominan la industria.
   * Ramas de Debian (Stable, Testing, Sid) y justificación de Debian para el curso.

5. 📌 **[05-historia-de-debian.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/05-historia-de-debian.md)**
   * Origen de Debian: Ian Murdock y el Manifiesto Debian (1993).
   * Línea de tiempo completa (de Buzz a Trixie).
   * La tradición de los nombres de Toy Story.
   * El Contrato Social de Debian y las DFSG.
   * El sistema de tres ramas (Unstable → Testing → Stable).
   * Legado: el árbol de distribuciones derivadas.
