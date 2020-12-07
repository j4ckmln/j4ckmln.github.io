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
<h2>Escaneo de puertos</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Nmap</div>
</div>

**General**

~~~bash
nmap -sP 192.168.0.0/24 # Escáner Ping
nmap -T4 -F 192.168.1.100 -vvv # Escáner rápido
nmap -sV -T4 -O -F –version-light 192.168.1.100 -vvv # Escáner rápido más agresivo y que obtiene más información
nmap -sS -sU -PN -p T:80,T:445,U:161 192.168.1.100 # Escáner TCP Syn y UDP 
nmap -v -Pn -n -T4 -sT -sV --version-intensity=5 --reason 192.168.1.100
nmap -v -Pn -n -T4 -sT -p- --reason 192.168.1.100 # Escáner completo
nmap -v -Pn -T4 -A -p- --script=default,vuln -oA full-tcp 192.168.1.100 # Full TCP + scrips + output (mi fav)
nmap -v -T4 -sU -A -p- --script=default,vuln -oA full-udp 192.168.1.100 # Full UDP + scrips + output (mi fav)
~~~

**Descubrimiento de puertos**

~~~bash
nmap -sL 192.168.1.100 # Lista los hosts y obtiene el hostname
nmap -sn 192.168.1.100 # Sólo descubre puertos, no escanea
nmap -Pn 192.168.1.100 # Deshabilita el descubrimiento, sólo escanea, es útil cuando el firewall no permite las peticiones ICMP (ping)
nmap 192.168.1.100 -n # Deshabilita la resolución DNS
~~~

**Ejemplos de objetivos específicos**

~~~bash
nmap 192.168.1.100
nmap 192.168.1.100-150
nmap 192.168.1.0/24
nmap domain.com
nmap 192.168.1.0/24 --exclude 192.168.1.100
nmap -iL ips.txt # Lista de ip objetivo
~~~

**Técnicas**

~~~bash
nmap -sS 192.168.1.100 # Por defecto sin root, no completa la comunicación TCP, es más rápido que '-sT'
nmap -sT 192.168.1.100 # Por defecto con root, completa la comunicación TCP, por lo que es más lento
~~~

~~~bash
nmap -sU 192.168.1.100 # Escáner UDP
~~~

**Tiempos de las peticiones**

~~~bash
nmap 192.168.1.100 -F # Escáner rápido
nmap 192.168.1.100 -O --osscan-limit # Deshabilita la detección del sistema operativo si se encuentra al menos un puerto abierto
nmap 192.168.1.100 --host-timeout 10s
nmap 192.168.1.100 --scan-delay 2s
nmap 192.168.1.100 --max-scan-delay 5s
nmap 192.168.1.100 --max-retries 3
nmap 192.168.1.100 --min-rate 100 # Mínimo de paquetes por segundo
nmap 192.168.1.100 --max-rate 110 # Máximo de paquetes por segundo
~~~

**Velocidad del escáner**
<table class="table-full">
<tr>
<td class="td-red"><b>Modo</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>T0-T1</td>
<td class="table-full">Lento, pero útil ante IDS (Intrusion Detection Systems)</td>
</tr>
<tr>
<td>T2-T3</td>
<td class="table-full">Normal</td>
</tr>
<tr>
<td>T4-5</td>
<td class="table-full">Agresivo, adecuado para redes que soporten bastante tráfico, o CTFs</td>
</tr>
</table>

**Evasión de IDS**

~~~bash
nmap 192.168.1.100 -f # Fragmentar los paquetes
nmap 192.168.1.100 -mtu 30 # Determinar el tamaño del offset
nmap 192.168.1.100 -D 192.168.1.101 # Escaner desde IP falsa
nmap -S domainone.com domaintwo.com # Realizar un escaner a un dominio (domaintwo.com) desde otro (domainone.com)
nmap 192.168.1.100 -g 53 # Usar un puerto origen específico
nmap 192.168.1.100 --proxies http://X.X.X.X:8080 # Utilizar un proxy
nmap 192.168.1.100 --data-length 150 # Enviar paquetes con datos aleatorios
~~~

**Salida**

~~~bash
nmap 192.168.1.100 -oX scan.xml # Salida xml
nmap 192.168.1.100 -oA scan # Todas las salidas (normal (oN), xml (oX), grepable (oG))
~~~

Convertir XML a HTML

~~~bash
xsltproc scan.xml -o scan.html
~~~

