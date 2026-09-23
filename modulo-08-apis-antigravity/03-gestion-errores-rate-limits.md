# Subtema 8.3: Resiliencia en APIs, Códigos HTTP y Rate Limits

---

## 1. Códigos de Estado HTTP Frecuentes

* `200 OK`: Éxito.
* `401 / 403`: Autenticación o permisos inválidos.
* `429 Too Many Requests`: Exceso de peticiones (*Rate Limit* superado).
* `500 / 503`: Error interno en el servidor remoto.

---

## 2. Reintentos con Exponential Backoff

Reintentar peticiones fallidas incrementando exponencialmente el tiempo de espera entre intentos:

```bash
curl --retry 5 --retry-delay 2 --retry-max-time 30 -H "Authorization: Bearer $API_KEY" https://api.antigravity.ai/v1/query
```
