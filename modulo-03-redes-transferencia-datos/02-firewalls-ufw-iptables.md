# Subtema 3.2: Firewalls Internos — Netfilter, UFW y Comportamiento en WSL2

---

## 1. El Subsistema Netfilter (`iptables` / `nftables`)

En Linux, el filtrado de paquetes de red se realiza en el kernel mediante **Netfilter**. `iptables` y `nftables` son las herramientas CLI para definir tablas y cadenas de reglas (`INPUT`, `OUTPUT`, `FORWARD`).

---

## 2. UFW (Uncomplicated Firewall)

UFW es un *frontend* simplificado que facilita la configuración del firewall:

```bash
# Habilitar firewall y establecer políticas por defecto
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80,443/tcp
sudo ufw enable
```

---

## 3. Matiz de Firewall en WSL 2 (Modo *Mirrored*)

En WSL 2 con modo de red espejo, la interfaz física es administrada prioritariamente por el **Firewall de Windows Host**. Por ello, en el curso demostraremos las reglas de UFW y filtrado `iptables` **dentro de contenedores Docker**, donde cada contenedor mantiene su propio espacio de nombres de red (*network namespace*) independiente.

