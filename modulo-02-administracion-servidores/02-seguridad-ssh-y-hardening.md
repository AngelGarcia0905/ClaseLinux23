# Subtema 2.2: Seguridad en Acceso SSH y Hardening de Servidores

---

## 1. El Protocolo SSH (Secure Shell)

SSH es el protocolo estándar para la administración remota cifrada de servidores Linux a través del puerto TCP 22.

---

## 2. Llaves de Cifrado (Ed25519 vs. RSA)

Se recomienda utilizar llaves de curva elíptica **Ed25519** debido a su mayor nivel de seguridad y rendimiento frente a llaves RSA tradicionales.

### Generación e Instalación de Llaves:
```bash
# 1. Generar la pareja de llaves (Privada y Pública)
ssh-keygen -t ed25519 -C "admin@biomedica.org"

# 2. Copiar la llave pública al servidor remoto
ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@192.168.1.78
```

---

## 3. Hardening de SSH (`/etc/ssh/sshd_config`)

Para proteger un servidor expuesto a la red, se deben aplicar las siguientes directivas de seguridad en `/etc/ssh/sshd_config`:

```text
# Deshabilitar autenticación por contraseña (obliga a usar llaves SSH)
PasswordAuthentication no

# Deshabilitar inicio de sesión directo del usuario root
PermitRootLogin no

# Restringir intentos de autenticación antes de cerrar la conexión
MaxAuthTries 3
```

Reiniciar el servicio para aplicar cambios:
```bash
sudo systemctl restart sshd
```

