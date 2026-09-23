# Subtema 9.1: Automatización de Tareas Programadas con `crontab`

---

## 1. Sintaxis de 5 Campos de Cron

```text
*  *  *  *  *  comando
|  |  |  |  |
|  |  |  |  +---- Día de la semana (0 - 6)
|  |  |  +------- Mes (1 - 12)
|  |  +---------- Día del mes (1 - 31)
|  +------------- Hora (0 - 23)
+---------------- Minuto (0 - 59)
```

### Comandos de Administración:
* `crontab -e`: Editar tareas activas.
* `crontab -l`: Listar programa de tareas.
* `crontab -r`: Eliminar tabla de cron.
