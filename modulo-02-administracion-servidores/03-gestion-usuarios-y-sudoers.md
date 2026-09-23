# Subtema 2.3: Gestión de Usuarios, Grupos y Configuración de Sudoers

---

## 1. Administración de Usuarios y Grupos

Linux separa las identidades de usuario y sus permisos grupales para garantizar el control de acceso en entornos multiusuario.

### Comandos Principales:
```bash
# Crear un grupo de trabajo
sudo groupadd biomedica

# Crear usuario con directorio home y shell bash
sudo useradd -m -s /bin/bash operador

# Asignar usuario a grupos adicionales (dialout para serie, docker)
sudo usermod -aG biomedica,dialout,docker operador

# Eliminar un usuario y su carpeta personal
sudo userdel -r operador
```

---

## 2. Privilegios Elevados (`/etc/sudoers`)

El comando `sudo` permite a usuarios autorizados ejecutar comandos con privilegios de superusuario (`root`).

### Edición Segura con `visudo`:
Nunca se debe editar `/etc/sudoers` con editores comunes. Se debe utilizar `sudo visudo`, el cual verifica la sintaxis antes de aplicar cambios para evitar bloquear el sistema.

### Reglas de Sudoers Frecuentes:
```text
# Permitir a un usuario ejecutar todos los comandos con sudo
operador ALL=(ALL:ALL) ALL

# Permitir a un grupo reiniciar un servicio específico sin pedir contraseña
%biomedica ALL=(ALL) NOPASSWD: /bin/systemctl restart servicio-medico
```