<h2>Reconocimiento de servicios</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">FTP</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>21</td>
<td class="table-full">Puerto común (puerto de control)</td>
</tr>
<tr>
<td>20</td>
<td class="table-full">Puerto para transferencia de datos</td>
</tr>
</table>

~~~bash
sudo nmap -v -p21 --script=default,vuln -oA ftp-scan 192.168.0.0/24
~~~

Intento de conexión como anonymous y banner (Banner grabbing):

~~~bash
ftp <ip>
> USER anonymous
> PASS loquesea
~~~

Lo mismo que antes, pero en modo pasivo:

~~~bash
ftp -p <ip>
> USER anonymous
> PASS loquesea
~~~

Conexión vía telnet:

~~~bash
telnet <ip> 21
anonymous
loquesea
~~~

Conexión vía web o carpetas:

~~~bash
ftp://ip
~~~

Ataque por fuerza bruta a servicio FTP:

<a href="fuerza-bruta#ftp-brute">Fuerza bruta FTP</a>

Montar FTP localmente:

~~~bash
mkdir /tmp/ftp-remoto
curlftpfs <usuario>:<contraseña>@<ip> /tmp/ftp-remoto
curlftpfs -o allow_other <usuario>:<contraseña>@<ip> /tmp/ftp-remoto # Permitir otros usuarios
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>22</td>
<td class="table-full">Puerto común (puerto de control)</td>
</tr>
<tr>
<td>22000,2000</td>
<td class="table-full">Otros puertos que suelen usarse</td>
</tr>
</table>

~~~bash
nmap -p 22 -n -v -sV -Pn --script ssh-auth-methods --script-args ssh.user=root 192.168.1.100
nmap -p 22 -n -v -sV -Pn --script ssh-hostkey 192.168.1.100
nmap -p 22 -n -v -sV -Pn --script ssh-brute --script-args userdb=/usr/share/wordlists/Seclists/Usernames/top-usernames-shortlist.txt,passdb=/usr/share/wordlists/Seclists/Passwords/darkweb2017-top10000.txt 192.168.1.100
~~~

Banner (banner grabbing):

~~~
telnet <ip> 22
~~~

Conexión vía OpenSSL:

~~~
openssl s_client -connect <ip>:<port>
~~~

Ataque por fuerza bruta a servicio SSH:

<a href="fuerza-bruta#ssh-brute">Fuerza bruta SSH</a>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">DNS</div>
</div>

**Nmap**
~~~bash
nmap -T4 -p 53 --script dns-brute 192.168.1.100
host -l domain.local 192.168.1.100 | grep "has address" | awk '{r=$4" "$1 ; print r}'
~~~

**Enumeración de servidores DNS**

