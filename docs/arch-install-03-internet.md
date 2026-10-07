## 3. Conectarse a Internet

Hay varias maneras de conectarse a la red dentro de la TTY de Arch Linux. Las más comunes se enlistan a continuación: 

### 3.1. Conexión Wireless (Inalámbrica): 

Para ello, usamos el comando `iwctl` (iNet Wireless Control). Ejecuta este comando en la TTY de la live ISO: 

```bash
iwctl
```

Entrarás en el modo interactivo del programa originalmente desarollado por intel. Luego lista los dispositivos: 

```bash
device list
```

Esto lista las tarjetas de conexión inalámbrica de la máquina, comúnmente las máquinas de uso personal tienen una única tarjeta de red y se le asigna el nombre de *wlan0* (Wireless LAN 0). Vamos a usarla para **escanear las redes  y actualizar la lista de redes disponibles**: 

```bash
station wlan0 scan
```

Con la lista de redes disponibles actualizada, vamos a mostrarlas con: 

```bash
station wlan0 get-networks
```

Nos conectamos a la red de preferencia con el siguiente comando (nos pedirá la contraseña en caso de tenerla):

```bash
station wlan0 connect "NOMBRE_RED"
```

### 3.2. Conexión Via Ethernet (Cableada):

Si tienes el cable conectado, Arch intenta levantar la red automáticamente mediante **dhcpcd**. para verificar si ya tienes internet, simplemente haz un ping: 

```bash
ping -c 3 ping.archlinux.org
```

Si no responde, intenta forzar la solicitud de IP con el comando:

```bash
systemctl start dhcpcd
```

### 3.3. Conexión en máquina Virtual con NAT (Network Address Translation):

Para activar la conexión a wifi directamente dentro una VM con traducción de IP mediante NAT, tienes que seguir una configuración un tanto diferente usando el comando `dhcpcd`. Primero ejecuta este *comando para hacer una lista de todas las conexiones actuales*:

```bash
ip link
```

Normalmente enlistará 2 conexiones: 

```bash
1. lo ...
2. enp1s0 ...
```

> [!IMPORTANT]
> **Explicación de las redes `lo`, `enp1s0`:**
>#### lo (Loopback Interface):
>Es una interfaz virtual de "bucle local", es una dirección que apunta a la propia máquina (`127.0.0.1`). Sirve para que los programas se comuniquen entre sí dentro de Arch sin necesidad de salir a internet. Si esta interfaz está caída, servicios como las bases de datos (como PostgreSQL) o incluso el servidor gráfico puede fallar.
>
>#### enp1s0 (Ethernet Interface)
>Es la tarjeta de red "Cableada" virtual. El nombre se desglosa así: 
>- `en`: Ethernet
>- `p1`: Bus PCI 1
>- `s0`: Slot 0
>  
>  Este es el nombre estandar que asigna el kernel para que no cambie de forma aleatoria entre reinicios. Si dice `state UP` significa que el "cable virtual" está conectado, pero aún no tienes internet porque no tienes una dirección IP asignada por el router de la VM. 
>

Para configurar la conexión a internet vamos a seleccionar la conexión 2. Para ello ejecuta el comando: 

```bash
dhcpcd enp1s0
```

### 3.4. Verificar la sincronización de la hora:

Una vez conectado, asegúrate de que el reloj esté sincronizado para que no te den error las firmas de los paquetes: 

```bash
timedatectl set-ntp true
```

Para comprobar que el comando anterior haya surtido efecto, ejecuta: 

```bash
timedatectl status
```

- tiene que salir algo como *"System clock synchronized: yes"*.

---

<div align="center">

[← Anterior](arch-install-02-live-iso.md) · [Índice](../README.md) · [Siguiente →](arch-install-04-particionado.md)

</div>
