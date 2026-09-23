# Subtema 5.1: Arquitectura de Docker — Contenedores vs. Máquinas Virtuales

---

## 1. El Demonio de Docker y Virtualización por Contenedores

**Docker** aísla aplicaciones compartiendo el kernel de la máquina anfitriona (*Host Kernel*), utilizando características nativas de Linux:
* **`namespaces`:** Aísla procesos (PID), red (NET), montajes (MNT) y usuarios (USER).
* **`cgroups` (Control Groups):** Limita y asigna cuotas de CPU, RAM y E/S de disco.

---

## 2. Contenedores vs. Máquinas Virtuales

* **Máquinas Virtuales:** Virtualizan hardware completo e incluyen un sistema operativo huésped completo (*Guest OS*).
* **Contenedores:** Paquetes ligeros sin SO propio; arrancan en milisegundos y consumen una fracción de RAM.

