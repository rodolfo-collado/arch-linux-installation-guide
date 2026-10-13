## 7. Montar esquema de particiones (BTRFS)

### 7.1. Montaje de subvolúmenes y partición EFI

- Montar la raíz `@` en la ruta `/mnt`:

```bash
mount -o subvol=@,compress=zstd,noatime /dev/nvme0n1p2 /mnt
```

- Crea las carpetas necesarias para alojar los `usuarios`, el `kernel` y los `snapshots` del sistema:

```bash
mkdir -p /mnt/{home,boot,.snapshots}
```

- Monta el subvolumen `@home` en la ruta `/mnt/home`:

```bash
mount -o subvol=@home,compress=zstd,noatime /dev/nvme0n1p2 /mnt/home
```

- Montar subvolumen `@snapshots` en la ruta `/mnt/.snapshots`:

```bash
mount -o subvol=@snapshots,compress=zstd,noatime /dev/nvme0n1p2 /mnt/.snapshots
```

- Montar `EFI` en la ruta `/mnt/boot`:

```bash
mount /dev/nvme0n1p1 /mnt/boot/
```

- Verificar puntos de montaje:

```bash
lsblk
```

Tras montar la partición `EFI` y los subvolúmenes de `Root`, la salida de `lsblk` debería mostrar los puntos de montaje asociados a cada partición:

```bash
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0    1G  1 loop /run/archiso/airootfs
sr0          11:0    1  1.5G  1 rom  /run/archiso/bootmnt
nvme0n1     259:0    0   426G  0 disk
├─nvme0n1p1 259:1    0     2G  0 part /mnt/boot
└─nvme0n1p2 259:2    0   424G  0 part /mnt/home
                                      /mnt/.snapshots
                                      /mnt
```

- Verifica explícitamente los puntos de montaje de los subvolúmenes:

```bash
findmnt -nt btrfs
```

> [!IMPORTANT]
> **Esquema utilizado en esta guía:**
> - `@` → `/`: archivos del sistema y paquetes instalados.
> - `@home` → `/home`: archivos y configuraciones de los usuarios.
> - `@snapshots` → `/.snapshots`: snapshots del sistema.
> - `EFI` → `/boot`: contiene el kernel y los archivos del bootloader.

### 7.2. Esquemas de montaje más comunes

El kernel se instala normalmente en `/boot`. La ubicación (o punto de montaje) de la partición EFI determina si comparte ese directorio con el kernel o si se monta por separado.

| Esquema | Montaje de la EFI | Ubicación del kernel | Ubicación del bootloader |
| --- | --- | --- | --- |
| EFI independiente | `/efi` | `/boot` dentro de `Root` | `/efi/EFI/` |
| Tradicional | `/boot` | `/boot` dentro de la EFI | `/boot/EFI/` |
| Mixto | `/boot/efi` | `/boot` dentro de `Root` | `/boot/efi/EFI/` |

Esta guía utiliza el esquema tradicional: la partición EFI se monta directamente en `/boot`, por lo que el kernel y el bootloader comparten esa partición.

<div align="center">

[← Anterior](arch-install-06-btrfs-subvolumenes.md) · [Índice](../../../README.md) · [Siguiente →](arch-install-08-pacstrap.md)

</div>
