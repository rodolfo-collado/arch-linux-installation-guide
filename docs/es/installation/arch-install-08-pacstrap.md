## 8. Instalación del Sistema Base

### 8.1. Activar el repositorio `multilib`

- Para instalar paquetes de 32 bits, activa el repositorio `multilib` editando `/etc/pacman.conf`:

```bash
nano /etc/pacman.conf
```

- Descomenta estas líneas eliminando el símbolo `#` al inicio:

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist
```

> Eliminar el símbolo `#` al inicio de una línea se conoce como **descomentar**.

> [!IMPORTANT]
> **Rol del repositorio `multilib`:**
> `multilib` proporciona versiones de 32 bits de algunas bibliotecas y aplicaciones. Es necesario para ejecutar determinados programas, como juegos y aplicaciones antiguas.

- Luego, busca estas líneas y descoméntalas en el mismo archivo:

```bash
Color
ILoveCandy #si no está, lo escribes abajo de Color
CheckSpace
ParallelDownloads = 5
```

> [!NOTE]
> **¿Qué hace cada opción?**
> - `Color`: mejora la visibilidad de la salida de `pacman`.
> - `ParallelDownloads`: permite descargar varios paquetes simultáneamente.
> - `ILoveCandy`: añade una animación de Pac-Man a la barra de progreso.

- Guarda el archivo con `CTRL + O`, confirma con `ENTER` y sal del editor con `CTRL + X`. Después, actualiza las bases de datos de los repositorios:

```bash
pacman -Sy
```

### 8.2. Instalar el sistema base con `pacstrap -K`

`pacstrap -K` instala los paquetes seleccionados dentro del sistema montado en `/mnt`. Utiliza esta tabla como referencia para construir el comando según tu hardware y tus necesidades:

| Función                     | Paquetes necesarios                                                                                                             |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Sistema base                | `base linux linux-headers linux-firmware`                                                                                       |
| Microcódigo del procesador  | `intel-ucode` o `amd-ucode`                                                                                                     |
| Gráficos Intel              | `mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver vulkan-mesa-layers lib32-vulkan-mesa-layers`                |
| Gráficos AMD                | `mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon libva-mesa-driver lib32-libva-mesa-driver`                                   |
| Gráficos NVIDIA RTX         | `nvidia-dkms nvidia-utils nvidia-settings libva-nvidia-driver egl-wayland`                                                      |
| Sistema de archivos (BTRFS) | `btrfs-progs`                                                                                                                   |
| Audio                       | `pipewire pipewire-pulse pipewire-alsa pipewire-jack lib32-pipewire rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf` |
| Terminal                    | `kitty`                                                                                                                         |
| Bluetooth y Wi-Fi           | `bluez bluez-utils networkmanager network-manager-applet`                                                                       |
| Bootloader                  | `refind`                                                                                                                        |
| Herramientas esenciales     | `git base-devel nvim nano terminus-font libva-utils tar zip unzip p7zip ark`                                                    |

También puedes añadir estos paquetes opcionales:

| Función                  | Paquetes opcionales                                  |
| ------------------------ | ---------------------------------------------------- |
| Multimedia y lectores    | `readest zathura zathura-pdf-mupdf imv mpv`           |
| Exploradores de archivos | `yazi thunar`                                        |
| Navegador web            | `firefox`                                            |
| Reproductor de música    | `strawberry`                                         |
| Notas                    | `obsidian`                                           |
| Juegos y mandos          | `gamemode lib32-gamemode steam hidapi steam-devices` |

#### 8.2.1. Ejemplos de comandos `pacstrap -K` personalizados

> [!NOTE]
> **Comandos en varias líneas:**
> El carácter `\` al final de una línea indica que el comando continúa en la siguiente. Puedes copiar y pegar los ejemplos completos tal como aparecen o escribirlos en una sola línea eliminando las barras invertidas y los saltos de línea. No dejes ningún espacio después de `\`.

##### a) ThinkPad T14 Gen 2 (Intel)

```bash
pacstrap -K /mnt \
  base linux linux-headers linux-firmware \
  intel-ucode \
  mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver vulkan-mesa-layers lib32-vulkan-mesa-layers \
  btrfs-progs \
  pipewire pipewire-pulse pipewire-alsa pipewire-jack lib32-pipewire rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf \
  kitty \
  bluez bluez-utils networkmanager network-manager-applet \
  refind \
  git base-devel nvim nano terminus-font libva-utils tar zip unzip p7zip ark \
  readest zathura zathura-pdf-mupdf imv mpv \
  yazi \
  firefox \
  strawberry \
  obsidian
```
##### b) ThinkPad T14 Gen 2 (Intel) con Steam y controladores para mandos

```bash
pacstrap -K /mnt \
  base linux linux-headers linux-firmware \
  intel-ucode \
  mesa lib32-mesa vulkan-intel lib32-vulkan-intel intel-media-driver vulkan-mesa-layers lib32-vulkan-mesa-layers \
  btrfs-progs \
  pipewire pipewire-pulse pipewire-alsa pipewire-jack lib32-pipewire rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf \
  kitty \
  bluez bluez-utils networkmanager network-manager-applet \
  refind \
  git base-devel nvim nano terminus-font libva-utils tar zip unzip p7zip ark \
  readest zathura zathura-pdf-mupdf imv mpv \
  yazi thunar \
  firefox \
  strawberry \
  obsidian \
  gamemode lib32-gamemode steam hidapi steam-devices
```

##### c) PC con Ryzen y NVIDIA RTX

```bash
pacstrap -K /mnt \
  base linux linux-headers linux-firmware \
  amd-ucode \
  nvidia-dkms nvidia-utils nvidia-settings libva-nvidia-driver egl-wayland \
  btrfs-progs \
  pipewire pipewire-pulse pipewire-alsa pipewire-jack lib32-pipewire rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf \
  kitty \
  bluez bluez-utils networkmanager network-manager-applet \
  refind \
  git base-devel nvim nano terminus-font libva-utils tar zip unzip p7zip ark \
  readest zathura zathura-pdf-mupdf imv mpv \
  yazi thunar \
  firefox \
  strawberry \
  obsidian
```

##### d) PC con Ryzen y Nvidia RTX con Steam y controladores para mandos

```bash
pacstrap -K /mnt \
  base linux linux-headers linux-firmware \
  amd-ucode \
  nvidia-dkms nvidia-utils nvidia-settings libva-nvidia-driver egl-wayland \
  btrfs-progs \
  pipewire pipewire-pulse pipewire-alsa pipewire-jack lib32-pipewire rtkit wireplumber sof-firmware alsa-firmware alsa-ucm-conf \
  kitty \
  bluez bluez-utils networkmanager network-manager-applet \
  refind \
  git base-devel nvim nano terminus-font libva-utils tar zip unzip p7zip ark \
  readest zathura zathura-pdf-mupdf imv mpv \
  yazi thunar \
  firefox \
  strawberry \
  obsidian \
  gamemode lib32-gamemode steam hidapi steam-devices
```

---

<div align="center">

[← Anterior](arch-install-07-montaje.md) · [Índice](../../../README.md) · [Siguiente →](arch-install-09-post-install.md)

</div>
