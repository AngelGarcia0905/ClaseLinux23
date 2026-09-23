# Módulo 3: Redes y Transferencia de Datos en Linux

---

## 1. El Modelo TCP/IP y Diagnóstico de Red en Linux

Linux es el núcleo de la infraestructura global de internet gracias a su robusta pila de red TCP/IP.

### Direccionamiento IP y Enrutamiento Local:
* **Interfaces de Red:** Representadas como `eth0`, `wlan0` o `eth0` en entornos virtuales.
* **Comando `ip`:** Herramienta moderna para administrar interfaces y tablas de ruteo (reemplaza a `ifconfig`).
  * `ip addr show`: Muestra direcciones IP asociadas a las interfaces.
  * `ip route show`: Muestra la tabla de enrutamiento y la puerta de enlace predeterminada (*default gateway*).

### Resolución de Nombres (DNS):
1. **Archivo `/etc/hosts`:** Tabla local estática que mapea direcciones IP con nombres de dominio antes de consultar servidores DNS externos.
2. **Archivo `/etc/resolv.conf`:** Define los servidores DNS (ej. `nameserver 1.1.1.1` o `8.8.8.8`) que utilizará el sistema.
3. **Herramientas de diagnóstico DNS:**
   * `dig domain.com`: Consulta detalles de registros DNS (A, AAAA, MX).
   * `nslookup domain.com`: Realiza consultas de nombres de dominio sencillas.

### Monitoreo de Puertos y Conexiones Activas:
* **`ss` (Socket Statistics):** Reemplazo moderno de `netstat`.
  * `ss -tulpn`: Lista todos los puertos en escucha (`l`), sockets TCP (`t`), UDP (`u`), mostrando el nombre del proceso (`p`) y números de puerto (`n`).
* **`lsof -i :8080`:** Muestra qué proceso específico está utilizando el puerto 8080.
* **`nc -zv <ip> <puerto>` (Netcat):** Prueba si un puerto TCP remoto está abierto y respondiendo.
* **`curl -I https://api.com`:** Envía una petición HTTP HEAD para verificar conectividad y cabeceras de respuesta de servidores web/APIs.

---

## 2. Firewalls Internos: UFW, `iptables` y `nftables`

Linux filtra y gestiona paquetes de red directamente a nivel del kernel usando el subsistema **Netfilter**.

```
                +------------------------------+
                |    Herramienta CLI (UFW)     |  <-- Nivel Usuario
                +--------------+---------------+
                               |
                +--------------v---------------+
                |     nftables / iptables      |  <-- Capa Intermedia
                +--------------+---------------+
                               |
                +--------------v---------------+
                |   Subsistema NETFILTER       |  <-- Nivel Kernel
                +------------------------------+
```

### `iptables` / `nftables`:
* **`iptables`:** El motor clásico de reglas organizado en tablas (filter, nat, mangle) y cadenas (INPUT, OUTPUT, FORWARD).
* **`nftables`:** El sucesor moderno de `iptables` con una sintaxis más limpia y mayor rendimiento en el procesamiento de paquetes.

### UFW (Uncomplicated Firewall):
UFW es un *frontend* simplificado para administrar reglas de `iptables`/`nftables` sin necesidad de escribir sintaxis de filtrado compleja.

* **Comandos esenciales de UFW:**
  * `sudo ufw status verbose`: Muestra el estado del firewall y las reglas activas.
  * `sudo ufw default deny incoming`: Deniega todo el tráfico entrante por defecto (Práctica recomendada).
  * `sudo ufw default allow outgoing`: Permite todo el tráfico saliente por defecto.
  * `sudo ufw allow 22/tcp`: Permite tráfico SSH entrante.
  * `sudo ufw allow 80,443/tcp`: Permite tráfico HTTP y HTTPS.
  * `sudo ufw allow from 192.168.1.50 to any port 5432`: Restringe el acceso a la base de datos PostgreSQL solo a una IP autorizada.
  * `sudo ufw enable`: Activa el firewall.

---

## 3. Transferencia Segura y Sincronización de Archivos (`scp` vs. `rsync`)

En entornos de IA y procesamiento biomédico, la transferencia eficiente de grandes conjuntos de datos (*datasets*) entre nodos locales y servidores es fundamental.

### `scp` (Secure Copy Protocol):
Copia archivos entre hosts a través de una conexión cifrada SSH. Es ideal para copias simples y únicas.

* **Ejemplo:**
  ```bash
  scp -i ~/.ssh/id_ed25519 dataset_ecg.csv usuario@192.168.1.100:/var/data/
  ```

### `rsync` (Remote Sync):
Es la herramienta estándar para sincronización de archivos. Utiliza un algoritmo de transferencia delta que transmite **únicamente las diferencias** entre archivos, ahorrando ancho de banda y tiempo.

* **Ventajas de `rsync`:**
  * Permite reanudar transferencias interrumpidas.
  * Preserva permisos, propietarios, fechas de modificación y enlaces simbólicos.
  * Comprime datos al vuelo durante la transmisión.

* **Sintaxis y flags recomendados:**
  ```bash
  rsync -avzP -e "ssh -i ~/.ssh/id_ed25519" ./datos_medicos/ usuario@servidor:/var/data/medica/
  ```
  * `-a` (archive): Mantiene permisos, marcas de tiempo, enlaces y propietarios de forma recursiva.
  * `-v` (verbose): Muestra detalles de la ejecución.
  * `-z` (compress): Comprime los datos durante el envío.
  * `-P` (progress / partial): Muestra barra de progreso y permite reanudar descargas cortadas.

