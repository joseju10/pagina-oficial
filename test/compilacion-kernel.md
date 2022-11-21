## Instalar Dependencias

---

En este punto lo primero que haremos será instalar los paquetes necesarios para poder compilar el kernel y posteriormente crear el fichero .deb correspondiente.

Primero actualizamos:

```shell
joseju@debian:~$ sudo apt update
```

Instalamos las dependencias:
```shell
joseju@debian:~$ sudo apt install build-essential qtbase5-dev libelf-dev pkg-config    
```

---

## Descargar Kernel

---

Una vez hemos instalado las correspondientes dependencias, descargaremos el archivo del kernel:

```shell
joseju@debian:~/compilacion_kernel$ wget https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x/linux-6.0.7.tar.xz
```

---Reducir módulos del Kernel

joseju@debian:~/compilacion_kernel$ tar -xvf linux-6.0.7.tar.xz
```

Posteriormente, accedemos a la carpeta y compilamos el archivo oldconfig:

```shell
joseju@debian:~/compilacion_kernel/linux-6.0.7$ make oldconfig
```
- Al compilarlo nos hará muchas preguntas, dejamos pulsado intro ya que por defecto indicamos que no.

Una vez hecho esto, se nos habrá generado un fichero oculto llamado .config:

```shell
usuario@debian:~/compilacion_kernel/linux-6.0.7$ ls -la | egrep '.config'
-rw-r--r--   1 usuario usuario     59 nov  3 16:00 .cocciconfig
-rw-r--r--   1 usuario usuario 252431 nov 17 14:12 .config
-rw-r--r--   1 usuario usuario    555 nov  3 16:00 Kconfig
```

En este fichero se almacenan los módulos que vienen por defecto y que posteriormente deberemos ir desactivando, pero este no es el caso, asi que prodeceremos a compilar este archivo para reducir módulos automáticamente:

```shell
usuario@debian:~/compilacion_kernel/linux-6.0.7$ make localmodconfig
```

---

## Crear imagen .deb

---

Ahora que hemos reducido los módulos, procederemos a generar nuestra imagen .deb del kernel, para ello:

```shell
usuario@debian:~/compilacion_kernel/linux-6.0.7$ make -j12 bindeb-pkg
```

- Esto tardará un rato ya que antes de reducir considerablemente el número de módulos, se tienen que cargar los que vienen por defecto.

---

## Instalar archivo .deb creado anteriormente

---

Primero, verificamos la versión del kernel que tenemos actualmente en funcionamiento:

```shell
usuario@debian:~$ uname -r
5.10.0-18-amd64
```

Ahora que hemos compilado y generado el fichero .deb, procederemos a instalar el fichero .deb que se nos ha generado y este se añadirá al grub como una nueva versión de kernel._

```shell
usuario@debian:~/compilacion_kernel/linux-6.0.7$ sudo dpkg -i ../linux-image-6.0.7_6.0.7-1_amd64.deb
```

En este punto reiniciamos la máquina y comprobamos que la nueva versión del kernel funciona correctmente

```shell
usuario@debian:~$ sudo reboot
```

---

## Reducir módulos del Kernel

---

Primero, comprobamos la versión de kernel actual tras el reinicio:

```shell
usuario@debian:~$ uname -r
6.0.7
```

Ahora que tenemos cargada la nueva versión de kernel en nuestro sistema operativo, debemos reducir el número de módulos a medida de las características del hardware de nuestro equipo.

Por ejemplo nuestro equipo funciona con un procesador intel, pues los módulos que sean amd los podemos desactivar.

Antes de editar los módulos, comprobamos los módulos que tiene actualmente el fichero .config:

```shell
usuario@debian:~/compilacion_kernel/linux-6.0.7$ egrep '=y' .config | wc -l
1627
usuario@debian:~/compilacion_kernel/linux-6.0.7$ egrep '=m' .config | wc -l
193
```

Para editar los módulos del kernel ejecutamos el siguiente comando:

```shell
usuario@debian:~/compilacion_kernel/linux-6.0.7$ make xconfig
```

Nos aparecerá una ventana gráfica donde deberemos ir desmarcando los módulos que sean necesarios.
