## 5. Formatear particiones

- Con las particiones `EFI` y `Root` creadas, identifica sus nombres para continuar con el formateo individual de cada una:

```bash
lsblk
```

Esto te muestra el árbol de particiones que tiene cada disco. Identifica los nombres de `EFI` y `Root`. La salida esperada de este comando es:

```bash
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0    1G  1 loop /run/archiso/airootfs
sr0          11:0    1  1.5G  1 rom  /run/archiso/bootmnt
nvme0n1     259:0    0  426G  0 disk
├─nvme0n1p1 259:1    0    2G  0 part
└─nvme0n1p2 259:2    0  424G  0 part
```

> Salida de ejemplo de un disco NVMe de 426 GB particionado con una `EFI` de 2 GB y una `Root` de 424 GB. Las entradas `loop0` y `sr0` pertenecen a la Live ISO y pueden ignorarse.

### 5.1. Partición EFI

- Formatea la partición EFI como `FAT32` y asígnale la etiqueta `EFI_ARCH`. Sustituye `nvme0n1p1` por el nombre de tu partición `EFI`:

```bash
mkfs.fat -F 32 -n EFI_ARCH /dev/nvme0n1p1
```

### 5.2. Partición Root

- Formatea la partición Root como `BTRFS` y asígnale la etiqueta `Arch_Linux`. Sustituye `nvme0n1p2` por el nombre de tu partición `Root`:

```bash
mkfs.btrfs -L Arch_Linux /dev/nvme0n1p2
```

> [!IMPORTANT]
> **¿Por qué BTRFS y no Ext4?**
>
> BTRFS permite crear subvolúmenes y snapshots gracias a su sistema **CoW** (*Copy-on-Write*). Esto permite regresar a un estado anterior si una actualización rompe el sistema.

Si `mkfs.btrfs` detecta un sistema de archivos existente y no permite continuar, fuerza el formateo con `-f`:

> [!WARNING]
> `-f` sobrescribe el sistema de archivos existente. Verifica nuevamente que la partición sea la correcta antes de ejecutarlo.

```bash
mkfs.btrfs -f -L Arch_Linux /dev/nvme0n1p2
```

---

<div align="center">

[← Anterior](arch-install-04-particionado.md) · [Índice](../README.md) · [Siguiente →](arch-install-06-btrfs-subvolumenes.md)

</div>
