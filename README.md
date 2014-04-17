zentyal-installation
====================

El proceso de instalación de mi Zentyal

# INSTALACIÓN ZENTYAL 3.4

## Boot
- Cuando se enciende la PC presionar F11 para seleccionar de qué dispositivo bootear.
- ELEGIR _Install Zentyal 3.4 (expert mode)_

## Interfaz eth
Interfaz usada para instalar: eth1 (Sundance)
(la de más abajo, interfaz externa enchufada al modem)

## Nombre PC y user
- nombre PC: zentyal
- user: santi
- pass: *****
- cifrar carpeta personal: No

## RAID por software
- **Manual**
- Crear particiones **indénticas**
    - 50GB / con marca de BOOT, primaria, al principio, ext4
    - 4GB de swap, lógica, al final
    - max en /home, lógica, ext4
- Crear RAID
    1. crear md RAID1
    2. nro dispositivos: 2
    - Repetir 2 pasos anteriores con 2 particiones ext4 y luego con 2 particiones swap
- Configurar RAID 
    - RAID 50GB:  ext4, formatear SI, en /
    - RAID 446GB: ext4, formatear SI, en /home
    - RAID 4GB:   swap
- Elegir que bootee degradado (si se rompe un disco que bootee igual)

## Configurando apt
Datos de proxy: vacío

## GRUB
- instalar GRUB: SI

## Reloj UTC
- SI

## Instalar componentes
Luego de reiniciar, continuar instalando...
- Gateway (Puerta de enlace)
- Infraestructure (Infraestructura)
- Office (Oficina)
+ Paquetes
    - Backup
    - UPS Management

# CONFIGURACIÓN

## Core

### Sistema

#### Copia de seguridad

##### Configuración y Estado

Antes, crear el direcotrio en home:

    $ sudo mkdir -p /home/backups/estudio-data

Luego,

    Método:              Sistema de ficheros
    Ordenador o destino: /home/backups/estudio-data
    Cifrado:             Deshabilitado
    Frecuencia de la copia de seguridad completa: Semanalmente (en Viernes)
    Frecuencia de backup incremental:             Diariamente
    El proceso de respaldo comienza a las:        22:00
    Guardar copias completas anteriores:          número máximo (1)

