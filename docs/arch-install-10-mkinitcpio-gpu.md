## 10. Configuración del mkinitcpio según hardware:

### 10.1. Configuración para gráficas Nvidia: 

Crear las variables de entorno dentro del archivo `environment`  para que Wayland/Hyprland puedan detectar y usar la gráfica de Nvidia.

entrar al `/etc/environment` y escribir: 

```bash
LIBVA_DRIVER_NAME=nvidia
XDG_SESSION_TYPE=wayland
GBM_BACKEND=nvidia-drm
__GLX_VENDOR_LIBRARY_NAME=nvidia
WLR_NO_HARDWARE_CURSORS=1 # a veces este no es tan necesario pero se sigue dejando por si el cursor se vuelve invisible o parpadea
```

Luego, tenemos que forzar el `modset` en el kernel. Para ello ejecuta: 

```bash
echo "options nvidia-drm modeset=1" > /etc/modprobe.d/nvidia.conf
```

> [!NOTE]
> **Datazo**
> El **Modesetting** es la capacidad del sistema para cambiar la resolución de pantalla, la profundidad de color y la tasa de refresco.
> 
> Por defecto, el driver propietario de Nvidia **no activa el modesetting automáticamente** por razones de compatibilidad con hardware muy antiguo.
> 
> Si no lo fuerzas, te encontrarías con varios problemas
> - **Pantallazo negro:** Hyprland ni siquiera arrancaría porque no encuentra un "buffer" donde dibujar la imagen.
> - **Tearing (Pantalla desgarrada):** Verías líneas horizontales al mover ventanas.
> - **Falta de aceleración:** El sistema no podría aprovechar la potencia de tu GPU correctamente desde el inicio.
> 
> Al meterlo en el `mkinitcpio.conf` y en los parámetros del bootloader, le estás diciendo al sistema: "Carga el driver de video antes que cualquier otra cosa y toma el control total de la pantalla de inmediato".

Luego, edita el archivo `mkinitcpio.conf`:

```bash
nano /etc/mkinitcpio.conf
```

Dentro busca la línea `MODULES=()` y pega:

```bash
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Guarda y escribe: 

```bash
mkinitcpio -P
```

### 10.2. Generación del mkinitcpio para gráficas AMD/Intel: 

Ejecutar únicamente: 

```bash
mkinitcpio -P 
```

---

<div align="center">

[← Anterior](arch-install-09-post-install.md) · [Índice](../README.md) · [Siguiente →](arch-install-11-usuario-sudo.md)

</div>
