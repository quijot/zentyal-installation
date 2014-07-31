# TWEAKS

## /etc/sudoers
Agregar esta línea para que sudo no pida password nunca más

        Defaults:santi      !authenticate

## /etc/zentyal/ebackup.conf

        volume_size = 600

## default editor

Para poner __vim__ como editor por defecto:

        $ update-alternatives --config editor

## devel en home

        $ cd
        $ ln -s /srv/ftp/varios/devel devel
