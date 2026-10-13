## 9. Configuración postinstalación del sistema base

### 9.1. Generar `fstab` y cambiar la raíz

- `fstab` es el archivo que indica qué sistemas de archivos debe montar el sistema durante el arranque. Genéralo a partir de las particiones montadas:

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

>La opción `-U` utiliza los identificadores universalmente únicos (UUID, por sus siglas en Inglés) de las particiones en lugar de sus nombres de dispositivo.

- Después, cambia la raíz de trabajo a la del sistema instalado:

```bash
arch-chroot /mnt
```

> [!CAUTION]
>Desde este momento, los comandos se ejecutan dentro del sistema instalado.

### 9.2. Configurar la zona horaria

- Lista las zonas horarias disponibles:

```bash
timedatectl list-timezones
```

- Para filtrar la lista por continente (por ejemplo América), utiliza:

```bash
timedatectl list-timezones | grep America
```

- También puedes consultar directamente la base de datos de zonas horarias:

```bash
ls /usr/share/zoneinfo/
```

- Establece la zona horaria de tu preferencia mediante un *enlace simbólico*. Este ejemplo utiliza `America/Managua`:

```bash
ln -sf /usr/share/zoneinfo/America/Managua /etc/localtime
```

> [!NOTE]
> **¿Qué es un enlace simbólico?**
> 
> Es un archivo que apunta a otro archivo o directorio. En este caso, `/etc/localtime` apunta a la zona horaria elegida dentro de `/usr/share/zoneinfo/`, permitiendo que el sistema utilice esa configuración sin duplicar el archivo original.

- Para verificar que el enlace simbólico apunta a la zona horaria escogida, usa el comando `readlink -f`:

```bash
readlink -f /etc/localtime
```

*Salida esperada* → `/usr/share/zoneinfo/America/Managua`

> [!TIP]
>**Deshacer enlace simbólico**
>- Usa el comando `unlink` para deshacer el enlace simbólico que apunta a la zona horaria que deseas cambiar:
>
>```bash
>unlink /etc/localtime
>```

- Sincroniza el reloj físico de la `BIOS` con la hora del sistema anteriormente configurada:

```bash
hwclock --systohc
```

> [!TIP]
> **Dual boot con Windows:**
> Linux espera que el reloj de la BIOS utilice UTC. Si Windows muestra una hora incorrecta al cambiar de sistema, configúralo para interpretar el reloj de hardware como UTC. Mantener la BIOS en UTC evita problemas entre ambos sistemas.

### 9.3. Configurar los locales

Los `locales` definen la codificación de texto, el idioma y otros formatos regionales del sistema. La configuración se realiza en tres pasos:

| Elemento | Función |
| --- | --- |
| `/etc/locale.gen` | Define qué locales estarán disponibles. |
| `locale-gen` | Genera los locales habilitados en `/etc/locale.gen`. |
| `/etc/locale.conf` | Define el locale predeterminado del sistema. |

- Edita `/etc/locale.gen`:

```bash
nano /etc/locale.gen
```

- Descomenta el locale que quieras utilizar. Por ejemplo:

```text
es_NI.UTF-8 UTF-8
```

- Guarda los cambios y genera los locales habilitados:

```bash
locale-gen
```

- Por último, establece el locale predeterminado en `/etc/locale.conf`:

```bash
echo "LANG=es_NI.UTF-8" > /etc/locale.conf
```

### 9.4. Configurar el teclado y la fuente de la consola

- Define la distribución del teclado y una fuente más grande para la consola. Sustituye `us-acentos` y `ter-122b` si prefieres otros valores:

```bash
echo -e "KEYMAP=us-acentos\nFONT=ter-122b" > /etc/vconsole.conf
```

### 9.5. Configurar el hostname

- Establece el nombre de la máquina. Sustituye `archlinux` por el nombre que prefieras:

```bash
echo "archlinux" > /etc/hostname
```

- Después, edita `/etc/hosts`:

```bash
nano /etc/hosts
```

- Añade el contenido siguiente y sustituye `archlinux` por el hostname elegido. Al terminar, guarda los cambios y sal de `nano`:

```text
127.0.0.1   localhost
::1         localhost
127.0.1.1   archlinux.localdomain archlinux
```

---

<div align="center">

[← Anterior](arch-install-08-pacstrap.md) · [Índice](../../../README.md) · [Siguiente →](arch-install-10-mkinitcpio-gpu.md)

</div>
