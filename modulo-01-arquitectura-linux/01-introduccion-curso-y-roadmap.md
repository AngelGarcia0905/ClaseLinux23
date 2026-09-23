# Subtema 1.1: Introducción General al Curso y Hoja de Ruta (Roadmap)

---

## 1. Visión General del Curso

Bienvenido al curso de **Linux, Inteligencia Artificial y Prototipos Biomédicos**. Este programa académico está diseñado para transformar el conocimiento teórico en capacidades prácticas de nivel profesional, integrando el desarrollo de sistemas operativos, la arquitectura de redes, la contenedorización, el cómputo de Inteligencia Artificial en el borde (*Edge AI*) y la adquisición de señales fisiológicas en tiempo real.

```
+-----------------------------------------------------------------------------------+
|                            HOJA DE RUTA DEL CURSO                                 |
+-----------------------------------------------------------------------------------+
| M01: Arquitectura Linux | M02: Adm. Servidores  | M03: Redes & Transferencia      |
| M04: Infraestructura IA | M05: Docker           | M06: Hardware Biomédico          |
| M07: Procesar Datos     | M08: APIs Antigravity | M09: Orquestación & Cron         |
|                         | M10: Troubleshooting Final                              |
+-----------------------------------------------------------------------------------+
```

---

## 2. Lo que se Llevará a Cabo en el Curso (Módulo por Módulo)

### 🔹 Bloque I: Fundamentos del Sistema Operativo y Redes (Módulos 1 al 3)
* **Módulo 1: Arquitectura y Ecosistema Linux**
  * Comprensión del kernel de Linux, llamadas al sistema (*syscalls*), demonio `systemd` y registros con `journalctl`.
  * Configuración del entorno de desarrollo sobre **Debian 13 (*trixie*)** y **WSL 2**.
  * Análisis conceptual de Entornos de Escritorio (KDE Plasma) y soporte gráfico con WSLg.
* **Módulo 2: Administración Profunda de Servidores**
  * Control estricto de permisos mediante notación octal (`chmod 755`, `644`) y gestión de propietarios (`chown`).
  * Hardening de acceso SSH con criptografía de curva elíptica **Ed25519** y restricción del usuario `root`.
  * Gestión de usuarios y asignación fina de privilegios elevables mediante `/etc/sudoers`.
* **Módulo 3: Redes y Transferencia de Datos**
  * Diagnóstico de sockets, puertos en escucha (`ss`, `lsof`) y pruebas de conectividad (`nc`, `curl`).
  * Configuración de firewalls internos (`iptables`/`nftables` y UFW).
  * Sincronización masiva y cifrada de datasets de salud con `rsync` y `scp`.

### 🔹 Bloque II: Inteligencia Artificial y Contenedores (Módulos 4 y 5)
* **Módulo 4: Infraestructura Híbrida para IA (Edge & Cloud)**
  * Arquitecturas de cómputo en el borde (*Edge*) frente a cómputo en la nube (*Cloud*).
  * Algoritmos de Machine Learning optimizados para CPU (Scikit-learn, NumPy, Pandas).
  * Introducción a la aceleración paralela con **NVIDIA CUDA**, Tensor Cores y diagnóstico con `nvidia-smi`.
* **Módulo 5: Arquitectura de Contenedores con Docker**
  * Diferencias estructurales entre Contenedores y Máquinas Virtuales (espacios de nombres y *cgroups*).
  * Redes integradas (`bridge`/`host`) y volúmenes persistentes para bases de datos biomédicas.
  * Construcción de imágenes optimizadas a partir de archivos `Dockerfile`.

### 3. Bloque III: Hardware Biomédico y Procesamiento (Módulos 6 y 7)
* **Módulo 6: Integración de Hardware Biomédico (PlatformIO)**
  * Protocolos de comunicación en serie: UART/RS-232 e I2C para sensores de señales fisiológicas (ECG AD8232, pulsioxímetros MAX30102, acelerómetros MPU6050).
  * Desarrollo en C++ con `platformio.ini` para ESP32/Arduino.
  * Diagnóstico del kernel con `dmesg`, detección de dispositivos USB y permisos del grupo `dialout` en `/dev/ttyUSB0`.
* **Módulo 7: Procesamiento de Flujos de Datos Médicos**
  * Construcción de expresiones regulares (Regex) para parsear historiales clínicos y registros de telemetría.
  * Transformación y filtrado de datos en tiempo real mediante `awk` y `sed`.
  * Tuberías de Linux (`|`) y redirección de descriptores estándar (`stdout`/`stderr`) sin saturación de memoria RAM.

### 🔹 Bloque IV: APIs, Orquestación y Depuración (Módulos 8 al 10)
* **Módulo 8: Interacción Avanzada con APIs y Antigravity**
  * Ingeniería de Prompts técnicos para guiar a Agentes de IA en la generación de código sin alucinaciones.
  * Extracción y manipulación de datos JSON en terminal mediante `jq`.
  * Gestión de errores HTTP (`429`, `503`) y reintentos con *exponential backoff*.
* **Módulo 9: Orquestación y Tareas Programadas**
  * Automatización de scripts de análisis periódico con la sintaxis de 5 campos de `crontab`.
  * Protección de secretos y claves de API usando archivos `.env` y permisos `chmod 600`.
* **Módulo 10: Troubleshooting de Prototipos Finales**
  * Depuración de fallas electrónicas (ruido analógico, rebotes de señal, caídas de voltaje).
  * Monitoreo de recursos y cazado de procesos congelados con `htop`, `ps` y `kill -9`.
  * Metodología de análisis de logs en 4 capas: *Hardware ➔ Kernel ➔ Contenedor ➔ API Cloud*.

---

## 3. Metodología de Trabajo en el Curso

Cada módulo incluye:
1. **Documentación Teórica Seccionada:** Fundamentos conceptuales detallados en archivos de lectura directa.
2. **Entorno de Laboratorio Real:** Prácticas ejecutadas sobre Debian 13 en WSL 2.
3. **Casos de Aplicación Biomédica:** Ejercicios basados en datos de señales de salud reales (ECG, SpO2, telemetría).
