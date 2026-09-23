# Subtema 1.4: Ecosistema de Distribuciones Linux y Elección de Debian

---

## 1. El Ecosistema de Distribuciones ("Distros")

Una **Distribución Linux** combina el kernel Linux con las herramientas del proyecto GNU, un gestor de paquetes específico, repositorios de software y configuraciones predeterminadas.

```
                  +-------------------------+
                  |      Kernel Linux       |
                  +------------+------------+
                               |
       +-----------------------+-----------------------+
       |                       |                       |
+------v------+         +------v------+         +------v------+
|Familia Debian|        |Familia RedHat|        | Familia Arch|
+------+------+         +------+------+         +------+------+
       |                       |                       |
  +----+----+             +----+----+             +----+----+
  |         |             |         |             |         |
Debian   Ubuntu         RHEL   Fedora/Rocky     Arch     Manjaro
```

### Principales Familias:
1. **Familia Debian:** Utiliza paquetes `.deb` y el gestor `apt` / `dpkg`. Prioriza la estabilidad y la libertad de software. Derivados: Ubuntu, Linux Mint, Raspberry Pi OS.
2. **Familia Red Hat:** Utiliza paquetes `.rpm` y el gestor `dnf` / `yum`. Enfocada en el ámbito corporativo e industrial. Derivados: RHEL, Fedora, Rocky Linux, AlmaLinux.
3. **Familia Arch:** Utiliza el gestor `pacman` y el modelo de actualización continua (*Rolling Release*). Enfocada en personalización y software *bleeding-edge*.
4. **Distribuciones Independientes Especializadas:** Alpine Linux (orientada a contenedores ultra-ligeros con paquetes `apk`).

---

## 2. Servidores: ¿Qué Distribuciones Usan y Por Qué?

En la infraestructura de servidores de producción se exigen tres criterios clave:
* **Predecibilidad y Estabilidad:** Que las librerías del sistema no cambien drásticamente entre actualizaciones rompiendo el código de producción.
* **Soporte de Seguridad a Largo Plazo (LTS):** Parches de seguridad continuos sin obligar a actualizar la versión base del sistema operativo.
* **Bajo Consumo de Recursos:** Ejecución en modo headless (sin interfaz gráfica) para destinar el 100% del hardware a las cargas de trabajo.

### Dominio de Servidores:
* **Debian & Ubuntu Server:** Dominan el desarrollo web, infraestructura de nube pública (AWS, GCP) y contenedores Docker.
* **RHEL / Rocky Linux:** Dominan en el sector bancario, gubernamental y empresarial con acuerdos de nivel de servicio (SLA).

---

## 3. Justificación de Debian en el Curso

Debian es denominado **"El Sistema Operativo Universal"**. Su desarrollo se organiza en tres ramas claras:
* **Debian Stable (Estable):** Probada exhaustivamente; es la versión utilizada en nuestro entorno (**Debian 13 *trixie***).
* **Debian Testing (Pruebas):** Preparación de la siguiente versión estable.
* **Debian Unstable / Sid (Inestable):** Desarrollo continuo.

### Razones para su uso en la clase:
1. **Conocimiento Base Transversal:** Al dominar Debian, se domina automáticamente Ubuntu, Linux Mint y Raspberry Pi OS.
2. **Estabilidad Inalterable:** Garantiza que las dependencias de Python, C++ y herramientas de red permanezcan funcionales sin romperse por actualizaciones sorpresa.
3. **Eficiencia en Contenedores:** Formato estándar para imágenes Docker (`debian:slim`).

