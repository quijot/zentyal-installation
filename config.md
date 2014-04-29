# CONFIGURACI�N

## Core

### Sistema

#### Copia de seguridad

##### Configuraci�n y Estado

Antes, crear el direcotrio en home:

    $ sudo mkdir -p /home/backups/estudio-data

Luego,

    M�todo:              Sistema de ficheros
    Ordenador o destino: /home/backups/estudio-data
    Cifrado:             Deshabilitado
    Frecuencia de la copia de seguridad completa: Semanalmente (en Viernes)
    Frecuencia de backup incremental:             Diariamente
    El proceso de respaldo comienza a las:        22:00
    Guardar copias completas anteriores:          n�mero m�ximo (1)

##### Inclusiones y Exclusiones

    Excluir ruta	/home/samba/shares/expedientes/RecycleBin
    Excluir ruta	/home/samba/shares/map/RecycleBin
    Excluir ruta	/home/samba/shares/software/RecycleBin
    Excluir ruta	/home/samba/shares/relevamientos/RecycleBin
    Excluir ruta	/srv/ftp/planos/RecycleBin
    Excluir ruta	/srv/ftp/set/RecycleBin
    Excluir ruta	/srv/ftp/varios/downloads
    Incluir ruta	/home/samba/shares
    Incluir ruta	/srv/ftp

### Mantenimiento

#### SAI

- UPS label: RS800
- Descripci�n: Back-UPS RS 800
- Driver: APC - Back-UPS RS USB - usbhid-ups
- Puerto: Autodetect
- Serial number: QB0506134358

Guardar, iniciar m�dulo e ir a Configuraci�n ... **FALTA** ver por qu� no arranca bien

### Red

#### Interfaces
- eth0, est�tico, 10.0.0.1 / 255.255.255.0
- eth1, DHCP, WAN

#### DNS
- 127.0.0.1
- X.X.X.X
    - (agregar a los que est�n, la IP de un DNS server conocido, por ej: 8.8.8.8, el DNS de Google)

#### Servicios

Para agregar **transmission**. A�adir un nuevo Servicio:

- Nombre del servicio: transmission

Luego entrar en _Configuraci�n_ del servicio y a�adir nuevo:
- Protocolo: TCP/UDP
- Puerto origen: Cualquiera
- Puerto destino: Puerto �nico 9091

## Gateway

### Cortafuegos

#### Filtrado de Paquetes

Para agregar **transmission**:

En _Reglas de filtrado desde las redes internas a Zentyal_ -> Configurar reglas -> A�adir nueva:

- Decisi�n: ACEPTAR
- Origen: Cualquiera
- Servicio: transmission (tiene que estar creado en Core -> Red -> Servicios)

## Infraestructure

### DHCP

(Activar m�dulo DHCP, luego entrar en eth0)

    Puerta de enlace predeterminada: Zentyal
    Dominio de b�squeda:             Ninguno
    Servidor de nombres primario:    DNS local de Zentyal
    Servidor de nombres secundario:  (si no anda as�, poner ac� 8.8.8.8)
    Servidor NTP:                    Ninguno
    Servidor WINS:                   Ninguno

#### Rangos DHCP

Agregar Rangos:

- Estudio: 10.0.0.10 a 10.0.0.25

### DNS

Habilitar el cach� de DNS transparente: False

#### Redireccionadores

agregar 8.8.8.8

#### Dominios

- Agregar nombres de m�quina
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

#### Configuraci�n

    Funci�n del servidor:       Controlador de dominio
    Reino:	                    ESTUDIO.LAN
    Nombre del dominio NetBIOS: ESTUDIO
    Nombre de m�quina NetBIOS:  zentyal
    Descripci�n del servidor:   Zentyal Server
    Habilitar perfiles m�viles: False
    Letra de unidad:            H:

### Compartici�n de Ficheros

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

- Acceso an�nimo: Lectura/escritura
- Directorios personales: False

Guardar cambios. Para poder edit/upload con user anonymous, editar /etc/vsftpd.conf y asegurarse de lo siguiente (cambiar cada vez que se reinicia):
    
    anon_world_readable_only=NO
    anon_other_write_enable=YES

y por �ltimo reiniciar el servicio de FTP

    $ sudo service vsftpd restart

## ToDo
- Impresoras
