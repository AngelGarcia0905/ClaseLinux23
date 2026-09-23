# Subtema 3.3: Transferencia Segura y Sincronización Masiva (`scp` vs. `rsync`)

---

## 1. Copia Segura con `scp` (Secure Copy)

Copia de archivos únicos a través del túnel SSH:
```bash
scp -i ~/.ssh/id_ed25519 dataset_ecg.csv usuario@192.168.1.78:/var/data/
```

---

## 2. Sincronización Masiva con `rsync`

`rsync` utiliza un algoritmo de transferencia delta que transmite **solo los bloques modificados**, optimizando el ancho de banda en grandes datasets médicos.

```bash
# Sincronización de carpetas con archivo y compresión al vuelo
rsync -avzP -e "ssh -i ~/.ssh/id_ed25519" ./datos_medicos/ usuario@servidor:/var/data/
```

* Flags clave: `-a` (archivar/recursivo), `-v` (detallado), `-z` (comprimir), `-P` (progreso y reanudación).

