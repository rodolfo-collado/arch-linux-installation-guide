## 08. Instalación del Sistema Base:

### 08.1. Activación del repositorio `multilib`:

Para una instalación de paquetes completa mediante `pacstrap -K` primero se tiene que editar el  archivo en la ruta `/etc/pacman.conf`:

```bash
nano /etc/pacman.conf
```

Dentro del archivo, se descomentan las líneas: 

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist # descomentar es quitar el '#' del inicio
```

> [!IMPORTANT]
> **Rol del repositorio `multilib` en el sistema:**
> Esto activa el repositorio de `multilib`, crucial para dotar al sistema operativo de la capacidad de compilar y ejecutar aplicaciones de 32 bits. Por defecto, Arch es estrictamente un sistema operativo de 64 bits. Por lo que al activar `multilib` le descargas al sistema una capa de librerías secundarias que le permiten a tu procesador moderno traducir y ejecutar ese código antiguo de manera nativa y sin problemas.

Luego, busca la siguientes líneas y descoméntalas en el mismo archivo:

```bash
Color
ILoveCandy #si no está, lo escribes abajo de Color
CheckSpace
ParallelDownloads = 5
```

> [!NOTE]
> **¿Qué hace cada cosa?**
>  - **`Color`**: Te da una mejor visibilidad para distinguir versiones y nombres de paquetes.
> - **`ParallelDownloads`**: ¡Vital para tu fibra óptica! Descarga 5 paquetes al mismo tiempo en lugar de uno por uno.
> - **`ILoveCandy`**: Convierte la barra de progreso aburrida en un Pac-Man animado.

Guardar el archivo y ejecuta este comando en la terminal: 

```bash
pacman -Sy #Actualiza el gestor de paquetes para descargar Multilibs
```

### 08.2. Comando pacstrap -K e instalación completa del sistema:

> [!IMPORTANT]
> **Lo esencial que no puede faltar dentro del `pacstrap -K` es:**
> - **Sistema base:** `base linux linux-headers linux-firmware` 
> - **Drivers de procesadores:** `intel-ucode` / `amd-ucode` 
> - **Gráficas Intel e integradas:** `mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver vulkan-mesa-layers lib32-vulkan-mesa-layers`
> - **Gráficas AMD e integradas:** `mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon libva-mesa-driver lib32-libva-mesa-driver`  
> - **Gráficas Nvidia RTX:** `nvidia-dkms nvidia-utils nvidia-settings libva-nvidia-driver egl-wayland` 
> - **Sistema de archivos:** `btrfs-progs` 
> - **Audio:** `pipewire pipewire-pulse pipewire-alsa pipewire-jack lib32-pipewire rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf` 
> - **Terminal:** `kitty`
> - **Bluetooth y WIFI:** `bluez bluez-utils networkmanager network-manager-applet` 
> - **Extras indispensables:** `git base-devel nvim nano terminus-font refind libva-utils tar zip unzip p7zip ark` 

> [!TIP]
> **Opcionales pero recomendados:**
> - **Lectores de archivos:** `zathura zathura-pdf-mupdf imv mpv` 
> - **Exploradores de archivos en terminal y ventana:** `yazi thunar` 
> - **Navegador:** `firefox`
> - **Reproductor:** `strawberry`
> - **Notas:** `obsidian`
> - **Gamemodes y extras de juegos:** `gamemode lib32-gamemode steam hidapi steam-devices`

#### 08.2.1. Pacstrap personalizado  (Thinkpad T14 gen 2 intel) + programas/binarios del usuario:

```bash
pacstrap -K /mnt base linux linux-headers linux-firmware intel-ucode mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver vulkan-mesa-layers lib32-vulkan-mesa-layers btrfs-progs pipewire pipewire-pulse pipewire-alsa pipewire-jack rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf kitty bluez bluez-utils networkmanager network-manager-applet git base-devel nvim nano terminus-font refind libva-utils tar zip unzip p7zip ark readest obsidian strawberry firefox gamemode lib32-gamemode steam hidapi steam-devices thunar yazi zathura zathura-pdf-mupdf imv mpv
```

> [!NOTE]
> **¿Qué instala este comando?**
>- Sistema base y kernel.
> - Drivers del procesador y gráfica Intel.
> - Herramientas de administrador de discos en formato BTRFS.
> - Drivers de audio
> - Drivers bluetooth
> - Drivers WIFI
> - Herramientas esenciales del sistema (terminal, GIT, editores de texto en terminal, Boot Manager,  exploradores de archivos, programas para reproducción de audio, video y documentos por defecto)
> - Steam y drivers necesarios para reconocer mandos. 
> - Apps del usuario (Obsidian, firefox, Strawberry, readest, etc).

---

<div align="center">

[← Anterior](arch-install-07-montaje.md) · [🏠 Índice](../README.md) · [Siguiente →](arch-install-09-post-install.md)

</div>
