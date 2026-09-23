# Guía de Estudio del Instructor: Linux, IA y Prototipos Biomédicos

### Módulo 1: Arquitectura y Ecosistema Linux
*   **Gestión del Kernel:** Entender cómo el núcleo administra la memoria, los procesos y la comunicación con el hardware.
*   **Sistema de Inicialización (systemd):** Estudiar cómo funcionan los servicios en segundo plano y cómo leer los registros del sistema usando `journalctl`.
*   **Ramas de Debian:** Conocer a fondo las diferencias de paquetería y estabilidad entre Debian Stable, Testing y Sid para justificar elecciones de arquitectura.

### Módulo 2: Administración Profunda de Servidores
*   **Sistemas de Permisos:** Dominar la notación octal (ej. `chmod 755`, `644`) y la gestión avanzada de propietarios con `chown`.
*   **Seguridad de Acceso:** Configuración detallada de llaves SSH (cifrado Ed25519) y modificación del archivo `sshd_config` para deshabilitar inicios de sesión con contraseñas o acceso root directo.
*   **Gestión de Usuarios:** Creación de grupos y asignación de privilegios específicos en el archivo `/etc/sudoers`.

### Módulo 3: Redes y Transferencia de Datos
*   **Modelo TCP/IP:** Comprensión del enrutamiento local, resolución DNS y cómo se abren/cierran los puertos en Linux.
*   **Firewalls Internos:** Entender cómo la herramienta UFW interactúa por debajo con `iptables` o `nftables`.
*   **Sincronización:** Uso de `rsync` y `scp` para la transferencia segura de archivos grandes (como datasets médicos) entre entornos locales y servidores.

### Módulo 4: Infraestructura Híbrida para IA (Edge & Cloud)
*   **Delegación de Cargas (Cloud Computing):** Estudiar cómo funcionan las peticiones a APIs externas (REST) para enviar datos a la nube cuando el hardware local (CPU) no es suficiente.
*   **Librerías de CPU:** Conocer el funcionamiento de modelos de machine learning clásicos (Scikit-learn, regresiones lineales, árboles de decisión) que no requieren aceleración por GPU.
*   **Hardware Dedicado (Opcional):** Repasar la arquitectura de CUDA y los núcleos Tensor de NVIDIA, solo para resolver dudas teóricas de los estudiantes.

### Módulo 5: Arquitectura de Contenedores con Docker
*   **El Demonio de Docker:** Entender la diferencia entre la arquitectura de un contenedor (que comparte el kernel) y una máquina virtual tradicional.
*   **Redes y Volúmenes:** Estudiar cómo Docker maneja sus redes internas (bridge/host) y cómo crear volúmenes persistentes para que no se borren las bases de datos médicas si el contenedor se reinicia.
*   **Construcción de Imágenes:** Escribir archivos `Dockerfile` desde cero, optimizando las capas de instalación de dependencias de Python y Linux.

### Módulo 6: Integración de Hardware Biomédico (PlatformIO)
*   **Protocolos de Comunicación:** Dominar cómo funcionan físicamente los buses I2C (usado por acelerómetros) y UART/Serial, incluyendo conceptos como bits de parada y paridad.
*   **Configuración de Entorno:** Entender la estructura de `platformio.ini`, la gestión de dependencias en C++ y las velocidades de transmisión (baud rates).
*   **Debugging de Hardware en Linux:** Uso intensivo del comando `dmesg` para rastrear cuándo se conecta o desconecta un dispositivo USB y cómo Linux le asigna un puerto (ej. `/dev/ttyACM0`).

### Módulo 7: Procesamiento de Flujos de Datos Médicos
*   **Expresiones Regulares (Regex):** Estudiar la sintaxis para buscar patrones complejos dentro de bases de datos de texto o historiales clínicos.
*   **Manipulación de Streams:** Dominar los comandos `awk` y `sed` para transformar, limpiar y extraer columnas específicas de datos generados por los sensores.
*   **Tuberías (Pipes):** Entender cómo redirigir la salida de un proceso hacia la entrada de otro (usando `|`, `>`, `>>`) de forma eficiente sin saturar la RAM.

### Módulo 8: Interacción Avanzada con APIs y Antigravity
*   **Ingeniería de Prompts para Código:** Aprender a estructurar peticiones técnicas para que los agentes generen scripts funcionales sin alucinaciones.
*   **Manejo de Respuestas Estructuradas:** Extraer y procesar datos en formato JSON desde la terminal (usando herramientas como `jq`).
*   **Gestión de Errores Externos:** Estudiar cómo manejar los límites de tasa (rate limits) y las caídas de conexión al comunicarse con los servidores de Google Antigravity.

### Módulo 9: Orquestación y Tareas Programadas
*   **Automatización con Cron:** Dominar la sintaxis de `crontab` para programar la ejecución de scripts de análisis en momentos específicos.
*   **Gestión de Variables de Entorno:** Cómo exportar y proteger credenciales (como tokens de acceso a APIs) en archivos `.bashrc` o `.env`.
*   **Redirección de Errores:** Aprender a separar la salida normal (stdout) de la salida de errores (stderr) para guardar registros de fallas precisos cuando los scripts automáticos se rompan.

### Módulo 10: Troubleshooting de Prototipos Finales
*   **Depuración Electrónica:** Saber identificar problemas físicos comunes: ruido eléctrico en sensores analógicos, rebotes en señales y caídas de voltaje en la protoboard.
*   **Monitoreo de Procesos:** Uso avanzado de `htop` o `top` para cazar scripts de IA que se hayan quedado en un bucle infinito consumiendo toda la CPU.
*   **Análisis de Logs Multi-capa:** Desarrollar la habilidad de rastrear un error desde el hardware (Arduino), pasando por el puerto serial de Linux, hasta la respuesta final de la API de Antigravity.