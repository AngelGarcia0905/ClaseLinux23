---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 3"
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

# Módulo 3: Redes y Transferencia de Datos
### TCP/IP, Diagnóstico de Puertos, UFW y rsync

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

- **3.1** Pila TCP/IP y Configuración de Interfaces (`ip`)
- **3.2** Resolución de Nombres DNS (`hosts` / `resolv.conf`)
- **3.3** Diagnóstico de Puertos y Sockets (`ss`, `lsof`, `nc`)
- **3.4** Firewalls Internos: Netfilter y UFW
- **3.5** Estrategia de Firewall en WSL 2 y Docker
- **3.6** Transferencia Segura con `scp` y Sincronización Delta con `rsync`

---

## 3.1 Pila TCP/IP y Comandos de Red

- **Interfaces:** `eth0`, `wlan0`, `wsl0`, `docker0`.
- **Comando `ip`:** Reemplazo moderno de `ifconfig`:
  - `ip addr show`: Direcciones IP asignadas.
  - `ip route show`: Tabla de enrutamiento y puerta de enlace (*gateway*).

---

## 3.2 Resolución DNS y Diagnóstico de Puertos

- **DNS en Linux:**
  - `/etc/hosts`: Mapeo estático IP-Dominio prioritario.
  - `/etc/resolv.conf`: Servidores DNS del sistema (`nameserver 1.1.1.1`).
- **Herramientas CLI:**
  - `ss -tulpn`: Lista sockets TCP/UDP y procesos en escucha.
  - `lsof -i :8080`: Identifica qué proceso ocupa el puerto 8080.
  - `nc -zv 192.168.1.78 22`: Prueba conectividad TCP remota.

---

## 3.3 Firewalls Internos (Netfilter & UFW)

- **Netfilter:** Subsistema del kernel configurado vía `iptables`/`nftables`.
- **UFW (Uncomplicated Firewall):**

```bash
# Configuración básica de políticas de UFW
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80,443/tcp
sudo ufw enable
```

---

## 3.4 Matiz de Firewall en WSL 2

- En WSL 2 con modo espejo (*mirrored*), el **Firewall de Windows Host** gobierna la interfaz física.
- **Estrategia de Clase:** Impartir demostraciones de UFW y reglas `iptables` **dentro de contenedores Docker**, aprovechando sus espacios de nombres de red (*network namespaces*) totalmente aislados.

---

## 3.5 Transferencia Cifrada (`scp` vs `rsync`)

- **`scp`:** Copia simple de archivos únicos sobre SSH:
  ```bash
  scp -i ~/.ssh/id_ed25519 datos.csv usuario@192.168.1.78:/var/data/
  ```
- **`rsync`:** Sincronización masiva optimizada con **algoritmo delta** (transmite solo bloques modificados):
  ```bash
  rsync -avzP -e "ssh -i ~/.ssh/id_ed25519" ./dataset/ usuario@servidor:/var/data/
  ```
  - `-a` (archivar), `-v` (detallado), `-z` (comprimir), `-P` (progreso).

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 3?
