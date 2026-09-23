# Módulo 10: Troubleshooting y Depuración de Prototipos Finales

---

## 1. Depuración de Hardware y Señales Electrónicas

Al desarrollar prototipos biomédicos e integraciones de hardware con Linux, los problemas físicos son tan comunes como los errores de código.

### Problemas Físicos Frecuentes y Diagnóstico:
1. **Ruido Eléctrico en Sensores Analógicos:**
   * **Síntoma:** Lecturas de ECG o temperatura extremadamente erráticas o fluctuantes.
   * **Causa:** Falta de desacoplamiento de tierra (*common ground*), fuentes de alimentación ruidosas o interferencia electromagnética de motores.
   * **Solución:** Compartir la tierra (GND) entre el microcontrolador y los sensores, agregar capacitores de desacoplamiento (100nF) y aplicar filtros digitales (Media Móvil o filtro Butterworth) en Linux.
2. **Rebotes de Señal (*Debounce*):**
   * **Síntoma:** Un botón de pánico o pulsador registra múltiples activaciones por un solo toque.
   * **Solución:** Implementar retrasos por software en el microcontrolador o arreglos RC en hardware.
3. **Caídas de Voltaje en la Protoboard:**
   * **Síntoma:** El microcontrolador se reinicia esporádicamente al activar un módulo Wi-Fi o relé.
   * **Solución:** Alimentar componentes de alto consumo mediante líneas de alimentación dedicadas en lugar de utilizar las salidas de reguladores integrados de 3.3V/5V de la placa.

---

## 2. Monitoreo de Recursos y Depuración de Procesos en Linux

Cuando un modelo de IA o un script de ingesta de datos entra en un bucle infinito, puede congelar el sistema consumiendo el 100% de la CPU o la memoria RAM.

### Herramientas de Inspección del Sistema:

* **`top` / `htop`:** Muestra la lista interactiva de procesos ordenados por consumo de recursos.
  * Presionar `P` para ordenar por uso de CPU.
  * Presionar `M` para ordenar por consumo de memoria RAM.
  * Identificar el **PID** (ID de Proceso) del script problemático.

* **`ps` (Process Status):**
  * `ps aux | grep python`: Busca todos los scripts de Python en ejecución mostrando su PID, usuario y comando completo.

* **Terminación de Procesos Bloqueados (`kill`, `killall`):**
  * `kill <PID>`: Envía la señal `SIGTERM` (15) permitiendo que el proceso se cierre de forma limpia.
  * `kill -9 <PID>`: Envía la señal `SIGKILL` (9) que fuerza la terminación inmediata del proceso por parte del kernel.
  * `killall -9 python3`: Fuerza el cierre de todos los procesos de Python3 descontrolados.

---

## 3. Análisis de Logs Multi-capa (Hardware -> Kernel -> Contenedor -> API)

En un prototipo complejo, una falla puede ocurrir en cualquier nivel de la arquitectura. El troubleshooting profesional requiere rastrear el flujo del error de forma metódica.

```
+------------------+     +------------------+     +------------------+     +------------------+
|  Microcontrolador| --> |  Puerto Serie    | --> |  Contenedor      | --> |  API Remota      |
|  (Sensor / ECG)  |     | (/dev/ttyACM0)   |     | (Docker / Python) |     | (Antigravity)    |
+------------------+     +------------------+     +------------------+     +------------------+
        |                         |                        |                        |
  Revisar cables/          Revisar kernel           Revisar logs             Revisar HTTP
  voltaje con              `dmesg -wH`              `docker logs -f`         `jq` / status
  polímetro                                                                  codes (429/500)
```

### Metodología de Depuración Paso a Paso:

1. **Capa 1: Hardware Físico**
   * Verificar LED de estado en la placa y comprobar con el multímetro la continuidad y voltajes en GND, VCC, SDA/SCL.
2. **Capa 2: Puerto Serie y Kernel de Linux**
   * Ejecutar `dmesg -wH` y verificar si Linux detecta el USB `/dev/ttyACM0`.
   * Probar la lectura cruda del puerto serie con `cat /dev/ttyACM0` o `minicom`.
3. **Capa 3: Servicio y Contenedores**
   * Consultar registros de systemd: `journalctl -u servicio_medico -f`.
   * Consultar registros de Docker: `docker logs --tail 100 -f contenedor_python`.
4. **Capa 4: Conexión y Respuestas de API**
   * Inspeccionar si la API devuelve errores `401` (token inválido) o `429` (límite de cuota) imprimiendo los códigos HTTP de respuesta.

