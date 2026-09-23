# Guía de Estudio del Instructor: Linux, IA y Prototipos Biomédicos

> 📄 **Documentación del Entorno:** Para revisar la configuración técnica completa del sistema desplegado (Debian 13 *trixie*, WSL 2 modo espejo, `usbipd-win` y soporte GUI), consulte [ENTORNO_LINUX.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/ENTORNO_LINUX.md).

---

### [Módulo 1: Arquitectura y Ecosistema Linux (Introducción General)](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/TEORIA.md)
* **[01-introduccion-curso-y-roadmap.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/01-introduccion-curso-y-roadmap.md):** Visión general del curso y hoja de ruta completa (Módulos 1 al 10).
* **[02-que-es-linux-y-kernel.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/02-que-es-linux-y-kernel.md):** Concepto de GNU/Linux, espacio de usuario vs kernel, syscalls, systemd y journalctl.
* **[03-wsl2-debian-y-entorno.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/03-wsl2-debian-y-entorno.md):** Entorno de clase (WSL 2, Debian 13 *trixie*, modo espejo, usbipd-win, WSLg y KDE Plasma).
* **[04-distribuciones-y-servidores.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-01-arquitectura-linux/04-distribuciones-y-servidores.md):** Familias de distribuciones, Linux en servidores y justificación de Debian.

---

### [Módulo 2: Administración Profunda de Servidores](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-02-administracion-servidores/TEORIA.md)
* **[01-permisos-octales-y-propietarios.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-02-administracion-servidores/01-permisos-octales-y-propietarios.md):** Notación octal/simbólica, lectura/escritura/ejecución y `chown`.
* **[02-seguridad-ssh-y-hardening.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-02-administracion-servidores/02-seguridad-ssh-y-hardening.md):** Llaves Ed25519, `ssh-copy-id` y configuración de `sshd_config`.
* **[03-gestion-usuarios-y-sudoers.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-02-administracion-servidores/03-gestion-usuarios-y-sudoers.md):** Comandos `useradd`/`usermod` y sintaxis de `/etc/sudoers` con `visudo`.

---

### [Módulo 3: Redes y Transferencia de Datos](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-03-redes-transferencia-datos/TEORIA.md)
* **[01-modelo-tcpip-y-puertos.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-03-redes-transferencia-datos/01-modelo-tcpip-y-puertos.md):** Pila TCP/IP, resolución DNS y diagnóstico (`ss`, `lsof`, `nc`).
* **[02-firewalls-ufw-iptables.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-03-redes-transferencia-datos/02-firewalls-ufw-iptables.md):** Netfilter, comandos de UFW y comportamiento en WSL 2.
* **[03-sincronizacion-rsync-scp.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-03-redes-transferencia-datos/03-sincronizacion-rsync-scp.md):** Transferencia cifrada con `scp` y algoritmos delta con `rsync`.

---

### [Módulo 4: Infraestructura Híbrida para IA (Edge & Cloud)](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-04-infraestructura-ia/TEORIA.md)
* **[01-edge-vs-cloud-computing.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-04-infraestructura-ia/01-edge-vs-cloud-computing.md):** Cómputo en el borde vs nube y arquitecturas híbridas.
* **[02-librerias-ia-para-cpu.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-04-infraestructura-ia/02-librerias-ia-para-cpu.md):** NumPy, SciPy, Pandas y Scikit-learn para CPU.
* **[03-hardware-dedicado-cuda-gpus.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-04-infraestructura-ia/03-hardware-dedicado-cuda-gpus.md):** Arquitectura CUDA, Tensor Cores y diagnóstico con `nvidia-smi`.

---

