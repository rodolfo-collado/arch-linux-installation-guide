## 11. Creación del Usuario del Sistema (Con Permisos `sudo`):

Primero, entrar al archivo visudo para habilitar permisos sudo:

```bash
EDITOR=nano visudo
```

Descomentar: 

```bash
%wheel ALL=(ALL:ALL) ALL
```

Luego, crea el usuario desde la terminal y mételo al grupo wheel (grupo de los que pueden usar `sudo`): 

```bash
useradd -m -G wheel,video,render,storage,input rc # El 'rc' es el nombre de ejemplo, cambiar al gusto
```

> [!NOTE]
> **Explicación de grupos y Banderas**
> Al crear el usuario, le asignamos "poderes" específicos sobre el hardware:
> 
> * **`-m`**: Crea la carpeta personal `/home/rc`.
> * **`-s /bin/bash`**: Define Bash como tu terminal interactiva por defecto.
> * **`wheel`**: Permite usar `sudo` para tareas administrativas.
> * **`video` & `render`**: Necesarios para el control de brillo y aceleración de la GPU.
> * **`storage`**: Permite montar discos y USBs sin pedir contraseña constantemente.
> * **`input`**: Mejora la respuesta de teclados y gestos en el touchpad de la laptop.
> *  **`-s /bin/bash`**: Define la "Shell" (intérprete de comandos) por defecto. Sin esto, el sistema podría asignarte `/bin/sh`, que es una terminal muy limitada, sin colores, sin autocompletado con tabulador y sin soporte para tus alias de `~/.bashrc`. Especificarlo asegura que al abrir tu terminal en Hyprland, tengas todo el poder de Bash disponible.

Setear contraseña de ese usuario: 

```bash
passwd rc
```

> [!IMPORTANT]
> **Añadir un user ya creado a diferentes grupos**
> Este comando debe ejecutarse dentro de un usuario:
> ```bash
> sudo usermod -aG video,render,storage,input rc
> ```
> 
> ### Explicación de la banderas `-aG`:
> - **`-a` (append):** Esta es la más importante. Significa "añadir". Si se te olvida la `-a`, el sistema **borrará** al usuario de todos sus grupos actuales (como `wheel`) y lo dejará solo en el nuevo.
> - **`-G` (groups):** Indica que lo que sigue es una lista de grupos suplementarios.

### 11.1. Crear las carpetas estándar (Downloads, Documents, etc) del usuario y habilitar red

Para ello, ejecuta: 

```bash
pacman -S xdg-user-dirs
```

Luego de la instalación, entra al usuario creado:

```bash
su - rc
```

Ejecuta este comando: 

```bash
xdg-user-dirs-update
```

Tras crear las carpetas, salir del usuario para continuar con la configuración: 

```bash
exit 
```

Para habilitar la red wireless, ejecuta este comando: 

```bash
systemctl enable NetworkManager
```

---

<div align="center">

[← Anterior](arch-install-10-mkinitcpio-gpu.md) · [🏠 Índice](../README.md) · [Siguiente →](arch-install-12-refind.md)

</div>
