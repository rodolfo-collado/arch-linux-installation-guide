## 01. Preparativos: 

Como **requisitos indispensables** para una instalación limpia de Arch Linux necesitas:

- Una partición en disco marcada como "Unallocated space" o un disco completo disponible. También puedes usar un disco con particiones ya formateadas para el proceso, solo ten en cuenta que *la instalación borrará la información de las particiones seleccionadas permanentemente*.
- Iso de arch descargada en torrent ([Link](https://archlinux.org/download/))
- Paciencia. La instalación de Arch requiere conocimientos técnicos sobre el sistema operativo que aprenderás sobre la marcha con la ayuda de esta guía. En caso de que surga un error inesperado, date tu tiempo de investigar sobre su resolución. 

---
### 01.1. Recomendaciones:

- Tener una partición EFI (`formato FAT32 o vfat del inicio del sistema de particiones`  ) de 512 MB o superior. En la guía se brindará la opción de instalar el kernel (la parte más pesada del boot) directamente en la partición principal del sistema (`root`) en caso que no dispongas de una *partición EFI lo suficientemente grande*. Lo recomendable es usar herramientas como `Gparted Live ISO` con `Ventoy` para extender el tamaño de la partición EFI existente.
- Instala todos los paquetes que necesites en el sistema desde el primer comando de instalación (`pacstrap -K`)  para iniciar con tus herramientas y programas de preferencia.
- Por cada error que aparezca, se recomienda buscar en la web o en la [Documentación Oficial de Arch Linux](https://wiki.archlinux.org/title/Main_page), Una de las más completas de todo el ecosistema Linux.

---

<div align="center">

[← Índice](../README.md) · [Siguiente →](arch-install-02-live-iso.md)

</div>
