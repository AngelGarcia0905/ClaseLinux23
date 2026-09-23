# Subtema 7.2: Transformación de Texto con `sed` y Filtrado Estructurado con `awk`

---

## 1. Editor de Flujo `sed`

* `sed 's/NORMAL/ACEPTABLE/g' log.txt`: Sustitución global de patrones.
* `sed '/^$/d' datos.csv`: Eliminación de líneas vacías en datasets.

---

## 2. Lenguaje de Procesamiento `awk`

* `awk -F',' '{print $1, $4}' telemetria.csv`: Extracción de columnas específicas.
* `awk -F',' '$3 > 100 {print "ALERTA:", $0}' paciente.csv`: Filtrado condicional en tiempo real.
