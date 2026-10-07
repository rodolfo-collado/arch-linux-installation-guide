## 6. Crear subvolúmenes (@, @home, @snapshots)

Montamos temporalmente la partición:

```bash
mount /dev/nvme0n1p3 /mnt
```

Creamos subvolúmenes (la raíz y el home donde se alojarán todos los usuarios):

```bash
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@snapshots
```

> [!IMPORTANT]
> **Aclaración del subolumen @Snapshots:**
>En este subvolumen se van a guardar todas las "fotos" del subvolumen de la raíz (`@`), esto en caso de que el sistema falle y puedas volver a un estado anterior donde todo estaba bien. El proceso de configuración de Snapshots (concretamente, con la herramienta **Snapper**) están explicadas en **Configuración de Snapshots BTRFS con Snapper**.

Desmontamos todas las particiones de la ruta `/mnt`:

```bash
umount /mnt
```

**Por qué:**  
- `@` → root  
- `@home` → home (directorio de usuarios)
- `@snapshots` → snapshots (Copias de seguridad del sistema en caso de fallo)

Separación lógica sin particiones físicas. Esto porque más adelante montaremos dichas rutas como `btrfs subvols`.

---

<div align="center">

[← Anterior](arch-install-05-formateo.md) · [Índice](../README.md) · [Siguiente →](arch-install-07-montaje.md)

</div>
