## Parcheo del núcleo
* [¿Qué es un parche?](#/3/2)
* [¿Por qué parchear?](#/3/6)
* [¿Cómo aplicar un parche?](#/3/7)
* [¿Cómo saber si se ha aplicado adecuadamente?](#/3/9)



## ¿Qué es un parche?

* Es un "remiendo" que se aplica a un código fuente para obtener otro "libre de errores".

* Dispongo del siguiente código denominado `old.py`:

``` python
def foo(a):
    print a % 2

foo(22)
```

* Añado un mensaje más explicativo al código anterior obteniendo el fichero `new.py`

``` python
def foo(a):
    print 'even' if a % 2 == 0 else 'odd'

foo(22)
```



## ¿Qué es un parche? (II)

* Preparamos el parche, un fichero que a partir de `old.py` me permite obtener `new.py`
``` bash
diff -u old.py new.py > patch.diff
```
* El resultado en el fichero `path.diff` conocido como parche

```python
--- old.py      2013-08-21 20:35:32.609986892 +0200
+++ new.py      2013-08-21 20:35:45.981987447 +0200
@@ -1,4 +1,4 @@
 def foo(a):
-    print a % 2
+    print 'even' if a % 2 == 0 else 'odd'

 foo(22)
```



## ¿Qué es un parche? (III)
* Para comprobar si el parche va a funcionar correctamente (`--dry-run`)
``` bash
patch --dry-run old.py -i patch.diff
```
* Para aplicar el parche (`-i`)
``` bash
patch old.py -i patch.diff -o updated.py
```
* Para deshacer los cambios incluidos en el parche (`-R`)
``` bash
patch new.py -R -i patch.diff
```



## ¿Por qué parchear?

* **Actualizar de versión** sin necesidad de descargarse el núcleo completo
 * 2.6.35 kernel source tree  + patch-2.6.35.4.bz2  = 2.6.35.4 kernel source tree
 + patch-2.6.36.bz2 = 2.6.
* **Usa drivers** de terceros o experimentales
* Arreglar un **error** o añadir una nueva **característica**.



## ¿Cómo aplicar un parche? (I)
### Método 1

* Nos posicionamos en el directorio raíz que contiene el código fuente del núcleo a parchear (`/usr/src/linux`).
* Ejecutamos el comando:
```shell
gzip -cd ../patch-version.gz | patch -p1
```
si el parche se comprimió con la utilidad [`gzip`](http://www.gzip.org/), o
```shell
bzip2 -dc ../patch-version.bz2 | patch -p1
```
si se comprimió con la utilidad [`bzip2`](http://www.bzip.org/).



## ¿Cómo aplicar un parche? (II)
### Método 2

* Usamos el *script* propio del núcleo:
```shell
/usr/src/linux/scripts/patch-kernel linux
```
* Este *script* aplica todos los parches contenidos en el directorio actual al núcleo que se encuentre en el parámetro que se le pase.



## ¿Cómo saber si se ha aplicado adecuadamente?

* Si todo ha ido bien, deberíamos obtener:
```bash
$ patch old.py -i patch.diff -o updated.py
patching file old.py
```
* Si no se puede aplicar el parche saldrá:
```bash
$ patch old2.py -i patch.diff -o updated.py
patching file old2.py
Hunk #1 FAILED at 1.
1 out of 1 hunk FAILED -- saving rejects to file updated.py.rej
```
