# Módulo 8: Interacción Avanzada con APIs y Antigravity

---

## 1. Ingeniería de Prompts para Generación de Código Técnico

La interacción eficiente con Agentes de Inteligencia Artificial (como Google Antigravity) requiere formular peticiones declarativas, precisas y contextualizadas para evitar respuestas ambiguas o alucinaciones sintácticas.

### Estructura de una Petición Técnica de Alto Rendimiento:
1. **Rol y Contexto:** Definir claramente el rol técnico del sistema (ej. *"Actúa como un arquitecto de software embebido en Linux"*).
2. **Requisitos de Entrada/Salida:** Especificar los tipos de datos exactos, nombres de funciones y librerías permitidas.
3. **Restricciones Explícitas:** Indicar explícitamente qué patrones evitar (ej. *"No uses bucles bloqueantes en el hilo principal de la interfaz"*).
4. **Manejo de Excepciones:** Indicar cómo debe reaccionar el script ante fallas de red o lecturas nulas.

---

## 2. Procesamiento de JSON en Terminal con `jq`

Las APIs modernas basadas en REST y GraphQL devuelven datos en formato JSON. En Linux, la herramienta de línea de comandos `jq` permite filtrar, transformar y extraer campos específicos sin requerir scripts externos de Python.

### Comandos de Ejemplo con `jq`:

* **Formatear y colorear JSON (*pretty print*):**
  ```bash
  curl -s https://api.antigravity.ai/v1/health | jq '.'
  ```
* **Extraer un campo específico:**
  ```bash
  curl -s https://api.antigravity.ai/v1/models | jq '.models[0].name'
  ```
* **Filtrar objetos por condición:**
  ```bash
  cat respuesta_api.json | jq '.results[] | select(.status == "COMPLETED") | .id'
  ```
* **Convertir respuestas JSON a formato CSV:**
  ```bash
  cat telemetria.json | jq -r '.data[] | [.timestamp, .paciente_id, .bpm] | @csv'
  ```

---

## 3. Gestión de Errores Externos y Resiliencia en APIs

Al comunicarse con servicios externos en la nube, las conexiones de red pueden sufrir caídas o saturaciones de límite de tasa (*rate limits*).

### Manejo de Códigos de Estado HTTP Comunes:
* **`200 OK`:** Transacción exitosa.
* **`400 Bad Request`:** Petición mal formada o parámetros JSON inválidos.
* **`401 / 403 Unauthorized`:** Token de autenticación (`Bearer token`) expirado o permisos insuficientes.
* **`429 Too Many Requests`:** Se ha superado la cuota de peticiones por minuto de la API.
* **`500 / 503 Internal Server Error`:** Fallo temporal en la infraestructura remota.

### Estrategia de Reintentos con *Exponential Backoff*:
Cuando una API responde con un error `429` o `503`, el cliente no debe reintentar inmediatamente en bucle. Debe implementar **reintento exponencial con aleatoriedad (jitter)**:

```
Reintento 1: Esperar 1 segundo
Reintento 2: Esperar 2 segundos
Reintento 3: Esperar 4 segundos
Reintento 4: Esperar 8 segundos + Jitter
```

* **Ejemplo de petición `curl` resiliente con reintentos:**
  ```bash
  curl --retry 5 --retry-delay 2 --retry-max-time 30 -H "Authorization: Bearer $API_KEY" https://api.antigravity.ai/v1/query
  ```