##### Inclusiones y Exclusiones

    Excluir por expresión regular	/home/samba/shares/*/RecycleBin
    Incluir ruta	/home/samba/shares
    Excluir ruta	/srv/ftp/varios
    Incluir ruta	/srv/ftp

### Mantenimiento

#### SAI

- UPS label: RS800
- Descripción: Back-UPS RS 800
- Driver: APC - Back-UPS RS USB - usbhid-ups
- Puerto: Autodetect
- Serial number: QB0506134358

Guardar, iniciar módulo e ir a Configuración ... **FALTA** ver por qué no arranca bien

### Red

#### Interfaces
- eth0, estático, 10.0.0.1 / 255.255.255.0
- eth1, DHCP, WAN

#### DNS
- 127.0.0.1
- X.X.X.X
    - (agregar a los que estén, la IP de un DNS server conocido, por ej: 8.8.8.8, el DNS de Google)

## Infraestructure

### DHCP

(Activar módulo DHCP, luego entrar en eth0)

    Puerta de enlace predeterminada: Zentyal
    Dominio de búsqueda:             Ninguno
    Servidor de nombres primario:    DNS local de Zentyal
    Servidor de nombres secundario:  (si no anda así, poner acá 8.8.8.8)
    Servidor NTP:                    Ninguno
    Servidor WINS:                   Ninguno

#### Rangos DHCP

Agregar Rangos:

- Estudio: 10.0.0.10 a 10.0.0.25

### DNS

Habilitar el caché de DNS transparente: False

#### Redireccionadores

agregar 8.8.8.8

#### Dominios

- Agregar nombres de máquina
  - zentyal
  - martin
  - misil
  - alberto
  - hplaser
  - myrna
  - renata
  - copernico

## Office

### Usuarios y Equipos
- Crear user _dadmin_ (pass: *****)
    - Cuota de usuario (MB): 50000 (jugar con esto, si no es suficientemente grande, da error de disco lleno en los "shares" de samba)
- Agregar user dadmin al grupo _Domain Admins_
- Activar cuenta de invitado _Guest_

### Dominio

#### Configuración

    Función del servidor:       Controlador de dominio
    Reino:	                    ESTUDIO.LAN
    Nombre del dominio NetBIOS: ESTUDIO
    Nombre de máquina NetBIOS:  zentyal
    Descripción del servidor:   Zentyal Server
    Habilitar perfiles móviles: False
    Letra de unidad:            H:

### Compartición de Ficheros

Antes, crear los directorios /srv/ftp/planos y /srv/ftp/varios:

    $ sudo mkdir /srv/ftp/planos
    $ sudo mkdir /srv/ftp/varios
    
#### Directorios compartidos (en todas marcar Acceso de Invitado)

    Nombre        Ruta            Comentario
    ----------------------------------------------------------------
    expedientes   expedientes     Expedientes
    relevamientos relevamientos   Archivos de relevamientos GNSS
    software      software        Software utilizado en el estudio
    planos        /srv/ftp/planos Planos pedidos al SCIT y propios
    map           map             Archivos de MAP
    varios        /srv/ftp/varios Varios

En los que tienen ruta completa (/srv/ftp/...) elegir _Ruta del sistema de ficheros_

#### Papelera de reciclaje
Habilitar Papelera de Reciclaje: True

### FTP

- Acceso anónimo: Lectura/escritura
- Directorios personales: False

Guardar cambios. Para poder edit/upload con user anonymous, editar /etc/vsftpd.conf y asegurarse de lo siguiente (cambiar cada vez que se reinicia):
    
    anon_world_readable_only=NO
    anon_other_write_enable=YES

y por último reiniciar el servicio de FTP

    $ sudo service vsftpd restart

## ToDo
- Impresoras

# INSTALACIÓN SOFTWARE Y RESTAURACIÓN BACKUP

## Restaurar Copias de Seguridad

### Restaurar todo

    $ sudo duplicity restore file:///<donde-esten-los-duplicity-xxx> target-dir

ej:

    $ sudo duplicity restore file:///mnt/Flash/backup_Estudio/estudio-data/ restore/

ej, restaurar expediente 4100, si los archivos _duplicity_ están en /mnt/Flash/backup_Estudio/estudio-data/

    $ sudo duplicity restore --file-to-restore home/samba/shares/expedientes/ABIERTOS/4100 file:///mnt/Flash/backup_Estudio/estudio-data/ restore/

## Instalar software

    $ sudo apt-get update && sudo apt-get upgrade
    & sudo apt-get autoremove
    $ sudo apt-get install
      man
      mlocate
      vim
      bash-completion
      ranger gpm atool caca-utils elinks-lite lynx w3m highlight ctags vim-doc vim-scripts w3m-img
      ntfs-config
      transmission-qt
      python-pip
      git
      usbmount pmount

### x2goserver
(http://wiki.x2go.org/doku.php/doc:installation:x2goserver)

    sudo apt-get install software-properties-common python-software-properties
    sudo add-apt-repository ppa:x2go/stable
    sudo apt-get update
    sudo apt-get install x2goserver
    sudo apt-get install x2golxdebindings  # if you use LXDE/lubuntu

- sugeridos: x2goserver-printing x2goserver-compat x2goserver-xsession x2goserver-fmbindings x2goserver-pyhoca rdesktop pulseaudio-utils
- recomendados: sshfs x2goserver-extensions

### gea
(http://github.com/quijot/gea)

sudo pip o lo que fuere:
- gea __FALTA ESTO__
    - python, django, grappelli, bla³


# Configurar en PCs de la red
...


# TWEAKS

# /etc/sudoers
Agregar esta línea para que sudo no pida password nunca más

        Defaults:santi      !authenticate

# /etc/zentyal/ebackup.conf

        volume_size = 600
