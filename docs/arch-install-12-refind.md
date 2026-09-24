## 12. Instalación y Configuración del Gestor de Arranque (Bootloader) rEFInd:

### 12.1. Instalación dentro de la raíz del sistema creado:

Ejecutar: 

```bash
refind-install
```

Este comando busca la partición `EFI` e instala sus dependencias en: 

```bash
EFI/refind
```

Como requisito, es necesario haber instalado el paquete `refind` en el proceso de instalación del sistema con el comando `pacstrap -K`.

---
### 12.2. Configuración de Parámetros de Arranque con el PARTUUID en el archivo `refind-linux.conf`:

Existen dos identificadores usados para etiquetar particiones de la unidad de almacenamiento. `UUID` y `PARTUUID`. A nosotros nos interesa el `PARTUUID` de la partición `BTRFS` en donde alojamos todo el sistema principal con el objetivo de decirle a `rEFInd` qué partición tiene que buscar para ejecutar el sistema operativo y en dónde está el kernel (en el caso de nosotros tenemos el kernel en la partición btrfs principal). Para ello: 

Obtener PARTUUID correcto:

```bash
lsblk -no PARTUUID /dev/nvme0n1p3 # Partición de ejemplo. Escribe el nombre de la partición que tú usaste para instalar el sistema operativo
```

**Ejemplo de respuesta:** 

```bash
59a25adf-2433-4e13-b0f8-3b0858a62713
```

Luego, entrar en el archivo que usa rEFInd (`refind-linux.conf`) para buscar la partición de la `root` con un editor. Se encuentra en el mismo directorio que el kernel: 

```bash
nano /boot/refind_linux.conf
```

Muchas veces viene con pocas opciones y un UUID/PARTUUID que en ciertos casos no coincide con el que tiene la partición del sistema principal.

Cambia la línea que dice `"Boot using default options"` y añade el `PARTUUID` que obtuviste del comando `lsblk` de manera manual. Además, especifica que el sistema de archivos es `BTRFS` y el `rootflags=subvol=@` para que refind sepa específicamente que subvolumen tiene que arrancar como root.

 **Contenido esperado:**

```bash
"Boot with standard options"  "root=PARTUUID=59a25adf-2433-4e13-b0f8-3b0858a62713 rw rootfstype=btrfs rootflags=subvol=@ loglevel=3" # El 'loglevel=3' es para que el kernel no ensucie la terminal con logs de errores menores de la BIOS. Útil para eficientizar el proceso de instalación.
```

**Si el número no coincide:**
→ Error `"Failed to switch root"`.

> [!TIP]
> **UUID vs. PARTUUID: Diferencias clave**
> 1. **UUID (Universally Unique Identifier):** Está vinculado al sistema de archivos (Btrfs, Ext4, etc.). Se genera al formatear una partición y cambiará si vuelves a formatearla, aunque el disco físico sea el mismo. Es el estándar recomendado para el archivo /etc/fstab.
> 
> 2. **PARTUUID (Partition UUID):** Está vinculado a la tabla de particiones (GPT) del disco duro. Se genera al crear la partición y no cambia aunque la formatees con un sistema de archivos distinto. Es el identificador que utiliza el bootloader para encontrar la partición raíz antes de montar el sistema.
> 
> 
> **Resumen:** El UUID identifica "qué hay dentro" de la partición (el formato), mientras que el PARTUUID identifica el "lugar físico" en la tabla del disco.

---

<div align="center">

[← Anterior](arch-install-11-usuario-sudo.md) · [🏠 Índice](../README.md) · [Siguiente →](arch-install-13-hyprland.md)

</div>
