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

<h2><b>OSINT</b></h2>
<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Información general</div>
</div>

**Información de la IP**

[http://ipv4info.com/](http://ipv4info.com/) 

[https://www.yougetsignal.com/](https://www.yougetsignal.com/)

**IntelligenceX**
<br>
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

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Puertos abiertos</div>
</div>

[https://www.shodan.io/](https://www.shodan.io/)

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