Conocer los servidores DNS del dominio objetivo servirá para luego poder tratar de realizar una transferencia de zona, entre otras cosas. [DNS Zone transfer](https://www.acunetix.com/blog/articles/dns-zone-transfers-axfr/)
~~~bash
dig example.com ns
~~~

**Transferencia de zona**

Si el servidor tiene habilitada la transferencia de zona, nos permitirá obtener los subdominios del propio servidor, sin necesidad de intentar obtenerlos por fuerza bruta.

* Dig
~~~bash
dig @<NS> <target-host-or-address> axfr
dig @ns1.example.com example.com axfr
dig @ns1.example.com -p <puerto> example.com axfr
~~~

* Host
~~~bash
host -t axfr domain.local ns1.domain.local
~~~

* Nslookup
~~~bash
nslookup -type=any
> set port <port>
> server <ns>
> <host>
~~~

* Nmap
~~~bash
nmap --script=dns-transfer-zone -p 53 domain
~~~

**Active Directory DNS**

* Dig
~~~bash
dig -t SRV _gc._tcp.domain.com # Global catalog
dig -t SRV _ldap._tcp.domain.com # LDAP Server
dig -t SRV _kerberos._tcp.domain.com # Kerberos KDC
dig -t SRV _kpasswd._tcp.domain.com # Kerberos password change
~~~

* Nmap
~~~bash
nmap --script dns-srv-enum --script-args “dns-srv-enum.domain='domain.com'”
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">POP3</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>110</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

**Nmap**

~~~bash
nmap -v -sV --version-intensity=5 --script pop3-capabilities -p 110 <IP> # S (CAPA, TOP, USER, SASL, RESP-CODES, LOGIN-DELAY, PIPELINING, EXPIRE, UIDL, IMPLEMENTATION)
nmap --script pop3-brute --script-args pop3loginmethod=SASL-LOGIN -p 110 <IP> # POP3 Accounts bruteforce
nmap --script pop3-brute --script-args pop3loginmethod=SASL-CRAM-MD5 -p 110 <IP>
nmap --script pop3-brute --script-args pop3loginmethod=APOP -p 110 <IP>
~~~

[POP3 Extension Mechanism](https://tools.ietf.org/html/rfc2449)

**Banner (Banner grabbing)**

* Netcat
~~~bash
nc <IP> 110
~~~

* Telnet
~~~bash
telnet <IP> 110
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">NTP</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>119</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

**Banner (Banner grabbing)**

* Netcat
~~~bash
nc -nv <IP> 119
~~~

* Telnet
~~~bash
telnet <IP> 119
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">RPC</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>135</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

**rpcclient**

* Conexión como anonymous
~~~bash
rpcclient -U "" -N <ip> # -N para acceder sin introducir contraseña
~~~

* Obtener información sobre el DC
~~~bash
srvinfo
~~~

* Obtener información sobre objetos como grupos
~~~bash
enumdomains
enumdomgroups
enumalsgroups builtin
~~~

* Obtener política de contraseña del dominio
~~~bash
getdompwinfo
~~~

* Enumerar dominios confiables
~~~
dsr_enumtrustdom
~~~

* Buscar usuario, grupo, etc
~~~
queryuser RID
querygroupmem 519
queryaliasmem builtin 0x220
lsaquery
~~~

* Convertir SID en nombres
~~~
lookupsids SID
~~~

[http://attackerkb.com/Windows/rpcclient](http://attackerkb.com/Windows/rpcclient)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Netbios</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>139</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

~~~bash
nbtscan <IP> # Identifica el host y dominio
nbstat
nmblookup -A <IP>
~~~


<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SMB</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>445</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

Sistemas operativos comunes de acuerdo a la versión de SMB que utilice.

<table class="table-full">
<tr>
<td class="td-red"><b>Versión</b></td>
<td class="td-red"><b>Sistemas operativos comunes</b></td>
</tr>
<tr>
<td>SMB1</td>
<td class="table-full">Windows 2000 / XP / 2003</td>
</tr>
<tr>
<td>SMB2.0</td>
<td class="table-full">Windows Vista / 2008</td>
</tr>
<tr>
<td>SMB2.1</td>
<td class="table-full">Windows 7 / 2008 R2</td>
</tr>
<tr>
<td>SMB3.0</td>
<td class="table-full">Windows 8 / 2012</td>
</tr>
<tr>
<td>SMB3.02</td>
<td class="table-full">Windows 8.1 / 2012 R2</td>
</tr>
<tr>
<td>SMB3.1.1</td>
<td class="table-full">Windows 10 / 2016</td>
</tr>
</table>

* Lista recursos compartidos
~~~bash
smbclient -L //<IP>
smbclient -L <IP>
~~~

* Conexión
~~~bash
smbclient \\\\x.x.x.x\\share
smbclient -U “DOMAINNAME\Username” \\\\IP\\IPC$ password
~~~

* Acceso sin contraseña
~~~bash
smbclient -U “” -N \\\\IP\\IPC$
~~~

* Nullinux para usuarios y recursos compartidos
~~~bash
nullinux -users -quick dc.domain.com
nullinux -all 192.168.1.0-100
nullinux -shares -U 'Domain\User' -P 'password' 192.168.1.100
~~~

~~~bash
smb-enum-domains
smb-enum-groups
smb-enum-processes
smb-enum-sessions
smb-os-discovery
smb-server-stats
smb-system-info
~~~

~~~bash
smb-ls # Intenta obtener información sobre ficheros compartidos
smb-mbenum # Enumera información manejada por Windows Master Browser
smb-print-text
smb-security-mode # Obtener información sobre el nivel de seguridad de SMB
~~~

**enum4linux**

~~~bash
enum4linux -v <IP> # Modo verbose
enum4linux -a <IP> # Hace todo
enum4linux -U <IP> # Lista los usuarios
enum4linux -u administrator -p password -U <IP> # Acceso con usuario y contraseña
enum4linux -r <IP> # Obtiene el nombre de usuario del rango RID (500-550, 1000-1050)
enum4linux -R 600-660 <IP> # Obtener nombre de usuario
enum4linux -G <IP> # Lista los grupos
enum4linux -S <IP> # Lista los recursos compartidos
enum4linux -s shares.txt <IP> # Ataque por diccionario para obtener los recursos compartidos
enum4linux -o <IP> # Obtener información del SO
enum4linux -i <IP> # Obtener información sobre las impresoras
~~~

**smbmap**

~~~
python smbmap.py -u <user> -p <password> -d workgroup -H 192.168.1.100
python smbmap.py -u <user> -p <password> -d ACME -H 192.168.1.100 -x 'net group "Domain Admins" /domain'
python smbmap.py -H 192.168.1.100 -u <user> -p <password> -r 'C$\Users'
~~~

[https://github.com/ShawnDEvans/smbmap.git](https://github.com/ShawnDEvans/smbmap.git)

<p style="color:#872d17"><b>Danger! Puede tirar el servidor:</b></p>

~~~bash
smb-vuln-conficker # Puede tirar el host objetivo
smb-vuln-ms06-025
smb-vuln-ms07-029
smb-vuln-ms08-067
smb-vuln-ms10-054
~~~

Estos supuestamente no tiran el host objetivo:
~~~bash
smb-vuln-ms10-061
smb-vuln-ms17-010
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">IMAP</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>143</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

* Nmap
~~~
nmap -v -sV --version-intensity=5 --script imap-capabilities -p 143 <IP>
~~~

**Banner**

* Telnet
~~~
telnet <IP> 143
~~~

* Netcat
~~~
nc -nv <IP> 143
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">IMAP</div>
</div>

<table class="table-full">
<tr>
<td class="td-red"><b>Puerto</b></td>
<td class="td-red"><b>Descripción</b></td>
</tr>
<tr>
<td>143</td>
<td class="table-full">Puerto común</td>
</tr>
</table>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">NFS</div>
</div>

~~~bash
sudo nmap -sV -v -p111 --script=default,vuln -oX nfs.xml 192.168.1.0/24
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

**Nmap**

~~~bash
nmap --script smtp-enum-users.nse -p 25 192.168.1.100
~~~

**smtp-user-enum**
~~~
smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.1.100
~~~

**ismtp**
[https://github.com/crunchsec/ismtp](https://github.com/crunchsec/ismtp)

~~~bash
./ismtp.py -h 10.11.1.229:25 -l 1 -e /usr/share/metasploit-framework/data/wordlists/unix_users.txt
./ismtp.py -h 10.11.1.23 -i sender@example.com -x
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">LDAP</div>
</div>

~~~bash
nmap --script=ldap-search -p 636,389 -v -oX ldap.xml -sV 192.168.1.0/24
~~~
~~~bash
ldapsearch -LLL -x -H ldap://<domain fqdn> -b '' -s base '(objectclass=*)'
ldapsearch -h "<hostname>" -p "<puerto>" -x -b "ou=<OU>,DC=<DC>,DC=<DC>,DC=<DC>" -v

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

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">VNC</div>
</div>

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

**Nmap**

~~~bash
nmap -Pn -n -T3 -v -sV --version-intensity=5 -Pn -p 80 --script=http-iis-webdav-vuln <IP> # ISS
nmap -Pn -n -T3 -v -sV --version-intensity=5 -Pn -p 80 --script=http-vuln-cve2010-0738 <IP> # JBOSS (CVE-2010-0738)
nmap -Pn -n -T3 -v -sV --version-intensity=5 -Pn -p 80 --script=http-vuln-cve2012-1823 <IP> # PHP-CGI (CVE-2012-1823)
nmap -Pn -n -T3 -v -sV --version-intensity=5 -Pn -p 80 --script=http-vuln-cve2013-0156 <IP> # RCE Ruby on Rails (CVE-2013-0156)
~~~

<div id="nmap-heartbleed"></div>

~~~bash
nmap -Pn -n -p 443 -v -T3 --script=ssl-heartbleed,ssl-enum-ciphers,ssl-known-key --script-args vulns.showall -sV --version-intensity=5 <IP> # Heartbleed
~~~

Ver detección y explotación de heartbleed con metasploit: <a href="enumeracion-con-metasploit#metasploit-heartbleed">Heartbleed con metasploit</a>

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

**CMSeek**

[https://github.com/Tuhinshubhra/CMSeeK.git](https://github.com/Tuhinshubhra/CMSeeK.git)

Analiza todos los tipos de CMS existentes hasta el momento.

~~~bash
python3 cmseek.py -u example.com
~~~

**CMSmap**

[https://github.com/Dionach/CMSmap.git](https://github.com/Dionach/CMSmap.git)

~~~
cmsmap.py https://example.com
cmsmap.py https://example.com -f W -F --noedb -d
cmsmap.py https://example.com -i targets.txt -o output.txt
~~~

**WPscan**

~~~bash
wpscan --url http://192.168.1.100/ --enumerate # Enumera todo
wpscan --url http://192.168.1.100/ --enumerate vp,vt,u -o wpscanner #Enumera vulnerabilidades en plugins (vp), vulnerabilidades en temas (vt) y nombres de usuarios
wpscan --url https://192.168.1.100/ -e vp,vt,u --disable-tls-checks # Lo mismo que el anterior, pero en caso de que la web tenga certificado TLS
~~~

**Joomscan**

[https://github.com/rezasp/joomscan.git](https://github.com/rezasp/joomscan.git)

~~~bash
perl joomscan.pl --url www.example.com
perl joomscan.pl -u www.example.com --random-agent # Utiliza un 'User-agent' aleatorio
~~~

**droopescan**

[https://github.com/droope/droopescan.git](https://github.com/droope/droopescan.git)

~~~
droopescan scan drupal -u http://example.org/ -t 32
droopescan scan drupal -U lista__urls.txt
~~~

Si se ejecuta sin 'drupal' tratará de identificar el CMS.

~~~
droopescan scan -u example.org
~~~

**Drupwn**

[https://github.com/immunIT/drupwn.git](https://github.com/immunIT/drupwn.git)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Detección de WAF</div>
</div>

Existen varios métodos y herramientas para averiguar si una aplicación web utiliza un WAF, y en caso afirmativo, cuál.

Algunas consideraciones generales:
* Comprobar los registros DNS.
* Búsqueda vía la propia web objetivo.
* Reconocimiento de puertos.
* Revisión de los certificados TLS.

<table class="table-full">
<tr>
<td class="td-red"><b>WAF</b></td>
<td class="td-red"><b>Puertos comunes</b></td>
</tr>
<tr>
<td>Checkpoint</td>
<td class="table-full">264,18264</td>
</tr>
<tr>
<td>Sonicwall</td>
<td class="table-full">4443</td>
</tr>
<tr>
<td>Sophos</td>
<td class="table-full">4443</td>
</tr>
<tr>
<td>Cisco-VPN</td>
<td class="table-full">500 (UDP)</td>
</tr>
</table>

Ejemplo de búsqueda de dispositivos con el puerto 500 abierto (comúnmente de cisco): [https://www.zoomeye.org/searchResult?q=port%3A500%20isakmp%20%2Bafter:%222019-01-01%22%20%2Bbefore:%222020-01-01%22&t=all](https://www.zoomeye.org/searchResult?q=port%3A500%20isakmp%20%2Bafter:%222019-01-01%22%20%2Bbefore:%222020-01-01%22&t=all)

**Nmap**

~~~bash
nmap -Pn -n -T3 -v -sV --version-intensity=5 -Pn -p 80 --script=http-waf-detect,http-waf-fingerprint <IP> # Detección
~~~

<table class="table-full">
<tr>
<td class="td-red"><b>WAF</b></td>
<td class="td-red"><b>Paths y redirecciones comunes</b></td>
</tr>
<tr>
<td>Fortinet-VPN-Gateways</td>
<td>/remote/login</td>
</tr>
<tr>
<td>Cisco-VPN</td>
<td>/+CSCOE+/logon.html</td>
</tr>
<tr>
<td>Netscaler</td>
<td>/vpn/index.html</td>
</tr>
<tr>
<td>Pulse-VPN-Gateways</td>
<td>/dana-na/</td>
</tr>
</table>


[OSINT Basics: Detecting and Enumerating Firewalls & Gateways](https://www.secjuice.com/osint-detecting-enumerating-firewalls-gateways/)


<h2>Recursos adicionales</h2>

[https://jonathansblog.co.uk/os-detection-techniques](https://jonathansblog.co.uk/os-detection-techniques)

[https://book.hacktricks.xyz/pentesting/ipsec-ike-vpn-pentesting](https://book.hacktricks.xyz/pentesting/ipsec-ike-vpn-pentesting)

[https://docs.binaryedge.io/search/#certissuerorganizationname-string](https://docs.binaryedge.io/search/#certissuerorganizationname-string)