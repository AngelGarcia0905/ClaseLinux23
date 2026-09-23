# Módulo 7: Procesamiento de Flujos de Datos Médicos en la Terminal

---

## 1. Expresiones Regulares (Regex) en Procesamiento de Datos

Las **Expresiones Regulares** son patrones de búsqueda que permiten filtrar, validar y extraer información estructurada desde registros médicos en texto plano (*logs*, archivos HL7, registros CSV).

### Metacaracteres Principales:
* `^`: Inicio de línea.
* `$`: Fin de línea.
* `.`: Cualquier carácter excepto salto de línea.
* `[0-9]`: Cualquier dígito del 0 al 9 (equivalente a `\d`).
* `[a-zA-Z]`: Cualquier letra mayúscula o minúscula.
* `*`: 0 o más repeticiones.
* `+`: 1 o más repeticiones.
* `?`: 0 o 1 repetición (opcional).
* `( ... )`: Grupo de captura.

### Ejemplo Práctico:
Validación de patrones de Frecuencia Cardíaca en un log: `BPM:\s*([0-9]{2,3})` extrae valores numéricos de 2 a 3 dígitos precedidos por la etiqueta "BPM:".

---

## 2. Manipulación de Streams con `awk` y `sed`

Linux ofrece potentes procesadores de texto en línea de comandos ideales para transformar grandes volúmenes de datos directamente en memoria sin saturar la RAM.

### `sed` (Stream Editor):
Se utiliza principalmente para realizar transformaciones de texto en línea, sustituciones e inserciones sobre un flujo de datos.

* **Sustitución global de patrones:**
  ```bash
  sed 's/NORMAL/ACEPTABLE/g' registros_pacientes.log
  ```
* **Eliminar líneas vacías:**
  ```bash
  sed '/^$/d' datos_crudos.csv
  ```
* **Extraer líneas específicas por rango:**
  ```bash
  sed -n '10,50p' datos_ecg.txt
  ```

### `awk` (Procesador de Lenguaje Orientado a Filas y Columnas):
`awk` analiza archivos línea por línea, dividiendo cada registro en campos (columnas) basados en un separador (por defecto, espacios o tabulaciones).

* **Variables incorporadas de `awk`:**
  * `$0`: La línea completa actual.
  * `$1, $2, ...`: El primer, segundo, etc., campo de la línea.
  * `FS`: Separador de campos de entrada (*Field Separator*).
  * `NR`: Número de registro (línea) actual.
* **Ejemplos prácticos en Linux:**
  * Extraer la fecha (columna 1) y el valor de SpO2 (columna 4) de un CSV separado por comas:
    ```bash
    awk -F',' '{print $1, $4}' datos_oximetria.csv
    ```
  * Filtrar lecturas anormales donde el pulso ($3) sea mayor a 100 BPM:
    ```bash
    awk -F',' '$3 > 100 {print "ALERTA TATICARDIA:", $0}' registros.csv
    ```

---

## 3. Redirecciones y Tuberías (*Pipes*)

El principio modular de Linux permite conectar programas pequeños creando canales de procesamiento complejos en tiempo real.

```
[ Proceso A (Sensor CSV) ] -- stdout --> | PIPE | -- stdin --> [ Proceso B (awk Filtro) ]
```

### Operadores de Redirección:
* `>`: Redirige la salida estándar (`stdout`) a un archivo, **sobrescribiendo** su contenido.
* `>>`: Redirige la salida estándar a un archivo, **añadiendo** el contenido al final (*append*).
* `<`: Redirige la entrada estándar (`stdin`) desde un archivo hacia un programa.
* `2>`: Redirige únicamente los mensajes de error (`stderr`) a un archivo.
* `&>`: Redirige tanto `stdout` como `stderr` al mismo destino.

### Tuberías (`|`):
Conecta la salida estándar (`stdout`) del comando de la izquierda directamente a la entrada estándar (`stdin`) del comando de la derecha.

* **Ejemplo de procesamiento de flujo biomédico completo:**
  ```bash
  cat telemetria_medica.log | grep "ERROR" | awk '{print $1, $5}' | sort | uniq -c > informe_fallas.txt
  ```

