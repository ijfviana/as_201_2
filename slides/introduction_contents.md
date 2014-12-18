## Introducción (I)

* La compilación del núcleo es una labor larga y tediosa.

<a class="fancybox" href="img/linux-kernel-lines_code.jpg" data-fancybox-group="gallery" title="Líneas de código">
<img height="500px" src="img/linux-kernel-lines_code.redimensionado640x480.jpg" alt="Líneas de código">
</a>

note:

* Linux es un núcleo que ha sufrido una gran evolución en los últimos años
* La gran cantidad de lineas de código hace que las compilación del mismo sea una labor tediosa y compleja.



## Introducción (II)

* El número de opciones, a la hora de compilar, es superior a las 3000

```
CONFIG_BLK_DEV_SD        [disk (sd) driver]
CONFIG_SD_EXTRA_DEVS     [extra slots for disks added later]
CONFIG_BLK_DEV_SR        [SCSI cdrom (sr) driver]
CONFIG_BLK_DEV_SR_VENDOR [allow vendor specific cdrom commands]
CONFIG_SR_EXTRA_DEVS     [extra slots for cdroms added later]
CONFIG_CHR_DEV_ST        [tape (st) driver]
CONFIG_CHR_DEV_OSST	 [OnSteam tape (osst) driver]
CONFIG_CHR_DEV_SG        [SCSI generic (sg) driver]
CONFIG_DEBUG_QUEUES      [for debugging multiple queues]
CONFIG_SCSI_MULTI_LUN    [allow probes above lun 0]
CONFIG_SCSI_CONSTANTS    [symbolic decode of SCSI errors]
CONFIG_SCSI_LOGGING      [allow logging to be runtime selected]

```

* La mejor idea es partir de la configuración por defecto de la distribución.

note:

* Configuring the kernel can be a daunting task because of the vast number of kernel options available—2,994, as of version 2.6.35.4.
* If you’re starting from your distribution’s version of the kernel, though, chances are most of the options won’t need to be changed;
* If you’re starting with a stock kernel, though, you’ll have to be very careful, since there are many types of configuration options that, if set incorrectly, can lead to an unbootable kernel.
* You may need to know a great deal about your computer’s hardware, and particularly its disk subsystem, to ensure the system can boot.
