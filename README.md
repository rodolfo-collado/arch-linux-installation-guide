# Arch Linux · Guía de instalación

<div align="center">

![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=archlinux&logoColor=white)
![Guía en español](https://img.shields.io/badge/Gu%C3%ADa_en_espa%C3%B1ol-2ea44f?style=for-the-badge&logo=readme&logoColor=white)
![14 capítulos](https://img.shields.io/badge/14_cap%C3%ADtulos-8b5cf6?style=for-the-badge&logo=bookstack&logoColor=white)
![Markdown](https://img.shields.io/badge/Markdown-0f172a?style=for-the-badge&logo=markdown&logoColor=white)
[![Licencia: CC BY-SA 4.0](https://img.shields.io/badge/Licencia-CC_BY--SA_4.0-lightgrey?style=for-the-badge)](LICENSE.md)

### · overview ·

Guía base para instalar Arch Linux sobre BTRFS, configurar el sistema y dejar listo un entorno Hyprland.

</div>

> [!IMPORTANT]
> Esta guía está organizada como un recorrido lineal. Sigue los capítulos en orden y utiliza los controles de navegación al final de cada nota para avanzar como si fuera un libro.

## Índice

| # | Capítulo | Descripción |
|:--:|---|---|
| 01 | [Preparativos](docs/arch-install-01-overview.md) | Requisitos y recomendaciones iniciales |
| 02 | [Primer booteo](docs/arch-install-02-live-iso.md) | Arranque desde la Live ISO |
| 03 | [Conexión a Internet](docs/arch-install-03-internet.md) | Wireless, Ethernet, NAT y hora |
| 04 | [Particionado](docs/arch-install-04-particionado.md) | Tabla de particiones para BTRFS |
| 05 | [Formateo](docs/arch-install-05-formateo.md) | Formateo de EFI y raíz |
| 06 | [Subvolúmenes BTRFS](docs/arch-install-06-btrfs-subvolumenes.md) | Creación de `@`, `@home` y `@snapshots` |
| 07 | [Montaje](docs/arch-install-07-montaje.md) | Montaje del esquema de particiones |
| 08 | [Sistema base](docs/arch-install-08-pacstrap.md) | `pacstrap`, repositorios y paquetes |
| 09 | [Post-instalación](docs/arch-install-09-post-install.md) | `fstab`, `arch-chroot`, localización y red |
| 10 | [mkinitcpio y GPU](docs/arch-install-10-mkinitcpio-gpu.md) | Configuración según el hardware |
| 11 | [Usuario y sudo](docs/arch-install-11-usuario-sudo.md) | Usuario, grupos y permisos |
| 12 | [rEFInd](docs/arch-install-12-refind.md) | Instalación y configuración del bootloader |
| 13 | [Hyprland](docs/arch-install-13-hyprland.md) | Instalación del entorno gráfico |
| 14 | [Reinicio](docs/arch-install-14-reboot.md) | Salida, desmontaje y primer arranque |

## Ruta recomendada

```text
Preparativos → Live ISO → Internet → Particionado → Formateo
      ↓
Subvolúmenes BTRFS → Montaje → Pacstrap → Post-instalación
      ↓
mkinitcpio/GPU → Usuario/sudo → rEFInd → Hyprland → Reinicio
```

## Estructura

- [`docs/`](docs/): los 14 capítulos de la guía de instalación.
- [`assets/`](assets/): imágenes utilizadas dentro de las notas.

## Licencia

La documentación original de esta guía está licenciada bajo [CC BY-SA 4.0](LICENSE.md). Puedes compartirla y adaptarla siempre que atribuyas al autor, indiques los cambios y mantengas la misma licencia en las versiones modificadas.

Autor: [rodolfo-collado](https://github.com/rodolfo-collado)

Las capturas, proyectos y referencias de terceros conservan sus respectivas licencias y créditos.

## Alcance

Este repositorio contiene únicamente la guía base de instalación, desde los preparativos hasta el primer reinicio. La información técnica proviene de las notas originales de la guía y se mantiene su orden y contenido.

<div align="center">

### · instalación limpia · sistema base · hyprland ·

</div>
