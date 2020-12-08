---
layout: article
title: Pentesting - Fuerza bruta
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h1>Acceso a servicios</h1>

<div class="grid" id="ssh-brute">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH</div>
</div>

~~~bash
ssh  root@192.168.1.100
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -p 22000 root@192.168.1.100
~~~