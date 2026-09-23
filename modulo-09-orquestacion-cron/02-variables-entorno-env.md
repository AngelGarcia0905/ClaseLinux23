# Subtema 9.2: Variables de Entorno, Archivos `.env` y Protección de Credenciales

---

## 1. Variables de Entorno (`.bashrc` y `.env`)

* **Exportar Variables:** `export API_KEY="sk_live_12345"`
* **Archivos `.env`:** Separación de secretos de aplicaciones de producción.

---

## 2. Hardening de Archivos de Secretos

Para evitar fugas de seguridad o lecturas no autorizadas:
```bash
chmod 600 .env
```
*(Asigna permisos exclusivos de lectura/escritura únicamente al propietario).*

