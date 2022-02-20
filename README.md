# Practica 1 DSI

## Índice
1. Introducción
2. Ralización de tareas previas
3. Configuración de la máquina virtual en el IaaS
4. Instalación de git y Node.js en la máquina virtual del IaaS

## 1. Introducción

En está práctica se configurará la máquina virtual del IaaS e instalación de heramientas necesarias para la realización de prácticas posteriores.


## 2. Relización de tareas previas

* Realizar encuesta sobre elección de grupo.
* Complementar encuesta sobre expectativa y conocimientos previos.
* Solicitar beneficios de estudiantes de GitHub Education.
* Darse de alta en GitHub Classroom.
* Verse vídeotutoriales sobre git, Markdown, GitHub Pages puesto no se tenían conocimiento previos sobre ellos.

## 3. Configuración de la máquina virtual en el IaaS

Antes de comenzar deberemos verificar si estamos conectados a al servicio VPN de la ULL en el que caso que nos encontremos fuera de la red universitaria. 

Una vez hecho esto nos dirigeremos a al [Servicio IaaS de la ULL](https://iaas.ull.es) y nos logaremos con nuestras credenciales.

![Login IaaS](https://gyazo.com/0bb18af32c40b6fd932911acce24e7e4.png)

Ejecutaramos la máquina virtual y abriremos la consola del explorador y ejecutaremos el siguiento comando.

```
usuario@ubuntu:~$ ifconfig -a
ens3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.6.128.245  netmask 255.255.252.0  broadcast 10.6.131.255
        inet6 fe80::21a:4aff:fe97:5151  prefixlen 64  scopeid 0x20<link>
        ether 00:1a:4a:97:51:51  txqueuelen 1000  (Ethernet)
        RX packets 976  bytes 77804 (77.8 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 86  bytes 12076 (12.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 84  bytes 6384 (6.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 84  bytes 6384 (6.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Con este comando obtendriamos la dirección ip de la máquina con la cual podremos hacer una conexión SSH.

```
orlando@ubuntu:~$ ssh usuario@10.6.128.245
```

Procederemos a cambiar el nombre del host con las siguiente líneas de comando.

```
usuario@ubuntu:~$ cat /etc/hostname
ubuntu
usuario@ubuntu:~$ sudo vi /etc/hostname
usuario@ubuntu:~$ cat /etc/hostname
iaas-dsi
```

También debemos cambiar el nombre en el fichero /etc/hosts tal como en se ve en:

```
usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...

usuario@ubuntu:~$ sudo vi /etc/hosts

usuario@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	iaas-dsi
...
```

Para que se hagan actualicen los cambios reiniciaremos la máquina:

```
usuario@ubuntu:~$ sudo reboot
Connection to 10.6.128.245 closed by remote host.
Connection to 10.6.128.245 closed.
```

Para no tener que acordarnos de la ip todas la veces que queramos conectarnos a la máquina virtual editaremos el fichero /etc/hosts añadiendo la línea con la ip de la máquina y por el nombre por el cual lo queramos sustituir tal como se ve abajo:

```
orlando@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu
...

orlando@ubuntu:~$ sudo vi /etc/hosts

orlando@ubuntu:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ubuntu

10.6.128.245	iaas-dsi
...
```

Ahora configaremos en la infraestructura de la clave pública-privada en la máquina local con el siguiente comando:

```
orlando@ubuntu:~$ ssh-keygen
```

Tomaremos las opciones por defecto en cada paso del script de generación de claves. Como resultado se habra generado la clave pública (id_rsa.pub) y la clave privada (id_rsa).

```
orlando@ubuntu:~$/.ssh$ ls -a
.  ..  config  id_rsa  id_rsa.pub  known_hosts
```

Copiaremos la clave pública desde la máquina local a la máquina virtual con el siguiente comando:

```
orlando@ubuntu:~$ ssh-copy-id usuario@iaas-dsi
The authenticity of host 'iaas-dsi (10.6.128.245)' can't be established.
ECDSA key fingerprint is SHA256:1Xm4M66FeBUSiykP7SqJgObwjmVs2gEouBhy1PTWDV4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
usuario@iaas-dsi's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'usuario@iaas-dsi'"
and check to make sure that only the key(s) you wanted were added.
```

Si intentamos hacer una conexión SSH, pero en vez de poner la ip (10.6.128.245) ponemos el nombre del host (iaas-dsi) nos debería acceder a la máquina virtual sin necesidad de introducir la contraseña.

```
orlando@ubuntu:~$ ssh usuario@iaas-dsi
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-99-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Feb 19 17:15:57 UTC 2022

  System load:  0.01               Processes:             149
  Usage of /:   38.5% of 19.56GB   Users logged in:       0
  Memory usage: 6%                 IPv4 address for ens3: 10.6.128.245
  Swap usage:   0%

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

0 updates can be applied immediately.

Last login: Fri Feb 18 22:02:17 2022
```

Generaremos las claves pública-privada en la máquina virtual también con los mismos pasos explicados anteriormente.

## 4. Instalación de git y Node.js en la máquina virtual del IaaS

Instalaremos git en la máquina virtual con el siguiente comando:

```
usuario@iaas-dsi:~$ sudo apt install git
```

Configuraremos git por primera vez con los siguientes comandos:

```
usuario@iaas-dsi:~$ git config --global user.name "JoseOrlandoNinaOrellana"
usuario@iaas-dsi:~$ git config --global user.email alu0101322308@ull.edu.es
usuario@iaas-dsi:~$ git config --list
user.name=JoseOrlandoNinaOrellana
user.email=alu0101322308@ull.edu.es
```

Configuraremos el prompt de la terminal descargando el script [git prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh) y editaremos el archivo .bashrc añadiendo las dos líneas al final del mismo y reiniciaremos la terminal para que se acualicen los cambios: 

```
usuario@iaas-dsi:~$ mv git-prompt.sh .git-prompt.sh
usuario@iaas-dsi:~$ vi .bashrc
usuario@iaas-dsi:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi:~$ exec bash -l
[~()]$
```

Si se ha hecho lo anterior correctamente podremos clonar el repositorio tal como se ve en el siguiente comando:

```
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct01-iaas-JoseOrlandoNinaOrellana.git
```

Ahora vamos a instalar Node Version Manger, el gestor de versiones de Node.js con el siguiente comando:

```
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
[~()]$exec bash -l
[~()]$nvm --version
0.39.1
```

Hecho esto, vamos a instalar la versión más reciente de Node.js:

```
[~()]$nvm install node
```

Vemos que la versión del Node.js que se ha instalado y la versión de Node Package Manager para verificar que se ha descargado correctamente:

```
[~()]$node --version
v17.4.0
[~()]$npm --version
8.3.1
```
