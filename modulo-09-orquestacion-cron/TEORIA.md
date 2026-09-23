# Módulo 9: Orquestación y Tareas Programadas en Linux

---

## 1. Automatización de Tareas con Cron y `crontab`

**Cron** es el servicio demonio de Linux encargado de ejecutar comandos o scripts de manera automática en fechas y horas programadas en segundo plano.

### Sintaxis del Archivo `crontab`:
El archivo `crontab` utiliza 5 campos de tiempo seguidos del comando a ejecutar:

```text
*  *  *  *  *  comando_a_ejecutar
|  |  |  |  |
|  |  |  |  +---- Día de la semana (0 - 6) (Domingo=0 u 7)
|  |  |  +------- Mes (1 - 12)
|  |  +---------- Día del mes (1 - 31)
|  +------------- Hora (0 - 23)
+---------------- Minuto (0 - 59)
```

### Comandos de Administración de Cron:
* `crontab -e`: Abre el editor para agregar o modificar tareas programadas del usuario actual.
* `crontab -l`: Lista todas las tareas programadas activas del usuario.
* `crontab -r`: Elimina la tabla de tareas programadas del usuario.

### Ejemplos Prácticos de Programación:
* **Cada 15 minutos:** `*/15 * * * * /usr/bin/python3 /app/respaldo.py`
* **Todos los días a las 3:30 AM:** `30 3 * * * /home/operador/scripts/limpiar_logs.sh`
* **Todos los lunes a las 8:00 AM:** `0 8 * * 1 /home/operador/scripts/reporte_semanal.sh`

---

## 2. Gestión Segura de Variables de Entorno (`.env` y `.bashrc`)

Las aplicaciones de software nunca deben incluir credenciales, llaves de API o contraseñas escritas directamente en el código fuente (*hardcoding*). Se deben usar **variables de entorno**.

### Tipos de Variables de Entorno:
1. **Temporales (Sesión Actual):**
   ```bash
   export ANTIGRAVITY_API_KEY="sk_live_123456789"
   ```
2. **Persistentes por Usuario (`~/.bashrc` o `~/.bash_profile`):**
   Agregar la directiva `export` al final del archivo `~/.bashrc` y recargar con `source ~/.bashrc`.
3. **Archivos `.env` para Aplicaciones y Docker:**
   Archivo de texto que contiene pares clave-valor.
   ```text
   DB_HOST=localhost
   DB_PORT=5432
   API_SECRET_KEY=9a8b7c6d5e4f
   ```

### Seguridad de Archivos de Credenciales:
Los archivos `.env` deben protegerse restringiendo sus permisos de lectura únicamente al usuario propietario:
```bash
chmod 600 .env
```
Además, se debe asegurar incluir `.env` en el archivo `.gitignore` para evitar subir claves privadas a repositorios remotos.

---

## 3. Redirección de Errores y Generación de Logs en Tareas Automáticas

Dado que los trabajos ejecutados por `cron` se corren en segundo plano sin una terminal interactiva adjunta, es fundamental redirigir los mensajes de salida y error hacia archivos de registro (*logs*).

### Separación de Descriptores de Archivo:
* **Descriptor 1 (`stdout`):** Salida estándar del programa.
* **Descriptor 2 (`stderr`):** Mensajes de error o advertencias del programa.

### Patrones de Redirección para Cron:

* **Guardar salida normal y errores en el mismo archivo de log:**
  ```bash
  0 2 * * * /usr/bin/python3 /app/script.py >> /var/log/app.log 2>&1
  ```
  *Explicación: `>>` añade la salida normal al archivo, y `2>&1` redirige la salida de error (2) hacia el mismo canal de la salida normal (1).*

* **Separar salidas en archivos distintos:**
  ```bash
  0 2 * * * /usr/bin/python3 /app/script.py > /var/log/salida.log 2> /var/log/errores.log
  ```

* **Descartar salidas no deseadas:**
  ```bash
  * * * * * /home/user/tarea_silenciosa.sh > /dev/null 2>&1
  ```

