---
layout: article
title: Pentesting - Elevación de privilegios
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

Durante la etapa de explotación se consigue acceso al sistema objetivo, en ocasiones con un usuario sin privilegios (no admin) y otras, con suerte, como el administrador del sistema.
* En linux, por defecto, como el usuario `root`.
* En windows, por defecto, como el usuario `nt authorized/system`.

Si nos encontramos con un usuario que no cuenta con altos privilegios, tendremos limitaciones para movernos por el sistema y al acceso de información, así como para realizar acciones que requieran privilegios de administrador. Pero, ante esto, podemos buscar fallos en el sistema, para poder ejecutar acciones con privilegios y obtener acceso como el usuario administrador, según el sistema operativo ante el que nos encontremos.

<h2><b>Flag SUID activado</b></h2>

¿No sabes qué es SUID?
Lectura obligatoria: '[Los bits SUID, SGID y sticky](https://www.ibiblio.org/pub/linux/docs/LuCaS/Manuales-LuCAS/doc-unixsec/unixsec-html/node56.html)'.

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Ficheros con flag SUID activado</div>
</div>

Tenemos la clásica búsqueda de binarios con el flag SUID activado.
Para agilizar la búsqueda, podemos hacer lo siguiente:

Descargar la herramienta [gtfo](https://github.com/mzfr/gtfo) que nos permitirá automatizar la búsqueda de binarios.

~~~bash
git clone https://github.com/mzfr/gtfo.git
cd gtfo
sudo ln -s $(pwd)/gtfo /usr/bin/gtfo # Añadir el binario a '/usr/bin'
~~~

Búsqueda de ficheros con el flag SUID activado:

~~~bash
find / -perm -4000 -type f 2>/dev/null
~~~

La salida de este fichero es algo parecido a lo siguiente (obviamente los binarios variarán).

~~~bash
/sbin/unix_chkpwd
/sbin/pam_timestamp_check
/sbin/pwdb_chkpwd
/usr/sbin/suexec
/usr/sbin/userhelper
/usr/bin/chage
/usr/bin/find
/usr/bin/rlogin
/bin/su
/bin/ping
/bin/umount
/bin/traceroute6
/bin/mount
~~~

Bucle `for` en bash para obtener información de todos los binarios:
¿De dónde sale el contenido del fichero `suid-bins.txt`?
De la clásica búsqueda de ficheros con el flag SUID activado.

~~~bash
for b in $(cat suid-bins.txt | awk -F'/' '{print $NF}'); do gtfo -b $b; done
~~~

<img src="/resources/output-images/gtfo-tool.png"/>

~~~bash
# Salida de "cat suid-bins.txt | awk -F'/' '{print $NF}'"
unix_chkpwd
pam_timestamp_check
pwdb_chkpwd
suexec
userhelper
chage
find
rlogin
su
ping
umount
traceroute6
mount
~~~








