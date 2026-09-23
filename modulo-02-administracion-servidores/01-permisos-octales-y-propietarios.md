# Subtema 2.1: Permisos Octales, Simbólicos y Gestión de Propietarios

---

## 1. El Sistema de Permisos de Linux

Linux utiliza un modelo de seguridad donde cada archivo o directorio posee permisos definidos para tres categorías de usuarios:
* **`u` (User / Propietario):** El usuario creador o dueño.
* **`g` (Group / Grupo):** El grupo asignado.
* **`o` (Others / Otros):** Todos los demás usuarios del sistema.

### Atributos de Permisos:
* **`r` (Read / Lectura - Valor 4):** Leer el contenido de un archivo o listar un directorio.
* **`w` (Write / Escritura - Valor 2):** Modificar un archivo o crear/borrar elementos en un directorio.
* **`x` (Execute / Ejecución - Valor 1):** Ejecutar un programa/script o entrar (`cd`) a un directorio.

---

## 2. Permisos Octales (`chmod`)

La notación octal suma los valores numéricos para cada categoría:

| Código Octal | Permisos Resultantes | Descripción y Casos de Uso |
|---|---|---|
| `chmod 755` | `rwxr-xr-x` | Scripts ejecutables y directorios públicos. |
| `chmod 644` | `rw-r--r--` | Archivos de datos o código fuente estándar. |
| `chmod 600` | `rw-------` | Archivos confidenciales (llaves SSH privadas `.id_ed25519`). |
| `chmod 700` | `rwx------` | Carpetas privadas del usuario. |

---

## 3. Permisos Simbólicos y Propietarios (`chown`)

* **Notación Simbólica:**
  * `chmod u+x script.sh`: Otorga permisos de ejecución al propietario.
  * `chmod g-w archivo.txt`: Quita permisos de escritura al grupo.
* **Gestión de Propietarios (`chown`):**
  * `sudo chown usuario:grupo archivo.txt`: Cambia el propietario y el grupo.
  * `sudo chown -R usuario:grupo /directorio`: Aplica el cambio de propietario recursivamente a toda una carpeta.
