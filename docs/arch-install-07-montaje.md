## 7. Montar esquema de particiones (BTRFS):

### 7.1. Montaje de Subvolúmenes y partición EFI:

Montar la raíz `@`:

```bash
mount -o subvol=@,compress=zstd,noatime /dev/nvme0n1p3 /mnt
```

Crear carpetas necesarias para alojar los usuarios y el kernel de linux dentro de la partición principal:

```bash
mkdir -p /mnt/{home,boot,.snapshots}
```

Montar subvolumen `@home`: 

```bash
mount -o subvol=@home,compress=zstd,noatime /dev/nvme0n1p3 /mnt/home
```

Montar subvolumen `@snapshots`:

```bash
mount -o subvol=@snapshots,compress=zstd,noatime /dev/nvme0n1p3 /mnt/.snapshots
```

Montar EFI (NO va kernel aquí, solo bootloader):

```bash
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
```

### 7.2. Claves de Diseño del Esquema de Particiones Aplicado: 

- `@ (/)`: Raiz del sistema. Ser un subvolumen BTRFS permite crear copias de seguridad (snapshots).
- `@home (/home)`: Subvolumen para las carpetas de cada usuario. También permite la creación de snapshots. 
- `@snapshots (/.snapshots)`: Subvolumen que almacena las copias de seguridad del sistema. 
- `/boot`: Directorio dentro de la raíz. Aloja el kernel de Arch Linux y el punto de montaje de la EFI por separado.
- `/boot/efi`: Punto de montaje de la partición EFI. Solo contiene archivos del gestor de Arranque (en nuestro caso será rEFInd) y demás archivos .efi.

Verificar que los subvolúmenes estén bien montados:

```bash
lsblk # tienen que salir varios puntos de montaje en la partición principal
```

O bien: 

```bash
findmnt -nt btrfs # Salen los montajes de forma más explícita 
```

### 7.3. Diferencias entre los Diferentes Esquemas de Particiones:

Al descargar el kernel con el comando` pacstrap -K`, este se aloja por defecto en el directorio `/boot` dentro del sistema. Por lo que podemos jugar con eso y dejar ese directorio dentro de la raíz` /` y montar la partición EFI directamente en `/boot`. Esto es un sistema en el cual, el kernel vive dentro de la partición raíz en el árbol de directorios principal, mientras que la EFI se mantiene solo para bootloader. Tambien existen otras configuraciones como montar la EFI en un directorio único dentro de la raíz (por ejemplo, otro directorio en el mismo nivel que `/boot` llamado `/efi` ). 
####  07.3.1. Esquemas de particiones más comúnes: 

##### a). Esquema de puntos de montaje independientes:

```bash
/  Directorio Raíz - Partición principal del sistema (en formato Ext4, BTRFS, etc)
│
├── boot/  Directorio regular - El kernel vive dentro de la raíz y no en la EFI
│   ├── initramfs-linux.img           <-- Initial RAM Disk
│   ├── initramfs-linux-fallback.img
│   ├── vmlinuz-linux                 <-- Kernel de Arch Linux
│   └── grub/                         <-- Archivos de configuración de grub (en cao)
│       └── grub.cfg
│
├── efi/   Punto de montaje de la EFI separado de /boot, útil para aplicar Single Responsability
│   └── EFI/
│       ├── BOOT/
│       │   └── BOOTX64.EFI           <-- Cargador de Arranque por defecto
│       └── arch/                     <-- Carpeta del gestor de Arranque (grub o systemd)
│           └── grubx64.efi / systemd-bootx64.efi
│
├── etc/
│   └── fstab                         <-- Mapa de particiones del sistema (guia de montaje para el kernel)
│
├── home/
├── root/
└── usr/
```

---
##### b). Esquema tradicional (montando el kernel en la partición EFI):

```bash
/  Directorio Raíz - Partición principal del sistema (Ext4, BTRFS, etc)
│
├── boot/  Punto de montaje de la EFI (FAT32) - El kernel y el gestor conviven aquí
│   ├── initramfs-linux.img           <-- RAM Disk
│   ├── initramfs-linux-fallback.img
│   ├── vmlinuz-linux                 <-- Kernel de Arch
│   └── EFI/                          <-- Directorio base UEFI (Puente de los directorios de la partición)
│       ├── BOOT/
│       │   └── BOOTX64.EFI           <-- Cargador por defecto
│       └── arch/                     <-- Gestor de arranque (grub/systemd)
│           └── grubx64.efi / systemd-bootx64.efi
│
├── etc/
│   └── fstab                         <-- Mapa de montaje
│
├── home/
├── root/
└── usr/
```

##### c). Esquema Mixto (Kernel y EFI en directorio /boot): 

```bash
/  Directorio Raíz - Partición principal del sistema (Ext4, BTRFS, etc)
│
├── boot/  Directorio regular - El kernel vive aquí, dentro de la raíz
│   ├── initramfs-linux.img           <-- RAM Disk
│   ├── initramfs-linux-fallback.img
│   ├── vmlinuz-linux                 <-- Kernel de Arch
│   │
│   └── efi/                          <-- Punto de montaje de la EFI (FAT32) DENTRO de /boot
│       └── EFI/
│           ├── BOOT/
│           │   └── BOOTX64.EFI       <-- Cargador por defecto
│           └── arch/                 <-- Gestor de arranque (típicamente GRUB)
│               └── grubx64.efi
│
├── etc/
│   └── fstab                         <-- Mapa de montaje
│
├── home/
├── root/
└── usr/
```

<div align="center">

[← Anterior](arch-install-06-btrfs-subvolumenes.md) · [Índice](../README.md) · [Siguiente →](arch-install-08-pacstrap.md)

</div>
