## 10. Configuración del mkinitcpio según hardware

### 10.1. Configuración para gráficas NVIDIA

- Entra al archivo `environment`:

```bash
nano /etc/environment
```

- Crea las variables de entorno para que `Wayland` y `Hyprland` puedan detectar y usar la gráfica. Guarda los cambios y sal del editor al final:

```bash
LIBVA_DRIVER_NAME=nvidia
XDG_SESSION_TYPE=wayland
GBM_BACKEND=nvidia-drm
__GLX_VENDOR_LIBRARY_NAME=nvidia
WLR_NO_HARDWARE_CURSORS=1
```

-  Activa el *modesetting* del módulo `nvidia_drm`:

```bash
echo "options nvidia-drm modeset=1" > /etc/modprobe.d/nvidia.conf
```

> [!NOTE]
> El *modesetting* permite que el kernel gestione la resolución, la profundidad de color y la tasa de refresco. Activarlo facilita que Wayland y Hyprland inicialicen correctamente la GPU NVIDIA desde el arranque.

Si no se activa, pueden aparecer problemas como los siguientes:

| Problema                                | Posible efecto                                                              |
| --------------------------------------- | --------------------------------------------------------------------------- |
| Pantalla negra                          | Hyprland no inicia correctamente porque la GPU no está disponible a tiempo. |
| Tearing                                 | Aparecen cortes o líneas horizontales al mover ventanas.                    |
| Falta de aceleración (lentitud general) | El sistema no utiliza correctamente la aceleración de la GPU.               |

> [!IMPORTANT]
> **¿Qué es `mkinitcpio`?**
> 
> Es la herramienta que Arch Linux utiliza para generar las imágenes `initramfs`, un sistema temporal que el kernel carga durante el arranque antes de montar el sistema raíz. Esta imagen incluye los módulos, firmware y herramientas necesarios para inicializar el hardware y continuar el proceso de arranque.

- Edita el archivo `mkinitcpio.conf`:

```bash
nano /etc/mkinitcpio.conf
```

- Busca la línea `MODULES=()` y añade los módulos de NVIDIA. Guarda los cambios y sal del editor al terminar:

```bash
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

- El comando `pacstrap` de instalación genera una imagen `initramfs` inicial al instalar el kernel. Como modificaste `mkinitcpio.conf`, vuelve a generarla para incluir los módulos de NVIDIA:

```bash
mkinitcpio -P
```

> [!NOTE]
> La bandera `-P` ( es la opción corta de `--allpresets`) procesa todos los presets disponibles en `/etc/mkinitcpio.d/` y genera las imágenes `initramfs` correspondientes a los kernels instalados.

### 10.2. Configuración para gráficas AMD/Intel

- Para gráficas AMD o Intel no es necesario añadir módulos específicos. Si modificaste `mkinitcpio.conf` o quieres actualizar las imágenes de arranque, ejecuta:

```bash
mkinitcpio -P
```

---

<div align="center">

[← Anterior](arch-install-09-post-install.md) · [Índice](../../../README.md) · [Siguiente →](arch-install-11-usuario-sudo.md)

</div>
