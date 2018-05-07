# Instalación desatendida de Ubuntu-18.04

Este instructivo se basó principalmente en la siguiente fuente:
[ASKUBUNTU](https://askubuntu.com/questions/806820/how-do-i-create-a-completely-unattended-install-of-ubuntu-desktop-16-04-1-lts)


## Uso

* Descargar el iso de Ubuntu-18.04 amd64 Desktop: http://releases.ubuntu.com/18.04/

* Montar el iso en algun directorio 
```
# mkdir -p /mnt/iso
# mount -o loop ubuntu-18.04-desktop-amd64.iso /mnt/iso
```

* Copiar el contenido de la iso hacia otra carpeta que sea editable
```
# mkdir -p /opt/ubuntuiso
# cp -rT /mnt/iso /opt/ubuntuiso
```

* Editar el archivo txt.cfg para agregar la opcion de la instalacion desatendida. Agregar la siguiente entrada:
```
label live-install
  menu label ^Install Ubuntu (UNATTENDED)
  kernel /casper/vmlinuz
  append  file=/cdrom/preseed.cfg auto=true priority=critical debian-installer/locale=en_US keyboard-configuration/layoutcode=us ubiquity/reboot=true boot=casper automatic-ubiquity initrd=/casper/initrd.lz quiet splash noprompt noshell ---
```

* Crear un archivo Preseed con la configuración deseada. Referencia: (https://help.ubuntu.com/lts/installation-guide/amd64/apb.html)

* Crear una nueva iso que contenga las modificaciones realizadas (txt.cfg, preseed.cfg)
```
# cd /opt/ubuntuiso
# mkisofs -D -r -V "UNATTENDED_UBUNTU" -cache-inodes -J -l -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -o /root/Downloads/ubuntu-18.04-amd64-unattended.iso /opt/ubuntuiso
```

* Liberar espacio en disco
```
# umount /mnt/iso
# rm -fr /mnt/iso
# rm -fr /opt/ubuntuiso
```

## Configuracion posterior

* Eliminar el icono de Amazon
```
$ sudo apt purge ubuntu-web-launchers
```

* Otros
```
d-i preseed/late_command string \
 in-target /bin/bash -c '/path/to/puppet apply /path/to/puppet.pp'; \
 in-target /bin/bash -c 'echo "Some status message"';

```

