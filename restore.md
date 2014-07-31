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
    $ sudo apt-get autoremove
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

### transmission

Directorio de escargas:

    /srv/ftp/varios/downloads
