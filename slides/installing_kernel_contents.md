## Instalando del núcleo

* [Instalar el fichero principal del núcleo](#/6/2)
* [Instalar los módulos del núcleo](#/6/3)



## Instalar el fichero principal del núcleo (I)

* El fichero principal del núcleo estará en el directorio `arch`.
```bash
/usr/src/linux/$ ls arch
alpha	  c6x	 hexagon  m68k	      openrisc	score  um	  xtensa
arm       cris	 ia64	  microblaze  parisc	sh     unicore32
avr3      frv	 Kconfig  mips	      powerpc	sparc  x86
blackfin  h8300  m32r	  mn10300     s390	tile   x86_64
```
* Si la arquitectura es i386 estará en `arch/x86/boot`
* Lo copiaremos en `/boot` indicando el nombre de la versión

```bash
/usr/src/linux/$ cp arch/x86_64/boot/bzImage /boot/bzImage-2.6.35.4
```

<aside class="notes">
* With the kernel compiled, you can copy it from its location in the kernel source tree to the /boot directory.
* If you’re compiling a kernel for an x86 or x86-64 system, it will be in the arch/x86/boot subdirectory of the kernel source tree
</aside>



## Instalar el fichero principal del núcleo (II)

* Opcionalmente se pude copiar el fichero [`System.map`](http://en.wikipedia.org/wiki/System.map)
* Este fichero tiene información de depuración.
```bash
/usr/src/linux/$ cp System.map /boot/System.map.2.6.35.4
```
* El fichero `System.map` es un enlace simbólico que apuntada al fichero `System.map` del kernel en ejecución
```bash
ln -s /boot/System.map-2.6.35.4 /boot/System.map
```

note:
* In addition to the kernel file, you may want to copy the System.map file.
* This file contains pointers to functions. In the kernel and is used for debugging kernel problems.
* Copying it isn’t critical, but it can be helpful.
* Once the system is running, /boot/System.map should be a symbolic link to the current System.map file.



## Instalar los módulos del núcleo (I)

* Debemos ejecutar
``` bash
/usr/src/linux/$ make modules_install
```
* Se crea un árbol de directorios en `/lib/modules/` con el mismo nombre que la versión del núcleo y que contiene los módulos (ficheros `.ko`)
``` bash
/lib/modules/3.8.0-31-generic$ ls
build             modules.builtin.bin  modules.inputmap
initrd            modules.ccwmap       modules.isapnpmap
kernel            modules.dep          modules.ofmap
modules.alias     modules.dep.bin      modules.order
modules.builtin   modules.ieee1394map  modules.seriomap
```

<aside class="notes">
Installing kernel modules is easy: As root, type make modules_install. The make utility will create a
subdirectory in /lib/modules named after the kernel version number, such as /lib/modules/2.6.35.4, and
then copy the kernel modules into this subdirectory, creating additional levels of subdirectories for
certain sets of modules. If you forget to install the kernel modules, the computer might not boot into the new kernel. If the computer does boot, any hardware or
other feature managed by a modular driver won’t work.
In operation, Linux requires information on the dependencies between kernel modules. These dependencies
are stored in a file called modules.dep, which is stored in the kernel modules directory. You can regenerate this file
for the currently running kernel by typing depmod as root. As part of the module installation process, depmod is
called to generate a modules.dep file for the new kernel modules.
Objective 201.4 refers to a depmod target to make. Such a target does not exist, as of the 2.6.35.4 kernel; however, as just noted, the
modules_install target does call the depmod utility.
</aside>



## Instalar los módulos del núcleo (II)

* El fichero `modules.dep` guarda dependencias entre módulos de forma que si se carga un módulo también lo hacen sus dependencias
```bash
/lib/modules/3.8.0-31-generic$ vi modules.dep
kernel/arch/x86/kernel/cpu/mcheck/mce-inject.ko:
kernel/arch/x86/kernel/msr.ko:
kernel/arch/x86/kernel/cpuid.ko:
kernel/arch/x86/kernel/microcode.ko:
kernel/arch/x86/crypto/ablk_helper.ko: kernel/crypto/cryptd.ko
kernel/arch/x86/crypto/glue_helper.ko:
...
```
* Si el fichero no existiera lo podemos re-generar usando el comando [`depmod`](http://linux.about.com/library/cmd/blcmdl8_depmod.htm)

note:

* Linux requires information on the dependencies between kernel modules.
* These dependencies are stored in a file called modules.dep, which is stored in the kernel modules directory.
*  You can regenerate this file by typing depmod as root.
