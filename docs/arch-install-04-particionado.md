## 4. Verificar la ruta de instalación y crear la tabla de particiones

- Lista los discos disponibles e identifica dónde instalarás Arch:

```bash
fdisk -l
```

- Abre el disco con `cfdisk`. En este ejemplo se utiliza `/dev/nvme0n1`:

```bash
cfdisk /dev/nvme0n1
```

> [!WARNING]
> No modifiques las particiones de Windows desde `cfdisk`. Si necesitas reducir o mover una partición de Windows, hazlo desde Windows utilizando sus propias herramientas de administración de discos.

- Selecciona `GPT` para una instalación con UEFI. Las opciones principales de `cfdisk` son:

| Opción     | Función                                                            | Uso en esta guía                                                                                 |
| ---------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| `[New]`    | Crea una partición en el espacio libre.                            | Crear EFI y Root.                                                                                |
| `[Delete]` | Elimina la partición seleccionada y la convierte en espacio libre. | Solo usar si estás seguro de que no contiene datos necesarios.                                   |
| `[Resize]` | Reduce o amplía la partición seleccionada.                         | Ajustar las particiones al tamaño preferido por el usuario. No usar para particiones de Windows. |
| `[Type]`   | Asigna el tipo de partición.                                       | Asignarle `EFI System` para EFI y `Linux root (x86-64)` para Root.                               |
| `[Help]`   | Muestra la ayuda de `cfdisk`.                                      | Consultar si necesitas más información.                                                          |
| `[Write]`  | Guarda los cambios en el disco.                                    | Confirma escribiendo `yes`; puede borrar datos existentes.                                       |
| `[Quit]`   | Sale de `cfdisk` sin guardar cambios pendientes.                   | Usar después de `[Write]`.                                                                       |

### 4.1. Particiones necesarias para Arch Linux con BTRFS

| Partición | Tipo en `cfdisk` | Formato | Tamaño orientativo | Uso |
|---|---|---|---|---|
| EFI | `EFI System` | `FAT32`/`vfat` | 512 MiB o más | Archivos de arranque. |
| Root | `Linux root (x86-64)` | `BTRFS` | 25–40 GB para pruebas; más de 80 GB para uso diario | Sistema operativo, usuarios y datos. |

> [!CAUTION]
> Puedes reutilizar la partición EFI de Windows, pero compartirla aumenta el riesgo de que una actualización o reparación de Windows modifique sus archivos de arranque. *Esto podría afectar el arranque de Arch y dejar inaccesibles archivos como el kernel o el cargador de arranque*. Haz una copia de seguridad antes de compartirla y, si es posible, utiliza una partición EFI independiente.

- Es recomendable utilizar una partición `EFI` diferente para cada sistema operativo que instales a futuro.
### 4.2. ¿Es necesaria una partición Swap?

| Situación                                                    | Recomendación                                                           |
| ------------------------------------------------------------ | ----------------------------------------------------------------------- |
| No necesitas hibernación                                     | Puedes omitir la partición Swap.                                        |
| Necesitas hibernación                                        | Usa una partición o archivo Swap con un tamaño aproximado al de la RAM. |
| Necesitas Swap, pero quieres mantener simple el particionado | Crea un `Swapfile` dentro de la partición root más adelante.            |

#### 4.2.1. Ejemplo de una tabla de particiones en una VM

- En una máquina virtual, el disco suele aparecer como `/dev/vda`. En hardware físico puede aparecer como `/dev/nvme0n1`, `/dev/sda` u otro nombre. Ejemplo:

![Ejemplo de una tabla de particiones de una máquina virtual](../assets/arch-install-partitioning-screenshot-001.png)

---

<div align="center">

[← Anterior](arch-install-03-internet.md) · [Índice](../README.md) · [Siguiente →](arch-install-05-formateo.md)

</div>
