# Subtema 7.3: Tuberías (*Pipes*) y Redirección de Descriptores Estándar

---

## 1. Operadores de Redirección

* `>`: Redirige `stdout` a un archivo (sobrescribe).
* `>>`: Anexa `stdout` al final de un archivo (*append*).
* `2>`: Redirige únicamente la salida de errores (`stderr`).
* `&>`: Redirige tanto `stdout` como `stderr`.

---

## 2. Tuberías en Linux (`|`)

Conectan la salida estándar de un proceso con la entrada del siguiente sin utilizar archivos intermedios en disco:

```bash
cat datos_salud.log | grep "ALERTA" | awk '{print $1, $4}' | sort | uniq -c > informe.txt
```
