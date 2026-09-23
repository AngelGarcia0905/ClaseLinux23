# Subtema 5.2: Redes Internas y Volúmenes Persistentes en Docker

---

## 1. Redes en Docker

* **`bridge` (Predeterminado):** Red interna aislada con mapeo explícito de puertos (`-p 8080:80`).
* **`host`:** Comparte directamente el espacio de nombres de red de la máquina anfitriona.

---

## 2. Persistencia de Datos

* **Volúmenes Administrados (`docker volume create datos_db`):** Guardados en `/var/lib/docker/volumes/`. Ideales para bases de datos relacionales o no relacionales.
* **Montajes Directos (*Bind Mounts*):** Mapeo de directorios locales (`-v $(pwd)/src:/app/src`) para desarrollo continuo.

