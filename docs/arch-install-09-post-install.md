## 09. Configuración Post Instalación del Sistema Operativo + Paquetes del usuario: 

### 09.1. Generar`fstab` (File System Table) y Hacer Cambio de Raíz: 

 El `fstab` funciona como el "Mapa de navegación de discos del sistema". **Le indica al kernel cuáles son las particiones, discos o unidades de almacenamiento que tiene que montar en el boot del sistema**. Para generarlo automáticamente con las particiones ya montadas del sistema

```bash
genfstab -U /mnt > /mnt/etc/fstab
```

**Por qué:**  
- Usa UUID automáticamente.
- Asegura que los subvolúmenes se monten correctamente al boot.
- Permite entrar a la raíz creada con el comando `arch-chroot /mnt`

Para cambiar a la raiz creada escribe: 

```bash
arch-chroot /mnt # al final se escribe la ruta en donde están montados los subvolúmenes BTRFS
```

---
### 09.2. Configuración de localización, input y red dentro del `arch-chroot /mnt`:

#### 09.2.1. Zona Horaria: 

Listar las zonas horarias con el comando *timedatectl*:

```bash
timedatectl list-timezones
```

Si deseas filtrar por continente (ejemplo, América), escribe: 

```bash
timedatectl list-timezones | grep America
```

> [!NOTE]
> **Todo en linux es un archivo:**
>Para ver los archivos de las zonas horarias de manera manual sin el comando **timedatectl** ejecuta: 
>```bash
>ls /usr/share/zoneinfo/
>```

Usar la base de datos de la zona horaria de Managua (o la de preferencia) como la hora local de la máquina. Esto se hace mediante un enlace simbólico entre el archivo de la zona horaria y el `localtime` con el comando *ln*:

```bash
ln -sf /usr/share/zoneinfo/America/Managua /etc/localtime
```

Para tomar la hora actual del sistema operativo (la que acabamos de configurar) y escribirla en el reloj físico de la BIOS ejecutamos: 

```bash
hwclock --systohc
```

> [!TIP]
> **Consejo para Dual Boot con windows:**
> Si usas el comando `hwclock --systohc`, el sistema asume por defecto que la BIOS debe estar en **UTC**. Esto es perfecto para Linux, pero si Windows te muestra la hora mal al cambiar de sistema, recuerda que es porque Windows espera que la BIOS esté en "Local Time". Es mejor dejar la BIOS en UTC y configurar Windows después para que lo entienda en las opciones "Date & Local Time"

### 09.3. Generación del archivo `/etc/locale.conf`: 

En este archivo se configura desde la configuración y la distribución de teclado que quieras usar, hasta el calendario por defecto, uso de "," o "." para delimitar decimales, unidad de medida de temperatura predeterminada, etc. Nos vamos a centrar en lo importante que es la `codificación y la distribución de teclados`:

Entrar al `locale.gen`:

```bash
nano /etc/locale.gen
```

Descomentar:

```
en_US.UTF-8 UTF-8
```

Guardar y generar el archivo de configuración con el comando: 

```bash
locale-gen
```

Creamos la variable `LANG=[CODIFICACIÓN DESCOMENTADA]` dentro del archivo generado. Se puede hacer con `echo` o editando manualmente  con `nano`:

```bash
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

### 09.4. Configuración del teclado en consola + Letra grande permanente:

Comandos con `echo`. También se puede editar de manera manual con editores de texto: 

```bash
echo -e "KEYMAP=us-acentos\nFONT=ter-132b" > /etc/vconsole.conf
```

### 09.5. Crear Hostname (nombre de la máquina)

Puedes crearla añadiendo el nombre que desees al archivo `etc/hostname` con `echo` o manualmente con `nano`:

```bash
echo "hyprbox" > /etc/hostname
```

Luego, editar el archivo en la ruta `/etc/hosts` para añadir la dirección **IP local que le permita a la máquina saber cuál es su nombre** sin tener que preguntarle a un DNS externo.

Ejecutar: 

```bash
nano /etc/hosts
```

contenido típico dentro de `/etc/hosts`:

```bash
127.0.0.1   localhost
::1         localhost
```

añadir la dirección local con el nombre de la máquina asignado anteriormente en el archivo `/etc/hostname`:

```bash
127.0.0.1   localhost
::1         localhost
127.0.1.1   hyprbox.localdomain hyprbox #Usar el nombre asignado por el user
```

---

<div align="center">

[← Anterior](arch-install-08-pacstrap.md) · [Índice](../README.md) · [Siguiente →](arch-install-10-mkinitcpio-gpu.md)

</div>
