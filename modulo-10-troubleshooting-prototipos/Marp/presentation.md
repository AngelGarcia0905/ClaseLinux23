---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 10"
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

# Módulo 10: Troubleshooting de Prototipos Finales
### Depuración Electrónica, Monitoreo y Logs Multi-capa

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

- **10.1** Depuración de Hardware Electrónico y Señales Fisiológicas
- **10.2** Tierra Común (GND), Filtrado y Debounce
- **10.3** Monitoreo del Sistema y Procesos (`top` / `htop` / `ps`)
- **10.4** Terminación Forzada de Procesos Bloqueados (`kill -9`)
- **10.5** Metodología de Análisis de Logs en 4 Capas
- **10.6** Integración Final: Hardware ➔ Kernel ➔ Docker ➔ API Cloud

---

## 10.1 Depuración Electrónica de Hardware

- **Ruido en Sensores Analógicos (ECG):**
  - Requiere compartir la tierra (*common ground*) entre el microcontrolador y los sensores.
  - Agregar capacitores de desacoplamiento (100nF) y filtros por software.
- **Rebotes de Señal (*Debounce*):**
  - Evita múltiples lecturas falsas al presionar botones o conmutadores.
- **Caídas de Voltaje:**
  - Separar líneas de alimentación de componentes de alto consumo (Wi-Fi, relés).

---

## 10.2 Monitoreo y Control de Procesos

- **Navegación en `htop`:** Ordenar por uso de CPU (`P`) y RAM (`M`).
- **Búsqueda con `ps`:** `ps aux | grep python`
- **Terminación de Procesos:**
  - `kill <PID>`: Señal `SIGTERM` (15) para cierre limpio.
  - `kill -9 <PID>`: Señal `SIGKILL` (9) para forzar terminación del proceso por el kernel.
  - `killall -9 python3`: Cierre masivo de scripts desbocados.

---

## 10.3 Análisis Metódico de Logs en 4 Capas

```
[ 1. Hardware Físico ] ➔ [ 2. Puerto Serie / Kernel ] ➔ [ 3. Servicio / Contenedores ] ➔ [ 4. API Cloud ]
 Multímetro / GND        dmesg -wH / /dev/ttyUSB0        docker logs -f / journalctl       jq / HTTP Status
```

1. **Hardware:** Verificar voltajes y continuidad.
2. **Kernel:** Confirmar asignación del puerto USB (`dmesg`).
3. **Contenedor:** Inspeccionar registros de systemd y Docker.
4. **API Cloud:** Verificar códigos de respuesta HTTP y cuotas (`429`).

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 10?
