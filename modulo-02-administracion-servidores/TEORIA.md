# Módulo 2: Administración Profunda de Servidores Linux

---

## 1. Sistema de Permisos de Archivos y Directorios

En Linux, la seguridad del sistema de archivos se basa en la asignación estricta de permisos para tres tipos de usuarios:
* **`u` (User / Propietario):** El usuario dueño del archivo o directorio.
* **`g` (Group / Grupo):** El grupo de usuarios asignado al archivo.
* **`o` (Others / Otros):** Cualquier otro usuario del sistema.

### Tipos de Permisos Básicos:
* **Lectura (`r` - Read):** Valor numérico `4`. Permite leer el contenido de un archivo o listar un directorio.
* **Escritura (`w` - Write):** Valor numérico `2`. Permite modificar un archivo o crear/eliminar archivos en un directorio.
* **Ejecución (`x` - Execute):** Valor numérico `1`. Permite ejecutar un archivo como programa/script o acceder (`cd`) a un directorio.

```
  r w x   r - x   r - -
 |     | |     | |     |
  User    Group   Others
  (7)      (5)     (4)
```

### Notación Octal y Simbólica (`chmod`):
* **Notación Octal (Numérica):** Suma los valores de permisos para cada categoría.
  * `chmod 755 archivo.sh`: Propietario (4+2+1=7: `rwx`), Grupo (4+0+1=5: `r-x`), Otros (4+0+1=5: `r-x`).
  * `chmod 644 datos.csv`: Propietario (4+2+0=6: `rw-`), Grupo (4+0+0=4: `r--`), Otros (4+0+0=4: `r--`).
  * `chmod 600 id_ed25519`: Propietario (6: `rw-`), Grupo (0: `---`), Otros (0: `---`). (Obligatorio para llaves SSH privadas).
* **Notación Simbólica:** Modifica permisos agregando (`+`) o quitando (`-`) atributos.
  * `chmod u+x script.sh`: Otorga permiso de ejecución al propietario.
  * `chmod g-w archivo.txt`: Quita permiso de escritura al grupo.

### Gestión de Propietarios y Grupos (`chown`, `chgrp`):
* `chown usuario:grupo archivo`: Cambia tanto el propietario como el grupo de un archivo.
* `chown -R usuario:grupo /directorio`: Aplica el cambio de propietario de forma recursiva a todo el contenido del directorio.

---

## 2. Seguridad de Acceso y Hardening SSH

El protocolo **SSH (Secure Shell)** es el estándar para la administración remota segura de servidores Linux mediante puerto TCP 22.

### Generación de Llaves de Cifrado (Ed25519 vs. RSA):
Se recomienda el algoritmo de curva elíptica **Ed25519** sobre el clásico RSA por ofrecer mayor velocidad y seguridad matemática con claves más cortas.

* **Comando de generación de llave:**
  ```bash
  ssh-keygen -t ed25519 -C "admin@servidor-biomedico.com"
  ```
* **Ubicación por defecto:**
  * Llave privada: `~/.ssh/id_ed25519` (¡Nunca debe compartirse!).
  * Llave pública: `~/.ssh/id_ed25519.pub` (Se copia al servidor en `~/.ssh/authorized_keys`).
* **Copiar llave al servidor:**
  ```bash
  ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@ip_del_servidor
  ```

### Aseguramiento de la Configuración SSH (`/etc/ssh/sshd_config`):
Para proteger un servidor de producción contra ataques de fuerza bruta, se deben modificar las siguientes directivas en `/etc/ssh/sshd_config`:

1. **Deshabilitar inicio de sesión por contraseña:**
   ```text
   PasswordAuthentication no
   ```
2. **Deshabilitar inicio de sesión directo del usuario root:**
   ```text
   PermitRootLogin no
   ```
3. **Restringir intentos de autenticación:**
   ```text
   MaxAuthTries 3
   ```
4. **Reiniciar el servicio para aplicar cambios:**
   ```bash
   sudo systemctl restart sshd
   ```

---

## 3. Gestión de Usuarios, Grupos y Sudoers

### Creación y Modificación de Usuarios y Grupos:
* **Crear grupo:** `sudo groupadd biomedica`
* **Crear usuario con carpeta personal y shell bash:**
  ```bash
  sudo useradd -m -s /bin/bash operador
  ```
* **Asignar usuario a grupos adicionales:**
  ```bash
  sudo usermod -aG biomedica,docker operador
  ```
* **Eliminar usuario y su carpeta personal:** `sudo userdel -r operador`

### Gestión de Privilegios Elevados (`/etc/sudoers`):
El archivo `/etc/sudoers` define qué usuarios o grupos pueden ejecutar comandos con privilegios de superusuario (`root`) usando `sudo`.

* **Edición segura con `visudo`:**
  Nunca se debe editar `/etc/sudoers` directamente con `nano` o `vim`. Se debe usar el comando `sudo visudo`, el cual valida la sintaxis antes de guardar para evitar bloquear el acceso al sistema.
* **Sintaxis de regla sudo:**
  ```text
  # Usuario  Hosts=(Usuarios_Objetivo) Comandos
  operador   ALL=(ALL:ALL) ALL
  
  # Permitir a un grupo reiniciar un servicio sin pedir contraseña:
  %biomedica ALL=(ALL) NOPASSWD: /bin/systemctl restart servicio-medico
  ```

