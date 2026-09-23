# Subtema 8.2: Extracción y Procesamiento de JSON en Terminal (`jq`)

---

## 1. Filtrado de Respuestas de API con `jq`

```bash
# Formatear respuesta JSON
curl -s https://api.antigravity.ai/v1/health | jq '.'

# Extraer un campo específico
curl -s https://api.antigravity.ai/v1/models | jq '.models[0].name'

# Convertir arreglo JSON a CSV
cat datos.json | jq -r '.data[] | [.timestamp, .bpm] | @csv'
```

