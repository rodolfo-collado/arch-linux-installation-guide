## 02. Primer Booteo: 

La TTY (Teletypewritter) es la terminal con la que interactúas con el sistema operativo mediante comandos. Por defecto viene con la distribución de teclado `us` (US QWERTY) y un tamaño de fuente demasiado pequeño si tu monitor es muy grande o de alta resolución.

### 02.1. Cambiar  la distribución de teclado: 

Para listar la distribuciones de teclados disponibles usa el comando `localectl`:

```bash
localectl list-keymaps
```

los teclados en español más comunes son: 

- `es`: Español España (ISO).
- `la-latin1`: Español Latinoamericano.
- `us-acentos`: Inglés Internacional (con Dead Keys al presionar la tecla `'`).

Para cargar cualquiera de estas distribuciones en la TTY se usa el comando `loadkeys`:

```bash
loadkeys us-acentos #Este es la distribución de Teclado Inglés US Internacional (con deadkeys para acentos)
```

###  02.2. Cambiar tamaño de letra: 

la Live ISO de arch tiene instalado el paquete `terminus-font`, el cual nos permite cambiar el tamaño de letra de la terminal de forma muy sencilla. 

Para listar los tamaños de letra disponibles ejecuta: 

```bash
ls /usr/share/kbd/consolefonts/ | grep ter-v
```

> [!NOTE]
> **Nomenclatura de los tamaños de letra:**
>
Los nombres parecen código en clave, pero siguen esta lógica: **`ter-v[tamaño][estilo]`**
> - **v16, v24, v32:** Es la altura en píxeles.
> - **n (normal):** Fuente estándar.
> - **b (bold):** Fuente en negrita (más gruesa).
> 

Para aplicar cualquiera de esos tamaños de fuentes usa el comando `setfont`:

```bash
setfont ter-v22b
```

### 02.3. Verificación de Arranque (Legacy o UEFI):

El modo de arranque moderno y usado por todos los sistemas operativos actuales es **UEFI** (Unified Extensible Firmware Interface). Anteriormente se usaban arranques con el estándar *BIOS* (Basic Input/Output System), lo que actualmente conocemos como arranque **LEGACY**.  Esto es de crucial importancia para saber cómo vamos a particionar el disco (GPT para UEFI, MBR para Legacy). En esta guía se explicará el proceso de particionado bajo un arranque UEFI.

### 02.4. Verificar el modo de arranque de tu máquina (Legacy o UEFI):

Ejecuta este comando:

```bash
ls /sys/firmware/efi/efivars
```

**Si existe** → estás en UEFI.
**Si no existe** → Entraste a la iso en modo `legacy`. Para entrar en modo `UEFI` ve a la bios y selecciona la opción de arrancar desde la USB con `UEFI`

Luego de ello, tendría que existir la ruta de variables `EFI`

---

<div align="center">

[← Anterior](arch-install-01-overview.md) · [🏠 Índice](../README.md) · [Siguiente →](arch-install-03-internet.md)

</div>
