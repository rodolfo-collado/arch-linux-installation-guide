## 11. Creación del usuario del sistema con permisos de `sudo`

- Edita el archivo `visudo` para habilitar los permisos `sudo` de los usuarios del sistema:

```bash
EDITOR=nano visudo
```

- Busca y descomenta. Después, guarda los cambios y sal del editor:

```bash
%wheel ALL=(ALL:ALL) ALL
```

- Crea el usuario y asígnale los grupos suplementarios necesarios. Sustituye `rudolf` por el nombre de usuario que prefieras:

```bash
useradd -m -s /bin/bash -G wheel,video,render,storage,input rudolf
```

| Elemento           | Función                                                                            |
| ------------------ | ---------------------------------------------------------------------------------- |
| `-m`               | Crea la carpeta personal del usuario, por ejemplo `/home/rudolf`.                  |
| `-s /bin/bash`     | Define Bash como la shell interactiva predeterminada.                              |
| `wheel`            | Permite utilizar `sudo` para realizar tareas administrativas.                      |
| `video` y `render` | Permiten acceder a funciones relacionadas con la GPU, como la aceleración gráfica. |
| `storage`          | Facilita el acceso a dispositivos de almacenamiento, como discos y memorias USB.   |
| `input`            | Permite acceder a dispositivos de entrada, como el teclado y el touchpad.          |

> [!TIP]
> Puedes crear más usuarios en el sistema. Si al momento de crearlos no los asignas al grupo `wheel`, no tendrán la posibilidad de ejecutar comandos que requieran un nivel de permisos más alto (con `sudo`). 

- Establecer la contraseña del usuario creado: 

```bash
passwd rudolf
```

> [!IMPORTANT]
> **Añadir un usuario existente a grupos adicionales**
> Si olvidaste añadir algún grupo al crear el usuario, ejecuta este comando dentro de `arch-chroot`, donde ya tienes permisos de `root`:
> 
> ```bash
> usermod -aG wheel,video,render,storage,input rudolf
> ```
> 
> **Partes del comando:**
> 
> | Elemento | Función |
> | --- | --- |
> | `usermod` | Modifica la configuración de un usuario existente. |
> | `-a` | Añade los grupos nuevos y conserva los grupos suplementarios actuales. Debe utilizarse junto con `-G`. |
> | `-G` | Indica la lista de grupos suplementarios que se asignará al usuario. |
> | `wheel,video,render,storage,input` | Grupos a los que se añadirá el usuario. Conserva únicamente los que necesites. |
> | `rudolf` | Usuario que se modificará. Sustitúyelo por el nombre real del usuario. |

### 11.1. Crear las carpetas estándar (Descargas, Documentos, etc.) del usuario y habilitar el servicio de red

- Instala el paquete de `xdg-user-dirs`:

```bash
pacman -S xdg-user-dirs
```

- Entra al usuario creado, cambia `rudolf` por el nombre real de tu usuario:

```bash
su - rudolf
```

- Una vez dentro, actualiza los directorios del usuario con este comando: 

```bash
xdg-user-dirs-update
```

- Comprueba que las carpetas se hayan creado correctamente:

```bash
ls
```

*Salida esperada* ↓

```bash
Descargas   Escritorio  Música      Proyectos  Vídeos
Documentos  Imágenes    Plantillas  Público
```

- Tras crear las carpetas, sal del usuario: 

```bash
exit 
```

- Por último, habilita el daemon `NetworkManager` para la red:  

```bash
systemctl enable NetworkManager
```

---

<div align="center">

[← Anterior](arch-install-10-mkinitcpio-gpu.md) · [Índice](../../../README.md) · [Siguiente →](arch-install-12-refind.md)

</div>
