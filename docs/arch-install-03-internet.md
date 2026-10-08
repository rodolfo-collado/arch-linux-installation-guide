## 3. Conexión a Internet y sincronización de la hora

### 3.1. Conexión inalámbrica (Wi-Fi)

- Ejecuta el comando `iwctl` (iNet Wireless Control):

```bash
iwctl
```

- Lista los dispositivos de red e identifica la interfaz inalámbrica:

```bash
device list
```

- En los siguientes comandos, reemplaza `wlan0` por el nombre que aparezca en tu sistema y escanea las redes disponibles:

```bash
station wlan0 scan
```

- Muestra las redes encontradas:

```bash
station wlan0 get-networks
```

- Conéctate a la red deseada escribiendo su nombre exacto entre comillas dobles (`"`). Se solicitará la contraseña si es necesaria:

```bash
station wlan0 connect "NOMBRE_RED"
```

- Sal de `iwctl`:

```bash
exit
```

- Verifica la conexión:

```bash
ping -c 3 google.com
```

### 3.2. Conexión vía Ethernet (cableada)

- La Live ISO configura automáticamente las conexiones Ethernet mediante `systemd-networkd` y `DHCP`. Verifica si la red está activa:

```bash
ping -c 3 ping.archlinux.org
```

- Si no responde, comprueba el estado de las interfaces y de la red:

```bash
ip link
```

```bash
networkctl status
```

- Si la interfaz Ethernet aparece desactivada o sin dirección IP, reemplaza `<INTERFAZ>` por su nombre y ejecuta:

```bash
ip link set <INTERFAZ> up
```

```bash
systemctl restart systemd-networkd
```

```bash
networkctl renew <INTERFAZ>
```

- Comprueba nuevamente el estado y la conexión:

```bash
networkctl status <INTERFAZ>
```

```bash
ping -c 3 ping.archlinux.org
```

### 3.3. Conexión en máquina Virtual con NAT (Network Address Translation)

- Lista las interfaces de red disponibles dentro de la VM:

```bash
ip link
```

- Normalmente verás una interfaz de loopback y otra Ethernet. En este ejemplo:

```bash
1. lo ...
2. enp1s0 ...
```

**Explicación de las interfaces `lo` y `enp1s0`:**

| Interfaz | Tipo     | Función                                                             | Detalles                                                                                                                            |
| -------- | -------- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `lo`     | Loopback | Permite la comunicación interna de la máquina mediante `127.0.0.1`. | No necesita conexión a Internet. Si está caída, algunos servicios locales podrían fallar.                                           |
| `enp1s0` | Ethernet | Interfaz de red cableada, física o virtual. | `en` = Ethernet, `p1` = bus PCI 1, `s0` = slot 0. `state UP` indica que está activa, pero todavía podría no tener una dirección IP. |

> El nombre de la interfaz Ethernet puede variar según el hardware o la máquina virtual.

- Comprueba el estado de la interfaz Ethernet. Sustituye `enp1s0` por el nombre que aparezca en tu VM:

```bash
networkctl status enp1s0
```

- Si la conexión NAT no se configura automáticamente, reinicia `systemd-networkd` y renueva la dirección IP. Sustituye `enp1s0` por el nombre real de la interfaz si es diferente:

```bash
systemctl restart systemd-networkd
```

```bash
networkctl renew enp1s0
```

- Comprueba el estado de la interfaz:

```bash
networkctl status enp1s0
```

- Verifica la conexión con un ping sencillo:

```bash
ping -c 3 google.com
```

### 3.4. Sincronización de hora

- Tras confirmar la conexión, sincroniza la hora para evitar errores en la firma de los paquetes del sistema:

```bash
timedatectl set-ntp true
```

- Comprueba si la sincronización ha sido exitosa:

```bash
timedatectl status
```

*Salida esperada* → System clock synchronized: yes

---

<div align="center">

[← Anterior](arch-install-02-live-iso.md) · [Índice](../README.md) · [Siguiente →](arch-install-04-particionado.md)

</div>
