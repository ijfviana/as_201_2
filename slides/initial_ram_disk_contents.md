## Construyendo discos RAM

* [¿Qué es un disco RAM?](#/7/1)
* [¿Cuándo se debería preparar un disco RAM ?](#/7/5)
* [¿Cómo crear un disco RAM?](#/7/6)



## ¿Qué es un disco RAM ? (I)

<a class="fancybox" href="img/disk_RAM_1.png" data-fancybox-group="gallery" title="Carga del sistema">
<img height="500px" src="img/disk_RAM_1.redimensionado640x480.png" alt="Carga del sistema">
</a>

note:
Durante el proceso de arranque del servidor, el boot loader lee el kernel main file de disco, lo carga en memoria y le pasa el control. El núcleo, entre otras cosas leerá los módulos.



## ¿Qué es un disco RAM ? (II)

<a class="fancybox" href="img/disk_RAM_2.png" data-fancybox-group="gallery" title="Problema carga del sistema">
<img height="500px" src="img/disk_RAM_2.redimensionado640x480.png" alt="Problema carga del sistema">
</a>

note:
El algunos casos los módulos residen en particiones tipo ext3, entre esos módulos está el que da soporte a ext3. Si el núcleo no tiene
soporte para ext3, no puede cargar los módulos.



## ¿Qué es un disco RAM ? (III)

<a class="fancybox" href="img/keys_in_the_car.png" data-fancybox-group="gallery" title="Las llaves dentro del coche">
<img height="500px" src="img/keys_in_the_car.png" alt="Las llaves dentro de coche">
</a>

<aside class="notes">
El problema es parecido a... cómo entran en el coche si las llaves las tengo dentro de coche
</aside>



## ¿Qué es un disco RAM ? (IV)

<a class="fancybox" href="img/disk_RAM_3.png" data-fancybox-group="gallery" title="Las llaves dentro del coche">
<img height="500px" src="img/disk_RAM_3.redimensionado640x480.png" alt="Las llaves dentro de coche">
</a>

<div class="alert alert-info">
  Colección de módulos y utilidades críticas que el boot loader lee y carga en memoria.
</div>

note:




## ¿Cuándo se debería preparar un disco RAM ?

* Cuando el kernel no incluya todos los módulos necesarios para el arranque
  * RAID
  * LVM
  * SAN
  * ext3



## ¿Cómo crear un disco RAM?

* [Usando mkintrd](#/7/7)
* [Usando mkinitramfs](#/7/11)



## Usando mkintrd (I)

* Es programa usado en Red Hat, Fedora y derivados.
* Ejemplo de uso:
```bash
# mkinitrd /boot/initrd-2.6.35.4.img 2.6.35.4
```
* Este comando crea `/boot/initrd-2.6.35.4.img` usando los módulos para 2.6.35.4 kernel.
* El fichero `/boot/initrd-2.6.35.4.img` está comprimido con la utilidad `gzip` y empaquetado con `cpio`
* Los módulos se deben encontrar en el directorio `/lib`



## Usando mkintrd (II)

<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th width="350em">Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`--version`</td>
<td>Muestra la versión.</td>
</tr>
<tr>
<td>`-v`</td>
<td>Modo detallado.</td>
</tr>
<tr>
<td>`--preload=module`</td>
<td>Carga el `module` indicado antes que los módulos SCSI</td>
</tr>
<tr>
<td>`--with=module`</td>
<td>Carga el `module` indicado después de los módulos SCSI.</td>
</tr>
<tr>
<td>`--builtin=module`</td>
<td>`mkinitrd` se comporta como si el `module` estuviera instalado en el núcleo</td>
</tr>
</tbody>
</table>



## Usando mkintrd (III)

<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th width="350em">Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-f`</td>
<td>Sobre escribe el fichero imagen creado.</td>
</tr>
<tr>
<td>`--fstab=filename`</td>
<td>Usa <code>filename</code> para determinar los FS soportados. Por defecto usa <code>/etc/fstab</code>.</td>
</tr>
<tr>
<td>`--image-version`</td>
<td>Añade la versión del núcleo a la ruta antes de crear la imagen.</td>
</tr>
<tr>
<td>`--nocompress`</td>
<td>No se comprime la imagen. Por defecto se comprime con <code>gzip</code>.</td>
</tr>
</tbody>
</table>



## Usando mkintrd (IV)

<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th width="420em">Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`--nopivot`</td>
<td>La imagen no usa la llamada al sistema <code>pivot_root</code>. Opción obsoleta.</td>
</tr>
<tr>
<td>`--omit-lvm-modules`</td>
<td>No añade los módulos LVM a la imagen.</td>
</tr>
<tr>
<td>`--omit-raid-modules`</td>
<td>No añade los módulos RAID a la imagen.</td>
</tr>
<tr>
<td>`--omit-scsi-modules`</td>
<td>No añade los módulos SCSI a la imagen.</td>
</tr>
</tbody>
</table>



## Usando mkinitramfs (I)

* Equivalente a `mkinitrd`
* La forma de usarlo es:
``` bash
> mkinitramfs -o /boot/initramfs-2.6.35.4.img 2.6.35.4
```



## Usando mkinitramfs (II)

<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th>Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`-d`</td>
<td>Establece el fichero de configuración. Por defecto es <code>/etc/initramfs-tools</code>.</td>
</tr>
<tr>
<td>`-k`</td>
<td>No borra el directorio temporal tral crear la imagen</td>
</tr>
<tr>
<td>`-o`</td>
<td>Establece el fichero donde se almacena la imagen.</td>
</tr>
<tr>
<td>`-r`</td>
<td>Establece la partición raiz.</td>
</tr>
<tr>
<td>`-v`</td>
<td>Entra en modo detallado.</td>
</tr>
</tbody>
</table>



## Usando mkinitramfs (III)

<table class="table table-condensed table-hover table-bordered">
<thead>
<tr>
<th width="500em">Opción</th>
<th>Explicación</th>
</tr>
</thead>
<tbody>
<tr>
<td>`--supported-host-version=v`</td>
<td>Consulta si se puede crear un disco RAM para el núcleo en ejecución de la versión indicada.</td>
</tr>
<tr>
<td>`--supported-target-version=v`</td>
<td>Consulta si se puede crear un disco RAM para el núcleo de la versión indicada.</td>
</tr>
<tr>
<td>`--version`</td>
<td>Establece la versión del núcleo</td>
</tr>
</tbody>
</table>
