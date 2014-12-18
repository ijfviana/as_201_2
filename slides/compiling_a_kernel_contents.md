## Construyendo el núcleo

* Sólo hay que ejecutar el comando `make`:
``` bash
ijfviana@vagalume:/usr/src/linux$ sudo make
scripts/kconfig/conf --silentoldconfig Kconfig
  SYSHDR  arch/x86/syscalls/../include/generated/asm/unistd_32.h
  SYSHDR  arch/x86/syscalls/../include/generated/asm/unistd_64.h
  SYSHDR  arch/x86/syscalls/../include/generated/asm/unistd_x32.h
  SYSTBL  arch/x86/syscalls/../include/generated/asm/syscalls_32.h
  SYSHDR  arch/x86/syscalls/../include/generated/asm/unistd_32_ia32.h
  SYSHDR  arch/x86/syscalls/../include/generated/asm/unistd_64_x32.h
  SYSTBL  arch/x86/syscalls/../include/generated/asm/syscalls_64.h
  HOSTCC  arch/x86/tools/relocs
  CHK     include/linux/version.h
  UPD     include/linux/version.h
...
```
* Hay que tener instaladas las herramientas de desarrollo (compiladores, librerías...)

<aside class="notes">
To build a kernel, type make in the kernel’s source directory. Assuming you have the necessary development tools
installed and that your kernel configuration contains no glaring errors, the build process will proceed smoothly.
This process requires no additional input from you, but it will take a while—several minutes to more than an hour,
depending on the speed of the computer and how many kernel features you’re compiling. As the process proceeds,
you’ll see summary lines displayed on the screen:
</aside>



## Construir el kernel (II)

* El resultado de este proceso es la construcción del núcleo y sus módulos
* Si queremos sólo construir el kernel podemos ejecutar:
```bash
ijfviana@vagalume:/usr/src/linux$ make bzImage
```
* Si sólo queremos los módulos
``` bash
ijfviana@vagalume:/usr/src/linux$ make modules
```

<aside class="notes">
The default make target for the Linux kernel builds both the main kernel file and all the separate kernel modules. 
This is equivalent to typing make all. You can build the kernel and modules separately by typing make bzImage to build the kernel or make modules to build
the modules. On some platforms, you can type make zImage instead of make bzImage.
</aside>
