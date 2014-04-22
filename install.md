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
