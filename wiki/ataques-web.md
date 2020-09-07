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
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSRF</div>
</div>

[https://github.com/jdonsec/AllThingsSSRF](https://github.com/jdonsec/AllThingsSSRF)


