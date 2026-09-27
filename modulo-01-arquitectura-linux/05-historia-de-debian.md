# Subtema 1.5: Historia de Debian y su Filosofía

---

## 1. Origen de Debian (1993)

**Debian** fue fundado el **16 de agosto de 1993** por **Ian Murdock**, un estudiante de la Universidad de Purdue (Indiana, EE.UU.), cuando tenía apenas 20 años. El nombre "Debian" es una contracción de los nombres **Deb**ra (su novia y posterior esposa) e **Ian** (el propio Murdock).

Murdock publicó el **Manifiesto Debian**, un documento fundacional donde estableció los principios que distinguirían a este proyecto de cualquier otra distribución comercial de la época:

> *"Debian será desarrollada abiertamente, en el espíritu de Linux y de GNU. Será distribuida libremente a través de Internet."*
> — Ian Murdock, Manifiesto Debian, 1993.

---

## 2. Línea de Tiempo de Debian

| Año | Evento Clave |
|---|---|
| **1993** | Ian Murdock anuncia el proyecto Debian y publica el *Manifiesto Debian*. |
| **1994** | Debian 0.91 — primera versión pública, con el gestor de paquetes `dpkg`. |
| **1996** | Debian 1.1 (*Buzz*) — primera versión con nombres de personajes de *Toy Story*. |
| **1998** | Se crea la **Debian Social Contract** y las **Debian Free Software Guidelines (DFSG)**. |
| **1999** | Nace `apt` (Advanced Package Tool), revolucionando la instalación de software en Linux. |
| **2000** | Debian 2.2 (*Potato*) — soporte para 6 arquitecturas de hardware simultáneamente. |
| **2005** | Ubuntu, creada por Mark Shuttleworth, se lanza basándose directamente en Debian *Unstable/Sid*. |
| **2011** | Debian 6.0 (*Squeeze*) — incluye un kernel 100% libre por defecto. |
| **2015** | Debian 8 (*Jessie*) — migración oficial de SysVinit a **`systemd`** como sistema de inicialización. |
| **2017** | Fallecimiento de Ian Murdock a los 42 años. La comunidad rinde homenaje permanente. |
| **2023** | Debian celebra su **30.º aniversario** como el proyecto de software libre colaborativo más longevo del mundo. |
| **2025** | Debian 13 (*Trixie*) — la versión estable actual utilizada en este curso. |

---

## 3. La Tradición de los Nombres: Toy Story

Todas las versiones de Debian llevan el nombre de un personaje de la película *Toy Story* de Pixar. Esta tradición se estableció porque **Bruce Perens**, uno de los primeros líderes del proyecto Debian, trabajaba en Pixar cuando era mantenedor principal.

| Versión | Nombre | Personaje |
|---|---|---|
| 1.1 | *Buzz* | Buzz Lightyear |
| 2.0 | *Hamm* | El cerdito alcancía |
| 3.0 | *Woody* | Woody, el vaquero |
| 8 | *Jessie* | La vaquera |
| 11 | *Bullseye* | El caballo de Woody |
| 12 | *Bookworm* | El gusano de libros |
| **13** | ***Trixie*** | **La triceratops azul** ← Nuestra versión |
| *Sid* | *Sid* | El niño que rompe juguetes (siempre es la rama *Unstable*) |

> **Dato importante:** La rama *Unstable* siempre se llama **Sid** (el niño destructivo de Toy Story que rompe juguetes), porque los paquetes en *Unstable* pueden romperse en cualquier momento. Es un nombre permanente que nunca cambia.

---

## 4. Modelo de Gobernanza y el Contrato Social

Debian no pertenece a ninguna empresa ni corporación. Es gobernada democráticamente por sus más de **1,000 Desarrolladores Debian (DD)** distribuidos en todo el mundo.

### Documentos Fundacionales:
* **Contrato Social de Debian (*Debian Social Contract*):**
  1. Debian siempre será 100% software libre.
  2. Contribuiremos de vuelta a la comunidad de software libre.
  3. No ocultaremos problemas.
  4. Nuestras prioridades son nuestros usuarios y el software libre.

* **Guías de Software Libre de Debian (*DFSG — Debian Free Software Guidelines*):**
  Criterios estrictos que definen si una licencia es compatible con la libertad del software. Las DFSG fueron la base directa sobre la cual se escribió la **Open Source Definition** de la *Open Source Initiative (OSI)*.

---

## 5. El Sistema de Tres Ramas

Debian organiza el flujo de sus paquetes en un sistema de tres ramas que garantiza que solo el software exhaustivamente probado llegue a producción:

```
   Desarrollador sube paquete
              |
              v
   +-------------------+
   | Unstable (Sid)    |   <-- Paquetes nuevos entran aquí primero
   +--------+----------+       (Puede romperse. Se llama "Sid" por algo.)
            |
            | Pasa pruebas automatizadas (mínimo 10 días sin bugs críticos)
            v
   +-------------------+
   | Testing (Trixie)  |   <-- Candidata a ser la siguiente Stable
   +--------+----------+       (Relativamente estable, pero sin garantías)
            |
            | Congelamiento + pruebas finales masivas
            v
   +-------------------+
   | Stable (Bookworm) |   <-- Solo recibe parches de seguridad
   +-------------------+       (Máxima confiabilidad para servidores)
```

---

## 6. Legado de Debian: El Árbol de Derivados

Debian es la distribución que más derivados ha generado en la historia de Linux. Su estabilidad, filosofía libre y sistema de paquetes `.deb` / `apt` han servido como base para cientos de proyectos:

* **Ubuntu** (2004) → Derivado de Debian *Sid/Testing*, financiado por Canonical.
* **Linux Mint** (2006) → Derivado de Ubuntu, enfocado en experiencia de escritorio.
* **Raspberry Pi OS / Raspbian** (2012) → Adaptación de Debian para la arquitectura ARM de la Raspberry Pi.
* **Kali Linux** (2013) → Basada en Debian *Testing*, enfocada en ciberseguridad y pentesting.
* **SteamOS** (2013/2022) → Sistema operativo de Valve para la consola Steam Deck, basado en Debian/Arch.

> **Cifra clave:** Según DistroWatch, más del **60%** de todas las distribuciones Linux activas son derivados directos o indirectos de Debian.
