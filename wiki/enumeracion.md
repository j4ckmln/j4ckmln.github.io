---
layout: article
title: Pentesting - Enumeración
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2>Reconocimiento de servicios</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH</div>
</div>

~~~bash
nmap -p 22 -n -v -sV -Pn --script ssh-auth-methods --script-args ssh.user=root 192.168.1.100
nmap -p 22 -n -v -sV -Pn --script ssh-hostkey 192.168.1.100
nmap -p 22 -n -v -sV -Pn --script ssh-brute --script-args userdb=/usr/share/wordlists/Seclists/Usernames/top-usernames-shortlist.txt,passdb=/usr/share/wordlists/Seclists/Passwords/darkweb2017-top10000.txt 192.168.1.100
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">FTP</div>
</div>

~~~bash
sudo nmap -v -p21 --script=default,vuln -oX ftp-scan.xml 192.168.0.0/24
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">DNS</div>
</div>

Nmap.
~~~bash
nmap -T4 -p 53 --script dns-brute 192.168.1.100
host -l domain.local 192.168.0.2 | grep "has address" | awk '{r=$4" "$1 ; print r}'
~~~

**Enumeración de servidores DNS**

Conocer los servidores DNS del dominio objetivo servirá para luego poder tratar de realizar una transferencia de zona, entre otras cosas. [DNS Zone transfer](https://www.acunetix.com/blog/articles/dns-zone-transfers-axfr/)
~~~bash
dig example.com ns
~~~

**Transferencia de zona**

Si el servidor tiene habilitada la transferencia de zona, nos permitirá obtener los subdominios del propio servidor, sin necesidad de intentar obtenerlos por fuerza bruta.
Dig.
~~~bash
dig @<name-server-of-target> <target-host-or-address> axfr
dig @ns1.example.com example.com axfr
~~~
~~~bash
host -t axfr domain.local ns1.domain.local
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">NFS</div>
</div>

~~~bash
sudo nmap -sV -v -p111 --script=default,vuln -oX nfs.xml 192.168.0.0/24
nmap -v -sV -p 111 --script=nfs-showmount 192.168.1.100
~~~

Si no se utiliza la opción de exportación squash, cualquier usuario root en el equipo cliente puede convertirse en un usuario con acceso privilegiado simplemente ejecutando la orden: su - . Conviene siempre tener activada alguna opción de squash.

~~~bash
rpcinfo -p 192.168.1.100
showmount -e 192.168.1.100
mkdir /mnt/nfs ; mount -t nfs -o nolock 192.168.1.100:/home/victim /mnt/nfs
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SMTP</div>
</div>

~~~bash
nmap --script smtp-enum-users.nse -p 25 192.168.1.100
smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.1.100
./ismtp.py -h 10.11.1.229:25 -l 1 -e /usr/share/metasploit-framework/data/wordlists/unix_users.txt
./ismtp.py -h 10.11.1.23 -i sender@example.com -x
~~~

[https://github.com/crunchsec/ismtp](https://github.com/crunchsec/ismtp)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">LDAP</div>
</div>

~~~bash
nmap --script=ldap-search -p 636,389 -v -oX ldap.xml -sV 192.168.0.0/24
~~~
~~~bash
ldapsearch -LLL -x -H ldap://<domain fqdn> -b '' -s base '(objectclass=*)'
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">RDP</div>
</div>

~~~bash
rdesktop -g 85% -r disk:share=/var/www -r clipboard:CLIPBOARD -u username -p password 192.168.1.10
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Netbios</div>
</div>

~~~bash
nbtscan 192.168.1.0/24
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Finger</div>
</div>

[https://github.com/Kan1shka9/Finger-User-Enumeration.git](https://github.com/Kan1shka9/Finger-User-Enumeration.git)

~~~bash
./finger_enum_user.sh users.txt
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">VPN</div>
</div>

[https://github.com/altjx/ipwn/tree/master/ike_aggressive_scanner](https://github.com/altjx/ipwn/tree/master/ike_aggressive_scanner)

~~~bash
sudo ike-scan -M 192.168.1.100
~~~

~~~bash
vncviewer 192.168.1.100::902
~~~

<h2>Aplicaciones web</h2>

<table class="table-full">
<tr>
<td class="td-red"><b>Puertos web más comunes</b></td>
</tr>
<tr>
<td>80,81,88,300,443,591,593,832,981,1010,1311,2082,2087,2095,2096,2480,3000,3128,3333,4243,4567,4711,4712,4993,5000,5104,5108,5800,6543,7000,7396,7474,8000,8001,8008,8014,8042,8069,8080,8081,8088,8090,8091,8118,8123,8172,8222,8243,8280,8281,8333,8443,8500,8834,8880,8888,8983,9000,9043,9060,9080,9090,9091,9200,9443,9800,9981,12443,16080,18091,18092,20720,28017</td>
</tr>
</table>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Información genérica</div>
</div>

**WhatWeb**

[https://github.com/urbanadventurer/WhatWeb.git](https://github.com/urbanadventurer/WhatWeb.git)

Recopilación de información básica de la web. Cookies, país, headers menos comunes, etc.

~~~bash
whatweb example.com
whatweb www.monsite.com -a 1 # Modo sigiloso
whatweb www.monsite.com -a 3 # Modo agresivo
~~~

**Nikto**

[https://github.com/sullo/nikto.git](https://github.com/sullo/nikto.git)

~~~bash
nitko -h http://domain.com
~~~

**ChopChop**

[https://github.com/michelin/ChopChop.git](https://github.com/michelin/ChopChop.git)

~~~bash
./gochopchop scan --url https://domain.com
./gochopchop scan --url https://domain.com --insecure
./gochopchop plugins --severity High
./gochopchop  scan --url https://domain.com --json --csv 
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Descubrimiento de directorios</div>
</div>

**Gobuster**

[https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)

~~~bash
gobuster dir -u http://192.168.1.100 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -o discov-80.txt
~~~

Si la web tiene certificado (https), se deberá utilizar, además, la opción '-k'

~~~bash
gobuster dir -u http://192.168.1.100 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -o discov-443.txt -k
~~~

**h2buster**

[https://github.com/00xc/h2buster](https://github.com/00xc/h2buster)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Captura de pantalla de sitios web</div>
</div>

**Aquatone**

[https://github.com/michenriksen/aquatone](https://github.com/michenriksen/aquatone)

~~~bash
cat iplist.txt | aquatone -ports xlarge -out webscreenshots
~~~

**EyeWitness**

[https://github.com/FortyNorthSecurity/EyeWitness.git](https://github.com/FortyNorthSecurity/EyeWitness.git)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Metadatos</div>
</div>

**FOCA**
[https://www.elevenpaths.com/es/labstools/foca-2/index.html](https://www.elevenpaths.com/es/labstools/foca-2/index.html)

**pyfoca**
[https://github.com/altjx/ipwn/tree/master/pyfoca](https://github.com/altjx/ipwn/tree/master/pyfoca)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">CMS</div>
</div>

~~~bash
wpscan --url http://192.168.1.100/ --enumerate vp,vt,u -o wpscanner
wpscan --url https://192.168.1.100/ -e vp,vt,u --disable-tls-checks
~~~


<h2>Recursos adicionales</h2>

[https://jonathansblog.co.uk/os-detection-techniques](https://jonathansblog.co.uk/os-detection-techniques)

[https://book.hacktricks.xyz/pentesting/ipsec-ike-vpn-pentesting](https://book.hacktricks.xyz/pentesting/ipsec-ike-vpn-pentesting)

