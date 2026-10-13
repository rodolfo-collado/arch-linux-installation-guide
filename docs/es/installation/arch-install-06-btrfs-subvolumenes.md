## 6. Crear subvolúmenes (@, @home, @snapshots)

Un subvolumen btrfs es un árbol de directorios independiente dentro del mismo sistema de archivos. Puede montarse y gestionarse por separado sin crear una partición física adicional. En esta instalación crearemos 3 subvolúmenes principales:

1. `@`: Contiene los archivos del sistema y los paquetes instalados.
2. `@home`: Contiene los archivos y las configuraciones de los usuarios.
3. `@snapshots`: Almacena snapshots del subvolumen raíz (`@`).

- Monta temporalmente la partición `Root` en `/mnt` para poder crear los subvolúmenes:

```bash
mount /dev/nvme0n1p2 /mnt
```

- Crea los subvolúmenes:

1. `@`:

```bash
btrfs subvolume create /mnt/@
```

2. `@home`:

```bash
btrfs subvolume create /mnt/@home
```

3. `@snapshots`:

```bash
btrfs subvolume create /mnt/@snapshots
```

> [!IMPORTANT]
> **Propósito de `@snapshots`:**
> Este subvolumen almacena los snapshots de `@` a modo de historial. Si una actualización rompe el sistema, puedes realizar un [rollback manual](../btrfs/snapper-rollback-manual.md) para volver a un estado anterior.

- Comprueba que los subvolúmenes se hayan creado correctamente:

```bash
btrfs subvolume list /mnt
```

- Desmonta la partición `Root` de `/mnt`:

```bash
umount /mnt
```

---

<div align="center">

[← Anterior](arch-install-05-formateo.md) · [Índice](../../../README.md) · [Siguiente →](arch-install-07-montaje.md)

</div>
