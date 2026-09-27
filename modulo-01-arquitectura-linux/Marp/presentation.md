---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 1"
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

# Módulo 1: Arquitectura y Ecosistema Linux
### Introducción General y Fundamentos del Sistema

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

- **1.1** Introducción General y Hoja de Ruta del Curso
- **1.2** ¿Qué es GNU/Linux y la Arquitectura del Kernel?
- **1.3** Inicialización con `systemd` y Registros en `journalctl`
- **1.4** ¿Qué es un "Entorno"? ¿Por qué Linux dentro de Windows?
- **1.5** Entorno de Clase: WSL 2, Debian 13 y Red en Modo Espejo
- **1.6** Soporte Gráfico (WSLg) y KDE Plasma
- **1.7** Distribuciones Linux, Servidores y Elección de Debian
- **1.8** Historia de Debian: De Ian Murdock a Trixie

---

## 1.1 Introducción y Hoja de Ruta del Curso

Este curso integra tres disciplinas en un solo flujo de trabajo:

**Sistemas Operativos** + **Inteligencia Artificial** + **Hardware Biomédico**

- **M1 – M3:** Fundamentos Linux, permisos, SSH y redes TCP/IP.
- **M4 – M5:** Infraestructura de IA (CPU/GPU, CUDA) y Docker.
- **M6 – M7:** Adquisición de señales biomédicas (ECG, SpO2) con UART/I2C.
- **M8 – M10:** APIs de IA, automatización con Cron y troubleshooting.

> **Metodología:** Cada módulo combina teoría + laboratorio práctico.

---

## 1.1 (cont.) ¿Por Qué Esta Combinación?

En el mundo real, un ingeniero biomédico necesita:

1. **Administrar un servidor Linux** donde corren modelos de IA para diagnóstico.
2. **Adquirir señales biológicas** desde sensores conectados por USB/serie.
3. **Automatizar pipelines** de datos que se ejecuten 24/7.

Linux es el sistema operativo que unifica las tres áreas: corre en servidores de hospitales, en Raspberry Pis que leen sensores, y en clústeres de GPU donde se entrenan modelos.

---

## 1.2 ¿Qué es GNU/Linux?

