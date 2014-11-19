# Configurar en PCs de la red

## w$:

### Shares de Samba

Equipo -> Herramientas -> Conectar a unidad de red...

    Unidad  Carpeta
    M:      \\zentyal\map
    P:      \\zentyal\Expedientes
    Q:      \\zentyal\Planos
    R:      \\zentyal\Relevamientos
    S:      \\zentyal\Software
    T:      \\zentyal\set
    V:      \\zentyal\varios

Poner que conecte al inicio.
Poner permisos para todos en todas las unidades.

## GNU/Linux:

### Shares de Samba

    $ sudo vim /etc/fstab

Agregar al final:

    //zentyal.estudio.lan/expedientes       /mnt/expedientes        cifs  uid=<linux-user>,username=dadmin,password=<password>,iocharset=utf8,sec=ntlm  0  0
    //zentyal.estudio.lan/map               /mnt/map                cifs  uid=<linux-user>,username=dadmin,password=<password>,iocharset=utf8,sec=ntlm  0  0
    //zentyal.estudio.lan/planos            /mnt/planos             cifs  uid=<linux-user>,username=dadmin,password=<password>,iocharset=utf8,sec=ntlm  0  0
    //zentyal.estudio.lan/relevamientos     /mnt/relevamientos      cifs  uid=<linux-user>,username=dadmin,password=<password>,iocharset=utf8,sec=ntlm  0  0
    //zentyal.estudio.lan/software          /mnt/software           cifs  uid=<linux-user>,username=dadmin,password=<password>,iocharset=utf8,sec=ntlm  0  0
    //zentyal.estudio.lan/varios            /mnt/varios             cifs  uid=<linux-user>,username=dadmin,password=<password>,iocharset=utf8,sec=ntlm  0  0

donde:

- <linux-user> es el nombre del usuario de esa PC (ej: santi, alberto)
- <password> es la password del usuario dadmin de Zentyal

#### Para usar el pen en una ruta fija

    # /dev/sdb1
    UUID=C03B-52D8 /media/<linux-user>/pen vfat defaults,auto,umask=000,users,rw 0 0

y para automount::

    # echo 'SUBSYSTEM=="block", RUN+="/bin/mount -a"' >> /etc/udev/rules.d/99-mount.rules

