---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 9"
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

# Módulo 9: Orquestación y Tareas Programadas
### Cron, Variables `.env` y Redirección de Logs

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

- **9.1** El Demonio de Programación `cron`
- **9.2** Sintaxis de 5 Campos en `crontab`
- **9.3** Gestión de Variables de Entorno (`.bashrc` y `.env`)
- **9.4** Protecciones y Permisos `chmod 600`
- **9.5** Redirección de Errores y Separación `stdout` / `stderr`
- **9.6** Matiz de Ejecución de Cron en WSL 2

---

## 9.1 Automatización con `crontab`

```text
*  *  *  *  *  comando
|  |  |  |  +---- Día de la semana (0 - 6)
|  |  |  +------- Mes (1 - 12)
|  |  +---------- Día del mes (1 - 31)
|  +------------- Hora (0 - 23)
+---------------- Minuto (0 - 59)
```

- `crontab -e`: Editar tareas activas.
- `crontab -l`: Listar tareas.
- **Ejemplo (Cada 15 min):** `*/15 * * * * /usr/bin/python3 /app/respaldo.py`

---

## 9.2 Variables `.env` y Secretos

- **Variables en Sesión:** `export API_KEY="sk_live_12345"`
- **Protección de Archivos `.env`:**
  - Evitar guardar claves en código fuente (*hardcoding*).
  - Restringir lectura solo al usuario propietario:
    ```bash
    chmod 600 .env
    ```

---

## 9.3 Redirección de Logs en Cron

Cron ejecuta tareas en segundo plano sin terminal interactiva. Se debe capturar la salida normal (1) y la salida de error (2):

```bash
# Guardar salida normal y errores en el mismo archivo
0 2 * * * /usr/bin/python3 /app/script.py >> /var/log/app.log 2>&1
```

> **Matiz WSL 2:** Si se cierra la terminal y WSL se suspende, las tareas de `cron` no se ejecutarán hasta reactivar la instancia.

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 9?
