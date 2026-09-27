---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 8"
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

# Módulo 8: Interacción con APIs y Antigravity
### Prompts Técnicos, Parseo JSON con jq y Resiliencia

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

- **8.1** Ingeniería de Prompts para Agentes de Código IA
- **8.2** Asignación de Roles, Contextos y Restricciones
- **8.3** Parseo de Respuestas JSON en Terminal con `jq`
- **8.4** Conversión de JSON a Estructuras CSV
- **8.5** Códigos de Estado HTTP y Manejo de Rate Limits (`429`)
- **8.6** Estrategia de Reintentos con *Exponential Backoff*

---

## 8.1 Prompts Técnicos para Agentes de IA

Estructura para guiar la generación de código funcional en **Google Antigravity**:

1. **Rol:** *"Actúa como un desarrollador experto en Linux embebido"*.
2. **Requisitos:** Especificar entradas, tipos de datos y funciones exactas.
3. **Restricciones:** *"No utilices bucles bloqueantes en el hilo principal"*.
4. **Resiliencia:** Definir manejo de excepciones ante fallos de red.

---

## 8.2 Procesamiento de JSON con `jq`

Extracción de campos estructurados sin scripts de Python:

```bash
# Pretty-print de JSON
curl -s https://api.antigravity.ai/v1/health | jq '.'

# Extraer un campo específico
curl -s https://api.antigravity.ai/v1/models | jq '.models[0].name'

# Convertir arreglo JSON a CSV
cat datos.json | jq -r '.data[] | [.timestamp, .bpm] | @csv'
```

---

## 8.3 Resiliencia en APIs HTTP

- **Códigos HTTP Clave:** `200 OK`, `401` (No autorizado), `429` (Rate limit superado), `503` (Servidor no disponible).
- **Exponential Backoff:** Reintentar incrementando exponencialmente el tiempo de espera ante un error `429` o `503`:

```bash
curl --retry 5 --retry-delay 2 --retry-max-time 30 -H "Authorization: Bearer $API_KEY" https://api.antigravity.ai/v1/query
```

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 8?
