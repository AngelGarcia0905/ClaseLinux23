---
marp: true
lang: es
theme: default
size: 16:9
paginate: true
header: "Linux, IA y Prototipos Biomédicos — Módulo 2"
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

# Módulo 2: Administración Profunda de Servidores
### Permisos Octales, Seguridad SSH y Sudoers

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

- **2.1** Sistema de Permisos de Archivos y Directorios (`rwx`)
- **2.2** Notación Octal y Simbólica (`chmod`)
- **2.3** Cambio de Propietarios y Grupos (`chown`, `chgrp`)
- **2.4** Seguridad SSH con Cifrado Ed25519
- **2.5** Hardening de Servidor SSH (`sshd_config`)
- **2.6** Gestión de Usuarios y Sudoers con `visudo`

---

## 2.1 Modelo de Permisos en Linux

Asignación estricta de permisos para tres categorías:
- **`u` (User):** Propietario del archivo.
- **`g` (Group):** Grupo asignado.
- **`o` (Others):** Demás usuarios del sistema.

### Atributos:
- **`r` (Read / 4):** Leer archivo o listar directorio.
- **`w` (Write / 2):** Modificar archivo o crear/borrar en directorio.
- **`x` (Execute / 1):** Ejecutar binario/script o entrar (`cd`) al directorio.

---

## 2.2 Notación Octal (`chmod`)

Suma de valores binarios por categoría:

| Comando | Permisos | Uso Frecuente |
|---|---|---|
| `chmod 755` | `rwxr-xr-x` | Scripts ejecutables y directorios públicos |
| `chmod 644` | `rw-r--r--` | Archivos de código o datasets estándar |
| `chmod 600` | `rw-------` | Llaves SSH privadas (`id_ed25519`) |

- **Simbólico:** `chmod u+x script.sh` / `chmod g-w datos.txt`

---

## 2.3 Propietarios y Grupos (`chown`)

Modificación de la pertenencia de archivos y directorios:

```bash
# Cambiar propietario y grupo simultáneamente
sudo chown usuario:grupo datos_salud.csv

# Aplicar cambio de forma recursiva a todo un directorio
sudo chown -R usuario:grupo /var/data/biomedica/
```

---

## 2.4 Seguridad SSH y Llaves Ed25519

Reemplazo de autenticación por contraseña por criptografía asimétrica de curva elíptica **Ed25519**:

```bash
# 1. Generar pareja de llaves SSH (Privada + Pública)
ssh-keygen -t ed25519 -C "admin@biomedica.org"

# 2. Copiar la llave pública al servidor remoto
ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@192.168.1.78
```

> **¡Regla de Oro!** `chmod 600 ~/.ssh/id_ed25519` (La llave privada nunca se comparte).

---

## 2.5 Hardening de SSH (`/etc/ssh/sshd_config`)

Aseguramiento del demonio SSH contra ataques de fuerza bruta:

```text
# Deshabilitar autenticación por contraseña
PasswordAuthentication no

# Deshabilitar inicio de sesión directo del usuario root
PermitRootLogin no

# Limitar intentos fallidos de autenticación
MaxAuthTries 3
```
- Aplicar cambios: `sudo systemctl restart sshd`

---

## 2.6 Sudoers y `visudo`

- **Gestión de Usuarios:**
  - `sudo useradd -m -s /bin/bash operador`
  - `sudo usermod -aG dialout,docker operador`
- **Configuración de `/etc/sudoers`:**
  - Usar siempre `sudo visudo` (valida sintaxis antes de guardar).

```text
# Permitir a un usuario ejecutar todo con sudo
operador ALL=(ALL:ALL) ALL
```

---

<!-- _class: cierre -->
<!-- _paginate: false -->
<!-- _header: "" -->
<!-- _footer: "" -->

# Gracias
### ¿Preguntas sobre el Módulo 2?
