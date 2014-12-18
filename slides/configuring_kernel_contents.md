## Configuración del núcleo

* [Información sobre el hardware](#/4/2)
* [Preparando el fichero de configuración](#/4/6)
* [Estableciendo opciones de configuración](#/4/11)

<aside class="notes">
Configuring the kernel can be a daunting task because of the vast number of kernel options available—2,994, as of
version 2.6.35.4. If you’re starting from your distribution’s version of the kernel, though, chances are most of the
options won’t need to be changed; you should be able to home in on whatever options you want to change to
achieve your goal in recompiling the kernel and leave the rest alone. If you’re starting with a stock kernel, though,
you’ll have to be very careful, since there are many types of configuration options that, if set incorrectly, can lead
to an unbootable kernel. You may need to know a great deal about your computer’s hardware, and particularly its
disk subsystem, to ensure the system can boot.
</aside>



## Información sobre el hardware (I)

<a class="fancybox" href="img/hardware.png" data-fancybox-group="gallery" title="Líneas de código">
<img height="500px" src="img/hardware.png" alt="Líneas de código">
</a>

Hay que conocer el hardware que está instalado.

<aside class="notes">
Before you fire up the Linux kernel configuration utilities, you should learn something about the hardware on your
system so that you’ll know which hardware modules to install. You should also know what filesystems and other
kernel-level features your system uses so that you can be sure to compile the necessary support into your new
kernel.
</a>



## Información sobre el hardware (II)

<a class="fancybox" href="img/chipset.png" data-fancybox-group="gallery" title="Chipset">
<img height="500px" src="img/chipset.redimensionado640x480.png" alt="Chipset">
</a>

<aside class="notes">
For the most part, the kernel includes drivers for hardware by the chipset it uses. The *chipset* is one or more
chips that provide the functionality for the subsystem in question. The chipset is often manufactured by a
company other than the one whose name appears on the product’s box, which can make identifying your
hardware difficult in some cases.
</aside>



## Información sobre el hardware (III)

<a class="fancybox" href="img/que_hardware_tengo.png" data-fancybox-group="gallery" title="Chipset">
<img height="500px" src="img/que_hardware_tengo.redimensionado640x480.png" alt="Chipset">
</a>

<aside class="notes">
Several approaches exist to solving this problem:
* Using Hardware Manuals
* Checking PCI Devices If you type lspci at a Linux shell prompt
* Checking USB Devices Typing lsusb
* Checking Loaded Modules If you type lsmod
* Using a Working Kernel as a Model
* Visual Inspection
</aside>



## Preparando el fichero de configuración (I)

Todo se hace mediante el comando `make`:

<div class="table-responsive">
<table class="table table-hover table-condensed table-bordered">
<thead>
<tr>
<th>Objetivo</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`mrproper`</td>
<td>Borrar configuraciones antiguas</td>
</tr>
<tr>
<td>`oldconfig`</td>
<td>Actualiza ficheros de configuración antiguos</td>
</tr>
<tr>
<td>`silentoldconfig`</td>
<td>Similar al comando anterior</td>
</tr>
<tr>
<td>`defconfig`</td>
<td>Fichero de configuración por defecto</td>
</tr>
<tr>
<td>`allmodconfig`</td>
<td>Fichero de configuración lo más modular</td>
</tr>
</tbody>
</table>
</div>

<aside class="notes">
Once you’ve identified your major hardware components, you can begin configuring the kernel. This is done with
the help of one or more make targets, as detailed in Table 2.2. A few other obscure options are available; see the
/usr/src/linux/README file for details. Additional make targets, described in later sections of this chapter, actually
build the kernel and help you install it.
</aside>



## Preparando el fichero de configuración (II)

* Antes de crear el núcleo hay que crear un fichero de configuración
* Lo habitual es partir de alguno de los localizados en `/boot`
```bash
/usr/src/linux$ ls /boot/config*
/boot/config-3.5.0-41-generic /boot/config-3.2.0-54-generic  /boot/config-3.8.0-31-generic
```
* Que copiaremos al directorio con los fuentes del núcleo a compilar
```bash
/usr/src/linux$ sudo cp /boot/config-3.2.0-53-generic .config
```

<aside class="notes">
If you’re building a new kernel and
have the source code for a working one available, you can copy the hidden .config file (which holds the kernel
configuration) from a working kernel’s source directory to the current directory...
</aside>



## Preparando el fichero de configuración (III)

* Una vez copiado se suele "limpiar" el directorio con lo fuentes

``` bash
/usr/src/linux$ sudo make mrproper
  CLEAN   scripts/basic
  CLEAN   scripts/kconfig
  CLEAN   include/config include/generated
```

* Este comando se encarga de borrar ficheros objeto antiguos, así evitaremos problemas con dependencias previas

<aside class="notes">
If you want to be sure that the kernel source tree is in a pristine state, you should begin by typing make
mrproper. This action cleans out all the old temporary and configuration files.
</aside>



## Preparando el fichero de configuración (IV)

* Actualizamos el fichero de configuración copiado con nuevo parámetros

``` bash
ijfviana@vagalume:/usr/src/linux$ sudo make silentoldconfig
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  SHIPPED scripts/kconfig/zconf.tab.c
  SHIPPED scripts/kconfig/zconf.lex.c
  SHIPPED scripts/kconfig/zconf.hash.c
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
scripts/kconfig/conf --silentoldconfig Kconfig
.config:4128:warning: symbol value 'm' invalid for FB_VESA
.config:5417:warning: symbol value 'm' invalid for ZCACHE
*
* Restart config...
*
*
* General setup
*
Prompt for development and/or incomplete code/drivers (EXPERIMENTAL) [Y/n/?] y
Cross-compiler tool prefix (CROSS_COMPILE) []
Local version - append to kernel release (LOCALVERSION) []
Automatically append version information to the version string (LOCALVERSION_AUTO) [N/y/?] n
Kernel compression mode
> 1. Gzip (KERNEL_GZIP)
  2. Bzip2 (KERNEL_BZIP2)
  3. LZMA (KERNEL_LZMA)
  4. XZ (KERNEL_XZ)
  5. LZO (KERNEL_LZO)
choice[1-5?]: 1
Default hostname (DEFAULT_HOSTNAME) [(none)] (none)
Support for paging of anonymous memory (swap) (SWAP) [Y/n/?] y
System V IPC (SYSVIPC) [Y/n/?] y
POSIX Message Queues (POSIX_MQUEUE) [Y/n/?] y
BSD Process Accounting (BSD_PROCESS_ACCT) [Y/n/?] y
  BSD Process Accounting version 3 file format (BSD_PROCESS_ACCT_V3) [Y/n/?] y
open by fhandle syscalls (FHANDLE) [Y/n/?] y
Export task/process statistics through netlink (EXPERIMENTAL) (TASKSTATS) [Y/?] y
  Enable per-task delay accounting (EXPERIMENTAL) (TASK_DELAY_ACCT) [Y/?] y
  Enable extended accounting over taskstats (EXPERIMENTAL) (TASK_XACCT) [Y/n/?] y
    Enable per-task storage I/O accounting (EXPERIMENTAL) (TASK_IO_ACCOUNTING) [Y/n/?] y
Auditing support (AUDIT) [Y/?] y
  Enable system-call auditing support (AUDITSYSCALL) [Y/n/?] y
  Make audit loginuid immutable (AUDIT_LOGINUID_IMMUTABLE) [N/y/?] (NEW)

```

<aside class="notes">
If you’re building a new kernel and then type make oldconfig or make silentoldconfig.If you don’t know how to respond to certain queries, using the default answer (which is capitalized in most
cases) is usually the best approach. Most features accept yes (y) and no (n) answers to their configuration prompts.
Many features can be built as modules; you type m to implement modular compilation. A few features, such as the
cross-compiler one in the preceding example, require free-form responses. Pressing the Enter key specifies the
default, which is usually acceptable.
If you have no working kernel source as a model, you might want to start by typing make allmodconfig. The
result, if you were to build the kernel immediately thereafter, would be a kernel that compiles as many
components as modules as possible. Given an appropriate initial RAM disk (described later, in “Preparing an Initial
RAM Disk”), such a kernel will work on most computers; but it may not be optimal. It will also take longer to
compile than an optimized kernel, since you’ll be compiling modules for huge numbers of hardware devices you
don’t have on your computer.
</aside>




## Preparando el fichero de configuración (V)

* Si no disponemos de un fichero de configuración podemos optar por crear una configuración por defecto para nuestra arquitectura
```bash
sudo make defconfig
*** Default configuration is based on 'x86_64_defconfig'
#
# configuration written to .config
#
```
* O crear un fichero de configuración lo más modular posible
```bash
ijfviana@vagalume:/usr/src/linux$ sudo make allmodconfig
scripts/kconfig/conf --allmodconfig Kconfig
#
# configuration written to .config
#
```



## Estableciendo opciones de configuración (I)

Modificamos el fichero de configuración usamos  `make`:

<div class="table-responsive">

<table class="table table-bordered table-condensed table-hover">
<thead>
<tr>
<th>Objetivo</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`config`</td>
<td>Configuración basada en entorno de texto.</td>
</tr>
<tr>
<td>`menuconfig`</td>
<td>Configuración basada en entorno de texto con menus</td>
</tr>
<tr>
<td>`xconfig`</td>
<td>Configuración en etorno gráfico Qt.</td>
</tr>
<tr>
<td>`gconfig`</td>
<td>Configuración en etorno gráfico Gtk.</td>
</tr>
</tbody>
</table>

</div>

Podemos obtener más información ejecutando `make help`



## Estableciendo opciones de configuración (II)

<a class="fancybox" href="img/gconfig.png" data-fancybox-group="gallery" title="gconfig">
<img height="500px" src="img/gconfig.png" alt="gconfig">
</a>

<aside class="notes">
At this point, you should probably optimize your configuration further by using the menuconfig, xconfig, or gconfig
target. Figure 2.1 shows the display created by typing make xconfig. Kernel options are arranged in a hierarchical
fashion. In Figure 2.1, the main categories are shown in the left panel, while the top-right panel displays sub-
options for whatever main category you’ve selected. Confusingly, the highest-level categories have sub-categories
that expand below them in the left panel, as well as sub-options that are accessible in the top-right panel.
Explanatory notes on the options appear in the bottom-right panel. You can select options by clicking them. A
check mark means that an option will be compiled directly into the kernel, while a dot means that the option will
be compiled as a module. (The gconfig target doesn’t support this feature; you must check for a Y, M, or N
character to the right of the option selection area. The text-based menuconfig uses a similar convention.) Not all
options can be compiled as modules; some just don’t work that way, and some options control features that aren’t
meaningful as modules. Such features give Y or N options, but not M.
</aside>



## Estableciendo opciones de configuración (III)

<div class="alert alert-info">
  Las opciones de configuración están divididas por grupos
</div>

<div class="row">
  <div class="col-md-6">
      <ul>
        <li> General Setup
        <li> Bus Options (PCI, etc.)
        <li> Executable File Formats/Emulations
        <li> Device Drivers
      </ul>
  </div>
  <div class="col-md-6">
    <ul>
        <li> File System
        <li> Kernel Hacking
        <li> Virtualization
      </ul>
  </div>
</div>