### [Módulo 5: Arquitectura de Contenedores con Docker](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-05-docker-contenedores/TEORIA.md)
* **[01-arquitectura-docker-vs-vms.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-05-docker-contenedores/01-arquitectura-docker-vs-vms.md):** Demonio de Docker, namespaces, cgroups y comparación con VMs.
* **[02-redes-y-volumenes-docker.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-05-docker-contenedores/02-redes-y-volumenes-docker.md):** Modos de red bridge/host y volúmenes vs bind mounts.
* **[03-construccion-dockerfile.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-05-docker-contenedores/03-construccion-dockerfile.md):** Sintaxis de Dockerfile, caché de capas y buenas prácticas.

---

### [Módulo 6: Integración de Hardware Biomédico (PlatformIO)](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/TEORIA.md)
* **[01-protocolos-uart-i2c.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/01-protocolos-uart-i2c.md):** Buses serie UART e I2C para sensores de salud (ECG, SpO2).
* **[02-configuracion-platformio-ini.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/02-configuracion-platformio-ini.md):** Archivo `platformio.ini`, dependencias C++ y baud rates.
* **[03-depuracion-hardware-dmesg-dialout.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-06-hardware-biomedico-platformio/03-depuracion-hardware-dmesg-dialout.md):** `dmesg`, grupo `dialout` y passthrough USB con `usbipd-win`.

---

### [Módulo 7: Procesamiento de Flujos de Datos Médicos](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-07-procesamiento-datos-medicos/TEORIA.md)
* **[01-expresiones-regulares-regex.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-07-procesamiento-datos-medicos/01-expresiones-regulares-regex.md):** Sintaxis de metacaracteres para logs de salud.
* **[02-manipulacion-streams-awk-sed.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-07-procesamiento-datos-medicos/02-manipulacion-streams-awk-sed.md):** Edición en línea con `sed` y filtrado por columnas con `awk`.
* **[03-tuberias-redirecciones.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-07-procesamiento-datos-medicos/03-tuberias-redirecciones.md):** Redirecciones (`>`, `>>`, `2>`) y tuberías (`|`).

---

### [Módulo 8: Interacción Avanzada con APIs y Antigravity](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-08-apis-antigravity/TEORIA.md)
* **[01-ingenieria-prompts-codigo.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-08-apis-antigravity/01-ingenieria-prompts-codigo.md):** Diseño de prompts técnicos para agentes de IA.
* **[02-procesamiento-json-jq.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-08-apis-antigravity/02-procesamiento-json-jq.md):** Filtrado y transformación de JSON en terminal con `jq`.
* **[03-gestion-errores-rate-limits.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-08-apis-antigravity/03-gestion-errores-rate-limits.md):** Códigos HTTP (429, 503) y reintentos con backoff.

---

### [Módulo 9: Orquestación y Tareas Programadas](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-09-orquestacion-cron/TEORIA.md)
* **[01-automatizacion-crontab.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-09-orquestacion-cron/01-automatizacion-crontab.md):** Sintaxis de 5 campos de `crontab` y comandos.
* **[02-variables-entorno-env.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-09-orquestacion-cron/02-variables-entorno-env.md):** Variables `.env`, `.bashrc` y permisos `chmod 600`.
* **[03-redireccion-errores-logs-cron.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-09-orquestacion-cron/03-redireccion-errores-logs-cron.md):** Redirección `2>&1` y auditoría de logs.

---

### [Módulo 10: Troubleshooting de Prototipos Finales](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-10-troubleshooting-prototipos/TEORIA.md)
* **[01-depuracion-electronica-hardware.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-10-troubleshooting-prototipos/01-depuracion-electronica-hardware.md):** Fallas físicas de hardware, ruido en sensores y debounce.
* **[02-monitoreo-procesos-htop-kill.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-10-troubleshooting-prototipos/02-monitoreo-procesos-htop-kill.md):** Monitoreo de recursos con `htop`, `ps` y terminación con `kill -9`.
* **[03-analisis-logs-multicapa.md](file:///g:/My%20Drive/ClaseLinux/ClaseLinux23/modulo-10-troubleshooting-prototipos/03-analisis-logs-multicapa.md):** Depuración en 4 capas (Hardware ➔ Kernel ➔ Contenedores ➔ API Cloud).