## 2. Primer Booteo

La TTY (Teletypewritter) es la terminal con la que interactúas con el sistema operativo mediante comandos. Por defecto viene con la distribución de teclado `us` (US QWERTY) y un tamaño de fuente demasiado pequeño si tu monitor es muy grande o de alta resolución.

### 2.1 Cambiar  la distribución de teclado

- Listar las distribuciones de teclados disponibles con el comando `localectl`.

```bash
localectl list-keymaps
```

>[!NOTE]
Distribuciones más comunes:
>
>- `es`: Español España (ISO).
>- `la-latin1`: Español Latinoamericano.
>- `us`: Inglés Estados Unidos.
>- `us-acentos`: Inglés Internacional (con Dead Keys al presionar la tecla `'`).

- Cargar una distribución de teclado en la terminal.

```bash
loadkeys us-acentos
```

###  2.2 Cambiar tamaño de letra

- Lista los tamaños de fuente disponibles.

```bash
ls /usr/share/kbd/consolefonts/ | grep ter-v
```

> [!NOTE]
> **Nomenclatura de los tamaños de letra:**
>
>Siguen esta lógica: **`ter-v[tamaño][estilo]`**
> - **v16, v24, v32:** Es la altura en píxeles.
> - **n (normal):** Fuente estándar.
> - **b (bold):** Fuente en negrita (más gruesa).

- Aplica el tamaño de fuente de tu preferencia con `setfont`.

```bash
setfont ter-v22b
```

### 2.3 Verificar el modo de arranque de tu máquina (LEGACY o UEFI)

- Verificar la existencia de las `efivars`.

```bash
ls /sys/firmware/efi/efivars
```

- *Si existe* → estás en `UEFI`.
- *Si no existe* → estás en `LEGACY`. Cambia las opciones de arranque del USB desde la BIOS a `UEFI`.

**Diferencias entre UEFI y LEGACY:**

El modo de arranque determina principalmente el método de instalación del bootloader y, en la mayoría de los casos, el tipo de tabla de particiones que conviene utilizar.

| Característica | `UEFI` | `LEGACY` |
|---|---|---|
| Estándar utilizado | `UEFI` (*Unified Extensible Firmware Interface*) | `BIOS` (*Basic Input/Output System*) |
| Tabla de particiones habitual | `GPT` | `MBR` |
| Partición de arranque | `EFI System Partition` en formato `FAT32` | Sector de arranque del disco |
| Instalación del bootloader | Dentro de la partición EFI | En el sector de arranque |

Para esta guía se utilizará la combinación moderna `UEFI + GPT`.

> [!CAUTION]
> No continuar hasta que la máquina esté en modo de arranque **UEFI**.

---

<div align="center">

[← Anterior](arch-install-01-overview.md) · [Índice](../README.md) · [Siguiente →](arch-install-03-internet.md)

</div>
