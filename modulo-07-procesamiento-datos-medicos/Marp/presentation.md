---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 7"
footer: "Jorge Angel Garcia Alvarado  |  2068683  |  IMC"
style: |
  section {
    font-family: "Segoe UI", "Helvetica Neue", Arial, sans-serif;
    font-size: 28px;
    color: #1f2933;
    background: #ffffff;
    padding: 70px 90px 60px 90px;
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
  section.cierre h3 { font-size: 28px; font-weight: 400; color: #52606d; margin: 0; }
---

<!-- _class: portada -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

![h:105](assets/logo_uanl.jpg)![h:105](assets/FIME_LOGO.png)

# Módulo 7: Procesamiento de Flujos de Datos Médicos
### Expresiones Regulares, awk, sed y Tuberías

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

- **7.1** Expresiones Regulares (Regex) en Logs de Salud
- **7.2** Edición de Flujos de Texto con `sed`
- **7.3** Procesamiento Orientado a Filas y Columnas con `awk`
- **7.4** Redirección de Descriptores (`stdout` / `stderr`)
- **7.5** Construcción de Tuberías (*Pipes*) en Tiempo Real

---

## 7.1 Expresiones Regulares (Regex)

Patrones de búsqueda para validar e inspeccionar registros médicos (archivos HL7, CSV):

- Metacaracteres: `^` (inicio), `$` (fin), `[0-9]+` (dígitos), `( ... )` (captura).
- **Ejemplo:** `BPM:\s*([0-9]{2,3})` extrae lecturas numéricas de ritmo cardíaco.

---

## 7.2 Manipulación con `sed` y `awk`

- **`sed` (Stream Editor):**
  - Sustitución global: `sed 's/NORMAL/ACEPTABLE/g' datos.log`
  - Eliminar líneas vacías: `sed '/^$/d' registros.csv`
- **`awk` (Procesador por Columnas):**
  - Extracción de fecha ($1) y SpO2 ($4) de un CSV:
    ```bash
    awk -F',' '{print $1, $4}' oximetria.csv
    ```
  - Filtrado por condición (BPM > 100):
    ```bash
    awk -F',' '$3 > 100 {print "ALERTA TATICARDIA:", $0}' telemetria.csv
    ```

---

## 7.3 Tuberías (`|`) y Redirecciones

Conexión directa de programas procesando streams en memoria RAM sin crear archivos temporales pesados:

```
[ Sensor / Cat ] -- stdout --> | PIPE | -- stdin --> [ awk Filtro ] -- stdout --> [ Archivo Log ]
```

```bash
cat telemetria.log | grep "ALERTA" | awk '{print $1, $4}' | sort | uniq -c > informe.txt
```
- Operadores: `>` (sobrescribir), `>>` (anexar), `2>&1` (combinar errores).

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 7?