- **Linux** es solo el **kernel** — el núcleo del sistema operativo.
- **GNU** (GNU's Not Unix) es el proyecto de Richard Stallman (1983) que creó las herramientas del espacio de usuario: `bash`, `gcc`, `coreutils`, `glibc`.
- **GNU/Linux** = Kernel Linux + Herramientas GNU + Software adicional.

> **Dato clave:** Linus Torvalds publicó Linux 0.01 el **17 de septiembre de 1991** con un famoso mensaje en comp.os.minix: *"I'm doing a (free) operating system… just a hobby, won't be big and professional."*

---

## 1.2 (cont.) Arquitectura del Kernel

```
  [ Aplicación (Python, Docker, Wireshark) ]
                     |
                (Syscalls) ← open(), read(), write(), fork()
                     |
         +-----------v-----------+
         |   KERNEL (Anillo 0)   |
         |  CFS · VFS · MM · Net|
         +-----------+-----------+
                     |
            [ Hardware Físico ]
```

El kernel es el **mediador exclusivo** entre cualquier aplicación y el hardware. Ningún programa toca disco, red o memoria directamente.

---

## 1.2 (cont.) Espacio de Usuario vs Espacio del Kernel

| Capa | Privilegios | Ejemplos |
|---|---|---|
| **User Space** (Anillo 3) | Restringido — no puede tocar hardware | Python, `bash`, Docker, VS Code |
| **Kernel Space** (Anillo 0) | Total — controla todo el hardware | Drivers, planificador CFS, VFS |

Si una app en User Space intenta acceder al disco directamente, el kernel la detiene con un **Segmentation Fault**. Toda comunicación pasa por la barrera de las **syscalls**.

---

## 1.3 `systemd` — El Administrador de Servicios

**`systemd` (PID 1):** Primer proceso en arrancar. Controla todos los servicios del sistema:

```bash
systemctl status sshd          # Estado del servidor SSH
systemctl start nginx          # Iniciar Nginx
systemctl enable docker        # Inicio automático con el sistema
systemctl restart networking   # Reiniciar la red
```

---

## 1.3 (cont.) `journalctl` — Registros Centralizados

**`journalctl`:** Gestor de logs indexados del sistema:

```bash
journalctl -u sshd -f          # Seguimiento en tiempo real de SSH
journalctl --since "1 hour ago" # Logs de la última hora
journalctl -p err               # Solo errores
```

> Todos los servicios escriben en un solo lugar. Ya no es necesario buscar en `/var/log/` archivo por archivo.

---

## 1.4 ¿Qué es un "Entorno"?

Un **entorno de ejecución** (*environment*) es el conjunto completo de:

- **Software** (kernel, bibliotecas, intérpretes)
- **Configuraciones** (variables de entorno, permisos)
- **Servicios** (red TCP/IP, systemd, acceso a disco)

…que un programa necesita para funcionar.

> **Ejemplo:** Un script de Python necesita un intérprete, librerías (`pip`), y un `PATH` configurado. Si falta cualquiera, falla. Eso es "su entorno".

Cuando decimos **"montamos un entorno Linux en Windows"**, significa que creamos una copia funcional completa del ecosistema Linux dentro de nuestra máquina Windows.

---

## 1.4 (cont.) ¿Por Qué No Instalamos Linux Directamente?

En un mundo ideal, cada alumno tendría Linux nativo. En la práctica:

- Las laptops vienen con **Windows preinstalado** y licenciado.
- Particionar el disco arriesga **pérdida de datos**.
- Administrar un **dual-boot** con GRUB añade complejidad innecesaria.

Solución: ejecutar Linux **dentro** de Windows, sin modificar el equipo.

---

## 1.4 (cont.) Tres Opciones para Linux en Windows

| Método | Ventaja | Desventaja |
|---|---|---|
| **Dual-Boot** | Linux nativo 100% | Hay que reiniciar para cambiar de SO |
| **Máquina Virtual** | Aislamiento total | Pesado (RAM), USB complejo |
| **WSL 2** | Arranque instantáneo + USB | Algunas diferencias con nativo |

> **Decisión:** Elegimos **WSL 2** porque combina un kernel Linux real con cero riesgo para las laptops de los alumnos.

---

## 1.5 WSL 2: Arquitectura Técnica

**WSL 2** no es un emulador. Ejecuta un **kernel Linux real** compilado por Microsoft sobre Hyper-V.

```
+---------------------------+---------------------------+
|   Apps Windows            |   WSL 2 (Debian 13)       |
|   (VS Code, PowerShell)   |   Kernel Linux real       |
|                           |   systemd + ext4          |
+---------------------------+---------------------------+
|         Hipervisor Hyper-V (Virtualización)            |
+-------------------------------------------------------+
|                  Hardware Físico                        |
+-------------------------------------------------------+
```

---

## 1.5 (cont.) WSL 1 vs WSL 2

| Característica | WSL 1 | WSL 2 |
|---|---|---|
| **Arquitectura** | Traducía syscalls a Windows NT | **Kernel Linux real** |
| **Rendimiento de Disco** | Lento (cada operación traducida) | Alto en `/home/` (ext4 nativo) |
| **Docker** | Parcial, con hacks | **100% nativo** |
| **`systemd`** | No disponible | **Funcional** (PID 1) |
| **Passthrough USB** | Limitado | Compatible vía `usbipd-win` |

---

## 1.5 (cont.) Infraestructura de Clase

| Componente | Valor |
|---|---|
| **Distribución** | Debian 13 (*Trixie*) — Rama Estable |
| **Kernel** | `6.18.33.2-microsoft-standard-WSL2` |
| **Usuario** | `Mundkey23` (uid 1000, grupo `sudo`) |
| **Red** | Modo espejo — IP `192.168.1.78` (compartida con Windows) |
| **USB** | `usbipd-win` 5.3.0 → `/dev/ttyUSB0` |

**Modo espejo (*Mirrored Mode*):** Permite que cualquier servidor que se levante en Debian sea accesible desde la red local del salón, sin problemas de NAT.

---

## 1.5 (cont.) WSL 2 vs Linux Nativo — Diferencias en Demo

| Aspecto | Linux Nativo | WSL 2 |
|---|---|---|
| **Kernel** | Distribuido por Debian | Compilado por Microsoft |
| **Arranque** | Menú GRUB al encender | `wsl -d Debian` |
| **Firewall** | `iptables`/`nftables` | Firewall de Windows tiene prioridad |
| **USB** | Plug-and-play | Requiere `usbipd attach --wsl` |
| **Servicios** | 24/7 mientras el servidor encienda | Se suspenden si se cierran las terminales |

---

## 1.6 Soporte Gráfico y KDE Plasma

### Capas de la GUI en Linux (modular e intercambiable):

```
Entorno de Escritorio (KDE, GNOME, XFCE)
    ↑
Gestor de Ventanas (KWin, Mutter)
    ↑
Servidor Gráfico (Wayland / X11)
    ↑
Kernel + Drivers
```

- **WSLg:** Apps gráficas de Linux abren como ventanas nativas de Windows.
- **KDE Plasma** fue evaluado (1365 paquetes) pero revertido → WSLg ahorra +1.5 GB de RAM.

---

## 1.7 Ecosistema de Distribuciones Linux

```
                 Kernel Linux
                      |
      +---------------+--------------+
      |               |              |
  Fam. Debian    Fam. Red Hat   Fam. Arch
      |               |              |
  +---+---+      +----+---+     +----+---+
  |       |      |        |     |        |
Debian  Ubuntu  RHEL   Fedora  Arch  Manjaro
```

---

## 1.7 (cont.) Comparativa de Familias

| Familia | Paquetes | Gestor | Enfoque |
|---|---|---|---|
| **Debian** | `.deb` | `apt` / `dpkg` | Estabilidad y libertad |
| **Red Hat** | `.rpm` | `dnf` / `yum` | Corporativo, SLAs |
| **Arch** | `.pkg.tar.zst` | `pacman` | Bleeding-edge, rolling release |
| **Alpine** | `.apk` | `apk` | Contenedores ultra-ligeros |

---

## 1.7 (cont.) Linux en Servidores — ¿Quién Domina y Por Qué?

Los servidores de producción exigen tres cosas:

1. **Estabilidad** — Las librerías no cambian inesperadamente.
2. **Soporte LTS** — Parches de seguridad sin actualizar el SO base.
3. **Bajo consumo** — Sin GUI, 100% de recursos para cargas de trabajo.

| Segmento | Distribución Dominante |
|---|---|
| Nube pública (AWS, GCP, Azure) | **Debian / Ubuntu Server** |
| Sector bancario y gubernamental | **RHEL / Rocky Linux** |
| Contenedores Docker | **Debian slim / Alpine** |
| IoT y embebidos | **Raspberry Pi OS (Debian)** |

---

## 1.7 (cont.) ¿Por Qué Debian para Este Curso?

Debian es **"El Sistema Operativo Universal"**:

1. **Conocimiento transferible** — Dominar Debian = dominar Ubuntu, Mint, Raspberry Pi OS.
2. **Estabilidad garantizada** — Las dependencias de Python, C++ y herramientas de red no se rompen.
3. **Base de contenedores** — `debian:slim` es la imagen Docker estándar.
4. **Libre de telemetría** — A diferencia de Ubuntu, no envía datos de uso.
5. **Temario concreto** — Las tres ramas (Stable, Testing, Sid) son demostrables en vivo.

---

## 1.8 Historia de Debian

**16 de agosto de 1993:** **Ian Murdock** (20 años, estudiante en Purdue) funda el proyecto Debian y publica el **Manifiesto Debian**.

> *"Debian será desarrollada abiertamente, en el espíritu de Linux y de GNU."*

El nombre **Debian** = **Deb**ra (su novia) + **Ian** (él mismo).

Ian falleció en 2017. La comunidad mantiene su legado con más de 1,000 desarrolladores activos en todo el mundo.

---

## 1.8 (cont.) Línea de Tiempo

| Año | Evento |
|---|---|
| 1993 | Fundación y Manifiesto Debian |
| 1996 | Debian 1.1 *Buzz* — primera versión con nombre de Toy Story |
| 1998 | Contrato Social de Debian + DFSG |
| 1999 | Nace `apt`, revolucionando la instalación de software |
| 2005 | Ubuntu se lanza basándose en Debian |
| 2015 | Debian 8 *Jessie* — migración a `systemd` |
| 2025 | Debian 13 *Trixie* — la versión que usamos en clase |

---

## 1.8 (cont.) La Tradición de Toy Story

Todas las versiones llevan nombre de un personaje de *Toy Story* de Pixar.
**Bruce Perens**, líder temprano del proyecto, trabajaba en Pixar.

| Versión | Nombre | Personaje |
|---|---|---|
| 1.1 | *Buzz* | Buzz Lightyear |
| 3.0 | *Woody* | El vaquero |
| 8 | *Jessie* | La vaquera |
| 12 | *Bookworm* | El gusano de libros |
| **13** | ***Trixie*** | **La triceratops azul** ← Nuestra versión |
| *Sid* | *Sid* | El niño que rompe juguetes (**siempre** = Unstable) |

---

## 1.8 (cont.) Las Tres Ramas de Debian

```
  +---------v---------+
  | Unstable (Sid)    |  ← Paquetes nuevos entran aquí
  +---------+---------+
            |  mín. 10 días sin bugs críticos
  +---------v---------+
  | Testing (Trixie)  |  ← Candidata a la siguiente Stable
  +---------+---------+
            |  congelamiento + pruebas masivas
  +---------v---------+
  | Stable (Bookworm) |  ← Solo parches de seguridad
  +-------------------+
```

> **Sid** siempre es la rama *Unstable* (el niño que rompe juguetes). Su nombre nunca cambia.

---

## 1.8 (cont.) El Contrato Social de Debian

Debian no pertenece a ninguna empresa. Es gobernada democráticamente por +1,000 Desarrolladores Debian (DD) en todo el mundo.

### Principios fundamentales:
1. Debian siempre será **100% software libre**.
2. Contribuiremos de vuelta a la comunidad de software libre.
3. **No ocultaremos problemas.**
4. Nuestras prioridades son nuestros **usuarios y el software libre**.

Las **DFSG** (Debian Free Software Guidelines) fueron la base directa de la **Open Source Definition** de la OSI.

---

## 1.8 (cont.) El Árbol de Derivados

Debian es la distribución que más derivados ha generado:

- **Ubuntu** (2004) — Financiada por Canonical, basada en Debian Sid.
- **Linux Mint** (2006) — Enfoque en experiencia de escritorio.
- **Raspberry Pi OS** (2012) — Debian para ARM.
- **Kali Linux** (2013) — Ciberseguridad y pentesting.
- **SteamOS** (2022) — Consola Steam Deck de Valve.

> **Cifra:** Más del **60%** de las distribuciones Linux activas son derivados directos o indirectos de Debian.

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 1?

