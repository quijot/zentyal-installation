# TWEAKS

# /etc/sudoers
Agregar esta línea para que sudo no pida password nunca más

        Defaults:santi      !authenticate

# /etc/zentyal/ebackup.conf

        volume_size = 600

# default editor

Para poner vim como editor por defecto:

        $ update-alternatives --config editor
