---
layout: article
title: Pentesting - Enumeración
aside:
  toc: true
sidebar:
  nav: side
header:
  theme: dark
  background: black
---

<h2>Reconocimiento de servicios</h2>

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SSH</div>
</div>

~~~bash
nmap -p 22 -n -v -sV -Pn --script ssh-auth-methods --script-args ssh.user=root 192.168.1.100
nmap -p 22 -n -v -sV -Pn --script ssh-hostkey 192.168.1.100
nmap -p 22 -n -v -sV -Pn --script ssh-brute --script-args userdb=/usr/share/wordlists/Seclists/Usernames/top-usernames-shortlist.txt,passdb=/usr/share/wordlists/Seclists/Passwords/darkweb2017-top10000.txt 192.168.1.100
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">FTP</div>
</div>

~~~bash
sudo nmap -v -p21 --script=default,vuln -oX ftp-scan.xml 192.168.0.0/24
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">DNS</div>
</div>

~~~bash
nmap -T4 -p 53 --script dns-brute
host -l domain.local 192.168.0.2 | grep "has address" | awk '{r=$4" "$1 ; print r}'
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">NFS</div>
</div>

~~~bash
sudo nmap -sV -v -p111 --script=default,vuln -oX nfs.xml 192.168.0.0/24
nmap -v -sV -p 111 --script=nfs-showmount 192.168.1.100
~~~

Si no se utiliza la opción de exportación squash, cualquier usuario root en el equipo cliente puede convertirse en un usuario con acceso privilegiado simplemente ejecutando la orden: su - . Conviene siempre tener activada alguna opción de squash.

~~~bash
rpcinfo -p 192.168.1.100
showmount -e 192.168.1.100
mkdir /mnt/nfs ; mount -t nfs -o nolock 192.168.1.100:/home/victim /mnt/nfs
~~~

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">SMTP</div>
</div>

~~~bash
nmap --script smtp-enum-users.nse -p 25 192.168.1.100
smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t 192.168.1.100
./ismtp.py -h 10.11.1.229:25 -l 1 -e /usr/share/metasploit-framework/data/wordlists/unix_users.txt
./ismtp.py -h 10.11.1.23 -i sender@example.com -x
~~~

[https://github.com/crunchsec/ismtp](https://github.com/crunchsec/ismtp)

<div class="grid">
  <div class="cell cell--20 cell--lg-20 content" id="custom-table-header">LDAP</div>
</div>

~~~bash
nmap --script=ldap-search -p 636,389 -v -oX ldap.xml -sV 192.168.0.0/24
~~~


