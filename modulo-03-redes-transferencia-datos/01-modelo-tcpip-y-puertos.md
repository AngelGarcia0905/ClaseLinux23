# Subtema 3.1: Modelo TCP/IP, Interfaces y Diagnóstico de Puertos

---

## 1. El Modelo TCP/IP en Linux

Linux implementa la pila de red TCP/IP directamente en el kernel, gestionando las interfaces físicas (`eth0`, `wlan0`) y virtuales (`wsl0`, `docker0`).

### Resolución de Nombres (DNS):
1. **`/etc/hosts`:** Mapeo estático IP-Dominio prioritario.
2. **`/etc/resolv.conf`:** Servidores DNS del sistema (`nameserver 1.1.1.1`).

---

## 2. Diagnóstico de Puertos y Sockets (`ss`, `lsof`, `nc`)

```bash
# Listar todos los puertos TCP/UDP en escucha mostrando el proceso
ss -tulpn

# Verificar qué proceso utiliza un puerto específico (ej. 8080)
lsof -i :8080

# Probar conectividad TCP hacia un puerto remoto
nc -zv 192.168.1.78 22

# Consultar registros DNS detallados
dig google.com
```

