## 05. Formatear particiones: 

Con las particiones EFI y Root creadas, recuerda hacer un formato de las mismas en sus respectivos filesystem. Para identifiicar los nombres de las particiones (que no es lo mismo que las etiquetas que les asignamos en `cfdisk`) usamos este comando: 

```bash
lsblk -f
```

Esto te muestra el árbol de particiones que tiene cada disco.  Identifica la partición EFI y la Root que creamos anteriormente y continua: 

### 05.1. Partición EFI:

Si instalaste windows primero y tu objetivo es tener un *Sistema dualboot*, la partición EFI del disco donde se instaló windows se creó automaticamente. Si este es el caso entonces **NO FORMATEES** dicha partición o romperás el inicio de windows. 

Ahora, si creaste la partición EFI desde cero con `cfdisk`   y dándole el Type de *EFI System*, ejecuta: 

```bash
mkfs.fat -F 32 /dev/nvme0n1p1 
```

*La ruta cambia en dependencia del nombre de partición que te haya dado* `lsblk -f`.

### 05.2. Partición Root:

El sistema de particiones que usaremos es BTRFS:

```bash
mkfs.btrfs -L Arch_Linux /dev/nvme0n1p3 #la '-L' permite poner una label a la partición. 
```

> [!IMPORTANT]
> **¿Por qué usar un sistema BTRFS y no el tradicional Ext4?**
>
>BTRFS permite *subvolúmenes y snapshots*. Esto es de gran utilidad por si rompimos algo en el sistema y queremos volver a un "estado anterior" de cuando todo estaba bien. Todo esto es gracias al funcionamiento **CoW** (Copy on Write) de BTRFS. No usaremos el esquema clásico de `/boot`, `/swap`, `/` porque perderíamos dicha función tan necesaria para distribuciones como Arch que, con una actualización general, cabe la posibilidad de que el sistema colapse y por ejemplo, no muestre video o se rompa una característica fundamental del sistema.

En caso de error por tabla de particiones existente (GPT, DOS, etc), forzar con:

```bash
mkfs.btrfs -f /dev/nvme0n1p3
```

---

<div align="center">

[← Anterior](arch-install-04-particionado.md) · [Índice](../README.md) · [Siguiente →](arch-install-06-btrfs-subvolumenes.md)

</div>
