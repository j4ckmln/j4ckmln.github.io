---
layout: article
title: Wiki
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<table class="table-full">
<tr>
<td class="td-red"><b></b></td>
<td class="table-full td-light-red space-top-botton">La wiki sigue en construcción, por lo que todavía faltan miles de cosas básicas.<br>Seguiré añadiendo más información poco a poco (las horas del día son limitadas xD).<br>¿Podría esperar a terminarlo? Sí, pero la poca información que vaya añadiendo puede ir siéndole útil a alguien :metal:</td>
</tr>
</table>

<h2>Antes de adentrarte en la wiki</h2>

**Algunas cosas que debes saber**

¿Por qué esta información? Para **identificar qué comando será ejecutado en cada máquina**, se utiliza esta información de referencia: 

* **Máquina atacante:** 192.168.1.150
* **Máquina víctima:** 192.168.1.100
* **Directorio del ataque** ~/j4ckmln

Es **importante contar con una carpeta para cada "proyecto"**, lo que nos permitirá guardar de forma ordenada la información.

Tener una estructura de ataque ordenada, aumenta el potencial de uso de esta, puesto que podremos **automatizar y mezclar fácilmente la información obtenida** de cada sistema objetivo que se tenga.

Los comandos de la wiki tendrán un **enfoque de automatización vía línea de comandos**, para hacer pruebas de forma masiva y no estar repitiendo las mismas acciones manuales contra cada sistema que nos encontremos.

<h2>Estructura de ataque</h2>

Para llevar mejor el control de la información que vas a obtener de los sistemas y poder automatizar las pruebas manuales contra los sistemas dentro del alcance, una buena forma es organizar la información en "árbol".

Los ejemplos partirán de esta estructura, obviamente cambiando las redes e IPs por las que necesites.

Teniendo un ejemplo de una red `192.168.1.0/24` e IPs `192.168.1.50,192.168.1.100,192.168.1.120`.

Partiendo del directorio del proyecto (~/j4ckmln).

~~~bash
mkdir -p ~/j4ckmln/$red/{$ip}
mkdir -p /home/kali/j4ckmln/192.168.1.0/{192.168.1.50,192.168.1.100,192.168.1.120} # Ejemplo de carpeta de información de las pruebas para cada host
~~~

One-liner de creación de carpetas
~~~bash
for red in $(ls); do for ip in $(ls $red); do cd ~/j4ckmln/$red/$ip; mkdir -p {nmap,exploits,discover,data,brute,creds,shells} ; touch ip.txt notes.txt url.txt cookies.txt ; pwd | awk -F'/' '{print $NF}' >> ip.txt ; pwd | awk -F'/' '{print "http://"$NF}' > urls.txt ; pwd | awk -F'/' '{print "https://"$NF}' >> urls.txt ; cd ~/j4ckmln ; done; done
~~~

Sólo la estructura de carpetas y ficheros:
~~~bash
mkdir -p {nmap,exploits,discover,data,brute,creds,shells} ; touch ip.txt notes.txt url.txt cookies.txt
~~~

<img src="/resources/output-images/work-folder.png"/>
