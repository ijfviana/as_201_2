## Configurando los gestores de arranque

* [GRUB Legacy](#/8/1)
* [GRUB 2](#/8/2)

<aside class="notes">
With your new kernel built and all its files in place, it’s time to add your new kernel to your boot loader
configuration.

A continuación explicaremos cómo configurar GRUB Legacy and GRUB 2.

If you installed a kernel in binary form from a distribution’s package manager, chances are you won’t need
to explicitly edit your boot loader configuration.
</aside>



## GRUB Legacy

* Se edita el fichero `/boot/menu.lst` o `/boot/grub/grub.conf` y se añade algo similar a

``` bash
title Fedora (2.6.32)
root (hd0,0)
kernel /vmlinuz-2.6.32 ro root=/dev/sda5
initrd /initrd-2.6.32
```

* Las rutas deben corresponderse con las localizaciones del núcleo y el disco RAM



## GRUB 2 (I)
### Modo automático

* Hacemos una copia de seguridad `/boot/grub/grub.cfg`
* Ejecutamos `update-grub` o `grub-mkconfig`
* Comprobamos que en `/boot/grub/grub.cfg` hay una entrada para el nuevo núcleo



## GRUB 2 (II)
### Modo manual

* A veces el modo automático falla porque el núcleo tiene un nombre no esperado
* En esta caso, primero editamos el fichero
``` bash
> vi  /etc/grub.d/40_custom file
```
* Y añadimos algo similar a:
``` bash
menuentry "Fedora (2.6.35.4)" {
set root=(hd0,1)
linux /vmlinuz-2.6.35.4 ro root=/dev/sda5
initrd /initrd-2.6.35.4
}
```



## GRUB 2 (III)

* Podemos cambiar las opciones por defecto del GRUB editando el fichero `/etc/default/grub`
* Si queremos indicar el núcleo por defecto podemos poner:

```
GRUB_DEFAULT=0
```

* Para finalizar ejecutamos `update-grub` o `grub-mkconfig`
