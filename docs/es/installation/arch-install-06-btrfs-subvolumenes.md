## 6. Crear subvolúmenes (@, @home, @snapshots)

Un subvolumen BTRFS es una raíz lógica dentro del sistema de archivos que se comporta de manera similar a una carpeta con la capacidad de montarse y gestionarse por separado. En esta instalación crearemos 3 subvolúmenes principales: 

1. `@`: Para archivos del sistema y paquetes instalados.
2. `@home`: Para archivos y configuraciones de los usuarios .
3. `@snapshots`: Alberga copias de seguridad de el subvolumen raíz.

- Para crear estos 3 subvolúmenes, montamos temporalmente la partición `Root` del sistema en el directorio `/mnt`:

```bash
mount /dev/nvme0n1p2 /mnt
```

Creamos los subvolúmenes:

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

[← Anterior](arch-install-05-formateo.md) · [Índice](README.md) · [Siguiente →](arch-install-07-montaje.md)

</div>
