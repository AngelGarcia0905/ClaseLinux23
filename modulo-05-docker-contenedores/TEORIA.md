# Módulo 5: Arquitectura de Contenedores con Docker

---

## 1. El Demonio de Docker y Arquitectura de Contenedores vs. Máquinas Virtuales

**Docker** es una plataforma de virtualización a nivel del sistema operativo que permite empaquetar aplicaciones junto con todas sus dependencias en unidades estandarizadas llamadas **contenedores**.

### Contenedores vs. Máquinas Virtuales (VMs):

```
+------------------------------------+      +------------------------------------+
|  Contenedor A  |   Contenedor B    |      |     VM 1          |      VM 2          |
| (App + Libs)   |   (App + Libs)    |      | (App+Libs+OS Guest)| (App+Libs+OS Guest)|
+------------------------------------+      +------------------------------------+
|        Motor de Docker             |      |            Hipervisor              |
+------------------------------------+      +------------------------------------+
|     KERNEL LINUX DE LA MÁQUINA     |      |       SISTEMA OPERATIVO BASE       |
+------------------------------------+      +------------------------------------+
|             HARDWARE               |      |              HARDWARE              |
+------------------------------------+      +------------------------------------+
```

* **Máquinas Virtuales:** Virtualizan el hardware completo. Cada VM incluye un sistema operativo huésped completo (*Guest OS*), lo que genera un alto consumo de memoria RAM, espacio en disco y tiempo de arranque.
* **Contenedores Docker:** Aíslan procesos utilizando características nativas del kernel de Linux (**cgroups** para limitar recursos y **namespaces** para aislar procesos, red y archivos). Todos los contenedores comparten el mismo kernel de la máquina anfitriona (*Host Kernel*), siendo ultra-ligeros y arrancando en milisegundos.

---

## 2. Redes y Volúmenes Persistentes en Docker

Por defecto, los contenedores son efímeros: cualquier dato guardado dentro de la capa del contenedor se borra si este se destruye. Para producción e infraestructura médica, se requiere persistencia y conectividad.

### Redes en Docker:
Docker crea interfaces de red virtuales para aislar la comunicación entre aplicaciones:

* **Modo `bridge` (Predeterminado):** Crea una red privada interna dentro del host. Los puertos deben mapearse explícitamente hacia afuera (`-p 8080:80`).
* **Modo `host`:** Elimina el aislamiento de red entre el contenedor y el host; el contenedor utiliza directamente las interfaces del sistema anfitrión.
* **Modo `none`:** Deshabilita la red completamente para máxima seguridad.

### Persistencia de Datos (Volúmenes vs. Bind Mounts):
* **Volúmenes Administrados (`docker volume`):** Gestionados totalmente por Docker dentro de `/var/lib/docker/volumes/`. Es la opción recomendada para bases de datos PostgreSQL o MongoDB.
  ```bash
  docker volume create datos_medicos
  docker run -v datos_medicos:/var/lib/postgresql/data postgres
  ```
* **Montajes Directos (*Bind Mounts*):** Asocian una carpeta específica del host dentro del contenedor. Muy utilizado durante el desarrollo para reflejar cambios en código fuente en tiempo real.
  ```bash
  docker run -v $(pwd)/src:/app/src python-app
  ```

---

## 3. Construcción de Imágenes Optimizadas (`Dockerfile`)

Un `Dockerfile` es un script de texto que contiene las instrucciones para construir una imagen de Docker reutilizable.

### Buenas Prácticas y Caché de Capas:
Cada instrucción (`FROM`, `RUN`, `COPY`) crea una capa de solo lectura en la imagen. Docker almacena en caché estas capas para acelerar compilaciones futuras. Las instrucciones que cambian con menos frecuencia (como instalar dependencias del sistema o `pip install`) deben colocarse antes de copiar el código fuente de la aplicación.

### Ejemplo de `Dockerfile` para una API en Python:

```dockerfile
# 1. Imagen base oficial ligera basada en Debian
FROM python:3.11-slim

# 2. Establecer carpeta de trabajo
WORKDIR /app

# 3. Instalar dependencias del sistema necesarias
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libpq-dev \
 && rm -rf /var/lib/apt/lists/*

# 4. Copiar lista de requerimientos e instalar (aprovecha la caché de capas)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 5. Copiar el resto del código de la aplicación
COPY . .

# 6. Exponer puerto del contenedor
EXPOSE 8000

# 7. Comando de ejecución por defecto
CMD ["python", "main.py"]
```

