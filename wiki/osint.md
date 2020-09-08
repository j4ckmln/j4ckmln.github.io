---
layout: article
title: OSINT
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2><b>Información general</b></h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Información del servidor</div>
</div>

**Información de la IP**

Herramientas de búsqueda de información relacionada con una ip, dominio, etc.

[https://www.robtex.com](https://www.robtex.com)

[http://ipv4info.com/](http://ipv4info.com/) 

[https://www.yougetsignal.com/](https://www.yougetsignal.com/)

[https://www.onyphe.io/](https://www.onyphe.io/)

[https://www.zoomeye.org/](https://www.zoomeye.org/)

Whois.
~~~bash
whois <IP>
~~~

**Búsqueda de la IP real detrás de Cloudflare o Tor**

Búsqueda de la IP del certificado SSL entre el servidor y cloudflare.

[https://censys.io/](https://censys.io/)

~~~
parsed.names: example.com and tags.raw: trusted
~~~

[https://github.com/m0rtem/CloudFail.git](https://github.com/m0rtem/CloudFail.git)

~~~
python3 cloudfail.py --target seo.com
python3 cloudfail.py --target seo.com --tor
~~~


**IntelligenceX**

[https://intelx.io/](https://intelx.io/)

Es una herramienta online que contiene distinta información sensible en relación a un dominio, ip, url, etc etc

A continuación, se muestran algunos ejemplos interesantes:

* Credenciales (ej: credenciales de correos)

    [https://intelx.io/?s=google.es](https://intelx.io/?s=google.es)

* IP's reales de sitios detrás de cloudflare (ej: 2020-07-25)

    [https://intelx.io/?did=734ac30d-caa9-4348-94ad-106cd5f940c2](https://intelx.io/?did=734ac30d-caa9-4348-94ad-106cd5f940c2)

**theHarvester**

[https://github.com/laramies/theHarvester.git](https://github.com/laramies/theHarvester.git)

Es utilizado para recopilar información, subdominios, direcciones de correo, nombres de empleados, puertos abiertos, etc.

~~~bash
theHarvester -d example.com -b google
theHarvester -d example.com -b linkedin
theHarvester -d example.com -b all
~~~

**Wolfram Alpha**

[https://www.wolframalpha.com/](https://www.wolframalpha.com/)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Subdominios</div>
</div>

**SubBrute**

[https://github.com/TheRook/subbrute.git](https://github.com/TheRook/subbrute.git)

~~~bash
./subbrute.py -p example.com
./subbrute.py -p example.com -r
~~~

**DNSDumpster**

[https://dnsdumpster.com/](https://dnsdumpster.com/)

**Pentest Tools**

[https://pentest-tools.com/](https://pentest-tools.com/)

**CRT.sh**

[https://crt.sh/](https://crt.sh/)

~~~bash
https://crt.sh/?q=%25.domain.com
~~~

**Knockknock**

[https://github.com/harleo/knockknock.git](https://github.com/harleo/knockknock.git)

~~~bash
knockknock -n google.com -p
~~~

**Turbolist3r**

[https://github.com/fleetcaptain/Turbolist3r](https://github.com/fleetcaptain/Turbolist3r)

**Virustotal**

[https://www.virustotal.com/gui/home/search](https://www.virustotal.com/gui/home/search)

**A la vieja usanza :eyes:**

Resolución DNS de IPs.
~~~bash
for x in {1..255}; do host 3.24.156.$x; done
~~~

Búsqueda de nuevos dominios de una IP.

[http://www.ipvoid.com/](http://www.ipvoid.com/)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Puertos abiertos</div>
</div>

**Shodan**

[https://www.shodan.io/](https://www.shodan.io/)

~~~bash
https://beta.shodan.io/search/filters
city:"San Diego,Austin"  
country:"ES"
geo:"21.454373,56.540394"
server: "gws" hostname:"google"
net:210.214.0.0/16
os:"windows 10"
port:22
port:<=1024 # Rangos de puertos
port:<1024,>6000
port:22 product:OpenSSH  
~~~

[https://github.com/jakejarvis/awesome-shodan-queries](https://github.com/jakejarvis/awesome-shodan-queries)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Búsqueda de vulnerabilidades/deficiciencias</div>
</div>

**Drupal**

[https://hackertarget.com/drupal-security-scan/](https://hackertarget.com/drupal-security-scan/)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Google Dorks</div>
</div>

[https://www.exploit-db.com/google-hacking-database](https://www.exploit-db.com/google-hacking-database)

[https://gbhackers.com/latest-google-dorks-list/](https://gbhackers.com/latest-google-dorks-list/)

[http://www.disoftin.com/2020/06/google-dorks-2020.html](http://www.disoftin.com/2020/06/google-dorks-2020.html)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Github</div>
</div>

[https://github.com/michenriksen/gitrob](https://github.com/michenriksen/gitrob)

[https://github.com/zricethezav/gitleaks](https://github.com/zricethezav/gitleaks)

**Dorks**

~~~
extension:pem private # Claves privadas SSH
extension:sql mysql dump # Dumps de mysql
extension:sql mysql dump password # Dumps de mysql con contraseñas
filename:wp-config.php # Fichero de configuración de wordpress
filename:.htpasswd # .htpasswd
filename:.git-credentials # Credeciales de git
filename:.bashrc password # Ficheros .bashrc con contraseñas
filename:.bash_profile aws # Claves de AWS en .bash_profiles
extension:json domain.com # Claves y credenciales de un dominio
HEROKU_API_KEY language:json # Claves de la API Heroku
filename:filezilla.xml Pass # Credenciales FTP
filename:recentservers.xml Pass # Credenciales FTP
filename:config.php dbpasswd # Crdenciales PHP
shodan_api_key language:python # Claves de la API de Shodan. Iterar con otros lenguajes
filename:logins.json # Claves guardadas de Firefox (key3.db)
filename:settings.py SECRET_KEY # Claves de Django (usually allows for session hijacking, RCE, etc)
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Investigación de personas</div>
</div>

En estos cheatsheets existen distintos métodos y búsquedas de información en relación a una 'X' persona, correos, nombres de usuarios, redes sociales, etc.

[https://hack2interesting.com/osint-cheat-sheet/](https://hack2interesting.com/osint-cheat-sheet/)

[https://www.compass-security.com/fileadmin/Datein/Research/White_Papers/osint_cheat_sheet.pdf](https://www.compass-security.com/fileadmin/Datein/Research/White_Papers/osint_cheat_sheet.pdf)

[https://inteltechniques.com/JE/OSINT_Packet_2019.pdf](https://inteltechniques.com/JE/OSINT_Packet_2019.pdf)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Metodologías de OSINT</div>
</div>

[https://cheatsheet.haax.fr/open-source-intelligence-osint/tools-and-methodology/methodology/](https://cheatsheet.haax.fr/open-source-intelligence-osint/tools-and-methodology/methodology/)

<h2><b>Leaks de credenciales</b></h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Credenciales</div>
</div>

**PwnedOrNot**

[https://github.com/thewhiteh4t/pwnedOrNot.git](https://github.com/thewhiteh4t/pwnedOrNot.git)

~~~bash
python pwnedornot.py -e email@test.com
python pwnedornot.py -f file.txt
~~~

**pwndb.py**

Busca credenciales expuestas utilizando un servicio onion 'http://pwndb2am4tzkvold.onion/' (Tor).

[https://github.com/davidtavarez/pwndb.git](https://github.com/davidtavarez/pwndb.git)

~~~bash
python pwndb.py --target @domain.com #Busca las credenciales de todos los emails de domain.com
python pwndb.py --target pepe@domain.com #Busca las credenciales de un email específico
~~~

~~~bash
python pwndb.py --list emails_file.txt
~~~

Ejemplo de emails_file.txt

~~~
juanpapafrancisco@gmail.com, romeoyjulietasantos@hotmail.com, dddd@email.com.do
jhonpope@vatican.com
friend@email.com, John Smith <john.smith@email.com>, Jane Smith <jane.smith@uconn.edu>
~~~

Utilización: [https://davidtavarez.github.io/2019/tutorial-pwndb-command-line-tool-python/](https://davidtavarez.github.io/2019/tutorial-pwndb-command-line-tool-python/)

**h8mail**

[https://github.com/khast3x/h8mail.git](https://github.com/khast3x/h8mail.git)

**Leaked**

[https://github.com/GitHackTools/Leaked](https://github.com/GitHackTools/Leaked)

<h2><b>Recursos varios</b></h2>

[https://github.com/jivoi/awesome-osint](https://github.com/jivoi/awesome-osint)

[https://urbansecurityresearch.com/2019/08/06/osint-cheet-sheet/](https://urbansecurityresearch.com/2019/08/06/osint-cheet-sheet/)

[https://auditoriadecodigo.com/cheat-sheet-auditoria-codigo-osint/](https://auditoriadecodigo.com/cheat-sheet-auditoria-codigo-osint/)