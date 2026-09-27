---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 5"
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

# Módulo 5: Arquitectura de Contenedores
### Docker Daemon, Redes, Volúmenes y Dockerfile

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

- **5.1** Demonio de Docker y Virtualización por Contenedores
- **5.2** Mecanismos del Kernel: `namespaces` y `cgroups`
- **5.3** Contenedores vs. Máquinas Virtuales Tradicionales
- **5.4** Redes en Docker: Modos `bridge`, `host` y `none`
- **5.5** Persistencia de Datos: Volúmenes Administrados vs. Bind Mounts
- **5.6** Construcción de Imágenes Optimizadas con `Dockerfile`

---

## 5.1 Contenedores vs. Máquinas Virtuales

- **Máquinas Virtuales:** Virtualizan hardware completo e incluyen un SO huésped completo (*Guest OS*). Consumo alto de RAM y disco.
- **Contenedores Docker:** Aíslan aplicaciones compartiendo el núcleo del sistema anfitrión (*Host Kernel*).

```
[ App A | Libs ]  [ App B | Libs ]
----------------------------------
        Motor de Docker
----------------------------------
    KERNEL LINUX DEL ANFITRIÓN
```

- **Mecanismos clave:** `namespaces` (aislamiento de PID/Red) y `cgroups` (límites de CPU/RAM).

---

## 5.2 Redes y Volúmenes Persistentes

- **Redes:**
  - **`bridge`:** Red privada interna con mapeo explícito de puertos (`-p 8080:80`).
  - **`host`:** Comparte la pila de red del anfitrión.
- **Persistencia de Datos:**
  - **Volúmenes Administrados (`docker volume create datos_db`):** Guardados en `/var/lib/docker/volumes/`. Ideales para bases de datos de salud.
  - **Bind Mounts (`-v $(pwd)/src:/app/src`):** Asocian carpetas locales para desarrollo activo.

---

## 5.3 Construcción de `Dockerfile`

Optimización de capas de caché para builds rápidos:

```dockerfile
# Imagen base ligera basada en Debian
FROM python:3.11-slim
WORKDIR /app

# Instalar requerimientos primero (aprovecha la caché)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código fuente
COPY . .
EXPOSE 8000
CMD ["python", "main.py"]
```

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 5?
